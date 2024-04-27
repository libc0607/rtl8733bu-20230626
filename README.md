# rtl8733bu-20230626
An RTL8731BU & RTL8733BU Linux driver, mainly modified for FPV.  
The modules have an ultra-small size (\~13mm\*12mm), \~2W total power consumption when \~20dBm (100mW) TX, dual-band Wi-Fi, 1T1R, MCS 0~7, 20MHz BW, LDPC encoding, and a reasonable price of 13.5 CNY (<\~$2, [BL-M8731BU4](https://www.b-link.net.cn/product_34_287.html)).  

Suitable for FPV beginners, or any **ultra-small size** all-in-one digital video transmitters.  
... But you should always use a better adaptor for RX with at least 2 antennas and LNAs.  

Check out the original driver tarball from the module vendors at [here](https://github.com/libc0607/rtl8733bu-20230626/blob/c42db387516b28bbd1fde8dca9b57788c046fcd0/RTL8733BU_WiFi_linux_v5.13.0.1-112-g10248f4f3_COEX20230616-330e.20230703.tar.gz).   
Android 4~12 driver is included also, but I have no idea how to use them.   

Still working in progress. Needs more tests.    
 - OpenIPC integration: done, tested on my MC-L12(SSC30KQ), see [here](https://github.com/libc0607/openipc-firmware/commit/6c452ecab5e4490241aa5850f610767a845b919d) for patch, then add ```BR2_PACKAGE_RTL8733BU_OPENIPC_FPV=y``` to your board config  
 - Packet injection: good, the MCS rates work well, LDPC encoding works too  
 - Monitor (RX): good   
 - Set TX power by ```iw```: supported, validated by my SDR receiver. Should set ```rtw_tx_pwr_by_rate=0 rtw_tx_pwr_lmt_enable=0``` when ```insmod```
 - Set Wi-Fi regd in OS: Set ```rtw_regd_src=1``` when ```insmod```
 - 10MHz BW: Not claimed to be supported by Realtek, no output when testing either  
 - Unlock center frequency: All frequencies between 5080MHz ~ 6030MHz (5MHz step) in 5GHz band. They really cost-downed it too much  
 - EDCCA Threshold: merged, not tested yet  
 - SIFS, Slot time, etc.: TBD  
