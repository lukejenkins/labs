# Attenuation Measurement

## Lab Objectives

* Students will use a Wi-Fi Access Point and a test device to measure the attenuation of different building materials.

## References

* [Wi-Fi Planning, Walls and dB's – Measuring Obstruction Losses For WLAN Predictive Modelling](https://web.archive.org/web/20211020235710/https://www.ekahau.com/blog/wi-fi-planning-walls-and-dbs-measuring-obstruction-losses-for-wlan-predictive-modelling/)

## Equipment

### Hardware

* A Wi-Fi Access Point
* A test device that can report RSSI values (laptop, android phone, etc.)
* A way to measure distance (tape measure, laser distance measurer, etc.)

### Software

* An application for the test device that can report RSSI values.
  * Examples are Kismet for Linux or WiGLE WiFi for Android.

## Instructions

In this lab we will be measuring the attenuation of different wall materials. Our text provides some example values for different wall types, but these values can vary widely based on the actual construction of the wall. By measuring the attenuation of walls in your own environment you can create a more accurate model for your own wireless designs.

Make sure you read the ekahau.com blog linked in the references. In this post Nigel Bowden explains the idea behind this testing methodology and why you want to do your testing at 4 to 5 meters (~12 to 15 ft).

For the first round of testing for this lab, set up an AP just outside of a door and your test device 12 to 15 feet away. With the door open, locate the AP in your test software and record the RSSI value. If your test device can be locked to a single channel, do so now. Do your best not to move either the AP or the test device during the testing.

Observe and record the RSSI value every 15-30 seconds. Do so at least 5 times, if there is any variation, make sure to note this. Average the values you recorded to get a final RSSI value for the open door test. In my test I get a value of -67dBm.

Now close the door and run the tests again. Make sure to take multiple readings and average them. After I closed my door without moving the AP or the test device I measured -72 dBm. This difference of 5dB is the amount the material of my door attenuates a 2.4GHz Wi-Fi signal. If I was doing a wireless design involving this type of door, I could now accurately model it.

To test a wall, you will have to move the AP from the near side to the far side. Keep the AP at least a foot away from the wall on both sides. Test at the middle of the wall and avoid testing next to a door.

In addition to the door, I want you to find three other materials each with a unique attenuation value around your house or around campus. Please include pictures of the wall material in your lab report so that we can discuss any interesting findings.

## Deliverables

* There will be an assignment on Canvas that asks about your findings and information about the building materials that you measured.
