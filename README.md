# SpinJoy (EA-JM01-Arcade-Spinner_Edition)

## Introduction
Spin Joy EA-JM01 Arcade Spinner Edition (Arcade Spinner for 1UP).

[Jump to Project Files](https://github.com/dekkit/SpinJoy_EA-JM01-Arcade-Spinner_Edition/tree/main/SpinJoy_EA-JM01-Arcade-Spinner_Edition)

After having no luck finding a previous ardunio project that had successfully converted EA-JM01 Arcade Spinner to USB (for PC / Mister etc), i decided to debug the hardware using Serial.print and rewrote a few lines in **drv_proc(void)** to properly read the inputs from the EA-JM01 (when turning slowly and also when rapidly turning in each direction).    The encoder does not behave exactly like other rotary encoders and as result most other arduino rotary encoder examples don't work well at all. 

This is a modification of https://github.com/Niels3RT/SpinJoy spinner/joystick combo, which was in turn a  modification of  Sorgeligs PaddleTwoControllersUSB project](https://github.com/MiSTer-devel/Retro-Controllers-USB-MiSTer/tree/master/PaddleTwoControllersUSB) in a single Arduino Pro Micro (ATmega32U4) board :)

The Arduino shows up as 2 devices, a digital joystick+12 buttons with added wheel+dial and a mouse that moves the x/y axes by rotating the spinner. This works nicely with Mister and was tested on Arkanoid and Block Gal cores.

Aside from adapting the code to use the EA-JM01 Arcade Spinner, it should function exactly like Spinjoy.

### Spinner (Arcade Spinner for 1UP)
The .ino code has been specifically adapted for use with an EA-JM01-Arcade-Spinner and should work with the range of generic clone arcade spinner for 1up / replacement 1up arcade spinner etc that you see online.    These can be purchased relatively cheaply from most online ecommerce shops.  I suspect they maybe a clone of a higher quality product, however they function really well for home DIY spinner projects.   They have nice durable  metalic / chrome 'look and feel'.  

If you look closely at the pictures you should see the PCB has a 6 pin socket (and you can see labels on the other side of the socket: GND, K1, K2, nc, nc, VIN).  These are the pins you need for this project.

![image](https://github.com/user-attachments/assets/eeb995bd-31ae-4390-8922-6c1055703a02)


Example of pin labels
![1000002434](https://github.com/user-attachments/assets/f4cad588-cbac-4ee7-b888-b21fcf2074b6)

Note: These clone Spinners do have a very slight resistance to them, so they do not spin endlessly like the original arcade spinners. 

### DIP Switches on Arcade Spinner 
The DIP1 and DIP2 control number of pulses per turn
| DIP 1  | DIP 2 | DIP 3| Description|
|------|--------|--------|--------|
|OFF| OFF| - |  16 |
|ON| OFF| - | 64 |
|OFF |ON| -| 512 |
|ON |ON |-| 1024 <---use this|

The DIP3 controls length of each pulse
| DIP 1  | DIP 2 | DIP 3| Description|
|------|--------|--------|--------|
| -| - | OFF| 8ms (Arcade 1Up default setup) |
|- | - | ON|  0.5ms (very quick)  <---use this|


### Joystick/Buttons
There are many Arcade Joystick/Button sets available on EBay, AliExpress or Amazon.


### Connecting/Assembling

![Arduino Pro Micro Pinout](Pinout.png?raw=true "Arduino Pro Micro Pinout")

**Rotary encoder 6 pin socket**

On the back of the encoder  pcb are  2 x sockets, the 6 pin socket is the one you should use to connect to the arduino as follows:
| PCB  | Function |
|------|--------|
| GND  | Ground |
| K1 | Enc A  |
| K2 | Enc B  |
| NC | Not Connected|
| NC | Not Connected|
| VIN / VCC | VCC (5v) |

NOTE: The PCB is labelled for each pin so check to ensure you have the pin order is the same as described here.   THe EA-JM01 Arcade Spinner works fine on 5v.


### Source parameters
Firmware has several definitions to tweak for best experience:

1. SPINNER_PPR: for EA-JM01 Arcade Spinner  LOWER =  more sensitive,  HIGHER = less sensitive. I set this to 8.
2. DEBOUNCE_TOP: a counter is used to debounce buttons and stick movement. Set to 1 to completely disable debouncing. The switches in my (cheeaaap) joystick/button set need a value of 32 to stop bouncing, your mileage may vary. The menus in Mister scripts seem to be a good place to test this. This counter is only applied to switch release to minimize lag.
3. USE_AUTOFIRE: comment to disable autofire function, buttons 11/12 are reported via USB in any case.
4. AUTOFIRE_TUNE_FACTOR: bigger number -> faster autofire rate modification, lower number -> finer adjustment, default is 4

### Autofire
Autofire for button 1 is enabled by pushing button 12 (as long as its down), if you plan to use it its a good idea to connect a switch there.

To modify the fire rate hold down button 11 and use the spinner. The new rate is saved to eeprom when button 11 is released.

## License
This project is licensed under the GNU General Public License v3.0.
