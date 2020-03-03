# Tasmota-Doorbell
A Tasmota esp8266 device used to watch the doorbell

Using the esp8266 (wemos mini d1) and a circuit designed to draw power from the doorbell line, I am monitoring the doorbell and the ambient temp/humidity in the attic.

My 'creation' has the following restrictions:
1. Device must allow normal doorbell use if the esp dies/hangs.
2. Device needs to run off the doorbell power supply.
3. Device is located in the attic at the transformer, and the home builder ran separate pairs from the door button and the bell to the transformer, so I have a central point to install the device.
4. I run Tasmota 6.6.0, and use MQTT secured to reach the home automation system.
5. The home automation system is out of scope to cover here but it's just a binary sensor, and can notify people via SMS.
6. This is a typical 10-16VAC powered doorbell with a door button that has a small led across the N.O. switch.
7. Pressing the switch powers the door chime/bell device.
8. I will use the A0 analog input on the ESP8266 and watch the voltage to the bell, instead of trying to pick a "resistor combination" that will trip a digital input. (Because voltages vary)
9. Tasmota allows a rule to be built to publish to MQTT when a specific analog value is reached, and RULE 5 sets a one-shot mode so we are not flooding the MQTT server.

The circuit I designed (with lots of googling, etc) is a simple bridge rectifier on the doorbell power, and a large cap (1000uF), feeding the doorbell circuits. The power line on the doorbell (after the switch) is watched via resistor bridge to the ESP8266 A0 line (20k/1k, assuming no more than 20V max on the doorbell circuit). The doorbell power (DC side) also feeds a 5v regulator to run the ESP8266 and DHT12 (treated as a DHT11 in Tasmota).

While pressing the doorbell puts a large load on the supply, (home builders are cheap, so are the parts they install), the caps and voltage difference is still enough to power the ESP8266. In my system, voltage drops from ~12VDC to 8VDC. So things continue to work.

I designed a small box to install the device into as well. STL files included.

After some testing I did find spurious garbage on the hardware RX line, and swapped ESP8266 devices, and was considering putting a 10k resistor on the RX line (to vcc) if needed.

Use at your own risk. No fuses installed, but a 1A fuse is probably a good idea (slow blow would be better to handle doorbell surges).

Also this isnt line voltage - less than 20VAC - so you're fairly safe - but shorting the transformer will blow an internal, non replaceable link, so you'll end up buying a new transformer. Or grab a cheap 12VDC 1A plug in one to replace it, if you have a plug in the attic.

Additional NOTE - since doorbell units are usually solenoid coils, you can get a huge backfeed when the circuit powers off (it is how your ignition coil actually works, sort of). Since we are running on DC now, you can put a clamping diode on the coil terminals to catch that spike. I highly recommend that. Putting it on the bell is better than at the circuit. In both places is not a bad idea either. Heck if you need it loop the wires through a toroid (not the AC power wires just the bell and button ones) and you'll block even more RF noise.

Also - put a small (33uF) and disc cap on the ESP8266 power pins to help filter there. 

NOTE - if you ever revert to AC REMOVE THE DIODE on the bell first! Otherwise you'll short the line. (sort of)

If you feel like it, feel free to [buy me a coffee!](https://www.buymeacoffee.com/rbef)

New Note - 1/15/20 - Tasmota has an issue with the DHT12 sensor. It goes NULL after a while. They have it fixed i think, with a recent dev branch update. I'm watching it. I have 2 devices that fail (mostly outside sensors too!) but indoor ones work? Could also be the source of the esp8266 Mini Di making a difference.. Cause i also have an outdoor sensor using DHT12 that works fine.

3/3/2020 - Nope. Those little blue DHT12 sensors must just be junk. Changing pins, adding pullups, updating to dev code, nothing seems to make them stable. An indoor sensor has now started to fail also. However, switching one of them back to the white (big case) DHT11 works? (for now)  I am going to order BME280 sensors. NOTE that you must make sure you get a BME not a BMP. The sensor should have a U in the next to last character on the case. If it has a K, its a BMP! Sellers dont know the difference so its up to you to watch for it and please, comment in the reviews if they get it wrong!
