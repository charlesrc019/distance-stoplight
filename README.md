# Distance Stoplight

## Overview
The **Distance Stoplight** is an IoT-based system designed to measure distances using an ultrasonic sensor and provide real-time data to a server or client. It utilizes an ESP8266 microcontroller, communicates via WiFi, and makes use of an ultrasonic sensor (HC-SR04) to measure distances in centimeters. The system can serve the measured data via a simple HTTP server, providing JSON responses to any connected clients.

This component is designed to be part of a two-part system. It measures the distance and sends the data to a server or makes it available for web clients. The system is intended for use in applications like automated stoplights or distance-triggered systems.

## Features
1. **Distance Measurement**:
   - Uses an ultrasonic sensor (HC-SR04) to measure distances.
   - Provides real-time distance measurements in centimeters.

2. **WiFi Connectivity**:
   - Connects to a WiFi network using the `WiFiManager` library.
   - Automatically connects to the network on startup or prompts the user to select a network.

3. **Web Server**:
   - Runs a web server on port 80 to provide distance information to clients.
   - Delivers the distance measurement in a JSON format on request.

4. **Network Information Logging**:
   - Sends network information (e.g., local IP address) to a specified server.
   - Uses the `HTTPClient` library to post data to a remote server.

5. **Data Serialization**:
   - Distance and update interval are serialized into JSON format for easy integration with external systems.
   
6. **Customizable Delay**:
   - The distance measurement is updated at a user-configurable interval defined by `MEASURE_DELAY` (default 750 ms).

## Installation Instructions
1. **Hardware Setup**:
   - Connect the ultrasonic sensor to the ESP8266 as follows:
     - `TRIG` pin to pin 13 on the ESP8266.
     - `ECHO` pin to pin 15 on the ESP8266.

2. **Software Setup**:
   - Upload the provided code to your ESP8266 using the Arduino IDE.
   - Ensure the necessary libraries are installed:
     - `ESP8266WiFi`
     - `DNSServer`
     - `WiFiManager`
     - `ArduinoJson`
     - `ESP8266HTTPClient`
     - `ESP8266WebServer`
     - `NewPing`

3. **WiFi Setup**:
   - On the first run, the device will create an access point for the user to connect and configure the WiFi credentials. After configuration, the ESP8266 will automatically connect to the specified WiFi network.

4. **Remote Server Configuration**:
   - The system posts network information (like the local IP address) to the specified server (`DB_URL`).
   - Make sure the server is prepared to accept POST requests and handle the `id` and `doc` parameters.

## Usage
- After the initial setup, the system will continuously measure distances using the ultrasonic sensor.
- The web server running on the ESP8266 will respond to HTTP requests on port 80 with the current distance measurement in JSON format:
  ```json
  {
    "update_interval": "750",
    "distance": "120"
  }
  ```
  This JSON response contains:
  - `update_interval`: The time interval (in milliseconds) between measurements.
  - `distance`: The current distance measured by the ultrasonic sensor in centimeters.

- The system will continue to update and serve the distance data at regular intervals defined by `MEASURE_DELAY`.

## Configuration
- **MEASURE_DELAY**: Adjust this variable to set the interval between distance measurements (default is 750 ms).
- **HOSTNAME**: The hostname for the ESP8266 device when connecting to WiFi (default is `"arduino2"`).
- **DB_URL**: The URL where network information is posted (default is `"http://www.chris-eng.com/api/data/MongoLite.php"`). Update this with the correct server URL where you want to post the network information.

## Troubleshooting
- **WiFi connection issues**: If the device is unable to connect to WiFi, reset the device and try again. Ensure that the credentials are correct.
- **Distance reading accuracy**: Ensure the ultrasonic sensor is connected correctly and is unobstructed. The sensor works best when there are no objects in the way of the sensor’s direct line of sight.
