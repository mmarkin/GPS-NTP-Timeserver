# GPS-NTP-Timeserver
WiFi enabled GPS fed NTP server based on ESP8266 and Arduino framework 

This project adds to the work of Cristiano Monteiro. His version is portable.<br>
https://github.com/Montecri/GPSTimeServer

This version adds a second OLED display to help verify that the server responds to the NTP requests. Every time an NTP request comes 
in from a client (i.e., a clock connected to the server's WiFi network) its IP address and the time the response was sent are shown 
on the OLED. It also shows the total number of clients connected. The second display is optional, the code posted here runs without it. 
However with multiple clocks it is a handy way to make sure they are all connected and being served. In server mode, ESP8266 
microcontrollers can handle up to eight WiFi clients.

![Displays_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/Display.JPG)

As on Cristiano's original version, the first OLED display shows the number of GPS satellites that are"in view", the resolution 
of the position fix, and the UTC time and date. <br>t 
The yellow LED shows that WiFi is enabled.<br> 
The green LED shows that the GPS data is valid and the server's internal clock is synchronized with it.<br>    
The red LED flashes every second when GPS data is available.

OLED displays can wear out if they are active all the time especially if they show information that does not change much.
This version includes provision to turn the OLEDs off if nobody is there to see them. As suggested by Brett Oliver, a
PIR motion sensor module can be connected to the ESP8266 to automatically turn on the OLEDs when someone is near and turn 
them off when they leave. A switch could be connected instead of the sensor if manual operation is desired.
See below for more details.

The I2C address of the second OLED module has to be changed. This is done by relocating a resistor on the module's circuit
board. There is a graphic on the board to show which resistor and where it goes for each of the two addresses. For example
with the recommended SSD1306 modules the address is changed from 0x78 to 0x7A. The resistor is a tiny surface mount part so 
it requires a bit of delicate soldering to make the change.

The U8g2 library the code uses works with many different OLED displays. 
You just need to use the constructor from the library and the I2C addresses that match the displays being used.
For example SH1106 modules could also be used. Constructors for SH1106 modules are included in the definitions.h file. 
To use them comment out the SSD1306 constructors and uncomment the SH1106 constructors. 
The I2C addresses specified in the code may also need to be changed to 0x3C and 0x3D. 

To use the server, set the clock so it joins the WiFi network specified by the SSID and password in the definitions.h 
file. Also set the clock so the address for the time server it calls is 192.168.4.1. That is the default IP address ESP 
devices use in server mode. The server will assign IP addresses to clients on its network starting with 192.168.4.2.

It is highly recommended to use PlatformIO to compile and edit the code. That way all the libraries needed will be 
downloaded and installed automatically. It is free like the Arduino IDE. However PlatformIO has more professional features 
that make it easier to develop a project like this once you get the hang of using it. All the files needed for this project 
have been provided. The code is in two files: definitions.h is in the "includes" directory and main.cpp is in the "src" 
directory. The platformio.ini file is also provided. 
 
I built a custom enclosure for the project using walnut and acrylic. 

![EnclosureFront_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/EnclosureFront.JPG)

![Enclosure_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/Enclosure.JPG)

Parts list:

- Amica NodeMCU (ESP8266 / ESP-12)                   
- DS3231 RTC
- Neo-6m V2 GPS
- 0.96" OLED Display (SSD1306 or similar. 
  If two are used be sure that it is possible to change the I2C address on one of them.) 
- Hi-Link 5V/3W Power supply
- Mini-360 DC-DC Buck converter
- Red, Green and Yellow LEDs
- Resistors (150, 100 and 150 Ohms respectivelly for above leds)
- Momentary push button switch
- HC-SR501 PIR motion sensor module or SPDT switch (both are optional)

---
<p align="center"><img src="https://user-images.githubusercontent.com/38574378/132773469-08fb7b59-2f9d-4641-9665-c8d50d3904bc.png">  
<b>ATTENTION</b><img src="https://user-images.githubusercontent.com/38574378/132773469-08fb7b59-2f9d-4641-9665-c8d50d3904bc.png"></p> 

Several DS3231 modules being sold today contain a hazardous design flaw in which it supplies a voltage to the battery cradle 
regardless if it came with a rechargeable battery or not. 
If it came with a CR2032 battery (non-rechargeable) the consequence is that it will swell, explode, or worse. 
If it came with a LIR2032 battery (rechargeable), the module being fed with 5v will generate an unsafe charging voltage for 
that battery.

There are workarounds for that so you do not need to toss your module away, the most popular being removing a diode and/or 
resistor.

There is a long discussion on the thread below about root cause and possible fixes:

https://forum.arduino.cc/t/zs-042-ds3231-rtc-module/268862/33

---

Libraries:

- paulstoffregen/Time@^1.6
- makuna/RTC@^2.3.5
- mikalhart/TinyGPS@0.0.0-alpha+sha.db4ef9c97a
- olikraus/U8g2@^2.28.8

Source code is also based on:<br>
- http://w8bh.net/avr/clock2.pdf
- https://forum.arduino.cc/t/ntp-time-server/192816
- http://www.brettoliver.org.uk/GPS_Time_Server/GPS_Time_Server.htm

---

Wiring

My time server does not have to be portable as Cristiano's is so it is AC powered only.

A PIR motion sensor can be connected to A0 on the NodeMCU to automatically turn the OLED displays on only when someone
is near to see them. Thanks to Brett Oliver who engineered the mod on his version of Cristiano's project. Alternatively, a simple 
SPDT switch that connects A0 to either ground or +3.3 volts could be used. If it is not desired to turn the displays off, 
just connect A0 permanently to +3.3 volts or comment out the "if (PIRvalue < 500)" block of statements in the main.cpp file.

Schematic diagram

![sketch_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/Schematic.png)

Breadboard layout

![sketch_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/sketch_bb.png)



Here are a couple of photos of the interior of the enclosure.

![intFront_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/InteriorFront.JPG)

The pushbutton enables or disables the WiFi. I did not use a PIR. The toggle switch turns the displays on and off.

![intBack_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/InteriorBack.JPG)

CAD drawing for the enclosure

![intFront_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/Enclosure.png)

More information about Cristiano's original version of this timeserver can be found here:<br>
https://www.linkedin.com/pulse/iot-maker-tale-stratum-1-time-server-built-from-scratch-monteiro/

