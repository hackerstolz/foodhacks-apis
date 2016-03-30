# Minodes Challenge

<p align="center">
    <img alt="Minodes Logo" src="http://minodes.com.www351.your-server.de/cmsImages/pressLogos/logo_minodes_turkis.png" width="250px" />
</p>

## TL;DR

We provide an API that give you proximity data of any Wifi device (phone, laptop, printer, etc.) at specific positions (3 at the hackathon venue).  We want you to build uses cases on top of it! Think about blending different use cases together, to build hacks and apps for the (food) retail of the future. 

Just some random projects ideas:
 - Build a live heat-map of a supermarket (a.k.a the hackathon location) e.g.: by trilateration
 - Use the proximity data to enable a rich customer journey a.k.a WIFI Beacons
 - Build a system that alerts you when some customers are coming into the store

We think our challenge is perfect to mix it with other ideas and add a localisation aspect to it!

Contact Alexander Mueller in case of any questions!

## Description

MinodesNow is a platform that enables developers to detect wifi devices near a certain location/node. For this purpose the nodes sniff which devices are currently looking for available wifi networks and read out this so called probe-request. These requests contain the client device mac address and a [RSSI](https://en.wikipedia.org/wiki/Received_signal_strength_indication)-value indicating the signal strength of the perceived probe-request.

MinodesNow parses these signal, filters out useless requests and provides a nice and clean interface to access this information easily to build wifi proximity information boosted applications. Example use-case that benefit from this technology might include wifi positioning for retailers, loyalty structure analytics for offline retailers among many others. 

## Goals

The team that uses our API in the most innovative way will win the challenge prize, so get creative about it!

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
The socket.io interface is a little bit more complex ;-), but still nothing too complex.

You can access with your user multiple nodes within the same socket connection. For this we facilitate the concept of rooms.

You need to connect to the socket.io server of now.minodes.com on port 80. Please also specify the namespace:
```javascript
  var socket = io('http://now.minodes.com/minodes');
```
After you connected to the server you need to authentificate yourself.You do this by emit an event to the server with your credentials as the payload.

```javascript
   socket.on('connect', function(){
            console.log('connected')
            socket.emit('authenticate', {"user": "foodhacks@minodes.com", "pass": "please_add_here"})
    });
```
Now you are good to go and can join a node of your choice.

```javascript
        socket.on('authenticated', function(data){
            console.log('Nodes available: ' + data['nodes'])
            socket.emit('join', {"node":data['nodes'][0]})
            //socket.emit('join', {"node":data['nodes'][1]}) to join all nodes
            //socket.emit('join', {"node":data['nodes'][2]}) to join all nodes
        });
```
After this step you listen to the signals of the specified nodes. Those will be emitted as a 'minodes_node_event' in the specific rooms

```javascript
  socket.on('minodes_node_event',function(node_id,data){
            console.log('Node event from: ' +node_id)
            console.log(data)
        });
```

This event will give you the node_id of the node where the signal comes from and the parsed wifi information itself:

```javascript
    {
    vendor: "Samsung", // The manufacturer of the wifi device / chip
    time: "2016-03-30 08:15:26.695836", // the UTC timestamp 
    minodes_id: "d301357e903107113676604fd3bf6aacf26ad5096bbbbf9d3fbf7f84",  // the hashed minodes id
    strength: -75 // the signal strength is decibel (db)
    }

```

That is basically all that you need to know, about our MinodesNow platform. 

### Example
We provide a python and javascript example usage with the auth hand-shake and some simple data receiver.

#### Python
This is a small python based example client.
```
pip install socketIO-client==0.6.5
```

```python
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
Here comes the javascript equivalent.

```javascript
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>MinodesNow Test Client</title>
    <script src="https://cdn.socket.io/socket.io-1.4.5.js"></script>

    <script>
        var socket = io('http://now.minodes.com/minodes');

        socket.on('connect', function(){
            console.log('connected')
            socket.emit('authenticate', {"user": "foodhacks@minodes.com", "pass": "please_add_here"})
        });

        socket.on('authenticated', function(data){
            console.log('Nodes available: ' + data['nodes'])
            socket.emit('join', {"node":data['nodes'][0]})
        });

        socket.on('joined',function(data){
            console.log(data)
        });

        socket.on('minodes_node_event',function(data){
            console.log(data)
        });

    </script>
</head>
<body>
Check the Dev console.
</body>
</html>
```
