# Preparing an OpenWRT Image with ImageBuilder

To generate an OpenWRT image for the class, we're going to use the ImageBuilder bundle on a Debian machine.

## Setup

Prep the machine you’re going to build the image on by installing the following:

```bash
sudo apt install build-essential file libncurses-dev zlib1g-dev gawk git gettext libssl-dev xsltproc rsync wget unzip python3 python3-distutils
```

## Download

Grab the Image Builder package from the OpenWRT download site. The URL structure is:

<https://downloads.openwrt.org/releases/24.10.3/targets/x86/64/>

where 24.10.3 is the current release. Check <https://downloads.openwrt.org/releases/> for the latest stable release.

The file we want is openwrt-imagebuilder-24.10.3-x86-64.Linux-x86_64.tar.zst which again will change with future releases. Use `curl` or `wget` to download it. For example:

```bash
curl -O -J https://downloads.openwrt.org/releases/24.10.3/targets/x86/64/openwrt-imagebuilder-24.10.3-x86-64.Linux-x86_64.tar.zst
```

## Unpack

We need to decompress the `.zst` file and then change to that directory. You can do this with the following commands:

```bash
tar --zstd -xvf openwrt-imagebuilder-*.tar.zst
cd openwrt-imagebuilder-*/
```

## Customize and Build

Now we can run the `make image` command to create a custom image, though we will want to pass some extra options to it.

We want to include an extra name for the image so we can easily identify it. In this case, we'll use `lab-image`.

The default image size is not large enough for our needs, so we will need to increase the root filesystem size. We can do this by setting the `ROOTFS_PARTSIZE` variable to `250` (MB).

For the image to work with Ventoy, we need to include the following two packages:

* `kmod-dax`
* `kmod-dm`

The x86_64 target does not include any wireless drivers, so we will need to include those. If you know exactly which wireless adapters you're going to use in the labs, you can include only those drivers. Otherwise, we can include a wide range of drivers to cover most adapters.

The x86_64 target also does not include most of the wireless tools we will need for the labs, so we will include those as well.

The format for the PACKAGES variable is a space-separated list of package names, with a `-` prefix to remove packages.

```bash
make image \
EXTRA_IMAGE_NAME="lab-image" \
ROOTFS_PARTSIZE="250" \
PACKAGES="-apk-mbedtls -libmbedtls -libustream-mbedtls -wpad-basic-mbedtls aircrack-ng airmon-ng atop blkid block-mount ca-bundle ccrypt coreutils-base64 curl diffutils dosfstools dropbearconvert exfat-fsck exfat-mkfs ethtool f2fs-tools fdisk file gpsd gpsd-clients gpsd-utils hcxdumptool hcxtools horst hostapd-openssl hostapd-utils htop ip-full ip6tables-nft ipset iptables-nft irqbalance iw-full iwcap iwinfo iwlwifi-firmware-ax200 iwlwifi-firmware-ax201 iwlwifi-firmware-ax210 iwlwifi-firmware-ax411 iwlwifi-firmware-be200 iwlwifi-firmware-iwl8265 iwlwifi-firmware-iwl9000 iwlwifi-firmware-iwl9260 kmod-ath kmod-ath10k kmod-ath11k kmod-ath11k-pci kmod-ath12k kmod-ath3k kmod-ath5k kmod-ath6kl kmod-ath6kl-sdio kmod-ath6kl-usb kmod-ath9k kmod-ath9k-common kmod-ath9k-htc kmod-cfg80211 kmod-dax kmod-dm kmod-fs-autofs4 kmod-fs-cifs kmod-fs-configfs kmod-fs-exfat kmod-fs-ext4 kmod-fs-msdos kmod-fs-ntfs3 kmod-fs-smbfs-common kmod-fuse kmod-hid kmod-hid-generic kmod-iwlwifi kmod-kvm-amd kmod-kvm-intel kmod-kvm-x86 kmod-mac80211 kmod-mt76 kmod-mt76-connac kmod-mt76-core kmod-mt76-sdio kmod-mt76-usb kmod-mt7601u kmod-mt7603 kmod-mt7615-common kmod-mt7615-firmware kmod-mt7615e kmod-mt7622-firmware kmod-mt7663-firmware-ap kmod-mt7663-firmware-sta kmod-mt7663-usb-sdio kmod-mt7663s kmod-mt7663u kmod-mt76x0-common kmod-mt76x02-common kmod-mt76x02-usb kmod-mt76x0e kmod-mt76x0u kmod-mt76x2 kmod-mt76x2-common kmod-mt76x2u kmod-mt7915-firmware kmod-mt7915e kmod-mt7916-firmware kmod-mt7921-common kmod-mt7921-firmware kmod-mt7921e kmod-mt7921s kmod-mt7921u kmod-mt7922-firmware kmod-mt7925-common kmod-mt7925-firmware kmod-mt7925e kmod-mt7925u kmod-mt792x-common kmod-mt792x-usb kmod-mt7992-23-firmware kmod-mt7992-firmware kmod-mt7996-233-firmware kmod-mt7996-firmware kmod-mt7996-firmware-common kmod-mt7996e kmod-mtk-t7xx kmod-nlmon kmod-nls-cp437 kmod-nls-iso8859-1 kmod-nls-utf8 kmod-nvme kmod-phy-airoha-en8811h kmod-phy-amd kmod-phy-aquantia kmod-phy-at803x kmod-phy-ax88796b kmod-phy-bcm84881 kmod-phy-broadcom kmod-phy-intel-xway kmod-phy-marvell kmod-phy-marvell-10g kmod-phy-micrel kmod-phy-microchip kmod-phy-qca83xx kmod-phy-realtek kmod-phy-smsc kmod-phy-vitesse kmod-phylib-broadcom kmod-phylib-qcom kmod-phylink kmod-rt2800-lib kmod-rt2800-mmio kmod-rt2800-pci kmod-rt2800-usb kmod-rt2x00-lib kmod-rt2x00-mmio kmod-rt2x00-pci kmod-rt2x00-usb kmod-rt61-pci kmod-rt73-usb kmod-rtc-pcf8563 kmod-rtl8192c-common kmod-rtl8192ce kmod-rtl8192cu kmod-rtl8192d-common kmod-rtl8192de kmod-rtl8192du kmod-rtl8192se kmod-rtl8723-common kmod-rtl8723be kmod-rtl8723bs kmod-rtl8812au-ct kmod-rtl8821ae kmod-rtl8xxxu kmod-rtlwifi kmod-rtlwifi-btcoexist kmod-rtlwifi-pci kmod-rtlwifi-usb kmod-rtw88 kmod-rtw88-8723d kmod-rtw88-8723de kmod-rtw88-8723ds kmod-rtw88-8723du kmod-rtw88-8723x kmod-rtw88-8812a kmod-rtw88-8812au kmod-rtw88-8821a kmod-rtw88-8821au kmod-rtw88-8821c kmod-rtw88-8821ce kmod-rtw88-8821cu kmod-rtw88-8822b kmod-rtw88-8822be kmod-rtw88-8822bu kmod-rtw88-8822c kmod-rtw88-8822ce kmod-rtw88-8822cu kmod-rtw88-88xxa kmod-rtw88-pci kmod-rtw88-sdio kmod-rtw88-usb kmod-rtw89 kmod-rtw89-8851be kmod-rtw89-8852ae kmod-rtw89-8852b-common kmod-rtw89-8852be kmod-rtw89-8852ce kmod-rtw89-8922ae kmod-rtw89-pci kmod-tun kmod-usb-acm kmod-usb-core kmod-usb-net kmod-usb-net-aqc111 kmod-usb-net-asix kmod-usb-net-asix-ax88179 kmod-usb-net-cdc-eem kmod-usb-net-cdc-ether kmod-usb-net-cdc-mbim kmod-usb-net-cdc-ncm kmod-usb-net-cdc-subset kmod-usb-net-lan78xx kmod-usb-net-pl kmod-usb-net-qmi-wwan kmod-usb-net-rndis kmod-usb-net-rtl8150 kmod-usb-net-rtl8152 kmod-usb-net-sierrawireless kmod-usb-net-smsc75xx kmod-usb-net-smsc95xx kmod-usb-net-sr9700 kmod-usb-ohci kmod-usb-ohci-pci kmod-usb-phy-nop kmod-usb-storage kmod-usb-storage-extras kmod-usb-storage-uas kmod-usb-test kmod-usb-uhci kmod-usb-wdm kmod-usb-xhci-hcd kmod-usb2 kmod-usb2-pci kmod-usb3 kmod-usbmon losetup lsblk lscpu luci-app-commands luci-app-irqbalance luci-app-nlbwmon luci-app-package-manager luci-app-statistics luci-ssl-openssl luci-theme-bootstrap mt7981-wo-firmware mtr-nojson nano-full openssh-client openssh-client-utils openssh-keygen openssh-server openssh-sftp-client openssh-sftp-server patch parted pciutils procps-ng-pkill resize2fs tcpdump tio tmux tree usb-modeswitch usbutils wget-ssl wifi-scripts wireless-tools wpa-cli wpa-supplicant-mesh-openssl"
```

This command will create an image file in the `bin/targets/x86/64/` directory. The image file we are interested in will be named something like `openwrt-24.10.3-x86-64-generic-ext4-combined.img.gz`.

## Using the Image with Ventoy

You can use the image file with Ventoy to boot it on a machine. First, ungzip the image file:

```bash
gunzip bin/targets/x86/64/openwrt-*-generic-ext4-combined.img.gz
```

Then copy the resulting `.img` file to the Ventoy partition on your USB drive. You can then boot from the USB drive and select the OpenWRT image from the Ventoy menu.

## References and Further Reading

* <https://openwrt.org/docs/guide-user/additional-software/imagebuilder>
* <https://www.ventoy.net/en/doc_openwrt.html>
* <https://github.com/ventoy/OpenWrtPlugin/> <-= We are including the required packages manually since we need to build a custom image anyway. This is for reference in case the list of packages required by Ventoy ever changes.
