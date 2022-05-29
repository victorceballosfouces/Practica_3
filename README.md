# Practica_3

## Codigo_3.1_Web

###  Main.cpp
```cpp
#include <Arduino.h>
#include <WiFi.h>
#include <WebServer.h>
// SSID & Password
void handle_root();

extern String HTML;
const char* ssid = "Xiaomi_11T_Pro"; // Enter your SSID here
const char* password = "f5cbd8a82232"; //Enter your Password here
WebServer server(80);
// Object of WebServer(HTTP port, 80 is defult)
void setup() {
Serial.begin(115200);
Serial.println("Try Connecting to ");
Serial.println(ssid);
// Connect to your wi-fi modem
WiFi.begin(ssid, password);
// Check wi-fi is connected to wi-fi network
while (WiFi.status() != WL_CONNECTED) {
delay(1000);
Serial.print(".");
}
Serial.println("");
Serial.println("WiFi connected successfully");
Serial.print("Got IP: ");
Serial.println(WiFi.localIP()); //Show ESP32 IP on serial
server.on("/", handle_root);
server.begin();
Serial.println("HTTP server started");
delay(100);
}
void loop() {
server.handleClient();
}
// Handle root url (/)
void handle_root() {
server.send(200, "text/html", HTML);
}
```
###  Web.cpp
```cpp
#include <Arduino.h>
#include <WiFi.h>
#include <WebServer.h>

// HTML & CSS contents which display on web server
String HTML = 
"<!DOCTYPE html> \
<html>\
<body>\
<h1> My Primera Pagina con ESP32 - Station Mode &#128221;</h1>\
<h5>Hello World<h1>\
</body>\
</html> ";
```
## Funcionamiento


## Codigo_3.2_Bluetooth
```cpp
#include <Arduino.h>

#include "BluetoothSerial.h"
#if !defined(CONFIG_BT_ENABLED) || !defined(CONFIG_BLUEDROID_ENABLED)
#error Bluetooth is not enabled! Please run `make menuconfig` to and enable it
#endif
BluetoothSerial SerialBT;
void setup() {
Serial.begin(115200);
SerialBT.begin("1234"); //Bluetooth device name
Serial.println("The device started, now you can pair it with bluetooth!");
}
void loop() {
if (Serial.available()) {
SerialBT.write(Serial.read());
}
if (SerialBT.available()) {
Serial.write(SerialBT.read());
}
delay(20);
}
```
## Funcionamiento

