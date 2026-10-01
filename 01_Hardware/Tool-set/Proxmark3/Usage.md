IMPORTANT THIS IS based on commit : d2783214e3001193d77cddedc721373bd1d16265 as head changes MIGHT break anything that happens here

Firstly you should follow the [Setup](Setup.md) otherwiese it WILL not work, you will need to compile and your own firmeware for this thing. There is no way around it. 


from here on outwards all commands will be executed from 
```shell
./pm3
```

so everything noted as shell is the shell of the proxmark and not bash shell!!



# Reading lf tags 

In this example we are dealing with a EM410x ( RF/64 ) Tag.


firstly we will read all the data of the tag with 

```proxmark 
lf read
```


then we need to use the we need to demulate the data 

we do that by using one of the following command

```proxmark
data rawdemod
```

the possible commandline arguments are as of now 


```proxmark 
    data rawdemod --fs             -> demod FSK - autodetect
    data rawdemod --ab             -> demod ASK/BIPHASE - autodetect
    data rawdemod --am             -> demod ASK/MANCHESTER - autodetect
    data rawdemod --ar             -> demod ASK/RAW - autodetect
    data rawdemod --nr             -> demod NRZ/DIRECT - autodetect
    data rawdemod --p1             -> demod PSK1 - autodetect
    data rawdemod --p2             -> demod PSK2 - autodetect
    
```


### Emulating an abitrary tag with the proxmark 3 


in this case we will emulate a EM410x Tag 


```proxmark 
lf em 410x sim --id <ID Data> --gap 0
```