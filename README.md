# Wardriving
The Art of Auditing Your Neighborhood 

## Introduction
  While the term may sound suspicious or ominous, Wardriving is simply the act of searching and mapping wireless access points. While it is not illegal to scan access points, it is illegal to steal or cause a denial of service so be sure to research your local laws and ordinances before undertaking this project. The objective of this project is to create a map of my town's wireless access points and create a statistical analysis of the varying network encryption protocols.
  
  
 ### SETUP

*Please note that this is the hardware I used, there are many other options so be sure to find something that works for you. All these products were sourced via amazon*
    
    
           HARDWARE
          - Raspberry Pi 4
          - ALFA AWUS036NH High Gain USB Wireless (Long-Range Wi-Fi Network Adapter) 
          - VK-162 G-MOUSE USB GPS DONGLE
          - RAVPOWER Portable Charger 
          
           SOFTWARE
          - Raspbian Buster
          - BalenaEtcher
          - PuTTY
          - Kismet
          
          
  
  
**Now that you have your hardware, let's get it setup**

1. Download the latest version of [Raspbian](https://www.raspberrypi.org/downloads/raspbian/)

2. Unzip and flash your new Raspbian OS on your microSD using [BalenaEtcher](https://www.balena.io/etcher/)

3. Find your raspberry pi on your network. (You can download [Wireless Network Watcher](https://www.nirsoft.net/utils/wireless_network_watcher.html) or access your router to locate the devices on your network.)

4. Once you locate the IP address of your Pi, we connect via SSH using [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

5. Once connected, you will be asked for your username and password. The default credentials are as follows:
```
Username: pi
Password: raspberry
```

6. First order of business: **CHANGE DEFAULT CREDENTIALS!** You can do this using the setup:

```sudo raspi-config```  or by just typing :  ```passwd```

7.  It is important to update the package lists and install the newest versions. 
```sudo apt-get update```
```sudo apt-get upgrade -y```

8. Now that the initial setup is complete, we need to download all the software used for wardriving

```sudo apt-get install -y screen gpsd libncurses5-dev libpcap-dev tcpdump libnl-dev gpsd-clients python-gps```

9. In order to get GPSD to work properly we must edit the configuration file, please note that **DEVICES** may be different depending on the GPS unit you purchased.  ```sudo nano /etc/default/gpsd```

```START_DAEMON="true"
GPSD_OPTIONS="-n"
DEVICES="/dev/ttyUSB0" 
USBAUTO="true"
GPSD_SOCKET="/var/run/gpsd.sock"
```

10. Now we install, unzip and configure Kismet! 
```
wget http://www.kismetwireless.net/code/kismet-2016-07-R1.tar.xz
tar -xvf kismet-2016-07-R1.tar.xz
cd kismet-2016-07-R1/
./configure
make dep
make
sudo make install
```

11. It is important to note the Wi-Fi card in use: ```ifconfig```
This command outputs information of all network interfaces - we may have to change kismet.conf to the proper network interface (in our case wlan1)

12. We may need to change this configuration file to denote the proper network interface (our ALFA card) ```sudo nano /usr/local/etc/kismet.conf```
*Please note that your file kismet.conf maybe located in a different folder.*

Uncomment, find and replace the line: ```#ncsource=wlan0``` to ```ncsource=wlan1```

13. Now that our setup is complete, it is important to test our build before starting our WarDrive. 
Type ```cpgs``` - This may take a while for the client to start and connect to the service, after many hours of beating my head against my desk I realized I needed to orient the GPS unit on my windowsill in order for it to connect. 


# The WarDrive
*Once our GPS unit is working its time to conduct our first trial run!*

1. Create a new directory ```mkdir test``` and move into your new folder ```cd test``` (It is important to be in the proper directory before starting kismet)
2. ```screen -S kismet``` (*Screen allows us to attach or detach to a session which keeps Kismet running when we are no longer near our wireless access point*)
2. Open Kismet : ```sudo kismet```
3. You can detach from your screen session using:
```
CTRL + A
D
```
4. When prompted, click **YES** to auto start Kismet Server.
5. Now Kismet is up and running! You are ready to start wardriving!


Once all the data is collected and you've returned to your lab, you can close Kismet using ```CTRL+C```
Now that we have our raw data we can use the script *netxml2kml.py* to change the file to .kml so we can upload our data to  [Wigle](https://wigle.net/map) or Google Earth.


**HAPPY AUDITING**










