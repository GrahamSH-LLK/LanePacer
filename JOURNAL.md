---
title: "LED Lane Pacer"
author: "Graham Howard"
description: "A pacing system for swimming with LED lights."
created_at: "2025-06-15"
---


## June 14th - Ideation and Sourcing
I came up with the basic idea today- put a LED strip on the bottom of the pool in order to pace swimmers. After coming up with the idea, I decided to get a sense for the budget.

First, I did some research into what types of LED strips would be appropriate for this use case. After some research, I found that ip68 rated strips are appropriate for the depth and length of submersion that these strips will be under (1-2hours, ~12ft). I was unsure if they would be available waterproofed in the length I needed.


I opened a RFQ on Alibaba for 25 meter lengths of ip68 rated LED strips, and got a handful of responses.

![image](https://github.com/user-attachments/assets/eaf14604-1bf4-44db-bcf7-bf7bfdc023a4)

The general consensus was that this is definitely an available product, appropriately rated for my use case. 

I created a drawing in Onshape to communicate the length and use case.
![image](https://github.com/user-attachments/assets/71dd48a0-1a26-4791-9e1d-502a37c75ca3)


## June 15th - Power and Driving

The primary issue I currently have to solve is safely and effectively powering them. I had a few plans originally:
- Powering with a box with battery pack, so I don't have to touch mains voltage
- Powering from mains, so I don't have to deal with battery safety 

I decided that the safest option would be a waterproofed box, powered by mains voltage with an appropriately rated power supply, since the power needs for this project are significant.

### Power Requirements
The current option that I'm planning on going with for LED strips would be at `24v`, `7w/m`. `6 lanes * 25m/lane * 7w/m = 1050w`, which is fairly hefty.
Unfortunately, powering all of this at 100% would get expensive in terms of finding an appropriate power supply or battery. 

However, my use case notably does _not_ require every LED lit up at once! Since I am just pacing swimmers, I should only ever require 6 lanes with up to 8 swimmers (1 ft moving sections). 

`6 lanes * 8 swimmers/lane * 0.3m/swimmer * 7w/m = 100.8w`- much more reasonable! 

The current plan is to have a single box providing power at the wall, with a waterproofed deck cable providing connection to each lane. 

### Driving the LEDs!
The other issue is driving the LEDs. My initial inclination was to base this on an ESP32 system, since it comes with free WiFi and Bluetooth connectivity. However, after doing some research, at the number of LEDs I am driving, this would have mediocre performance and would likely not allow for the update speeds I would require. 

A friend recommended Teensy, and after looking into it, it has great WS8218b support with a [DMA library](https://www.pjrc.com/teensy/td_libs_OctoWS2811.html). According to their documentation, I could reach around 50Hz update rate with my 4500LEDs as opposed to the ESP32's 16Hz. 

Since I still need support for WiFi/Bluetooth connection for control, I'm adding an ESP32 coprocessor. This is the current block diagram:

![lanepacer drawio](https://raw.githubusercontent.com/GrahamSH-LLK/LanePacer/b294c3a648f139af1e34843678cf5fcd9e2c41a6/lanepacer.drawio.svg)


