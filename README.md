# -Ultrasonic-Gate-Opener-with-Servo-Motor

This project uses an ultrasonic distance sensor (HC-SR04) and a servo motor to create an automatic gate system. When an object is detected within a certain distance (20 cm), the servo motor opens the gate for 3 seconds and then closes it.

## 🔧 Components Used

- Arduino Nano
- HC-SR04 Ultrasonic Sensor
- Tower Pro SG90 Servo Motor
- Breadboard and Jumper Wires
- USB cable and optional external power supply (for servo)

## 🛠 Circuit Diagram

markdown
![Circuit Diagram](./circuit-diagram-servo-distance)


## 🔌 Wiring

| Component       | Arduino Nano Pin |
|----------------|------------------|
| HC-SR04 VCC     | 5V               |
| HC-SR04 GND     | GND              |
| HC-SR04 Trig    | D8               |
| HC-SR04 Echo    | D7               |
| Servo Signal    | D3               |
| Servo VCC       | 5V (or external) |
| Servo GND       | GND              |

## 📜 Code

cpp
#include <Servo.h>

const int trigPin = 8;
const int echoPin = 7;
Servo gateServo;

long duration;
int distance;

void setup() {
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  gateServo.attach(3);       // Connects servo to pin D3
  gateServo.write(0);        // Initial position: gate closed
  Serial.begin(9600);
}

void loop() {
  // Send ultrasonic pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Read echo response
  duration = pulseIn(echoPin, HIGH);
  distance = duration * 0.034 / 2;

  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Check if object is within 20 cm
  if (distance > 0 && distance <= 20) {
    gateServo.write(90);   // Open gate
    delay(3000);            // Wait 3 seconds
    gateServo.write(0);     // Close gate
  }

  delay(500); // Delay before next reading
}


## 📁 How to Use

1. Connect the components as shown in the diagram.
2. Upload the code to your Arduino Nano.
3. Open the Serial Monitor to see distance readings.
4. Move an object within 20 cm of the sensor — the servo will rotate to open the gate, wait 3 seconds, then return to closed position.
