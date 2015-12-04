# React Native Tutorial

### Installation

```
git clone
cd AwesomeProject
npm install
npm start
open ios/AwesomeProject.xcodeproj
```

This opens X-Code. Hit `Cmd - R` to build (_see below for port issues_)

##### Port issues

* It defaults to using port 8081 but for some reason it seems to be constantly in use on my machine.

I changed the `start` script in `package.json` to use a custom port (I chose 8080)

```javascript
  "scripts": {
    "start": "node_modules/react-native/packager/packager.sh --port=8080"
  },
```

* In X-Code under `Libraries/React.xcodeproj` select Build Phases > Run Script.

Either remove this entry, having called npm start yourself, or edit the port.

```shell
if nc -w 5 -z localhost 8080 ; then
  if ! curl -s "http://localhost:8080/status" | grep -q "packager-status:running" ; then
    echo "Port 8080 already in use, packager is either not running or not running correctly"
    exit 2
  fi
else
  open $SRCROOT/../packager/launchPackager.command || echo "Can't start packager automatically"
fi
```

* In X-Code `AwesomeProject/AppDelegate.m` Change the port on this line.
```objective-c
jsCodeLocation = [NSURL URLWithString:@"http://localhost:8080/index.ios.bundle?platform=ios&dev=true"];
```


* To use Chrome Devtools go to `Libraries/RCTWebSocket.xcodeproj/RCTWebSocketExecutor.m` in X-Code
```objective-c
NSInteger port = [standardDefaults integerForKey:@"websocket-executor-port"] ?: 8080;
```

The porting solutions were found in this [StackOverflow question](http://stackoverflow.com/questions/30216417/react-native-port-8081-already-in-use-packager-is-either-not-running-or-not-r) and the [react troubleshooting](http://facebook.github.io/react-native/docs/troubleshooting.html).
