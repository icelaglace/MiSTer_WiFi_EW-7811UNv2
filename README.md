You now don't need this driver for the MiSTer FPGA!
It has been included inside the Linux Kernel since 5.14.

Thanks to Alexey for this!

### Installation
- Download the .ko file in the Releases folder : [LINK](https://github.com/icelaglace/MiSTer_WiFi_EW-7811UNv2/blob/main/releases/8188eu.ko)
- Connect to the MiSTer via SSH/FTP using either Serial Console or Ethernet or simply plug the SD Card in your computer.
- Transfer the .ko file to ```/lib/modules/(YOUR KERNEL VERSION)/```
- Use a keyboard and press F9 or connect to SSH and type : 
```
depmod -a
modprobe 8188eu
reboot
```

### Compilation (Step by step)
On Debian / Ubuntu, I used WSL Ubuntu on my end : 
- Install ARM cross-compile toolchain & build essentials : ```apt-get install gcc-arm-linux-gnueabihf build-essential```
- Install Bison : ```apt-get install bison```
- Install Flex : ```apt-get install flex```
- Install OpenSSL-dev ```apt-get install libssl-dev```
- Create a folder called ```build``` by typing ```mkdir build```
- Go to the folder : ```cd build```
- Clone the Linux MiSTer Kernel : ```git clone https://github.com/MiSTer-devel/Linux-Kernel_MiSTer```
- Clone this repo : ```git clone https://github.com/icelaglace/MiSTer_WiFi_EW-7811UNv2```
- Delete ```.git``` folder inside the Linux-Kernel_MiSTer & MiSTer_WiFi_EW-7811UNv2 folders (Recommended by Sigurd Bøe)
- Go to the Linux MiSTer Kernel folder and type : 
```
make ARCH=arm mrproper && make ARCH=arm MiSTer_defconfig && make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- EXTRAVERSION=-MiSTer modules_prepare
```
- Once it's done, go to MiSTer_WiFi_EW-7811UNv2 folder and type : 
```
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- clean && make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- KSRC=../Linux-Kernel_MiSTer modules
```
*KSRC is the path of the Linux kernel you just compiled, change accordingly if you didn't follow the exact instructions*

### Compilation (copy and paste) for Debian-based distributions
```
apt-get install gcc-arm-linux-gnueabihf build-essential
apt-get install bison
apt-get install flex
apt-get install libssl-dev
mkdir build && cd build
git clone https://github.com/MiSTer-devel/Linux-Kernel_MiSTer
git clone https://github.com/icelaglace/MiSTer_WiFi_EW-7811UNv2
cd Linux-Kernel_MiSTer
rm -rf .git
make ARCH=arm mrproper && make ARCH=arm MiSTer_defconfig && make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- EXTRAVERSION=-MiSTer modules_prepare
cd ../MiSTer_WiFi_EW-7811UNv2
rm -rf .git
make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- clean && make ARCH=arm CROSS_COMPILE=arm-linux-gnueabihf- KSRC=../Linux-Kernel_MiSTer modules
```

### Troubleshoot
- If you don't have the .bin ```/lib/firmware/rtlwifi``` called ```rtl8188eufw.bin```,go to https://github.com/wkennington/linux-firmware and download : ```rtlwifi/rtl8188eufw.bin```
- Transfer the .bin file to ```/lib/firmware/rtlwifi```

Thanks to [this guide](https://github.com/MiSTer-devel/Main_MiSTer/wiki/MISTER-CUSTOM-WIFI-DRIVER-COMPILATION-GUIDE) for the instructions :)

