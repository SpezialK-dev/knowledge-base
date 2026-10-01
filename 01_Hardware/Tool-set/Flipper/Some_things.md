This File contains some things that you can do with the flipper that you might not know, 
these are all done with the Momentum Firmware. Maybe even with the Standart firmware. But I have not tested it. 

# Testing speakers

GPIO > Signal Generator 
You use the Signal Generator to test the speakers

There you use the PWM Generator. 
The Settings are as follows: 
GPIO Pin : 2(A7)
Frequency : 1000 Hz
Pulse Width : 50 %

you now need to connect the Pin A7 to one of the speaker wires. 
and the second speaker wire you need to connect to the GND(8) Pin.

This should generate an audio beep tone through the speakers. 


# UART

Simply use the UART Terminal 

GPIO > Uart Terminal

The settings are dependent on your device that you are looking at but you can choose the pins and also need to connect the ground Pin and then this should work. 


# Continuity tester

just simply use the wiretester mode. This is actually a really good continuity tester. Then simply insert to male to male cables into the marked pins and you are good to go and test for continuity