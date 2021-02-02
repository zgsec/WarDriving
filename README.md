# WarDriving
How a Raspberry Pi Could Ruin Your Life

## Introduction
  While the term may sound suspicious or ominous, Wardriving is simply the act of searching and mapping wireless access points. While it is not illegal to scan access points, it is illegal to steal or cause a denial of service so be sure to research your local laws and ordinances before undertaking this project. 
  
  
 ### SETUP

*Please note that this is the hardware I used, there are many other options so be sure to find something that works for you! All of these products were sourced via amazon*
    
    
           HARDWARE
          - Raspberry Pi 4
          - ALFA AWUS036NH High Gain USB Wireless (Long-Range Wifi Network Adapater) 
          - VK-162 G-MOUSE USB GPS DONGLE
          - RAVPOWER Portable Charger 
          
           SOFTWARE
          - Raspbian Buster
          - BalenaEtcher
          - PuTTY
          - Kismet
          
          
  
  
**Now that you have your hardware, let's get it setup and ready to WARDRIVE!**

1. Download the latest version of [Raspbian](https://www.raspberrypi.org/downloads/raspbian/)

2. Unzip and flash your new Raspbian OS using [BalenaEtcher](https://www.balena.io/etcher/)

3. Find your raspberry pi on your network. (You can download [Wireless Network Watcher](https://www.nirsoft.net/utils/wireless_network_watcher.html) or access your router to locate the devices on your network.

4. Once you locate the IP address of your Pi, we connect via SSH using [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html)

5. Once connected, you will be asked for your username and password - the defaults are normally:
```
Username: pi
Password: raspberry
```

6. **BE SURE TO CHANGE THE DEFAULT CREDENTIALS! You can do this using the setup**:

```sudo raspi-config```  or by just typing :  ```passwd```

7. It is important to update after setup so it is necessary to insert the following commands:
```sudo apt-get update```
```sudo apt-get upgrade -y```

8. Now that the initial setup is complete, we must install the necessary software to perform our audit.
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

11. It is important to note the wifi card in use, therefore we must first type : ```ifconfig```
This command outputs information of all network interfaces - we may have to change kismet.conf to the proper network interface (in our case wlan1)

12. We may need to change this configuration file to denote the proper network interface (our ALFA card) ```sudo nano /usr/local/etc/kismet.conf```
*Please note that your file kismet.conf may be locate in a different folder.*

Find and replace the line that contains : ```#ncsource=~~wlan0~~ wlan1```

13. Now that we have everything downloaded, it is important to test our build before starting our WarDrive. 
Type ```cpgs``` - This may take awhile for the client to start and connect to the service, after many hours of beating my head against my desk I realized I needed to orient the GPS unit on my windowsill in order for it to connect. 


# The WarDrive
*Once our GPS unit is working its time to conduct our first trial run!*

1. Create a new directory ```mkdir test``` and move into your new folder ```cd test``` (It is important to be in the proper directory before starting kismet)
2. ```screen -S kismet``` (*Screen allows us to attach or detach to a session which keeps Kismet running when we are no longer near our wireless access point*)
2. Open Kismet '''sudo kismet'''
3. Once running you can detach from your screen session using:
```CTRL + A
D
```
4. When prompted, click **YES** to auto start Kismet Server.
5. Now Kismet is up and running! You are ready to start wardriving!


Once all the data is collected and you've returned back to your lab, you can close Kismet using ```CTRL+C```


Your results can be used to map out the geographical location using [Wigle] (https://wigle.net/map) or Google Earth.


**HAPPY AUDITING**










