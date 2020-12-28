This is a POC of a terminal-webapp interop demo.
The demos would consist of
- a terminal running termshare in copilot mode
- a browser running a simple SPA (not the bultin termshare default term.js)

# Demo scenarios

## term -> browser

You type a special command in terminal and the SPA app runing in the browser gets updated.
The special command can be:
- a special bash function
- a special bash comment (like a line starting with doubble ##)

## browser -> term

You can interact with the browser, and something happens in the connected terminal:
- click on something in the browser, detail is "echo"-d into the terminal
- click in the browser executes a command in the terminal

## install poc binary
This branch contains a slightly modified termshare:
- it can have a fixed session name: so a websocket client can use a fixed address, instead of the random UUID
- the daemon accepts multiple copilot

The prebuild osx binary can be downloaded
```
curl -Lo /tmp/termshare https://github.com/lalyos/termshare/releases/download/remotectrl/termshare
chmod +x /tmp/termshare
/tmp/termshare -v
```

## minimal poc

start the daemon (locally in multicopilot mode without tls):
```
PORT=7777 ./termshare -s 127.1:7777 -n -d -m
```

in a new window/pane start the termshare session:
```
PORT=7777 ./termshare -n -s 127.1:7777 -c -f termshare
```

Now open a browser with the built-in xterm.js: <a href="http://127.0.0.1:7777/termshare">http://127.0.0.1:7777/termshare</a>

open up developer toolbars console:
```
ws = new WebSocket("ws://127.0.0.1:7777/termshare")
ws.send("echo hello\n")
```

