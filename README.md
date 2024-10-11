This version is the same as the one in the Main branch, except it uses a PIR instead of a SPDT center-off toggle switch to
control the OLED displays. 

A short press on the pushbutton enables or disables WiFi.   
A long press (> 600 ms) switches the first display between showing the satellite and time information
and the latitude and longitude received from GPS.

Both displays are enabled when the PIR detects motion.   

A toggle switch can still be used instead of a PIR to turn the displays on and off if desired. 
It just has to connect the ESP's A0 pin to 3.3 volts to turn the displays on.   

If compiling this code with the Arduino IDE follow the directions in the Main branch except add the 
mathertel/OneButton v2.6.1 library as well as the ones mentioned. 
