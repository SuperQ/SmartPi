# SmartPi Energy Monitor 

[![CircleCI](https://circleci.com/gh/nDenerserve/SmartPi.svg?style=svg)](https://circleci.com/gh/nDenerserve/SmartPi)

[SmartPi open source](http://www.enerserve.eu.eu/products/smartpi).

## Forum
https://forum.enerserve.eu

## Installation
The easiest way is to download a prebuild image.
Further information under: https://www.enerserve.eu/service.html

Download Raspbian Bullseye Lite (64bit) (the big bullsyeye should also work) from https://www.raspberrypi.com/software/ and copy it on your SD card. The easiest way is to use the Raspbbery Pi Imager.

Create a user with the name smartpi and a password of your choice. We use the password smart4pi here. During installation, please use the password you have chosen and replace smart4pi with the one you have chosen.

##### Update packet list and update packages

    sudo apt update
    sudo apt upgrade

##### Install additional packages.

    sudo apt install sqlite3 git i2c-tools avahi-daemon libpam0g-dev


##### Enable i2c kernel module

`i2c-dev` is required for communicating with the SmartPi.

To check to see if the module is loaded:

    sudo lsmod | grep i2c

This should return something like this:

    i2c_dev                20480  0
    i2c_bcm2835            16384  0


If the module is not listed, activate i2c via 
sudo raspi-config or add it in following way to the system: 

    echo 'i2c-dev' | sudo tee -a /etc/modules
    sudo modprobe 'i2c-dev'

##### Test if i2c is correctly enabled:

    i2cdetect -l

The output should list your I2C channel. "i2c-1" in my case.

    i2c-1   i2c             20804000.i2c                            I2C adapter

##### Scan I2C bus for connected devices
Channel 1 in my case

    i2cdetect -y 1

In case of an SmartPi connected to an RPI3, the output should look like this:

         0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
    00:          -- -- -- -- -- -- -- -- -- -- -- -- --
    10: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
    20: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
    30: -- -- -- -- -- -- -- -- 38 -- -- -- -- -- -- --
    40: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
    50: -- 51 -- -- -- -- -- -- -- -- -- -- -- -- -- --
    60: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
    70: -- -- -- -- -- -- -- --

##### Remove old go version

    sudo apt-get remove golang
    sudo apt-get autoremove

##### Install go
Download the archive and extract it into /usr/local, creating a Go tree in /usr/local/go.
Currently version 1.17.2 is up to date. You may need to adapt the filename according to latest version.

    cd /usr/local
   # curl -s https://storage.googleapis.com/golang/go1.17.4.linux-arm64.tar.gz | sudo tar -xvz

    sudo wget https://go.dev/dl/go1.18.2.linux-arm64.tar.gz 
    sudo tar -xvzf go1.18.2.linux-arm64.tar.gz
    echo 'PATH="/usr/local/go/bin:${PATH}"' | sudo tee -a /etc/profile




In order for the `${PATH}` to be updated, you will need to logout.

Create a directory to contain your Go workspace, for example `${HOME}/go`,
and set the GOPATH environment variable to point to that location.

    mkdir "${HOME}/go"
    export GOPATH="${HOME}/go"

##### Building source

    cd ~
    git clone github.com:nDenerserve/SmartPi.git
    cd ~/SmartPi
    make

NOTE: Executables files are located in the bin directory



## Change Log

### 11/28/11/16
 * Added MQTT Client
 * producecounter and consumecounter files make use of Databasedir -> co-located to rrd database
 * fixed "}" compilation issue
 * Added this readme.md

### 02/10/17
 * changed from rrdtool to sqlite3
 * added csv-export
 * changed from Bootstrap to Angular Material
 * change datelayout in API to RFC3339
 * fixed errors in datehandling
 * added week consumption

 ### 05/24/22
 
