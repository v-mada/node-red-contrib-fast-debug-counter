## Overview
This node is based on the official debug node from Node-RED. It displays selected message properties in the debug sidebar tab and optionally the runtime log with the addition of showing a counter of messages on its status.

## How to use
For displaying messages on the sidebar it works exactly as the official debug node:

![](https://raw.githubusercontent.com/rafaelmuynarsk/node-red-contrib-fast-debug-counter/main/gif/debug.gif)

The counter on this node is prepared to receive a huge amount of messages in a short interval of time. The following example has two flows:


1. The first one updates its status on all 10000 messages that are injected on the flow. It takes more than 20 seconds to achieve that.
2. The second one is this node, it only updates the messages when the difference of time between the last message and the actual one is bigger than 100 milliseconds. The counter node receives its result immediatly.

![](https://raw.githubusercontent.com/rafaelmuynarsk/node-red-contrib-fast-debug-counter/main/gif/speed.gif)

### Flows from GIFs:

```json
[{"id":"ac81780c.ea5eb8","type":"subflow","name":"counter","info":"","category":"utils","in":[{"x":640,"y":260,"wires":[{"id":"9db79af4.3b8d18"}]}],"out":[],"env":[],"color":"#FFF0F0","icon":"node-red-contrib-message-counter/message-counter.png","status":{"x":900,"y":160,"wires":[{"id":"6862a135.2ca69","port":0}]}},{"id":"9db79af4.3b8d18","type":"function","z":"ac81780c.ea5eb8","name":"Counter","func":"var count = context.get('count')||0;\ncount += 1;\ncontext.set('count',count);\nmsg.count = count;\nnode.status({fill:\"blue\", shape:\"ring\", text:count});\nreturn msg;\n","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":860,"y":260,"wires":[[]],"icon":"node-red-dashboard/ui_numeric.png"},{"id":"6862a135.2ca69","type":"status","z":"ac81780c.ea5eb8","name":"","scope":null,"x":740,"y":160,"wires":[[]]},{"id":"4d8450d412c6a5cf","type":"counter","z":"5424eef231a914dc","name":"","active":true,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","statusVal":"","statusType":"auto","x":860,"y":1060,"wires":[]},{"id":"0a5caad3674e0820","type":"inject","z":"5424eef231a914dc","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":700,"y":1060,"wires":[["4d8450d412c6a5cf"]]},{"id":"82d6229408d9b448","type":"function","z":"5424eef231a914dc","name":"send 10000 messages","func":"for (let i = 0; i < 10000; i++) {\n\tnode.send(msg)\n}","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":900,"y":1260,"wires":[["aafdf6be128499a8"]]},{"id":"027da97261601435","type":"inject","z":"5424eef231a914dc","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":700,"y":1260,"wires":[["82d6229408d9b448"]]},{"id":"225472f16a10979c","type":"counter","z":"5424eef231a914dc","name":"","active":false,"tosidebar":true,"console":false,"tostatus":false,"complete":"payload","targetType":"msg","statusVal":"","statusType":"auto","x":1100,"y":1340,"wires":[]},{"id":"aafdf6be128499a8","type":"subflow:ac81780c.ea5eb8","z":"5424eef231a914dc","name":"","x":1100,"y":1260,"wires":[]},{"id":"ec439b9c1ddc3007","type":"function","z":"5424eef231a914dc","name":"send 10000 messages","func":"for (let i = 0; i < 10000; i++) {\n\tnode.send(msg)\n}","outputs":1,"noerr":0,"initialize":"","finalize":"","libs":[],"x":900,"y":1340,"wires":[["225472f16a10979c"]]},{"id":"fe32a3477df41aca","type":"inject","z":"5424eef231a914dc","name":"","props":[{"p":"payload"},{"p":"topic","vt":"str"}],"repeat":"","crontab":"","once":false,"onceDelay":0.1,"topic":"","payload":"","payloadType":"date","x":700,"y":1340,"wires":[["ec439b9c1ddc3007"]]},{"id":"5524ce8fde1748cf","type":"comment","z":"5424eef231a914dc","name":"First sampe ","info":"","x":790,"y":1000,"wires":[]},{"id":"0678f00b67a5fb62","type":"comment","z":"5424eef231a914dc","name":"Second sample","info":"","x":800,"y":1200,"wires":[]}]
```

