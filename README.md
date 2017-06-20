# Python Library for Intertechno Bluetooth Gateway
Using this small helper library, you can set the timer configuration of your intertechno bluetooth switch.

## Installation
Primary use of this library is in a raspbian environment. But it should work in any debian-based distro.

First install some packages using apt-get:
```
sudo apt-get install bluez python-bluez python-pip
```

Installation using pip:
```
pip install intertechno
```

For more information about controlling intertechno devices with a raspberry pi check out:
http://www-2016.1.smart-home-technology.ch/en/blog/control-your-intertechno-devices-with-a-raspberry-pi

## Example
```
import intertechno
import bluetooth as bt
import datetime

# compose timers
ts = intertechno.Timers("TOM#4")

ts.add('C07', time_on=(7, 30), time_off=(8, 00), repeat=False, days=['sun'])
ts.add(4, time_on=(23, 59), time_off=(00, 00), dim=0.5, random=True, repeat=True, days=True)
ts.add(7, time_on=(8, 30), random=True, repeat=True, days=True)

ts.add(7, time_off=(8, 14))

msg = ts.compose()

# send over bluetooth
sock = bt.BluetoothSocket(bt.RFCOMM)
sock.connect(('00:07:80:0A:00:D1', 1))
sock.send(msg)
sock.close()
```

This example creates the message 

```
?TOM#4+CNi+C07HeIAAB+04HX7AAH#+07*Ie**H#+07***IOD#+*********+*********+*********+*********+*********+*********+*********+*********+*********+*********+*********
```

and sends it to the bluetooth switch with the address 00:07:80:0A:00:D1
