```cpp
#include <Wire.h>          // Include I2C communication library
#include <Servo.h>         // Include Servo motor control library
#include <VL53L0X.h>       // Include library for the Time-of-Flight distance sensors

// ===================== SENSOR SETUP =====================
#define LOX1_ADDRESS 0x30  // Define custom I2C address for sensor 1
#define LOX2_ADDRESS 0x31  // Define custom I2C address for sensor 2

#define SHT_LOX1 7         // Pin connected to XSHUT on sensor 1 (for reset/init)
#define SHT_LOX2 6         // Pin connected to XSHUT on sensor 2 (for reset/init)

VL53L0X sensor1;           // Create sensor 1 object
VL53L0X sensor2;           // Create sensor 2 object

// ===================== SERVO SETUP =====================
Servo servo1;              // Create servo 1 object
Servo servo2;              // Create servo 2 object

#define SERVO1_PIN 9       // Signal pin for servo 1
#define SERVO2_PIN 10      // Signal pin for servo 2

const uint8_t closedAngle = 0;   // Position for "off" or "closed"
uint8_t activeAngle1 = 45;       // Variable for servo 1 "on" position
uint8_t activeAngle2 = 45;       // Variable for servo 2 "on" position

unsigned long servoStartTime = 0;        // Variable to store when the dispenser started
const unsigned long servoDuration = 5000UL; // How long to dispense (5 seconds)

bool servosActive = false;               // State tracker for if servos are currently moving
bool modeSelected = false;               // State tracker for if a menu option was picked

// ===================== LAST VALID READINGS =====================
int lastValidDistance1 = -1;             // Store previous sensor 1 reading (-1 means no data)
int lastValidDistance2 = -1;             // Store previous sensor 2 reading (-1 means no data)

// ===================== FUNCTION DECLARATIONS =====================
void initSensors();         // Prototypes for functions defined later in the script
void handleMenu();
void startServos();
void updateServos();
void displayMenu();
void tuneSensor(VL53L0X &sensor);

// ===================== SETUP =====================
void setup() {
  Serial.begin(9600);       // Initialize serial monitor at 9600 baud
  Wire.begin();             // Initialize I2C bus

  pinMode(SHT_LOX1, OUTPUT); // Set sensor 1 shutdown pin as output
  pinMode(SHT_LOX2, OUTPUT); // Set sensor 2 shutdown pin as output

  // Shut both sensors down first to prepare for individual addressing
  digitalWrite(SHT_LOX1, LOW); // Force sensor 1 into reset
  digitalWrite(SHT_LOX2, LOW); // Force sensor 2 into reset
  delay(10);                   // Brief pause to ensure reset

  initSensors();               // Run the custom sensor initialization logic

  servo1.attach(SERVO1_PIN);   // Assign pin 9 to servo 1
  servo2.attach(SERVO2_PIN);   // Assign pin 10 to servo 2
  servo1.write(closedAngle);   // Move servo 1 to closed position
  servo2.write(closedAngle);   // Move servo 2 to closed position

  displayMenu();               // Print the mode options to Serial
}

// ===================== LOOP =====================
void loop() {
  handleMenu();                // Check for user input in Serial monitor

  if (modeSelected && !servosActive) { // If system is armed but not currently dispensing
    bool valid1 = false;       // Local flag for sensor 1 success
    bool valid2 = false;       // Local flag for sensor 2 success

    uint16_t mm1 = sensor1.readRangeSingleMillimeters(); // Get distance from sensor 1
    if (!sensor1.timeoutOccurred()) {                    // If sensor responded in time
      lastValidDistance1 = mm1 / 10;                     // Convert mm to cm and save
      valid1 = true;                                     // Mark as successful read
    }

    uint16_t mm2 = sensor2.readRangeSingleMillimeters(); // Get distance from sensor 2
    if (!sensor2.timeoutOccurred()) {                    // If sensor responded in time
      lastValidDistance2 = mm2 / 10;                     // Convert mm to cm and save
      valid2 = true;                                     // Mark as successful read
    }

    Serial.print(F("S1: "));                             // Log status to Serial
    if (valid1) {
      Serial.print(lastValidDistance1);                  // Print current distance
      Serial.print(F(" cm (valid)"));
    } else {
      Serial.print(F("invalid"));                        // Show error if read failed
      if (lastValidDistance1 != -1) {                    // If we have a past value, show it
        Serial.print(F(" / last="));
        Serial.print(lastValidDistance1);
        Serial.print(F(" cm"));
      }
    }

    Serial.print(F(" | S2: "));
    if (valid2) {
      Serial.print(lastValidDistance2);                  // Print current distance
      Serial.println(F(" cm (valid)"));
    } else {
      Serial.print(F("invalid"));
      if (lastValidDistance2 != -1) {
        Serial.print(F(" / last="));
        Serial.print(lastValidDistance2);
        Serial.print(F(" cm"));
      }
      Serial.println();
    }

    // Checking logic for triggering the dispenser
    if (lastValidDistance1 != -1 && lastValidDistance2 != -1) { // If both sensors have data
      if (lastValidDistance1 >= 20 || lastValidDistance2 >= 20) { // If distance > 20cm (empty)
        Serial.println(F(">>> REFILL NOW"));
      } else {                                                    // If object is close
        Serial.println(F("Triggered!"));
        startServos();                                            // Open the doors
      }
    } else {
      Serial.println(F("Waiting for valid readings..."));        // Waiting for first data
    }

    delay(300);                                                   // Slow down loop slightly
  }

  updateServos();                                                 // Check if it's time to close
}

// ===================== SENSOR INIT =====================
void initSensors() {
  Serial.println(F("Initializing sensors..."));

  // Start sensor 1 only
  digitalWrite(SHT_LOX1, HIGH); // Wake up sensor 1
  delay(10);                    // Wait for it to boot

  sensor1.setTimeout(500);      // Set 0.5s timeout for I2C
  if (!sensor1.init()) {        // Try to initialize
    Serial.println(F("Sensor 1 FAILED"));
    while (1) {}                // Halt if sensor fails
  }
  sensor1.setAddress(LOX1_ADDRESS); // Change address from default 0x29 to 0x30
  tuneSensor(sensor1);              // Apply performance settings
  Serial.println(F("Sensor 1 OK"));

  // Start sensor 2
  digitalWrite(SHT_LOX2, HIGH); // Wake up sensor 2 (sensor 1 stays on at its new address)
  delay(10);

  sensor2.setTimeout(500);
  if (!sensor2.init()) {
    Serial.println(F("Sensor 2 FAILED"));
    while (1) {}
  }
  sensor2.setAddress(LOX2_ADDRESS); // Change address from default 0x29 to 0x31
  tuneSensor(sensor2);              // Apply performance settings
  Serial.println(F("Sensor 2 OK"));
}

void tuneSensor(VL53L0X &sensor) {
  sensor.setSignalRateLimit(0.1);                               // Set minimum signal strength
  sensor.setVcselPulsePeriod(VL53L0X::VcselPeriodPreRange, 18); // Increase pulse period
  sensor.setVcselPulsePeriod(VL53L0X::VcselPeriodFinalRange, 14); // Increase pulse period
  sensor.setMeasurementTimingBudget(200000);                    // Set 200ms budget for accuracy
}

// ===================== MENU =====================
void displayMenu() {
  Serial.println();                                     // Print formatting and options
  Serial.println(F("--- MODE SELECT ---"));
  Serial.println(F("1: 50/50"));
  Serial.println(F("2: 60/30"));
  Serial.println(F("3: 30/60"));
}

void handleMenu() {
  if (!modeSelected && Serial.available()) {            // If waiting for input and serial data arrives
    char c = Serial.read();                             // Read the incoming byte

    if (c == '1') {                                     // Set angles based on user choice
      activeAngle1 = 65;
      activeAngle2 = 65;
    }
    else if (c == '2') {
      activeAngle1 = 70;
      activeAngle2 = 55;
    }
    else if (c == '3') {
      activeAngle1 = 55;
      activeAngle2 = 70;
    }
    else {
      return;                                           // Ignore invalid keys
    }

    Serial.println(F("Mode selected. System armed."));
    modeSelected = true;                                // System is now ready to detect

    // Reset saved readings for fresh cycle
    lastValidDistance1 = -1;
    lastValidDistance2 = -1;
  }
}

// ===================== SERVO CONTROL =====================
void startServos() {
  Serial.println(F("Inside startServos"));

  servo1.write(activeAngle1);   // Move servo 1 to its "on" position
  servo2.write(activeAngle2);   // Move servo 2 to its "on" position

  servoStartTime = millis();    // Record the timestamp when we opened
  servosActive = true;          // Flag that we are in a dispensing state
}

void updateServos() {
  // Check if servos are open and if 5 seconds (servoDuration) have passed
  if (servosActive && millis() - servoStartTime >= servoDuration) {
    servo1.write(closedAngle);  // Close servo 1
    servo2.write(closedAngle);  // Close servo 2

    servosActive = false;       // Clear active flag
    modeSelected = false;       // Require user to pick a mode again

    Serial.println(F("Cycle complete."));
    displayMenu();              // Re-prompt user
  }
}
```
