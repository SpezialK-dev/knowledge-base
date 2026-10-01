---
tags:
  - Information_collection
  - GRUB
  - Bootloaders
TLDR: Some Tips and Tricks for Grub
---
# Summary of the page 

Covers the following information :
- deaktivating signature checking in grub 
- getting into the grub shell 

# Information 
## deactivating signature checking in grub 

https://www.gnu.org/software/grub/manual/grub/html_node/Using-digital-signatures.html

with 

```
set check_signatures=no
```
typed into the grub shell theoretically you can deactivate checking the signatures embedded into the grub

## Getting into grub shell

if you have no timeout and you dont see a grub screen but you know you can edit the grub config simply press F8+any other key [Source](https://srobb.net/rhboot.html). Atleast it worked under Fedora. 


