# zigstar uzg-01 + zigbee2mqtt quick start

Example of validating that your new ZigStar UZG-01 works. This is working for
me as of 2025-10-20. Hardware/firmware versions may have changed since then.

NOTE: This is just to prove your hardware works. Don't use as-is for your zigbee
network. You probably want a more reliable way to keep Zigbee2MQTT and the MQTT
broker running. Also you probably don't want the data in mosquitto_data and
z2m_data to be public.

# Why this repo?
I had to piece this info together from multiple incomplete and incorrect
sources, which was pretty frustrating.

- which docs are the official docs?
- https://uzg.zig-star.com/ is still up, but the docs are outdated (eg. LED and
  button info is incorrect)
- https://xzg.xyzroe.cc/ just talks about firmware. What about my hardware?
- https://www.zigbee2mqtt.io/guide/installation/02_docker.html#running-the-container
  - great, but I don't have a MQTT broker
  - also, it fails to communicate with the ZigStar until you put it into USB
    mode. This is a perfect pairing with the incorrect LED/button docs above
    :chefs-kiss:

# Hardware
- zigstar uzg-01
  - I bought from [little bird](https://littlebirdelectronics.com.au/products/zigstar-uzg-01-universal-zigbee-gateway-1?_pos=1&_psq=zig&_ss=e&_v=1.0)
  - flashed with zstack, uses CC2652P7 chip
  - sticker on device: uzg-poe-0.6
  - not sure what firmware it's running. Apparently it's the latest, which at
    the time of writing seems to be [20241001](https://github.com/xyzroe/XZG/releases/tag/20241001).
    It's unclear whether this is all the firmware. Outdated docs indicate
    there's multiple boards in the device, each with their own firmware:
    [old? zigstar docs: flashing and updating](https://uzg.zig-star.com/flashing-and-updating/)
- ikea tradfri lamp + rodret switch

# Quick start
- clone this repo and cd in
- plug in the ZigStar USG to your computer with a USB cable. Wait for the white
  light to start flashing (white flashing = running in network mode, no
  connection)
- press and hold the button under the antenna until the red light flashes
- wait a few seconds - the red light should come on permanently (red solid = USB
  mode active)
- run `ls /dev/serial/by-id/`, make sure there's a device in there. Update
  ./docker-compose.yml -> devices to match your device
- run `docker-compose up`
- you should see successful connection messages from zigbee2mqtt and mosquitto
  in the docker logs
- browse to http://localhost:8080 to see the zigbee2mqtt web UI
- go to the Devices tab in the web UI sidebar, and click on "Permit Join"

- lamp
  - try this a few times until it works
    - switch your ikea light on/off 5 times quickly, leaving it in the on state.
      It should start pulsing brighter/dimmer
    - you should see the device appear in the devices list in the web UI
    - troubleshooting: the number of on/off switches, and how long you delay
      between each, seems to affect the pairing process. Mine worked after
      switching on/off exactly 4 times, then on, as quickly as I could (~2Hz)

- switch
  - same as above, try until it works
    - press the reset switch under the battery cover 4 times quickly
    - you should see the device appear in the device list
    - if it disappears, try again

- click on the light device in the web UI, and go to the 'Exposes' tab. You
  should be able to switch the light on/off, and alter its brightness. Yay!

# References
- [LED and buttons](https://xzg.xyzroe.cc/hardware/)
- [Zigbee2MQTT: getting started](https://www.zigbee2mqtt.io/guide/getting-started/)
- [Zigbee2MQTT: IKEA device docs](https://www.zigbee2mqtt.io/supported-devices/#v=IKEA)
- [ZigStar flashing and updating](https://uzg.zig-star.com/flashing-and-updating/)
  - old?
