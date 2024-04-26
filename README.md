# rtl8733bu-20230626
An RTL873xBU Linux driver, mainly modified for FPV.  
The modules have an ultra-small size (\~13mm\*12mm), \~2W total power consumption when \~20dBm TX, dual-band Wi-Fi, and a reasonable price of \~15 CNY (\~$2). 

Check out the original driver tarball from the module vendors at [here](https://github.com/libc0607/rtl8733bu-20230626/blob/c42db387516b28bbd1fde8dca9b57788c046fcd0/RTL8733BU_WiFi_linux_v5.13.0.1-112-g10248f4f3_COEX20230616-330e.20230703.tar.gz). Android 4~12 driver is included also, but I have no idea how to use them.   

Still working in progress.   
 - Packet injection: seems working, needs more test  
 - Monitor (RX): good  
 - Set TX power by ```iw```: supported, validated by my SDR receiver   
 - 10MHz BW: Not supported by Realtek, not claimed to be supported by Realtek, no output when testing either  
 - Unlock center frequency: 5080MHz ~ 6030MHz. They really cost-downed it too much  
 - EDCCA Threshold: merged, not tested yet  
 - SIFS, Slot time, etc.: TBD  
