/*
  Project: Automatic Dustbin with Ultrasonic Sensor and Servo Motor
  Description:
    This project uses an Arduino, an ultrasonic sensor (HC-SR04), and a servo motor
    to create an automatic dustbin. When the ultrasonic sensor detects an object,
    such as a hand, within 5 cm, the servo motor rotates to open the dustbin lid
    by 80 degrees. After a 1-second delay, the servo returns the lid to the closed
    position at 0 degrees.

  Components:
    - Ultrasonic Sensor (HC-SR04): Measures distance to detect objects within a close range.
    - Servo Motor: Opens and closes the dustbin lid based on detection.
    - Arduino Microcontroller: Processes data from the ultrasonic sensor and controls the servo motor.

  Applications:
    - Touchless Waste Disposal: Enables hygienic, hands-free access to the dustbin.

  Wiring:
    - Trig Pin (Ultrasonic) -> Pin 3 on Arduino
    - Echo Pin (Ultrasonic) -> Pin 2 on Arduino
    - Servo Signal Pin -> Pin 9 on Arduino
*/

#include <Servo.h>

// Define pins for ultrasonic sensor
#define trigPin 3   // Trigger pin for the ultrasonic sensor
#define echoPin 2   // Echo pin for the ultrasonic sensor

Servo servo; // Create a Servo object to control the servo motor

void setup() {
  Serial.begin(9600);    // Initialize serial communication for debugging
  pinMode(trigPin, OUTPUT); // Set trigger pin as output
  pinMode(echoPin, INPUT);  // Set echo pin as input
  servo.attach(9);       // Attach the servo to pin 9
  servo.write(0);        // Initialize servo position at 0 degrees (lid closed)
}

void loop() {
  long duration, distance; // Variables for the duration of echo pulse and calculated distance

  // Trigger an ultrasonic pulse
  digitalWrite(trigPin, LOW);  // Ensure trigger pin is low initially
  delayMicroseconds(2);        // Wait 2 microseconds
  digitalWrite(trigPin, HIGH); // Set trigger pin high to start pulse
  delayMicroseconds(10);       // Wait 10 microseconds for pulse to send
  digitalWrite(trigPin, LOW);  // Set trigger pin low again

  // Measure the duration of the echo pulse
  duration = pulseIn(echoPin, HIGH); // Capture time in microseconds for echo to return
  distance = (duration / 2) / 29.1;  // Convert duration to distance in cm

  // Check if a hand or object is within 5 cm to open the dustbin lid
  if (distance <= 5) {
    Serial.print("Object detected at: ");
    Serial.print(distance);
    Serial.println(" cm");

    // Move the servo to 80 degrees to open the lid
    servo.write(80);         // Open lid by setting servo to 80 degrees
    delay(1500);             // Wait 1 second with lid open

    // Move the servo back to 0 degrees to close the lid
    servo.write(0);          // Close lid by setting servo back to 0 degrees
  } else {
    // Inform if no object is within the 5 cm range
    Serial.println("No object within 5 cm range");
  }

  delay(500); // Delay before the next reading
}
