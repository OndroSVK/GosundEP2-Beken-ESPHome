# GosundEP2-Beken-ESPHome
Success story about convincing Gosund EP2 to play along with ESP Home


This project is about smart socket GOSUND EP 2 powered by ESPHome and LibreTiny.  
It runs on a BK7231 chip and integrates with Home Assistant.

It can serve as proof of concept for different devices with Beken chips. 
I will explain steps by step how to build code from scratch.



## DISCLAIMER: ⚠️ 
In my case, I physically opened the smart socket and accessed potentially dangerous high-voltage components.
I have formal training in electrical engineering and sufficient experience to do so safely.
Do not attempt anything that could compromise your safety or well-being.
If you're unsure or inexperienced, please consult a qualified professional.

## Features
- Relay control
- Voltage and current measurement (BL0942)
- Device identify
- Home Assistant support



## Step 1: research

Collecting as much evidence as possible: how to access chip and flash software
In my case I did a lot of try and fail until I finnally managed to make it work.

## If you are lucky enough, you have the option to OTA update your device with custom firmware.
ither you device allows it out of the box,
or you can try "cloudcutter" approach. If that's the case, you can fast travel to step... IDK Building Firmware


Main reason of my initial struggle was that in older versions of EP2, Gosund used ESP chips, therefor Tasmota and Tasmotizer was way to go.
But my sockets got the Beken BK7231 and it was the 1.st time I saw them. 


https://github.com/openshwprojects/OpenBK7231T_App



## Step 2: Opening socket

from this source you can get an idea how to physically open the socket body and access electronics.
https://community.home-assistant.io/t/gosund-ep2-plug-with-esphome/433586
here in detail with photos
https://github.com/arendst/Tasmota/discussions/10350

## TL;DR: 
Goal is to remove transparent plastic part from white part. It is glued somewhere inside. Good aimed hit with hammer will break it loose.
Keep hiting socket lightly with a hammer while turning it around. Ideally have it in 20degre angle with translarent part on surface 
and you should hit socket approx 15mm from edge of transparent part. Once thhe glue snaps you can remove transparent part by just pullling it out.
Under transparent plastic, you will find L and N hole spring-loaded safety cover, under that 3 screws holding Grounding in place.
Remove screws and unsolder grounding terminal. After removing it, it will expose 2 more little screws, after removing them you can finally access electronics 
by pushing Plug pins "inside". 

## Step 3: Access PCB, solder terminals
Once you will have the insides out :D you will locate chip and TX RX terminals, in my case chip was NAME apart from that, you also need to connect Vcc and Gmd.
My lazy/genius approach is to locate voltage stabilizer, in 99% that will be [AMS1117](http://www.advanced-monolithic.com/pdf/ds1117.pdf)
So you can use that as clue. I was using [USB<->Serial](https://www.google.com/search?q=usb+serial+cp2102&client=safari&sca_esv=df3771177ddad875&rls=en&biw=1512&bih=814&sxsrf=AE3TifP3M7WHgPfcH9y5i5j5JHQNiEganQ%3A1751711371333&ei=i_5oaP-RFIi7i-gPvqnjqQ4&oq=usb+serial+CP&gs_lp=Egxnd3Mtd2l6LXNlcnAiDXVzYiBzZXJpYWwgQ1AqAggAMgYQABgWGB4yBhAAGBYYHjIGEAAYFhgeMgYQABgWGB4yBhAAGBYYHjIGEAAYFhgeMgYQABgWGB4yBhAAGBYYHjIGEAAYFhgeMgYQABgWGB5IpiVQmglYzRZwAXgBkAEAmAGCAaAB8QKqAQMwLjO4AQHIAQD4AQGYAgSgAokDwgIKEAAYsAMY1gQYR8ICDRAAGIAEGLADGEMYigXCAgUQABiABMICCBAAGIAEGMsBmAMAiAYBkAYKkgcDMS4zoAfuErIHAzAuM7gHggPCBwUyLTMuMcgHFA&sclient=gws-wiz-serp) That has only 5V output so I have connected that to IN on AMS1117 and GND on GND

![AMS1117-pinout-datasheet](https://github.com/user-attachments/assets/b45a4c09-6fcd-4157-bea2-08239b2e7f2a)


https://github.com/OpenBekenIOT/hid_download_py



## YAML Configuration
See `bksocket.yaml` in this repository.

