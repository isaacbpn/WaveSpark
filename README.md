# ⚡ WaveSpark - Touch-Free Desk Switch

A beginner-friendly physical computing project that changes colors when you wave your hand over it. Perfect as a warm-up hardware project to learn microcontrollers and sensors!

## 🛠️ Hardware Requirements
* **Microcontroller:** Arduino Uno, Nano, or compatible board
* **Sensor:** HC-SR04 Ultrasonic Distance Sensor
* **Outputs:** 2x LEDs (e.g., Green for standby, Blue for action)
* **Protection:** 2x 220-ohm Resistors
* **Prototyping:** 1x Solderless Breadboard & Jumper wires

## 🔌 Wiring Diagram
Connect your hardware components to the Arduino pins as follows:

| Component | Component Pin | Arduino Pin |
| :--- | :--- | :--- |
| **HC-SR04 Sensor** | VCC | 5V |
| **HC-SR04 Sensor** | Trig | Pin 9 |
| **HC-SR04 Sensor** | Echo | Pin 10 |
| **HC-SR04 Sensor** | GND | GND |
| **Standby LED (Green)**| Anode (+) via Resistor | Pin 11 |
| **Action LED (Blue)**  | Anode (+) via Resistor | Pin 12 |
| **Both LEDs**          | Cathode (-) | GND |

## 💻 Arduino Source Code (`WaveSpark.ino`)

```cpp
/**
 * @file WaveSpark.ino
 * @brief Touch-free proximity switch using an ultrasonic sensor.
 */

// Pin Definitions
const int TRIG_PIN = 9;      // Ultrasonic sensor Trigger
const int ECHO_PIN = 10;     // Ultrasonic sensor Echo
const int LED_NORMAL = 11;   // Standby status LED
const int LED_WAVE = 12;     // Active hand-wave LED

// Configuration Constraints
const int TRIGGER_DISTANCE_CM = 15; // Distance threshold to trigger the switch

void setup() {
  // Initialize Sensor Pins
  pinMode(TRIG_PIN, OUTPUT);
  pinMode(ECHO_PIN, INPUT);
  
  // Initialize LED Pins
  pinMode(LED_NORMAL, OUTPUT);
  pinMode(LED_WAVE, OUTPUT);
  
  // Set default initial state
  digitalWrite(LED_NORMAL, HIGH);
  digitalWrite(LED_WAVE, LOW);
}

void loop() {
  // Send a 10-microsecond pulse to trigger the ultrasonic sensor
  digitalWrite(TRIG_PIN, LOW);
  delayMicroseconds(2);
  digitalWrite(TRIG_PIN, HIGH);
  delayMicroseconds(10);
  digitalWrite(TRIG_PIN, LOW);
  
  // Measure the bounce-back duration in microseconds
  long duration = pulseIn(ECHO_PIN, HIGH);
  
  // Calculate distance in centimeters (speed of sound is ~0.034 cm/us)
  long distance = duration * 0.034 / 2;

  // Evaluation logic for proximity detection
  if (distance > 0 && distance < TRIGGER_DISTANCE_CM) { 
    digitalWrite(LED_NORMAL, LOW);  // Deactivate standby indicator
    digitalWrite(LED_WAVE, HIGH);   // Activate interaction indicator
  } else {
    digitalWrite(LED_NORMAL, HIGH); // Revert to standby indicator
    digitalWrite(LED_WAVE, LOW);    // Deactivate interaction indicator
  }
  
  // Short polling delay to stabilize sensor readings
  delay(100); 
}
```

## 🚀 How to Run It
1. Clone this repository or copy the code block above into the **Arduino IDE**.
2. Save the file as `WaveSpark.ino`.
3. Select your target Arduino board and correct COM Port from the IDE menu.
4. Click **Upload** to flash the code onto your hardware.
