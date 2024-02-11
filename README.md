# pi_fast_headless_config
Setting up a raspberry pi to be able to remotely connect through SSH and to its GUI through VNC, without need of external keyboard or mouse.

## Idea
This quick tutorial was created because I'm quite broke and I need something similar as a google chromecast to watch netflix with my friends at the living room. So why not try mith my raspberry pi. This is quite simple to do, just install VNC in your raspberry pi and your pc or phone and done! works like teamviewer and Netflix runs acceptable using a raspberry pi 3 B+. For sure, newer versions would be even better.

The problem was that I had no keyboard or mouse at my new home to do the initial set up and install vnc in my pi. So, I decided to do it editing the OS files in the SD card. This way I can just insert the SD card in the raspberryPi and turn it, then everything will be ready to use it on my phone. While looking for the steps to do that I noticed that they were all separated in many simple small google searches. 
I decided to summarize all then in just one tutorial, since I'll definetely use it again.

If you just need the raspberry configured for remote access, do steps 1-5.
If you want to also set up the VNC, continue until last step.

# RECIPE

## 1 - Flash Raspbian in SD card
Using the raspberry pi flasher, free download at https://www.raspberrypi.com/software/

## 2 - Enable ssd
Navigate to your SD card directory.
In MacOs would be:

'''
cd /Volumes/{Flash card}
'''


Inside boot directory (or bootfs in my case).
Create an empty file named ssh (without any file extension). This file tells the Raspberry Pi to enable SSH on boot.

'''
nano ssh
'''

Just like this ssh is enabled. But it's just going to work if you have a first user set up in your raspberry.



## 3 - Create a first user

create an encrypted password:

'''
echo '{yourPwd}' | openssl passwd -6 -stdin
'''

In the same boot directory.
Create a file called userconf.txt.

'''
nano userconf.txt
'''

Fill with just one line as follow: 

username:encryptedPwd



## 4 - Set up Wifi

In the same boot directory.
Create a file named wpa_supplicant.conf.  

'''
nano wpa_supplicant.conf
'''

fill it with:

'''
 country=your_country_code
ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1

network={
    ssid="your_wifi_ssid"
    psk="your_wifi_password"
}
'''

## 5 - SSH connection to raspberry pi

Insert the SD card in the raspberry, turn it on and wait 2 minutes to its first boot complete.

With your computer connected to the same Wifi Network, check the machines connected to your local network.

'''
arp -a
'''

If you find a raspberry there, try to ping it.

'''
ping raspberrypi
'''

Try to ssh connect to it.

'''
ssh pi@raspberrypi
'''


## 6 - Install VNC

In the raspberry terminal:
Run the config to enable VNC.

'''
raspi-config
'''

Find it in interfaces (I think).

install VNC:

'''
sudo apt install realvnc-vnc-server realvnc-vnc-viewer
'''

Install RealVNC app in your phone and connect to:

default address: raspberry.local:0 
name: raspberrypi

## 7 - enable audio through HDMI   

This is just because I wanted to watch Netflix and somehow the audio was not being outputed through HDMI. This will fix it.

'''
sudo apt install leafpad
sudo leafpad /boot/config.txt
''' 

uncomment the line: 
hdmi_drive=2

save and reboot.
