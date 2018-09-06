---
layout: post
author: justin
title: "The Lexmark X125 Printer’s LCD"
---
I took another printer off of my stack yesterday. This one happened to be a Lexmark X125. My original intent was to turn it into a [PCB printer](http://hackaday.com/2012/04/24/printing-pcbs-on-a-junked-epson-printer/){:target="_blank"}, but after trying a few test prints I found that there wasn’t anymore ink left in it and I wasn’t about to buy ink for a printer that I got for free; there are plenty more where that one came from. Thus, I began to destructionate.

![lexmark x125](/assets/img/lexmark-x125-top.jpg)

![lexmark x125 opened](/assets/img/lexmark-x125-opened.jpg)

The last printer I disassembled had a pretty nice little LCD in it, so I figured that I would at least have a look at the LCD in this one to see if it was even worth my time. Digging in I found that there were 14 pins leading into the LCD:

![lexmark x125 lcd pcb](/assets/img/lexmark-x125-pcb1.jpg)

![](/assets/img/open-cover-and-remove-paper-jam.jpg)

A quick google search of the part number on the back of the LCD (“P1620B”) lead me to [this thread](http://www.lynxmotion.net/viewtopic.php?t=5099){:target="_blank"}. Reading through I saw that someone noted that the 14 pin version (more commonly 16 pins I think) was probably just missing pins 15 and 16 which were for a backlight. Thinking about it some more, it kind of reminded me of [one of the LCDs that Sparkfun carries](http://www.sparkfun.com/products/255){:target="_blank"}. If it was only missing a backlight then it would be extremely easy to interface to since the Arduino environment comes with example code for this kind of LCD.  So I desoldered the LCD from the ribbon cable…

![](/assets/img/lexmark-x125-pcb-cable-cut.jpg)

And I soldered some header pins on the LCD (sorry, no “exciting” pictures for this part). And I wired it up to an [Arduino Pro Mini](http://www.sparkfun.com/products/11113){:target="_blank"} on a breadboard according to the wiring example for the [LCD page on the Arduino site](http://arduino.cc/en/Tutorial/LiquidCrystal){:target="_blank"}. And I pulled up the “Hello World” example code for the LCD. And… it wouldn’t compile. … well back to the pile Arduino 0022. I pulled up the example code on Arduino 0022, compiled successfully, and spank:

![hello world](/assets/img/lexmark-x125-hello-world.jpg)

That was a lot easier than last time.

Got any working LCDs you’ve salvaged from old printers/electronics? Drop a link in the comments.
