---
layout: post
title: Digital Photo Frame (Part 1)
comments: true
---

I've been cleaning out my home office recently, throwing out stuff that has accumulated over the years. Items that I've been keeping 'just in case' I need them in the future. I came across all of my old laptops (2 Dells & a MacbookPro) that weren't in a working state for various reasons. They were old & not even worth fixing for the kids to use.


![Old Dell Laptop](/images/2016_02_old_dell_laptop.jpg)
<em>My pride & joy from 10 years ago</em>

The LCD screens from all 3 laptops still worked though. Searching around on the net, I found the [e-qstore](http://www.ebay.com.au/usr/e-qstore) on ebay selling [LCD Controller Boards](http://www.ebay.com.au/itm/121059321784) that convert old LCD screens into monitors that accepts HDMI, DVI & VGA. 

I've always wanted an excuse to play with the [Raspberry Pi](https://www.raspberrypi.org/), and having 3 LCD screens ready to go was perfect. I ordered a couple of [Raspberry Pi 2 Model Bs](http://littlebirdelectronics.com.au/products/raspberry-pi-2-model-b-2), it's amazing to have a linux host that you can SSH into that is so small!

The first problem I wanted to solve was the displaying of family photos. We have an old digital photo frame, which has a really low resolution and photos need to be loaded in via an SD card (a non trivial workflow, so it was rarely done). I had a beautiful 1920x1200 LCD screen from my old MacbookPro which was ready to be used!

## Provisioning and deploying to a Raspberry Pi

My day job involves [striving for immutable infrastructure](http://www.slideshare.net/PeterLeschev/how-atlassians-build-engineering-team-has-scaled-to-150k-builds-per-month-and-beyond-puppetconf-2015), so I found it surprising that the initial provisioning of Pis & code deployment is relatively painful & manual. I wanted to have an automated approach to provisioning & updating my code. After a little research, I found [resin.io](https://resin.io/) which [painlessly](http://docs.resin.io/#/pages/installing/gettingStarted.md) handles the initial provisioning of your Pi and updating of your code running on the device. You setup an 'application' in their web UI, download an SD card image (which can also preburn your wifi password for you), copy it across to your SD card, fire the Pi up, and it automatically connects to the resin.io servers as a device that runs your specific application. 

You then setup a ``git remote`` in your git repository pointing to resin.io, push to resin, and as part of the ``git push``, it builds a docker image of your application. Once the build is completed, your Pi auto updates (by downloading the new docker image, killing the old container, and spinning up a new one). It couldn't be easier!

Functionality that impressed me on the resin.io web application:

* Dashboard status of all your devices, including progress of app updates & the commit hash of the code that is running
* View the logs of your application in real time
* Remote console into your container

it's just missing telemetry data from the device, and we'd be all set!

## Getting photos onto the Raspberry Pi

I wanted the digital photo frame to be easy to use. Given that we're an Apple house, sharing photos from our iPhones to the digital photo frame via AirDrop would have been ideal. Unfortunately, it doesn't look like anyone has implemented a non-iOS AirDrop [client yet](http://stackoverflow.com/a/11794768), so I had to settle for integration via email for now.

## Displaying the photos

Of the [thousands of DYI digital photo frames](https://www.google.com/search?q=digital+photo+frame+fbi) around already, [FBI](http://manpages.ubuntu.com/manpages/gutsy/man1/fbi.1.html) - a linux framebuffer imageviewer comes up as the imageviewer to use. The problem that need solving was that when calling the fbi command, it kicks off a subprocess, and the original process quits immediately, so it is non-trivial to wait for the fbi slideshow to finish and not leave zombie processes around. Digging around, I found that the [phusion-baseimage](https://hub.docker.com/r/phusion/baseimage/) docker image has [solved this problem](https://blog.phusion.nl/2015/01/20/docker-and-the-pid-1-zombie-reaping-problem/). 

## Show me the code

 [All the code is available here](https://github.com/pleschev/digital_photo_frame). It's pretty simple. Just a Dockerfile that installs all the required dependencies & kicks off a simple Ruby script (Wrapped in [phusion-baseimage's my-init](https://github.com/phusion/baseimage-docker/blob/master/image/bin/my_init) script), which:

* polls an IMAP enabled gmail address every minute, downloading all emails, pulling out all the attachments it can find, scaling it down & auto rotating based on EXIF data and finally storing them in the ``/data`` partition (this is a resin.io directory location that applications can use to store application data that is persisted across different versions of the application)
* an endless loop that kicks off a slideshow of all the images in the ``/data`` directory

If there are no photos available to show, an informational message telling users which email address to send photos to is shown.

## What does it look like?

![LCD Screen, controller & Raspberry Pi](/images/2016_02_lcd_controller_and_pi.jpg)
<em>LCD Screen, controller & Raspberry Pi</em>

In a word, suboptimal. It's functional, but there's a lot of work still to do to make it presentable!

## Still to do

So I have the functional requirements of the digital photo frame sorted out, I still have to:

* Combine the 2 power sources (The LCD controller requires 12V whereas the Pi requires 5V) so we only have 1 AC connection
* Find a suitable photo frame, mount the LCD screen & PCBs securely, in a kid friendly manner

