# Control the mount, the camera and other accessories with Raspberry Pi 3

Carrying a laptop for astrophotography can sometimes be annoying. In the field, you have to worry about battery life, long cables connecting all your stuff (camera, focuser, mount, filterwheel, autoguider...), drivers and compatibility, etc. It can quickly get messy. A good alternative is to use a USB Powered Hub attached to the scope or the mount, but that only solves a third of the problems mentioned above. In order to get out of trouble, I found a solution that is light, portable, functional and cheap for my astrophotography setup.

![Here is a picture of my current setup (some stuff may be missing).](https://github.com/Kelian98/Easy-astrophotography-equipment-control/blob/master/Equipment_with_annotations.jpg)
## Equipment needed

In this section, we will assume that you have some scope, GoTo mount, camera, guiding camera. That's all we need for setting up a portable computer capable of managing all these devices. More accessories can be add including filter wheel, electronic focuser, etc.

For our system, we will use :

- [Raspberry Pi 3](https://www.amazon.com/s?k=raspberry+pi+3&ref=nb_sb_noss_1:// "Raspberry Pi 3") with case, heatsinks and Micro SD Card (32Go or more recommended)
- [USB Power Bank](https://www.amazon.com/s?k=power+bank&ref=nb_sb_noss_2 "USB Power Bank") with a capacity of at least 10,000mAh and 2.1-2.5A USB output
- [Specific cable](https://www.firstlightoptics.com/sky-watcher-mount-accessories/lynx-astro-ftdi-eqdir-usb-adapter-for-sky-watcher-eq5-pro-heq5-syntrek-pro-az-eq5-gt-az-eq6-gt-and-eq8-mounts.html "Specific cable") to connect your mount to the board (in this case, EQDIR cable for SkyWatcher mounts)
- [GPS USB Dongle](https://www.amazon.com/s?k=vk-172&ref=nb_sb_noss_1 "GPS USB Dongle") to obtain precise location (I use a Vk-172, but Vk-162 with a better antenna is also compatible)

As you can see, it costs around 100\$, far cheaper than a dedicated astro computer running on Windows with commercial softwares which run on 12V power supply.

## Software

We will run [Astroberry Server](https://github.com/rkaczorek/astroberry-server "Astroberry Server") on the Raspberry Pi 3. This is an open-source modified version of Ubuntu Mate 16.04 created by [Radek Kaczorek](https://github.com/rkaczorek "Radek Kaczorek") that contains all we need.
You can get instructions [here](https://github.com/rkaczorek/astroberry-server#how-to-use-it "here").
The system features many astronomy softwares including Kstars and Ekos we will be mainly using.

### 1. Install Astroberry Server

First, get the image from this link : https://drive.google.com/file/d/1zGwXLWDD8hubpuarafMWPft6F6Q4bV8R/view
Then, if you are on Windows, download the latest version of Etcher from this link : https://www.balena.io/etcher/
Finally, get the free version of Winrar to unpack Astroberry image file : https://www.win-rar.com/start.html?&L=0

Now that we have finished with softwares installations, let's put Astroberry on the Raspberry Pi 3.

1. Unpack .xz extension file downloaded above with WinRar.
2. Simply insert your Micro SD Card (inside the adaptor for SD format) in your SD Card slot of your computer.
3. Start Etcher, select the previously unpacked .img extension file, select the drive of your SD Card, and click on Flash !
4. Wait until process is finished...
5. When it's done, eject your SD Card from your computer and insert it into the Raspberry Pi 3.

### 2. Additionnal drivers

Start the Raspberry Pi 3 with SD Card and plug in a mouse, a keyboard and a monitor.
If everything has been done correctly, it will boot and get you to Astroberry desktop.
You can connect it to your personal newtork by clicking on WLAN logo at the right-top of the screen. Enter your network information and you will be connected to Internet.

#### 2.1 DSLR

This sub-section aims to install required drivers if you want to use a DSLR not directly recognized by Ekos. In my case, I wasn't able to control my Nikon D3300 on Windows despite all attempts with many softwares (Sequence Generator Pro, BackyardNikon, APT Astrophotography Tool, etc).

I found a driver called gPhoto for Linux ([here](http://www.gphoto.org/proj/libgphoto2/support.php "here") you can find all compatible DSLR Cameras) that was compatible with my camera. I was able to find a good tutorial that worked for me to install it on the Raspberry Pi 3.
Just need to follow the instructions : [Install libgphoto2 and gphoto2 from source on Raspberry Pi](https://hyfrmn.wordpress.com/2015/02/03/install-libgphoto2-and-gphoto2-from-source-on-raspberry-pi/ "Install libgphoto2 and gphoto2 from source on Raspberry Pi")

#### 2.2 GPS

If you use the Vk-162 or Vk-172, you can follow this steps :

1. Plug the GPS in Raspberry Pi USB port.
2. Open a command terminal by pressing CTRL + ALT + T or right-click on Desktop and select "Open in terminal"
3. Install gpsd package : `sudo apt-get install gpsd`
4. To see on which port the GPS is connected, type : `ls /dev/tty*`. When plugging/unplugging the GPS, some address such as /dev/ttyACM0 or /dev/ttyACM1 should appear and disappear. Keep it in mind.
![](https://raw.githubusercontent.com/Kelian98/Easy-astrophotography-equipment-control/master/GPS_current_port.png)
5. Now you have to configure the GPS default file. Type `sudo pico /etc/default/gpsd` and edit DEVICES="port obtained at step 3".
![](https://raw.githubusercontent.com/Kelian98/Easy-astrophotography-equipment-control/master/edit_default_file.png)
6. Press CTRL + X to exit and save changes by pressing Y when asked.
7. Again in terminal, type : `service gpsd restart`
8. Finally to know if the GPS is working, look if the green LED is blinking and type : `cgps -s`, you should see current information received by the GPS
9. The GPS is now working !

You can also watch this video with similar process : https://www.youtube.com/watch?v=tQz8Fo5u7Lc&t=820s

> Note 1 : Indoor, the GPS may be unable to find signal. I recommend to do this outside.

> Note 2 : I always plug the GPS in the same USB port in order to keep the default file the same. Otherwise, I would probably have to repeat steps 3 and 4 each time I plug it in another USB port.

### 3. Setting up Kstars and Ekos

I will not explain in depth how to setup Ekos with your equipment because there are plenty of good tutorials online, here is a list :

- Old but great tutorial for general use and configuration : https://www.youtube.com/watch?v=wNpj9mNc0RE (only interface has changed)
- This playlist explains each module and their usage : https://www.youtube.com/playlist?list=PLn_g58xBkqHuPUUOnqd6TzqabHQYDKfK1
- A short live session which explores some modules and functionnalities : https://www.youtube.com/watch?v=3uwyRp8lKt0
- Official documentation that refers to tutorials : https://www.indilib.org/about/ekos.html

For specific topics, you can search on the [INDI official forum](https://www.indilib.org/forum.html "INDI official forum"), ask on Facebook groups...
