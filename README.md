# MegaPi RPi3 - Makeblock Library v3.24

Allow control of MegaPi via RPi3 with Python/Node

##Connecting RPi3 & MegaPi:

The RaspberryPi communicates with MegaPi (and many microcontrollers) via the Serial Port.
Skim over the eLinux page, our use case is the latter listed but it's good to understand what is going on.

[eLinux RPi Serial Connection](http://elinux.org/RPi_Serial_Connection)

Now we have a at least very basic understanding of how the 2 are going to communicate we need to make sure
our RPi3 is ready to communicate with MegaPi, by default it's setup for the Serial Console (covered in link above).
So we need to disable that, so we can use it.

1. From a terminal:
```
sudo raspi-config
```
   Go to the *"Advanced Options"* find the *"Serial"* option and disable it.

2. RPi3 now disables the mini-UART when the Serial Console is turned off, what this means for you
   is that the RaspberryPi won't recognize the port the MegaPi is connected to.  We can fix this by
   telling our RaspberryPi to enable the UART on boot.
   From a terminal:
```
sudo nano /boot/config.txt
```
   Find the line (or add if missing); *"enable_uart=0"* and change it to *"enable_uart=1"*

   Setting the enable_uart to 1, now also sets the core_freq so it's no longer necessary to worry
   about having differing baud-rates on the mini-UART.

   **Note:** I've not noticed any issues running on the mini-UART (/dev/ttyS0), if you do not need bluetooth you
   can always disable that functionality on the RaspberryPi and use the original UART (/dev/ttyAMA0) to communicate
   with the MegaPi. To do this you need to add another line to /boot.config.txt "dtoverlay=pi3-disable-bt"


###Python Code

```python
from megapi import *
if __name__ == '__main__':
   bot = MegaPi()
   bot.start('/dev/ttyS0')
   bot.sleep(.1)
   bot.encoderMotorRun(1, 100)
```

###Serial Byte Spec
```
 ff 55 len idx action device port  slot  data  a
  0  1  2   3   4      5      6     7     8 
```

**MORE SOON**

##Firmware version

Examples/Firmware_for_MegaPi_New <- To distinguish between MakeBlock Offical Firmware

##Tested Functions / Sensors

#Me UltraSonic Sensor
#Encoder Motor (all functions)

##Coming soon:
1. Easier control of arm grabber
2. Gyro sensor compatibility


