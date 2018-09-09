---
layout: post
author: justin
title: "Using the Pocket Progmammer with ATMega328p"
---
![](/assets/img/atmega328.jpg)
I thought that I would have to learn AVR code in order to program my ATMega328P micro. Then I got to reading on the arduino.cc website and found out that by changing a setting in the arduino software’s preferences.txt file I could simply use the arduino software and arduino code for programming my chip.

**Parts I used:**

- [Pocket AVR Programmer](http://www.sparkfun.com/products/9825){:target="_blank"}
- [ATMega328P with arduino bootloader](http://www.sparkfun.com/products/9217){:target="_blank"}
- [16MHz Crystal](http://www.sparkfun.com/products/536){:target="_blank"}

Follow the instructions in [this tutorial](http://www.sparkfun.com/tutorials/93){:target="_blank"} to get your breadboard set up.

Make sure to put a 16MHz crystal in your circuit if you are using the ATMega328P with the arduino bootloader (between pins 9 and 10 on the ATMega). I spent a while trying to figure out that I just needed to put in a crystal because the micro had been preconfigured to use a 16MHz crystal instead of the internal oscillator.

K ready go.

1. Make sure your arduino software isn’t open.
1. Change the preferences file. Located here: C:\Documents and Settings\<USER>\Application Data\Arduino\preferences.txt
1. Change this line: `upload.using=bootloader`
1. To look like this: `upload.using=usbtinyisp`
1. Change “usbtinyisp” to match whatever programmer you are using. A list of values for this are in programmers.txt under your arduino install location:
`...arduino-0022\hardware\arduino\programmers.txt`
Read more about this [here](http://arduino.cc/en/Hacking/Programmer){:target="_blank"}.
![](/assets/img/preferences_file.jpg)
1. Save preferences.txt
1. Now write some arduino code. I started with blinking an LED to make sure I had it right. Then I uploaded some code to turn a servo. Use [this awesome page](http://tinkerlog.com/2009/06/18/microcontroller-cheat-sheet/){:target="_blank"} to figure out how the pins from your arduino translate to pins on the ATMega.
1. When you have your code ready to upload, use the Tools>Board menu to select a board with the same microcontroller that you’re using. I selected the Arduino Uno because I know it uses the ATMega328.
![](/assets/img/choose_board.jpg)
1. Now all you have to do is press the upload button in the arduino software and your code will upload to the ATMega and run.

Here’s a video of the action. I’ve hooked up a servo to my breadboard (arduino pin 9 = ATMega pin 15), and uploaded some code to turn it clockwise and counterclockwise.

<iframe width="560" height="315" src="https://www.youtube.com/embed/IGJm8JtelSc" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>
