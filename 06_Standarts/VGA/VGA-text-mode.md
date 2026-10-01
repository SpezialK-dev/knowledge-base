# Sources 

https://wiki.osdev.org/VGA_Hardware
https://www.breatharian.eu/hw/picovga/index_en.html

# Memory in drivers

In text mode the memory layout is quite simpel in normal mode 4 64K banks are used
## Plane 0 

Contians all the charackter codes to for each cell

## Plane 1 
contains the respective attributes

## Plane 2 
Plane 2 contains the font data. For each of the 256 available characters this plane has 32 bytes reserved. Each byte represents one horizontal cross section through each character. The first byte of each group defines the top line, each next byte describes the rows below it. For every set bit, the foreground color is used, For every cleared bit, the background color is used. 

