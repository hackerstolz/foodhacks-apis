# Minodes Wifi Proximity Challenge

<p align="center">
    <img alt="Minodes Logo" src="http://minodes.com.www351.your-server.de/cmsImages/pressLogos/logo_minodes_turkis.png" width="250px" />
</p>


## Challenge Description
MinodesNow is a platform that enables developers to detect wifi devices near a certain location/node. For this purpose the nodes sniff which devices are currently looking for available wifi networks and read out this so called probe-request. These requests contain the client device mac address and a [RSSI](https://en.wikipedia.org/wiki/Received_signal_strength_indication)-value indicating the signal strength of the perceived probe-request.

MinodesNow parses these signal filters out useless requests and provides a nice and clean interface to access this information easily to build wifi proximity information boosted applications. Example use-case that benefit from this technology might include wifi positioning for retailers, loyalty structure analytics for offline retailers amoung many others. 

## Goals
TBA

## API Description

We used socket.io to implement MinodesNow. Socket.io mainly uses websockets but also provides fall-back options to polling, while keeping the same interface.

Our platform was mainly build to show case the information you get from Wifi data and enable external developers to build upon this data. Therefore, we provide a very simple interface to access the data.

First of all you need to get access to the API. Therefore, please contact Alexander Mueller. 

The API consists out of a classical restful part and socket io part.

### Restful Routes
The routes are secured by basic authentification.

#### Node List
Returns the list of nodes that you are able to access. Please use this ids to join this nodes later.

```
GET /nodes HTTP/1.1
Host: now.minodes.com
Authorization: Basic add it here
Cache-Control: no-cache
```

#### Minodes ID
This returns the minodes id of a MAC address. This is basically a salted hash of the mac address. 
This will be used in the whole minodes environment to identify a wifi device.

```
GET /phone/test_mac_adress/minodes_id HTTP/1.1
Host: now.minodes.com
Authorization: Basic add it here=
Cache-Control: no-cache
```

### Socket.io Interface
The socket.io Interface is a little bit more complex

TBA

### Example
We provide a python and javascript example usage with the auth hand-shake and some simple data receiver.

#### Python
This is a small python based example client.
```
pip install socketIO-client==0.6.5
```

```
#!/usr/bin/env python3
from socketIO_client import SocketIO, BaseNamespace

class Namespace(BaseNamespace):
    _connected = True

    def on_connect(self):
        print('connected')

    def on_error(self, data):
        print('error')

    def on_event(self, event, *args):
        print(event, args)

    def on_authenticated(self, args):
        print('Authentificated')
        print(args)
        self.emit('join', {'node': 1})

    def on_joined_node(self, args):
        print('Joined room')
        print(args)

    def on_minodes_node_event(self, args):
        print(args)


with SocketIO('http://now.minodes.com', 80, Namespace) as socketIO:
    nsp = socketIO.define(Namespace, '/minodes')

    nsp.emit('authenticate', {'user': 'foodhacks@minodes.com', 'pass': 'please_add_here'})

    socketIO.wait(seconds=60)
```
#### Javascript
TODO
