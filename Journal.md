# RP2040 DBUG Probe

## 25/12/25

I broke my old UART to USB adapter and recently saw an article about the [Raspberry Pi Debug Probe](https://www.raspberrypi.com/documentation/microcontrollers/debug-probe.html) and though why not make one myself 🤔

So that's exactly what I did; I started off with just researching about the Debug probe and was figuring out how exactly to execute this project, and luckily for me the schematics were open sourced for the debug probe. 
It was a really simple board with just a RP2040, some passive, a LDO and headers!! And so the journey began, I started off with the minimal RP2040 schematics which I copied off the hardware design guide, and then all I really needed to do was to add the connectors. At this point in time I referred to the Debug probe schematics and noticed that they actually included a level shifter on the RX lines of the boards, so I just decided to copy their schematics; but here's where things started getting bad, KiCad started bugging out for me, it was no longer detecting new symbols added using easyeda2kicad and that really slowed me down. I spent 30 minutes trying to solve the issue with no success, In the end I decided to reinstall easyeda2kicad and delete the old symbol and footprint folders; This initially did not work but after restarting my computer and KiCad everything started to work and I could finally finish up my schematics.


![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/6fd142acf3fd280e_f12gSD0mdy4AAAAASUVORK5CYII_)


Moving on to the PCB; first step was to setup the board outline, I went with the original raspberry pi debug probe board dimensions of 22x32mm which I got from their mechanical drawing.
![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/70bd4c67a04c8ed6_gOUAT0wAAAABJRU5ErkJggg__)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/03ccf3239cd3b31e_QQIECBAgAABAgQIECBAgACBegIqHgIECBAgQIAAAQIECBAgQIAAgXSBP324FmUdK4B3AAAAAElFTkSuQmCC)

While doing the layout I decided to assign some net classes and color code them to visualize the layout better.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/ffcb2194f9a1fb1f_bhmmD2LOkoAAAAAElFTkSuQmCC)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/8c56d00f448841e0_uEYF4FdZc3IAAAAASUVORK5CYII_)


Routing the board was pretty fun. I did however had to reroute the QSPI flash traces a couple dozen times cause I kept changing the layout.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/e298bf4b5be99068_EF0rFBP670udASDEnazU66KPbaRjJ7Wmp2HSFetp4HbVes4VCpl6baQMaO5v8HgZ6KYvezGVUAAAAASUVORK5CYII_)

Pro Tip to anyone who's doing a RP2040/RP2350 or honestly any design where the chip more than one rail; go with a power pour as shown below. Make the outer fill region be 3v3 or whatever has the most pins and let the inner pour be the one with less pins.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/b622bc2fdeb7a441_xQHWDRyWuQQAAAABJRU5ErkJggg__)


Here's some routing progression pictures :D

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/5af7452ff4fbc281_wfkH_PuJJI4DgAAAABJRU5ErkJggg__)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/5efc7f9c3c8fd197_wcaCQ7UUAdvAAAAAABJRU5ErkJggg__)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/946ac9b5463b066c_cFgAAAAAAAAAAAHZxHcforIpg1DJB3ZxyZAAAAAAAAAAAAORHwXeNzqoIRi1jWvIOAAAAAAAAAACAfDCxSrQdwWgOhIvbAgAAAAAAAAAAAKPmOk50Uyr_P8J4xtYmCR1_AAAAAElFTkSuQmCC)


I went with a 3V3 pour on top and a ground plane on the bottom layer, I also tried my best to keep a solid ground plane and avoid routing anything on the bottom layer, since this was a pretty easy board I managed to route everything on just two layers (this is my first 2 layer board in a very long time).

This is how the board looked after I finished routing everything up

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/62b5e73331007dc7_yATV6MOuTyYAAAAASUVORK5CYII_)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/64ca18c767f5c8ee_P8sMY1Ag_grEAAAAAElFTkSuQmCC)

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/5643ff5a2ca17ac1_P8sMY1Ag_grEAAAAAElFTkSuQmCC)


Now all that was left to do was add some testpoints and silkscreen. I added test points for the voltage rails and the swd interface of the rp2040 itself as I couldn't route it out to some headers. For silkscreen I just added some labels for the pin headers and test points and yeah that was it, didn't really add anything as this was a pretty small board, though I might add the oshw logo on this later.

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/33842c2c418040cc_w8_o0N0HfAWbQAAAABJRU5ErkJggg__)



I was basically done now! all that was left to do was to get the production files and setup a github repo; but before that we must run DRC and well uh 

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/71c899fc5caa11d2_wM442cOmKci8gAAAABJRU5ErkJggg__)

It's not as bad as it looks though, most of these were because I forgot to change the default  constraints in KiCad and the rest were errors because of the hole in the USB-C footprint which I just excluded.

Finished everything up by assigning lcsc numbers to components and generating a BOM

![image](https://hc-cdn.hel1.your-objectstorage.com/s/v3/272ab69b42a04976_sHdvgWAAAAAElFTkSuQmCC)


## Time Spent: 6.5 Hours

