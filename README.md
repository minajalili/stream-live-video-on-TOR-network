# stream live video on TOR network

in this project we gonna stream live video on TOR network. the scenario is this we use a server for publishing data on it and a stream app for record live video and publish on server. for connecting to TOR network in android device we use tor-android app, and also in device that server run on it, we have TOR network.

## installing
let's start to installing every thing we need for run this project.

server
------
in first level we should have a server for publishing. in this project we use Ant Media server you can download one of releases from [here](https://github.com/ant-media/Ant-Media-Server/releases/) and after extract the file at the same directory, just run this command :

```bash
$./start.sh
```
now for testing the server open the browser and enter this address
```
http://localhost:5080
```
if you see the login page, that's means you did every thing in right way.

# TOR
after installing tor on our system, we should make a hidden server
in this directory /etc/tor
and change the torrc file like below
```
HiddenServiceDir /var/lib/tor/hidden_service/
HiddenServicePort 5080 127.0.0.1:5080
HiddenServicePort 1935 127.0.0.1:1935
HiddenServicePort 80 127.0.0.1:80
```
for accessing onion address
```
$sudo cat /var/lib/tor/hidden_service/hostname
```
in the result you have an address with ".onion" domain and reachable via the Tor network. with onion address you can access to your server login page from any device which connected to Tor.
## Tor on android
for publishing on tor network you should be connect that, so in this case we can use  [this](https://github.com/guardianproject/tor-android) project for our connection. for more details you can look at this [link](https://mstajbakhsh.ir/compiling-tor-in-android/) that's helpful for compiling this project.
## Android Stream App 
finally, in last level we have to install an app for publishing live stream.
**Yasea**  is an Android streaming client, that we use it in project.
It's available [here](https://github.com/begeekmyfriend/yasea).
before installing this app you have to make some changes.
in RtmpConnection class, in socket connection part replace this code
```java
// socket connection  
SocketAddress addr = new InetSocketAddress("127.0.0.1", 9050 );  
Proxy proxy = new Proxy(Proxy.Type.SOCKS, addr);  
socket = new Socket(proxy);  
InetSocketAddress dest = new InetSocketAddress( host , port );  
Log.d(TAG, "connect() called. Host: " + host + ", port: " + port + ", appName: " + appName + ", publishPath: " + streamName);  
rtmpSessionInfo = new RtmpSessionInfo();  
rtmpDecoder = new RtmpDecoder(rtmpSessionInfo);  
try {  
    socket.connect(dest, 3000);  
  inputStream = new BufferedInputStream(socket.getInputStream());  
  outputStream = new BufferedOutputStream(socket.getOutputStream());  
  Log.d(TAG, "connect(): socket connection established, doing handhake...");  
  handshake(inputStream, outputStream);  
  Log.d(TAG, "connect(): handshake done");  
} catch (IOException e) {  
    e.printStackTrace();  
  mHandler.notifyRtmpIOException(e);  
 return false;}
```
here we make a socks proxy to connect Tor service.
## Test
after doing all levels, here is the result
**NOTE** if you feel high latency, please check your bandwidth limits and player buffering.

<p align='center'>
    <img src="https://github.com/minajalili/stream-live-video-on-TOR-network/blob/master/Screnshots/photo_2020-12-14_14-49-47.jpg" height="450px"/>
    <img src="https://github.com/minajalili/stream-live-video-on-TOR-network/blob/master/Screnshots/photo_2020-12-14_14-50-14.jpg" height="450px"/>
    <img src="https://github.com/minajalili/stream-live-video-on-TOR-network/blob/master/Screnshots/photo_2020-12-14_14-50-21.jpg" height="450px"/>
</p>
