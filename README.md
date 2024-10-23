# Heart-beat-sensor-using-aurdino-uno
// Pin definitions
const int sensorPin = A0;    // Analog input pin for the heartbeat sensor
int sensorValue = 0;         // Variable to store sensor value
int threshold = 550;         // Threshold for detecting a heartbeat
unsigned long lastBeatTime = 0; // To store the time of the last beat
int beatsPerMinute = 0;      // Variable to store BPM
int beatCount = 0;           // Counting heartbeats in a time interval

// Time variables
unsigned long intervalStartTime = 0; // Start time for counting beats
const int intervalLength = 10000;    // 10 seconds interval for accurate BPM

void setup() {
  Serial.begin(9600);        // Initialize serial communication at 9600 bps
  pinMode(sensorPin, INPUT); // Set the sensor pin as an input
}

void loop() {
  // Read the value from the sensor
  sensorValue = analogRead(sensorPin);

  // Check if the sensor value is above the threshold, indicating a heartbeat
  if (sensorValue > threshold && millis() - lastBeatTime > 250) {
    lastBeatTime = millis();  // Update the time of the last beat
    beatCount++;              // Increment the heartbeat count
  }

  // Calculate BPM every 10 seconds
  if (millis() - intervalStartTime >= intervalLength) {
    // Calculate BPM: (beats in the interval / interval length) * 60 seconds
    beatsPerMinute = (beatCount * 60000) / (millis() - intervalStartTime);

    // Print the BPM to the Serial Monitor
    Serial.print("Heart Rate: ");
    Serial.print(beatsPerMinute);
    Serial.println(" BPM");

    // Reset variables for the next interval
    intervalStartTime = millis();
    beatCount = 0;
  }
}
