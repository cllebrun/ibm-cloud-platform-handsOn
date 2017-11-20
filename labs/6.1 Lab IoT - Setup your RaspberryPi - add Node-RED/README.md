
# Introduction

This tutorial demonstrates how to setup your Raspberry Pi and install the Node-RED [Node-RED](http://www.nodered.org) tool for wiring together sensors,  APIs, data and code.


# Objectives
* You will setup your Raspberry Pi.
* You will install Node-RED on your Pi.


# Pre-Requisites
Material:
*   1 raspberry pi 2 or 3
*	sensors
*	minimum 8GB micro sd card
*	Ethernet connection (+Ethernet cable) if using a Raspberry Pi 2 or wifi for Raspberry Pi 3
*	USB Keyboard
*	USB mouse
*	Hdmi Screen (+hdmi cable)
*	optional: grove pi + grove sensors or Sense hat




# 1. Setup the Raspberry Pi

If you did not bought a pre-installed Raspbian card (Follow the steps at https://www.raspberrypi.org/learning/software-guide/quickstart/) to:
-	Format the SD card
-	Download NOOBS
-	Install NOOBS on the SD card

If you bought a pre-installed Raspbian card: 
-> You can directly go to the nex step

-	Insert the SD card in the Raspberry Pi, plugin mouse, screen, keybord, power.
-	Start the Raspberry Pi and install Raspbian from NOOBS

Follow these steps at: 
https://www.raspberrypi.org/learning/software-guide/quickstart/



# 2. Setup Node-RED

•	Install/Upgrade Node-RED

When your Raspberry has started, you need to upgrade your Node.js version and install the latest version of Node-RED for Raspberry Pi
Open a command line:
```
  $ bash <(curl -sL https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/update-nodejs-and-nodered)
```

 
It can take few minutes.

•	Autostart on boot

To make sure Node-RED will run when the Pi boots, setup the autostart on boot:
```
  $ sudo systemctl enable nodered.service
```


•	Running Node-RED
To start Node-RED, you can either:

- on the Desktop, select Menu -> Programming -> Node-RED.
- or run 
```
  $ node-red-start
```
in a new terminal window.
Note: closing the window (or ctrl-c) does not stop Node-RED running. It will continue running in the background.
To stop Node-RED, run the command 
```
  $ node-red-stop
```
To see the log, run the command 
```
  $ node-red-log
```

•	Adding nodes to preloaded version

Nodes can be now be managed via the built in palette manager. You can also use NPM to install/update nodes in the palette.
When you have started your Node-RED local in your browser, you can manage the palette


# 3. If you have a GrovePi

If you are using a GrovePi board and GrovePy sensors to easily connect and use sensors with the RaspberryPi: https://www.dexterindustries.com/shop/grovepi-starter-kit-2/
You need to follow some steps to setup. 
•	Change directories go to an appropriate location on your Pi where you want the GrovePi files to be stored (We recommend that you do it on the Desktop because it is easy to access and compatible with all our examples too). Clone the GrovePi git repository:
```
  $ git clone https://github.com/DexterInd/GrovePi
```
•	When the repository is done downloading, there should be a new folder called “ GrovePi “ . Go to the Scripts folder in the GrovePi folder:
```
  $ cd GrovePi/Script
```
•	Make the install.sh bash script as executable. We do this by modifying the permissions of the script 
```
  $ sudo chmod +x install.sh 
```
•	Start the script. You must be the root user, so be sure to include “ sudo ” !
```
  $ sudo ./install.sh
```
Press “ Enter ” to start when you are prompted.
The script will download packages from the internet which are used by the GrovePi. Press “ y ” when the terminal prompts and asks for permission to start the download.
The Raspberry Pi will automatically restart when the installation is complete.
(for more information: https://seeeddoc.github.io/GrovePiPlus/res/Setting_Up_Software_for_GrovePi.pdf)
•	Install the Grove Pi nodes:
Locate in your Node-RED repository:

```
  $ cd .node-red
```
Install the following nodes libraries:

```
  $ npm install node-red-grovepi-nodes
```
```
  $ npm install node-grovepi
```

Stop and Start Node-RED:
```
  $ node-red-stop
  $ node-red-start
```