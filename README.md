# Tasmota-Doorbell
A Tasmota esp8266 device used to watch the doorbell

Using the esp8266 (wemos mini d1) and a circuit designed to draw power from the doorbell line, i am monitoring the door bell and the ambient temp/humidity in the attic.

My 'creation' has the following restrictions:
1. Device must allow normal doorbell use if the esp dies/hangs.
2. Device needs to run off the doorbell power supply.
3. Device is located in the attic at the transformer, and the home builder ran separate pairs from the door button and the bell to the transformer, so I have a central point to install the device.
4. I run Tasmota 6.6.0, and use MQTT secured to reach the home automation system.
5. The home automation system is out of scope to cover here but its just a binary sensor, and can notify people via SMS.
6. This is a typical 10-16VAC powered doorbell with a door button that has a small led across the N.O. switch.
7. Pressing the switch powers the door chime/bell device.
8. I will use the A0 analog input on the ESP8266 and watch the voltage to the bell, instead of trying to pick a "resistor combination" that will trip a digital input. (Because voltages vary)
9. Tasmota allows a rule to be built to publish to MQTT when a specific analong value is reached, and RULE 5 sets a one-shot mode so we are not flooding the MQTT server.

The circuit i designed (with lots of googling, etc) is a simple bridge rectifier on the doorbell power, and a large cap (1000uF), feeding the doorbell circuits. The power line on the doorbell (after the switch) is watched via resistor bridge to the ESP8266 A0 line (20k/1k, assuming no more than 20V max on the doorbell circuit). The doorbell power (DC side) also feeds a 5v regulator to run the ESP8266 and DHT12 (treated as a DHT11 in Tasmota).

While pressing the doorbell puts a large load on the supply, (home builders are cheap, so are the parts they install), the caps and voltage difference is still enough to power the ESP8266. In my system, voltage froms from ~12VDC to 8VDC. So things continue to work.

I designed a small box to install the device into as well. STL files included.

After some testing i did find spurious garbage on the hardware RX line, and swapped ESP8266 devices, and was considering putting a 10k resistor on the RX line (to vcc) if needed.

Use at your own risk. No fuses installed, but a 1A fuse is probably a good idea (slow blow would be better to handle doorbell surges).

Also this isnt line voltage - less than 20VAC - so youre fairly safe - but shorting the transformer will blow an internal, non replaceable link, so you'll end up buying a new transformer. Or grab a cheap 12VDC 1A plug in one to replace it, if you have a plug in the attic.
