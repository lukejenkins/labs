# Rate vs. Range Lab

Version 4.0

## Lab Objectives

Students will use an AP in root mode and an AP in WGB mode to explore the impact distance has on an 802.11 link.

## References

How to interpret 802.11ac datarates - http://mcsindex.com/ 

## Equipment

### Hardware

* A Wi-Fi Access Point
* A laptop
* The USB Thumb Drive you've been using this semester
* The USB Wi-Fi adapter you've been using this semester
* A way to measure distance (tape measure, laser distance measurer, etc.)

### Software

* Kali Linux via the USB Thumb Drive
* wavemon - <https://github.com/uoaerg/wavemon>
  * Connect to the internet and then install using the command: sudo apt-get install wavemon

## Instructions

<https://docs.google.com/spreadsheets/d/1LYqMqhJcZvHn-4zFQEF8YbkQ26pqHANetXyg8OTlfwU/edit?usp=sharing>

For the first step, power up your BeaconBit AP and connect to it with your linux laptop. Open a terminial and check that you've gotten an IP address in the 192.168.4.x range.

Place your laptop approximately 3 feet from the AP. Open a terminal and run the command: ```wavemon -i wlan1```. This will open the wavemon application and show stats for the wlan1 interface. (Make sure to use the correct interface for your USB Wi-Fi adapter, it may be wlan0 or wlan2 depending on your system.)

Record the Signal Strength and Signal to Noise ratio from the output of this command.



========= break =========

Now move the your laptop to be as far apart as possible from within the same room. Avoid obstructions between your client the AP. Ideally the devices will be at least 10 feet apart; measure and record the distance.

Remember that we’re looking for the Signal Strength and Signal to Noise Ratio from the third command, and the retry stats and breakdown of data rates used from the fourth.

For the third test, place the APs at least 20 feet apart. I expect that with this iteration of the testing, there will be some obstacles such as walls in the path. Take notes of what the distance is and noting any obstacles in the path. Go through the testing procedure as before, recording the same data points.

========= break =========

For the fourth run, get as much distance between the APs as you can, while still getting a 99 percent or 100 percent success rate at the end of the ping command. (e.g. Success rate is 99 percent (5999/6000), round-trip min/avg/max = 1/3/52 ms ). Measure the distance as accurately as is reasonable, and describe the type and number of the obstacles between the two APs. And example of this would be ”50ft, with two walls made of drywall and a single outdoor wall made of brick.”

Synthesise, chart, and graph the four data sets I’ve asked for, and make sure to include your conclusions as to the effect of distance and different building materials have on an 802.11 link.

Deliverables:

The following need to be uploaded to Canvas before the due date for this lab:

* A completed Google Spreadsheet using Luke’s template. Each team member should submit a copy of the spreadsheet.
* Documentation (pictures are nice) of the setup
