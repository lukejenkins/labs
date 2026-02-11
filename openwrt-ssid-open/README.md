# Setting up an Open SSID in OpenWrt

## Introduction

We're going to try out OpenWrt, a popular open-source firmware for routers and embedded devices. We're going to go a bit off of the beaten path by using an x86 computer to boot OpenWrt via Ventoy. In this lab, you'll learn how to configure an open wireless network with no encryption.

You will need to get the OpenWrt image for the course from the instructor.

## Directions

After you have the OpenWrt image on your USB drive, plug it into your computer and boot from it. On the computer, you'll need to hit a key on bootup to go into the boot menu (F12 on our lab machines). Once in that menu, select the USB drive as the boot device.

Select the OpenWrt option from the Ventoy menu and hit enter. You will see a boot menu for OpenWrt; you can either hit enter on the default option or wait for the timer to expire. You will see the OpenWrt boot messages scroll by. After a minute or so, the messages will stop scrolling and you might see a login prompt. If you do not, you can hit enter to bring it up.

We actually do not need to use the OpenWrt command line for this lab, so you can ignore the login prompt. Instead, plug a second computer into the network port of the computer running OpenWrt. The computer running OpenWrt will act as a DHCP server and assign an IP address to the second computer. On the second computer, open a web browser and go to <http://192.168.1.1>. You should see the OpenWrt WebUI login page. There is no password set by default, so just hit enter or click the "Log in" button.

![0001.png](images/0001.png)

You will be presetned with the default Status page of OpenWrt. At the top you will see a banner asking you to set a root password. If we don't set one, we will have this banner at the top of every page we visit. Click on the "Go to password configuration button.

![0002.png](images/0002.png)

Set a password and click the "Save & Apply" button. You will be returned to the Status page, and the banner will be gone. The Status page shows an overview of your system including the router's current configuration and resource usage:

![0003.png](images/0003.png)
![0004.png](images/0004.png)
![0005.png](images/0005.png)

Now, let's configure a wireless network. Click on the "Network" menu at the top and select "Wireless" from the dropdown.

![0006.png](images/0006.png)
![0007.png](images/0007.png)

This page shows all of the Wi-Fi radios in the system. There might be one or more. The radio for the Panda Wireless adapter provided for the course should have a label of "Sabrent NTWLAC 802.11ac/b/g/n". In my example screenshot, it is the second radio, which has the label "radio1". We want to modify the SSID for this radio, so click on the "Edit" button for the SSID directly after the radio we've been discussing.

![0008.png](images/0008.png)
![0009.png](images/0009.png)

This is the configuration page for the SSID. This page has multiple tabs where you can configure different aspects of the wireless network. On the "General Setup" tab, you'll see various configuration options. Scroll down to find the channel width setting and change it to 20MHz:

![0010.png](images/0010.png)
![0011.png](images/0011.png)
![0012.png](images/0012.png)

Continue scrolling through the remaining configuration options:

![0013.png](images/0013.png)
![0014.png](images/0014.png)

Locate the channel setting and change it to channel 165.

![0015.png](images/0015.pn
g)![0016.png](images/0016.png)
![0017.png](images/0017.png)

The default configuration is to create an SSID named "OpenWrt". Change the "ESSID" field from OpenWrt to something unique, like "cybr3720-\<your-last-name\>-open".

![0018.png](images/0018.png)
![0019.png](images/0019.png)

After entering your SSID name, click on the "Wireless Security" tab. This is where we can set up encryption for the SSID. For now, we'll create an open network, so leave it as "No Encryption".

![0020.png](images/0020.png)

![0021.png](images/0021.png)
![0022.png](images/0022.png)

Verify that "No Encryption" is selected for this open network, then scroll down to the bottom of the page and click the "Save" button to save your changes.

![0023.png](images/0023.png)
![0024.png](images/0024.png)

You'll be returned to the Wireless Overview page with pending changes. Click "Save & Apply" at the top to apply the changes to your wireless configuration.

![0025.png](images/0025.png)
![0026.png](images/0026.png)

The system will apply your configuration changes. Your open SSID is now configured and active.

![0027.png](images/0027.png)

Congratulations! You have successfully configured an open SSID on your OpenWrt router. You can now test connecting to the network from a wireless device and verify its configuration using Kismet as described in the deliverables below.

## Deliverables

### Question 1

This lab requires multiple computers, so you might need to work with a lab partner. If you do, please include their name in the LMS.

### Question 2

After you have set up your open SSID, take a screen shot of the "Wireless Overview" page of the OpenWrt WebUI. Upload that screenshot to the LMS.

Here is an example of what I'm looking for:
![deliverables-q2.png](images/deliverables-q2.png)

### Question 3

After you have set up your open SSID, capture beacons from it using Kismet. Take a screenshot of the "Wi-Fi (802.11)" section of the Device Info page for your AP. Make sure that it includes the SSID, the Encryption type, the Channel, and the 802.11d Country. Upload that screenshot to the LMS.

Here is an example of what I'm looking for:
![deliverables-q3.png](images/deliverables-q3.png)
