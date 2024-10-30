This version is the same as the one in the Main branch, except instead of a toggle switch it uses a PIR 
(passive infrared) module to enable both OLED displays only when it detetcts someone is nearby. 

A short press on the pushbutton still enables or disables WiFi.   
A long press (> 800 ms)  on the button switches the first display between showing the time/date/satellite information
and the latitude/longitude received from GPS.

A toggle switch can still be used if desired instead of a PIR module to turn the displays on and off. 
It just has to connect the ESP's A0 pin to 3.3 volts to turn the displays on.   

If compiling this code with the Arduino IDE follow the directions in the Using Arduino IDE file in the Resources directory
of the Main branch. But also add the OneButton v2.6.1 library by Matt Hertel as well as the ones mentioned. PlatformIO does this 
automatically, of course. 
