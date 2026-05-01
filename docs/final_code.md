```cpp
#include <Wire.h>           // Include I2C communication library for sensors
#include <Servo.h>          // Include the standard Arduino Servo control library
#include <VL53L0X.h>        // Include the library for the VL53L0X Time-of-Flight sensor

// ===================== SENSOR SETUP =====================
#define LOX1_ADDRESS 0x30   // Define a unique I2C address for the first sensor
#define LOX2_ADDRESS 0x31   // Define a unique I2C address for the second sensor
#define SHT_LOX1 7          // Define the shutdown (XSHUT) pin for sensor 1
#define SHT_LOX2 6          // Define the shutdown (XSHUT) pin for sensor 2

VL53L0X sensor1;            // Create an object instance for the first distance sensor
VL53L0X sensor2;            // Create an object instance for the second distance sensor

// ===================== SERVO SETUP =====================
Servo servo1;               // Create a servo object to control the first motor
Servo servo2;               // Create a servo object to control the second motor
#define SERVO1_PIN 9        // Assign digital pin 9 to the first servo
#define SERVO2_PIN 10       // Assign digital pin 10 to the second servo

const uint8_t closedAngle = 0;   // Set the default resting/closed angle to 0 degrees
const uint8_t activeAngle = 45;  // Set the base operational/shaking angle to 45 degrees

// ===================== STATE MANAGEMENT =====================
int jitterCount1 = 0;       // Variable to store how many shakes Servo 1 should perform
int jitterCount2 = 0;       // Variable to store how many shakes Servo 2 should perform

bool servosActive = false;   // Flag to track if the servos are currently in motion
bool modeSelected = false;   // Flag to track if the user has picked a menu option

int lastValidDistance1 = -1; // Stores the most recent successful reading from sensor 1
int lastValidDistance2 = -1; // Stores the most recent successful reading from sensor 2

// ===================== SETUP =====================
void setup() {
  Serial.begin(57600);       // Initialize serial communication at 57600 baud rate
  Wire.begin();              // Initialize the I2C bus

  pinMode(SHT_LOX1, OUTPUT); // Set sensor 1 shutdown pin as an output
  pinMode(SHT_LOX2, OUTPUT); // Set sensor 2 shutdown pin as an output

  digitalWrite(SHT_LOX1, LOW); // Pull sensor 1 shutdown pin low to reset
  digitalWrite(SHT_LOX2, LOW); // Pull sensor 2 shutdown pin low to reset
  delay(10);                   // Short pause to ensure sensors have powered down

  initSensors();               // Call helper function to assign addresses and boot sensors

  servo1.attach(SERVO1_PIN);   // Connect the first servo object to its physical pin
  servo2.attach(SERVO2_PIN);   // Connect the second servo object to its physical pin
  
  servo1.write(closedAngle);   // Move servo 1 to the starting closed position
  servo2.write(closedAngle);   // Move servo 2 to the starting closed position

  displayMenu();               // Print the control options to the Serial Monitor
}

// ===================== LOOP =====================
void loop() {
  handleMenu();                // Check for user input from the Serial Monitor

  if (modeSelected && !servosActive) { // Run sensor logic only if armed and not currently moving
    bool valid1 = false;       // Local flag to track if sensor 1 read correctly this loop
    bool valid2 = false;       // Local flag to track if sensor 2 read correctly this loop

    uint16_t mm1 = sensor1.readRangeSingleMillimeters(); // Get distance reading in mm from sensor 1
    if (!sensor1.timeoutOccurred()) {                    // Check if the sensor responded in time
      lastValidDistance1 = mm1 / 10;                     // Convert mm to cm and update global variable
      valid1 = true;                                     // Mark this reading as valid
    }

    uint16_t mm2 = sensor2.readRangeSingleMillimeters(); // Get distance reading in mm from sensor 2
    if (!sensor2.timeoutOccurred()) {                    // Check if the sensor responded in time
      lastValidDistance2 = mm2 / 10;                     // Convert mm to cm and update global variable
      valid2 = true;                                     // Mark this reading as valid
    }

    Serial.print(F("S1: "));                             // Label for sensor 1 output
    if (valid1) { Serial.print(lastValidDistance1); Serial.print(F(" cm")); } // Print cm if valid
    else { Serial.print(F("invalid")); }                 // Print error message if reading failed

    Serial.print(F(" | S2: "));                          // Separator for sensor 2 output
    if (valid2) { Serial.print(lastValidDistance2); Serial.println(F(" cm")); } // Print cm if valid
    else { Serial.println(F("invalid")); }               // Print error message if reading failed

    if (lastValidDistance1 != -1 && lastValidDistance2 != -1) { // If both sensors have data
      if (lastValidDistance1 >= 6 || lastValidDistance2 >= 6) {  // Check if bin is empty (distance > 6cm)
        Serial.println(F(">>> REFILL NOW"));                     // Alert user to refill
      } else {                                                   // If something is detected close by
        Serial.println(F("Triggered!"));                         // Confirm the system is acting
        runJitterCycle();                                        // Execute the servo shaking routine
      }
    }
    
    delay(300); // Wait 300ms before the next sensor poll to prevent spamming
  }
}

// ===================== SERVO CONTROL =====================

void runJitterCycle() {
  servosActive = true;                         // Set flag to prevent loop logic interference
  int maxShakes = max(jitterCount1, jitterCount2); // Determine which servo needs more shakes
  
  Serial.println(F("Dispensing..."));          // Log status to serial
  
  for (int i = 0; i < maxShakes; i++) {        // Loop through the shake count
    // 1. Move Up (Higher Jitter)
    if (i < jitterCount1) servo1.write(activeAngle + 10); // Adjust servo 1 upward
    if (i < jitterCount2) servo2.write(activeAngle + 10); // Adjust servo 2 upward
    delay(100);                                           // Wait for movement
    
    // 2. Move Down (Lower Jitter)
    if (i < jitterCount1) servo1.write(activeAngle - 5);  // Adjust servo 1 downward
    if (i < jitterCount2) servo2.write(activeAngle - 5);  // Adjust servo 2 downward
    delay(100);                                           // Wait for movement

    // FIX: Snap a servo closed the INSTANT its specific count is reached
    if (i == (jitterCount1 - 1)) {             // Check if servo 1 finished its shakes
      servo1.write(closedAngle);               // Immediately return servo 1 to closed position
    }
    if (i == (jitterCount2 - 1)) {             // Check if servo 2 finished its shakes
      servo2.write(closedAngle);               // Immediately return servo 2 to closed position
    }
  }

  Serial.println(F("Cycle complete. Returning to menu.")); // Notify user cycle is over
  
  servosActive = false;  // Allow sensors to be read again
  modeSelected = false;  // Reset selection to require new user input
  displayMenu();         // Show menu again for the next operation
}

void shakeGatesManual() {
  Serial.println(F(">>> MANUAL SHAKE..."));    // Log manual override status
  for (int i = 0; i < 2; i++) {                // Run exactly two shake cycles
    servo1.write(activeAngle + 10);            // Move both servos up
    servo2.write(activeAngle + 10);
    delay(100);                                // Wait
    servo1.write(activeAngle - 5);             // Move both servos down
    servo2.write(activeAngle - 5);
    delay(100);                                // Wait
  }
  servo1.write(closedAngle);                   // Force servo 1 to closed state
  servo2.write(closedAngle);                   // Force servo 2 to closed state
  Serial.println(F("Manual shake complete.")); // Log completion
  displayMenu();                               // Return to main menu
}

// ===================== HELPERS =====================

void initSensors() {
  Serial.println(F("Initializing sensors...")); // Start of initialization sequence
  digitalWrite(SHT_LOX1, HIGH);                 // Power on the first sensor
  delay(10);                                    // Wait for boot-up
  sensor1.init();                               // Initialize internal sensor settings
  sensor1.setAddress(LOX1_ADDRESS);             // Assign the custom I2C address 0x30
  sensor1.setMeasurementTimingBudget(200000);   // Set high accuracy timing (200ms)

  digitalWrite(SHT_LOX2, HIGH);                 // Power on the second sensor
  delay(10);                                    // Wait for boot-up
  sensor2.init();                               // Initialize internal sensor settings
  sensor2.setAddress(LOX2_ADDRESS);             // Assign the custom I2C address 0x31
  sensor2.setMeasurementTimingBudget(200000);   // Set high accuracy timing (200ms)
  Serial.println(F("Sensors OK"));              // Confirm setup success
}

void displayMenu() {
  Serial.println();                             // Print empty line for readability
  Serial.println(F("--- JITTER DISPENSER MENU ---")); // Header
  Serial.println(F("1: S1 x5 | S2 x5"));        // Option 1 description
  Serial.println(F("2: S1 x3 | S2 x5"));        // Option 2 description
  Serial.println(F("3: S1 x5 | S2 x3"));        // Option 3 description
  Serial.println(F("4: STUCK - Shake Gates x2")); // Option 4 description
}

void handleMenu() {
  if (!modeSelected && Serial.available()) {    // If waiting for input and data is in buffer
    char c = Serial.read();                     // Read the incoming character
    if (c == '1') { jitterCount1 = 5; jitterCount2 = 5; modeSelected = true; }      // Set mode 1
    else if (c == '2') { jitterCount1 = 3; jitterCount2 = 5; modeSelected = true; } // Set mode 2
    else if (c == '3') { jitterCount1 = 5; jitterCount2 = 3; modeSelected = true; } // Set mode 3
    else if (c == '4') { shakeGatesManual(); return; }                              // Run manual shake
    else { return; }                                                                // Ignore other keys

    Serial.println(F("Mode selected. System armed.")); // Confirm selection to user
  }
}
```
