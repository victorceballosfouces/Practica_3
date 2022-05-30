# Practica_3
El objetivo de la practica es comprender el funcionamiento de wifi y bluetooth de la propia esp32. Para lo cual realizaremos una practica donde generaremos un web server utilizando nuestra ESP32 y tambien una comunicacion serie con una aplicacion de un movil con bluetooth.
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

![Image text](https://github.com/victorceballosfouces/Practica_3/blob/main/Practica_3.1/Imagen2_3.1.png)

![Image text](https://github.com/victorceballosfouces/Practica_3/blob/main/Practica_3.1/Imagen_3.1.png)

## Funcionamiento
Primero, con el archivo Web.cpp declaramos una variable tipo string donde crearemos nuestro fichero HTML. Aqui solamente escribimos dos lineas de texto en la web con dos headers (h1,h5).
Ya en el main incluimos nuestro fichero adicional que incluye solamente la pagina html con extern String HTML. Después declaramos las variables que nos daran la SSID y contraseña para conectarnos a la red Wifi y el objeto Web ```WebServer server(80)```. En el setup() inicializamos el puerto serie y nos intentamos conectar al servidor wifi con la id y password proporcionadas anteriormente ```WiFi.begin(ssid, password)```. Una vez que hemos comprobado si nos conectamos correctamente, sacamos por el puerto serial la IP y definimos la ruta donde estará el objeto server ```server.on("/", handle_root)```. Handle_root es la función que almacena el método ```server.send(200, "text/html", HTML)``` que es el encargado de cargar nuestro fichero html creado en Web.cpp. Finalmente iniciamos el servidor HTTP con ```server.begin()``` y en el loop llamando simplemente a ```server.handleClient()``` empezamos todo el proceso.

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
EL siguiente código nos permite conectarnos al bluetooth serie con nuestro bluetooth convencional. Para esto usaremos una aplicación android en el móvil "Serial Bluetooth Terminal" y se creará un puente entre lo que escribamos en dicha aplicación y las salidas que se obtienen a traves de la impresión serie. Respecto al código, inicializamos el puerto serie y el serial BT estableciendo como se llamará.
```
Serial.begin(115200);
SerialBT.begin("1234");
```
Después en el loop, definimos dos condicionales ifs con los que leemos el BT de nuestra aplicación móvil escrbiendolo en el serial BT de la esp32 y viceversa.
```
if (Serial.available()) {
SerialBT.write(Serial.read());
}
if (SerialBT.available()) {
Serial.write(SerialBT.read());
}
```
