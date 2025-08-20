# Li-Fi--based-data-transmission
To implement a simple Li-Fi system using visible light to transmit text messages from one Arduino to another using an LED (as transmitter) and a photodiode (as receiver).
//Transmitter code
// Li-Fi Transmitter using LED
const int ledPin = 9;
String message = "HELLO";

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(9600);
}

void loop() {
  for (int i = 0; i < message.length(); i++) {
    char c = message[i];
    for (int j = 7; j >= 0; j--) {
      bool bitValue = bitRead(c, j);
      digitalWrite(ledPin, bitValue);
      delay(100); // bit delay
    }
  }
  delay(2000); // Wait before sending again
}
//Receiver code
// Li-Fi Receiver using Photodiode
const int sensorPin = A0;
int threshold = 500; // Adjust based on ambient light

void setup() {
  Serial.begin(9600);
}

void loop() {
  char receivedChar = 0;

  for (int i = 7; i >= 0; i--) {
    int sensorValue = analogRead(sensorPin);
    bool bitValue = sensorValue > threshold ? 1 : 0;
    bitWrite(receivedChar, i, bitValue);
    delay(100); // bit delay (must match transmitter)
  }

  Serial.print(receivedChar); // Print received character
}
