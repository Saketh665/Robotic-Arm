# Robotic-Arm
Using NODEMCU
#include <ESP8266WiFi.h>
#include <Servo.h>

// Define servo objects for each joint of the robotic arm
Servo baseServo;    // Base rotation
Servo shoulderServo; // Shoulder movement
Servo elbowServo;    // Elbow movement
Servo wristServo;    // Wrist movement
Servo gripServo;    // Gripper

// Pins where the servos are connected
const int basePin = D1;
const int shoulderPin = D2;
const int elbowPin = D3;
const int wristPin = D4;
const int gripPin = D5;

// WiFi credentials
const char* ssid = "your_SSID";
const char* password = "your_PASSWORD";

void setup() {
  // Initialize serial communication
  Serial.begin(115200);

  // Attach the servos to the corresponding pins
  baseServo.attach(basePin);
  shoulderServo.attach(shoulderPin);
  elbowServo.attach(elbowPin);
  wristServo.attach(wristPin);
  gripServo.attach(gripPin);

  // Initial positions of the servos
  baseServo.write(90);     // Middle position for base
  shoulderServo.write(90); // Middle position for shoulder
  elbowServo.write(90);    // Middle position for elbow
  wristServo.write(90);    // Middle position for wrist
  gripServo.write(0);      // Gripper closed

  // Connect to WiFi
  connectToWiFi();
}

void loop() {
  // Control robotic arm via serial commands or any input method
  if (Serial.available()) {
    char command = Serial.read();  // Read command from serial

    // Example command-based control
    switch (command) {
      case 'w': // Move shoulder up
        moveServo(shoulderServo, 10);  // Increase the angle
        break;
      case 's': // Move shoulder down
        moveServo(shoulderServo, -10);  // Decrease the angle
        break;
      case 'a': // Rotate base left
        moveServo(baseServo, -10);
        break;
      case 'd': // Rotate base right
        moveServo(baseServo, 10);
        break;
      case 'e': // Extend elbow
        moveServo(elbowServo, 10);
        break;
      case 'q': // Retract elbow
        moveServo(elbowServo, -10);
        break;
      case 'r': // Move wrist up
        moveServo(wristServo, 10);
        break;
      case 'f': // Move wrist down
        moveServo(wristServo, -10);
        break;
      case 'g': // Open gripper
        moveServo(gripServo, 10);
        break;
      case 'h': // Close gripper
        moveServo(gripServo, -10);
        break;
    }
  }
}

void moveServo(Servo &servo, int angleStep) {
  int currentAngle = servo.read();  // Get the current position
  int newAngle = constrain(currentAngle + angleStep, 0, 180);  // Calculate new angle within limits
  servo.write(newAngle);  // Set the servo to the new angle
}

void connectToWiFi() {
  Serial.println("Connecting to WiFi...");
  WiFi.begin(ssid, password);

  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("WiFi connected!");
}

