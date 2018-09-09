---
layout: post
author: justin
title: "GPS Clock using ArduPilot and EM-406"
---
So we got tired of not being able to see what time it was when we were watching movies/shows on Netflix. We could have just bought a clock, but where’s the fun in that? I had an EM-406 GPS module and an ArduPilot lying around, and I heard about [Sparkfun’s 12ft GPS Wall Clock](http://www.sparkfun.com/tutorials/47){:target="_blank"} so I figured I would give it a try.

I had built a clock before just using an Arduino board, but the clock would get out of sync after a couple of days because of the little bit of processing time that was taken by the Arduino. So I put that aside and was planning on getting a [Real Time Clock DS3234 IC from Sparkfun](http://www.sparkfun.com/products/10079){:target="_blank"}, but the chip is a bit expensive, hard to solder, and the [breakout board](http://www.sparkfun.com/products/10160){:target="_blank"} is even more expensive. Not that I couldn’t afford to spend $20 on this, I just had [more exciting projects](http://justpushbuttons.com/blog/?page_id=117){:target="_blank"} on my mind so it went to the back burner.

When I heard that you could use a GPS module to get very accurate UTC, I was like “Allllllrighty then!” and I started working. I had a little trouble programming the ArduPilot until I realized that you have to connect a power supply other than the FTDI cable to power the MCU. Once I did that, I got a simple blinky sketch running, and then I started working with the EM-406 module. When the EM-406 module has a signal, it’s LED blinks. Once it’s blinking you can get UTC time from it using an NMEA library, do some funky magic and “badda bing, badda boom” you’ve got a clock. See my code below:

{% highlight c %}
#include <wprogram.h>
#include <nmea.h>
#include <SoftwareSerial.h>

#define txPin 6  //to the RX pin
#define rxPin 12  //not used
SoftwareSerial mySerial =  SoftwareSerial(rxPin, txPin);

NMEA gps(GPRMC);  // GPS data connection to GPRMC sentence type
float gpsTime;

void setup() {
  Serial.begin(4800);

   pinMode(txPin, OUTPUT);
   mySerial.begin(9600);
   mySerial.print(0x76,BYTE); // Reset Command

   mySerial.print(0x7A,BYTE); // Command byte
   mySerial.print(0x05,BYTE); // bright display

   //display the colon
   mySerial.print(0x77,BYTE); // Command byte
   mySerial.print(0x10,BYTE); // colon

   demo();
}

void loop() {
  if (Serial.available() > 0 ) {
    // read incoming character from GPS
    char c = Serial.read();

    // check if the character completes a valid GPS sentence
    if (gps.decode(c)) {
      // check if GPS positioning was active
      if (gps.gprmc_status() == 'A') {
        gpsTime = int(gps.gprmc_utc() / 100) - 600;
        if (gpsTime < 0) {           gpsTime = 2400 + gpsTime;         }         if (gpsTime >= 1300) {
          gpsTime = gpsTime - 1200;
        }
        if (getPlace(1000, gpsTime) == 0) {
          mySerial.print(0x78,BYTE);
          mySerial.print(getPlace(100, gpsTime),BYTE);
          mySerial.print(getPlace(10, gpsTime),BYTE);
          mySerial.print(getPlace(1, gpsTime),BYTE);
        }
        else {
          mySerial.print(getPlace(1000, gpsTime),BYTE);
          mySerial.print(getPlace(100, gpsTime),BYTE);
          mySerial.print(getPlace(10, gpsTime),BYTE);
          mySerial.print(getPlace(1, gpsTime),BYTE);
        }
      }
    }
  }
}

int getPlace(int place, int startingValue) {
  startingValue = startingValue - (startingValue % place);
  startingValue = startingValue / place;
  int placeValue = startingValue % 10;
  return placeValue;
}

void demo() {
  delay(80);
  mySerial.print(8,BYTE);
  mySerial.print(0x78,BYTE);
  mySerial.print(0x78,BYTE);
  mySerial.print(0x78,BYTE);
  delay(80);
  mySerial.print(0x78,BYTE);
  mySerial.print(8,BYTE);
  mySerial.print(0x78,BYTE);
  mySerial.print(0x78,BYTE);
  delay(80);
  mySerial.print(0x78,BYTE);
  mySerial.print(0x78,BYTE);
  mySerial.print(8,BYTE);
  mySerial.print(0x78,BYTE);
  delay(80);
  mySerial.print(0x78,BYTE);
  mySerial.print(0x78,BYTE);
  mySerial.print(0x78,BYTE);
  mySerial.print(8,BYTE);
}
{% endhighlight %}

**Pictures!**
![The front of it](/assets/img/clock-front.jpg)
![The power source](/assets/img/clock-power.jpg)
![The innards](/assets/img/clock-guts.jpg)
![Action shot!](/assets/img/clock-working.jpg)
