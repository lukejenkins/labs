# Preparing for the Rate vs. Range Lab

For the Rate vs. Range lab, we want to set up multiple testing stations for the students, each consisting of an AP and a way to generate traffic.

We're going to use an Openwrt AP with librespeed-go. The following assumes a fresh install of Openwrt.

## AP Prep

### Install librespeed-go - Openwrt 24.10

For Openwrt APs running 24.10, use the following commands:

```bash
opkg update
opkg install librespeed-go
```

### librespeed-go - Openwrt Newer than 24.10

Starting with the major version post-24.10 the Openwrt project moved from opkg to apk.

For Openwrt APs running versions newer than 24.10, use the following commands:

```bash
apk --update-cache add librespeed-go
```

### librespeed-go customization

The following took a bit of digging. I couldn't find any of this info specific to running librespeed-go on Openwrt, so I had to cobble some things together.

If there are better ways, please let me know.

```bash
# There isn't an obvious place to put the following, but /www/X seems to be a common place to put web resources, so let's use it.
mkdir -p /www/librespeed

# Here are the example assets from the librespeed github repo. We don't actually need most of these, but I'd already written the list so why not.
cat > /tmp/librespeed-urls.txt << EOF
https://raw.githubusercontent.com/librespeed/speedtest-go/refs/heads/master/web/assets/example-singleServer-customSettings.html
https://raw.githubusercontent.com/librespeed/speedtest-go/refs/heads/master/web/assets/example-singleServer-basic.html
https://raw.githubusercontent.com/librespeed/speedtest-go/refs/heads/master/web/assets/example-singleServer-chart.html
https://raw.githubusercontent.com/librespeed/speedtest-go/refs/heads/master/web/assets/example-singleServer-gauges.html
https://raw.githubusercontent.com/librespeed/speedtest-go/refs/heads/master/web/assets/example-singleServer-pretty.html
https://raw.githubusercontent.com/librespeed/speedtest-go/refs/heads/master/web/assets/example-singleServer-progressBar.html
https://raw.githubusercontent.com/librespeed/speedtest-go/refs/heads/master/web/assets/example-multipleServers-full.html
https://raw.githubusercontent.com/librespeed/speedtest-go/refs/heads/master/web/assets/example-multipleServers-pretty.html
https://raw.githubusercontent.com/librespeed/speedtest-go/refs/heads/master/web/assets/index.html
https://raw.githubusercontent.com/librespeed/speedtest-go/refs/heads/master/web/assets/speedtest.js
https://raw.githubusercontent.com/librespeed/speedtest-go/refs/heads/master/web/assets/speedtest_worker.js
EOF

# openwrt doesn't have proper wget, working around that limitation
xargs -n 1 uclient-fetch -P /www/librespeed/ < /tmp/librespeed-urls.txt

# always clean up your messes
rm /tmp/librespeed-urls.txt

mv /www/librespeed/index.html /www/librespeed/index.html.bak

# I like this page the best, feel free to choose another or make your own custimized version.
cp /www/librespeed/example-singleServer-progressBar.html /www/librespeed/index.html

# Set the path for our customized web assets and enable librespeed-go in uci
uci set librespeed-go.config.assets_path="/www/librespeed"
uci set librespeed-go.config.enabled='1'
uci commit

# enable the service, start it (first install doesn't come started), and do a restart in case it was already running.
service librespeed-go enable
service librespeed-go start
service librespeed-go restart
```

## AP Customization

For each AP you're going to set up for the students, you'll need to make some changes.

The high level list of things to customize:

* Create and configure an SSID
* Set IP ranges to be unique per AP so that the testing fails if a student is still connected to a previous AP
* Change the title page of the speed test server

### Create and configure an SSID

Different models of APs will be a bit different, especially with regard to WLAN radio numbers supporting different bands and standards. Take the following as general guidance.

This section can be done via the luci webui of Openwrt, but I'm going to give you CLI instructions.

Check to see which radio is a 5GHz capable radio, in the example below, the 5GHz radio was `radio1`.

```bash
# Set the radio mode to use for each AP. I like to have different APs running different configs, here are some examples.
#uci set wireless.radio1.htmode='HT20'
uci set wireless.radio1.htmode='VHT20'
#uci set wireless.radio1.htmode='HE20'

# I keep 5GHz channel 165 available for classroom use, so I'll use that on all of my APs and physically seperate them.
uci set wireless.radio1.channel='165'

# Setting the country code is important, some clients do not like leaving this undefined.
uci set wireless.radio1.country='US'

# This command can be used to trim slower data rates. I like to keep them all enabled but using different values per AP might be interesting for the students 
uci set wireless.radio1.cell_density='0'

# Set a unique SSID per AP. Something easy for the students to recognize, but also unique per AP used for the lab.
#uci set wireless.default_radio1.ssid='WLAN-Lab-range1'
uci set wireless.default_radio1.ssid='WLAN-Lab-range2'
#uci set wireless.default_radio1.ssid='WLAN-Lab-range3'

# Use WPA2-PSK with an easy to type PSK. These APs will only be online during the lab, so let's go with 12345678
uci set wireless.default_radio1.encryption='psk2+ccmp'
uci set wireless.default_radio1.key='12345678'
uci set wireless.default_radio1.ieee80211w='2'
uci set wireless.default_radio1.ocv='0'

# enable the radio
uci set wireless.radio1.disabled='0'

# Save your work
uci commit
```

### Set Unique per AP IP Ranges and hostnames

```bash
# Each AP should have a unique IP so that the speed test only works if the student is connected to the correct AP.
#uci set network.lan.ipaddr='192.168.101.1'
uci set network.lan.ipaddr='192.168.102.1'
#uci set network.lan.ipaddr='192.168.102.1'

# Set thehostame of the AP so we know which one we're accessing via SSH
#uci set system.@system[0].hostname='WLAN-Lab-range1'
uci set system.@system[0].hostname='WLAN-Lab-range2'
#uci set system.@system[0].hostname='WLAN-Lab-range3'

# Save your work
uci commit
```

### Customize the title page of the speed test server

```bash
# Different example pages have slightly different wording at the top of the page. You might need to adjust the text sed is searching for.
#sed -i 's/LibreSpeed Example/WLAN-Lab-range1 LibreSpeed/g' /www/librespeed/index.html
sed -i 's/LibreSpeed Example/WLAN-Lab-range2 LibreSpeed/g' /www/librespeed/index.html
#sed -i 's/LibreSpeed Example/WLAN-Lab-range3 LibreSpeed/g' /www/librespeed/index.html
```

## Future improvements

* Add WLAN stats to the speedtest page
  * Currently the lab needs the device to generate the WLAN stats (RSSI, data rates, retries, etc.). It would be more approachable if it was done on the AP.
* URL isn't easy to remember or access. Make it easier, perhaps captive portal style.
* Limit APs to a single active speed test at a time. Auto start the speedtest when it is a user's turn.

## References and Further Reading

* <https://github.com/librespeed/speedtest-go>
* <https://github.com/openwrt/packages/tree/openwrt-24.10/net/librespeed-go>
