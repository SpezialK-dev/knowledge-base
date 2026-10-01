Mifare Tags work on [13.56Mhz](https://en.wikipedia.org/wiki/MIFARE) 
# Mifare Classic 1k 

## IOS
as it appears IOS [does not](https://forums.developer.apple.com/forums/thread/133179) support reading of Mifare Classic 1K cards. You can use an external reader to still read them but it does not work out of the box as it does under android. Other Mifare standarts [seem supported](https://developer.apple.com/documentation/corenfc/nfcmifaretag). 


# Mifare Ultralight C 

## Sources
- [fact sheet](https://www.nxp.com/docs/en/fact-sheet/MIFARE-ULTRALIGHT-C-FS.pdf)
- [product sheet](https://www.nxp.com/docs/en/data-sheet/MF0ICU2.pdf)


## Security 


- 3DES Authentication 
- "Anti cloning support" by unique 7 byte Serial number for each device
- ...

### Some more in depth 
[how these are used sometimes](https://tech.springcard.com/2010/mifare-ultralight-c-low-cost-yet-high-security/).
[how authentication works](https://www.rfidcard.com/understanding-mifare-ultralight-c-and-its-role-in-secure-access-control/)

### looking into breaking the security of these tags

what we currently have : we have a reader 

### How the authentication works 

[how authentication works](https://www.rfidcard.com/understanding-mifare-ultralight-c-and-its-role-in-secure-access-control/)
based on the mentioned article the authentication works as follows 


1. The reader device sends a random number to the card.
2. The card encrypts the random number using the 3DES algorithm and sends the encrypted data back to the reader device.
3. The reader device decrypts the data using the same 3DES key and checks the decrypted data against the original random number. If the data matches, the authentication process is complete, and communication between the card and the reader device can proceed.

so if we wanted to emulate this we would need the key.  

The key is a `root` key and the ID of the card. Meaning if you get a root key of an installation you could crack all of them. If they are based on physical installations and not something else. 