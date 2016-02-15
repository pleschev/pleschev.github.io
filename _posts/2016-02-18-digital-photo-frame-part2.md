---
layout: post
title: Digital Photo Frame (Part 2)
comments: true
---

## Removing the need for 2 power sources

Now that the digital photo frame is functional, I wanted to remove the need for 2 AC cords & adaptors. I currently need one 12V input for the LCD controller and the Raspberry Pi that takes 5V. 

![Existing setup of LCD Screen, controller & Raspberry Pi before combining power sources](/images/2016_02_lcd_controller_and_pi.jpg)
<em>Setup before combining the power sources</em>

A work colleague suggested I use a [Pololu 5V, 2.5A Step-Down Voltage Regulator D24V25F5](https://www.pololu.com/product/2850) that takes an input voltage of up to 38V and reduces down to 5V. The first step was to solder the pin headers to the step down PCB. By using jumper wires, I'll be able to reuse the board without having to resolder everything.

![Voltage step PCB](/images/2016_02_stepdown_pcb.jpg)
<em>Voltage step-down PCB with the header pins soldered in</em>

I found the pins of the power socket on the LCD controller LCD using a trusty multi-meter, and soldered some jumper wires in

![LCD Controller with jumper wires](/images/2016_02_lcd_controller.jpg)
<em>Jumper wires intercepting the 12V source</em>

The Raspberry Pi takes a 5V input via a MicroB USB socket. Again, I didn't want to lock myself into using this particular Raspberry Pi for this project, so I didn't want to directly solder on the Pi and run the risk of destroying it as well. So I picked up a [MicroB USB DIY connector shell](https://www.adafruit.com/products/1826). 

![USB Shell and jumper wires](/images/2016_02_usb_shell.jpg)
<em>My USB socket powered by a couple of jumper wires</em>

The hardest part of this 'build' was working out which particular USB pins were GND & 5V when soldering the jumper wires on the USB connector. Unfortunately the adafruit site doesn't have the information, so had to look elsewhere. This [hobbytronics.co.uk](http://www.hobbytronics.co.uk/usb-microb-plug-diy) link provides the pin numbers of connector shell, and this [pinouts.ru](http://pinouts.ru/PortableDevices/micro_usb_pinout.shtml) link provides the breakdown of what each pin does.

It was fun to pull out the soldering iron. I haven't touched it since my uni days. Here's a pic of all of the pieces put together, yay to 1 less power source!

![End state with combined power source](/images/2016_02_end_state_combined_power_source.jpg)
<em>End state with the combined power source</em>

You can probably tell that the LCD screen in the original photo and the LCD screen in the end state photo are different, indeed they are, read on to find out the reason...

## Disaster strikes

While trying to remove the frame around the LCD screen, I must have bent the LCD screen too far and cracked the screen. My beloved MacBook Pro LCD screen was destroyed. It was pretty disappointing as this was the LCD screen which supported the highest resolution. Now I'm down to 2 LCD screens that I can use, and a spare LCD controller (I have my eye on my wife's current laptop now!)

![Cracked LCD Screen](/images/2016_02_cracked_lcd_screen.jpg)
<em>Oh noes!</em>

## Still to do

* Find a suitable photo frame, mount the LCD screen & PCBs securely, in a kid friendly manner
