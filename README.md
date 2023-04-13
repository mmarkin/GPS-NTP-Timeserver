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

As on Cristiano's original version, the first OLED display shows the number of GPS satellites that are "in view", the resolution 
of the position fix, and the UTC time and date. This version adds a switch so the first display can also show the latitude and
longitude. This time server is GPS-based so it might as well be able to show where it is located. The switch can also turn the
displays off since OLED displays can wear out if they are active all the time, especially if they show information that does not 
change much. See the schematic diagram below for more details.

![Displays2_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/Display2.JPG)

The I2C address of the second OLED module has to be changed. This is done by relocating a resistor on the module's circuit
board. There is a graphic on the board to show which resistor it is and where it goes for each of the two addresses. For example
with the recommended SSD1306 modules the address is changed from 0x78 to 0x7A. The resistor is a tiny surface-mount part so 
it requires a bit of delicate soldering to make the change.

The U8g2 library that the code uses to drive the OLED displays works with many different displays. 
You just need to use the constructor from the library and the I2C addresses that match the displays being used.
For example SH1106 modules could also be used. Constructors for SH1106 modules are included in the definitions.h file. 
To use them comment out the SSD1306 constructors and uncomment the SH1106 constructors. 
The I2C addresses specified in the code may also need to be changed to 0x3C and 0x3D. 

Also as on Cristiano's original version, the yellow LED shows that WiFi is enabled. The green LED shows that the GPS data is 
valid and the server's internal clock is synchronized with it. The red LED flashes every second when GPS data is available.

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
- SPDT center off toggle switch

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

Schematic diagram

![sketch_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/resources/Schematic2.png)

Breadboard layout

![sketch_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/resources/sketch2_bb.png)

I designed a printed circuit board for the project and had a few made. The Gerber files are in the "resources" folder.

![server2_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/Server%202.JPG)

Here are a couple of photos of the interior of the enclosure.

![intFront_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/NewInteriorFront.JPG)

![intBack_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/NewInteriorBack.JPG)

The pushbutton enables or disables the WiFi. The toggle switch selects what is shown on the first OLED display and 
also turns the displays on and off.

CAD drawing for the enclosure

![CAD-_bb-menor](https://github.com/mmarkin/GPS-NTP-Timeserver/blob/main/IMAGES/Enclosure.png)

More information about Cristiano's original version of this timeserver can be found here:<br>
https://www.linkedin.com/pulse/iot-maker-tale-stratum-1-time-server-built-from-scratch-monteiro/

