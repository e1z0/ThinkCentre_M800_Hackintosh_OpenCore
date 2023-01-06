# Lenovo ThinkCentre M800 MacOS with OpenCore 0.88 Boot Loader

```
Manufacturer: LENOVO
Product Name: 10FXS05000
Version: ThinkCentre M800
Family: ThinkCentre M800
SKU Number: LENOVO_MT_10FX_BU_LENOVO_FM_ThinkCentre M800
Release Date: 04/07/2016
```

# Why ?

Small machine, great specs, low energy consumption (_espacially actual in Europe at this time_), low price, why not? This tutorial is for complete dummies, who wants their machines running **MacOS** but don't know how. This tutorial is very basic and dedicated for newbies and unexperienced users with little tech background.

# Status

| Device | Status |
|------------|-----------------------------------------|
| VGA        | Working with hardware acceleration      |
| Audio      | Working, only DP->HDMI tested           |
| Lan        | Working                                 |
| Wifi       | If you need wifi/bt please read further |
| Sleep      | Not working yet                         |

*Tested MacOS Release: MacOS Monterey 12.6.2.*


# Pictures

<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_2.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_3.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_4.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_5.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_6.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_7.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_8.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_9.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_10.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_11.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_12.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_13.jpeg" width="400"/>
<img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/thinkcentre_m800_macos_14.jpeg" width="400"/>

# How Install ?

This tutorial assumes that you are already running MacOS computer. This can be easily adapted to Linux also.

* **Update your BIOS**, use tutorial down here in this **README**.
* **Create USB Flash Disk**, open **DiskUtility**, press **CMD+2** then select your target flash disk and left click **Erase**.
* Select **Mac OS Extended (Journaled)** and **GUID** Partition Map. This will create two paritions. <img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/diskutility_pic.png" width="400"/>
* Open Terminal and run command `diskutil` you will need to know usb flash disk **EFI partition identifier**, in my case it will be **disk5s1** <img src="https://raw.githubusercontent.com/e1z0/ThinkCentre_M800_Hackintosh_OpenCore/master/pics/diskutil_pic.png" width="400"/>
* Mount flash disk **EFI partition** `sudo mkdir /Volumes/EFI;sudo mount -t msdos /dev/disk5s1 /Volumes/EFI`
* Copy **OpenCore EFI folder** to flash disk `cp -r OpenCore/EFI /Volumes/EFI/EFI`
* **Unmount** the flash disk partitions `diskutil unmountDisk disk5`
* **Remove the flash disk** and insert it to target computer, select to **boot from USB disk**.
* Press **<Space>** on Install MacOS item, more options will show
* Select **GrubMod**
* Type `setup_var 0x197 0x00` and `reboot` this should unlock the MSR2 
* After reboot load again and select MacOS Install.
* Follow on screen instructions
* If you have problems please create **new issue**. 

*Depending on the various usb flash disk manufacturers and/or devices, some gadgets reported to not working on some usb ports. So please make sure to test it on all usb 
ports if it's not loading in the first time.*

# Update BIOS

You need to update the bios version to **FWJTBF** which is included in this repository `Bios/fwjtbfusa.zip`

* Create FreeDOS bootable USB Flash disk using [Rufus](https://rufus.ie/en/).
* Then replace files from previous mentioned **.zip** file.

# How to manually configure (optional)

The main OpenCore Boot Loaderc onfiguration file located in **OpenCore/EFI/OC/config.plist**. On flash drive it's: **EFI/OC/config.plist**. You can use another MacOS 
computer to configure bootloader configuration.

Plug USB Flash device and mount:
```
sudo mkdir /Volumes/EFI;sudo mount -t msdos /dev/disk5s1 /Volumes/EFI
```

Run plist editor:
```
python3 ./Tools/ProperTree/ProperTree.py
```
Unmount before unplugging
```
sync;sudo diskutil unmountDisk disk5
```
Remove the USB Flash drive and connect to the target computer..

**If you are unable to run ProperTre that means you need to install some python prerequisites**. You can install them by issuing the command `./Tools/ProperTree/install-requirements`. This requires [brew](https://brew.sh) to be installed already on your system.


# Display port and Display port to HDMI

For the default configuration only **Display port to Display port** is supported. If you are using some **Display Port > HDMI** converter cables/adapters you have to use the 
following configuration in **config.plist**

```
<key>boot-args</key>
<string>-v keepsyms=1 debug=0x100 -igfxhdmidivs -igfxlspcon swd_panic=1</string>
```


# Reconfigure USB ports (optional)

* Load **windows** or **Windows PE** (Windows installation DVD/USB media) when setup starts press Shift+F5 to run command prompt
* Launch **Windows.exe** from **Tools/USBToolBox**
* Follow on screen instructions

_If you are using USB Mouse/Keyboard this should help to identify the peripherals, use 255 identifier for such hardware.._ 
Left others in the default 3 identifier. Personally i'm using configuration only on used ports, leaving unused ports not configured at all.

# Wireless Bluetooth (optional)

I don't have Wireless or bluetooth in my model, so can't confirm anything. But I heard that for Wireless and Bluetooth you need to buy the [BCM94360NG](https://www.aliexpress.com/item/4000632365086.html) and add the following to your OC `config.plist` **DeviceProperties** 
section:
```
			<dict>
				<key>AAPL,slot-name</key>
				<string>Internal@0,28,0/0,0</string>
				<key>device_type</key>
				<string>Network controller</string>
				<key>model</key>
				<string>BCM4360NG WiFi Fenvi</string>
			</dict>
```



