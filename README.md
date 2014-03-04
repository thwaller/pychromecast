pychromecast
============

Allows to remote control the Chromecast from Python. It currently supports:
 - Read Chromecast device status
 - Read application status
 - Read the status of the current content being played*
 - Control content: play, pause, change volume, mute or skip*

*: only supported by apps that support the RAMP-protocol.

How to use
----------

PyChromecast depends on the Python packages requests, autobahn and twisted. Make sure you have these dependencies installed using `pip install requests autobahn twisted`.

    >> import time
    >> import pychromecast

    >> cast = pychromecast.PyChromecast("192.168.1.9")
    >> print cast.device
    DeviceStatus(friendly_name='Living Room', model_name='Eureka Dongle', manufacturer='Google Inc.', api_version=(1, 0))

    >> print cast.app
    AppStatus(app_id='YouTube', description='YouTube TV', state='running', options={'allowStop': 'true'}, service_url='http://192.168.1.9:8008/connection/YouTube', service_protocols=['ramp'])

    >> time.sleep(1)  # sleep 1s so RAMP connection has time to init

    >> ramp = cast.get_protocol(pychromecast.PROTOCOL_RAMP)
    >> ramp
    RampSubprotocol(Epic sax guy 10 hours, 810.4/36001, playing)

    >> ramp.pause()

Websocket protocol
------------------

Most apps can be communicated with using a websocket. For their own apps Google uses the RAMP protocol to control the media and give information on current running feedback.

At this point in time the RAMP protocol is the only protocol that is implemented. Except for Netflix all major applications on the Chromecast speak the RAMP protocol (ie YouTube, Google Music, HBO, HULU).

Known issues
------------

If the Ramp client disconnects or your Python script exits right after issuing a command the command might not be send. This is because Twisted is event-driven and might not be ready yet to handle the sending of the command.

Since Google has opened up the Chromecast SDK not all apps seem to be reachable using the DIAL-api. Therefore not all apps are exposed via the app attribute.

