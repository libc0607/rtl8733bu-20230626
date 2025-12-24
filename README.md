# rtl8733bu-20230626
RTL8731BU & RTL8733BU Linux driver, mainly modified for FPV.  
This repository will be my playground (mainly for dirty low-level hacks. so, no performance guarantee in this repo).  
OpenIPC had already added this driver, see [OpenIPC/realtek-wlan](https://github.com/OpenIPC/realtek-wlan/tree/rtl8733bu_fpv).  

Download pre-built OpenIPC firmware with this driver integrated [here](https://github.com/libc0607/openipc-firmware/releases/tag/latest) to help test new features.  
Installation: see [installation](https://github.com/libc0607/rtl88x2eu-20230815?tab=readme-ov-file#installation)  

Check out the original driver tarball from the module vendors at [here](https://github.com/libc0607/rtl8733bu-20230626/blob/c42db387516b28bbd1fde8dca9b57788c046fcd0/RTL8733BU_WiFi_linux_v5.13.0.1-112-g10248f4f3_COEX20230616-330e.20230703.tar.gz).   
Android 4~12 driver is included also, but I have no idea how to use them.   

Any Wi-Fi adaptor/module with RTL8731BU/RTL8733BU chipset should work. The VID/PID may vary.  
[BL-M8731BU4_datasheet_V1.0.1.0_230323.pdf](https://github.com/user-attachments/files/16636235/BL-M8731BU4_datasheet_V1.0.1.0_230323.pdf)  

Suitable for FPV beginners, or any **ultra-small size** all-in-one digital video transmitters.  
**Not suitable for RX**.

Tested:
 - Build on kernel 6.8
 - OpenIPC integration
 - Packet injection: Good, legacy/MCS rates work well, LDPC encoding works too (in most cases, see "known bugs" below)  
 - Monitor (RX): Good for most cases (see below) 
 - TX power unlocked by ```iw```: supported, validated by my SDR receiver. Should set ```rtw_tx_pwr_by_rate=0 rtw_tx_pwr_lmt_enable=0``` when ```insmod```
 - Set Wi-Fi regd in OS: Set ```rtw_regd_src=1``` when ```insmod```
 - 5MHz/10MHz Injection: Seems good, set to 20MHz channel first, then ```echo <5/10> > /proc/net/rtl8733bu/<wlan0>/narrowband```
 - Unlock center frequency: All frequencies between 5080MHz ~ 6030MHz (5MHz step) in 5GHz band. They really cost-downed it too much
 - Temperature readout: working, see ```thermal_state``` in procfs
 - dkms install
 - Channel state scanning (ACS) -- see [here](https://github.com/libc0607/rtl88x2eu-20230815?tab=readme-ov-file#noise-monitor-acs) for usage
 - For reusing chips from dismantled Wi-Fi modules, check my schematic [here](https://github.com/user-attachments/files/24323819/SCH_RTL873xBU_Demo_2025-12-23.pdf) (**USE IT AT YOUR OWN RISK** -- It works in my test, but it's NOT the official design)

Need test or can be further investigated:
 - EDCCA Threshold: merged, not tested yet  
 - SIFS, Slot time, ACK timeout, etc.
 - \~4k Maximum MTU: merged, not tested yet

Known bugs/issues:
 -  Monitor RX can not decode any LDPC-encoded packet in any bandwidth. **873xb does not support LDPC RX** (hard-coded [here](https://github.com/libc0607/rtl8733bu-20230626/blob/40d5fb8f51d441f77ff66d33ec8ad2dbe60715c0/hal/rtl8733b/rtl8733b_ops.c#L3218)).
 -  Injecting LDPC-encoded packets is not working well. In most cases, it works, but only when injecting with certain MCS & packet length combinations, the packet does not go into the air. It could be some firmware or silicon bug. If it's not working, you can -- a. slightly change the packet length, b. change MCS, or c. just disable the LDPC
 -  No narrowband AP/STA support (needs additional firmware)
