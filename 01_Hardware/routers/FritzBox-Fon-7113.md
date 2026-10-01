
# Normal Information 

[User manual(DE)](https://avm.de/fileadmin/user_upload/DE/Handbuecher/FRITZ_Box/Weitere_Box/Handbuch_FRITZ_Box_Fon_WLAN_7113.pdf)
[boxmatrix ](https://boxmatrix.info/wiki/FRITZ!Box_Fon_WLAN_7113)

## my specific version

[Boxmatrix DE Version](https://boxmatrix.info/wiki/FRITZ!Box_Fon_WLAN_7113_DE)
I have the De Version

# Interfaces

## JTAG 

on the bottom side we have 2x 14 pins for Jtag interfaces 
these are just pads and I wont use them for now. 
## Serial 


We have a 4 pin serial these are really pins so we just need to find the pinout

![[FritzBox-Fon-7113-Serial_Pinout.png]]
### Pin Layout 
from left to right 
the leftmost pin(the rectangular one) is pin 1 and the rightmost pin is 4.

| Number | Pin meaning |
| ------ | ----------- |
| 1      |             |
| 2      |             |
| 3      | TX          |
| 4      | Ground      |

Pin 3 was was detected based on using a Multimeter

Transmits regular data during boot up so its definitely the transmit pin. 
These pins are sending out in the RS232 Serial format more Information [here](https://www.raveon.com/wp-content/uploads/2019/01/AN236SerialComm.pdf). 

The baude rate is : 
tested with a flipper zero 

| tested baude rate | success |
| ----------------- | ------- |
| 9600              |         |
| 115200            |         |
