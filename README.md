# qemu-raspbian-network
Launch a raspbian image on qemu with network connectivity

```
git clone https://github.com/nachoparker/qemu-raspbian-network.git
cd qemu-raspbian-network
wget https://downloads.raspberrypi.org/raspbian_lite_latest -O raspbian_lite_latest.zip
unzip raspbian_lite_latest.zip
sudo ./qemu-pi.sh *-raspbian-jessie-lite.img
```

# Run raspbian with 1G RAM with 4 core

```
wget http://downloads.raspberrypi.org/raspbian/images/raspbian-2016-05-31/2016-05-27-raspbian-jessie.zip
unzip 2016-05-27-raspbian-jessie.zip
sudo losetup -f --show -P 2016-05-27-raspbian-jessie.img
sudo mkdir /mnt/rpi
sudo mount /dev/loop0p1 /mnt/rpi
cp /mnt/rpi/kernel7.img bcm2709-rpi-2-b.dtb .
sudo umount /mnt/rpi
sudo losetup -d /dev/loop0
qemu-system-arm \
 -M raspi2 \
 -append "rw earlyprintk loglevel=8 console=ttyAMA0,115200 dwc_otg.lpm_enable=0 root=/dev/mmcblk0p2" \
 -cpu arm1176 \
 -dtb bcm2709-rpi-2-b.dtb \
 -sd buster.img \
 -kernel kernel7.img \
 -m 1G \
 -smp 4 \
 -serial stdio
```

Note that it is recommended to use `qemu-arm` not older than 2.8.0 (see code)

See details on https://ownyourbits.com/2017/02/06/raspbian-on-qemu-with-network-access/
