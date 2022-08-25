# node-red-contrib-ember
Simple Ember+ client node for broadcast automation (LAWO, DHD, etc.). Ember+ is open source and implemented in hard- and software products. More information can be found here: [Ember+ control protocol](https://github.com/Lawo/ember-plus/wiki)

## install
Inside your node-red directory, install the NPM node-red-contrib-ember package.

```
npm install node-red-contrib-ember
```

##  emberplus-server node 
First of all there is at least one emberplus-server node to be configured. Simply set an IP address and a port (usually 9000). It is possible to add several emberplus-server nodes for different targets (for example all your audio mixers).
Optionally, provide a coma separated list of paths to limit the tree.
If no path provided, the tool will download the entire tree - this could take a long time if the tree is really big.

![Select/Create an Ember connection](https://github.com/dufourgilles/node-red-contrib-emberplus/blob/master/images/ember_node_start.png)

![Define a new Ember connection](https://github.com/dufourgilles/node-red-contrib-emberplus/blob/master/images/server_create.png)

## emberplus node
Each emberplus node is associated with a previously configured emberplus-server node. Further an object path can be selected if the node should subscribe a specific object in the Ember+ tree. If you don't know the path, click on the looking glass symbol on the right.

![Create your node](https://github.com/dufourgilles/node-red-contrib-emberplus/blob/master/images/ember_node_create.png)

### Input Pin
The input pin of the node takes different type of message payloads.
For a Parameter node, the message payload is used to set the value of the parameter.
The two types of message payloads are:
- msg.payload takes the plain value to be set to the specified path. Example: 
```
true
```
- msg.payload contains path and value to the specified node. Example:
```
{"full":{"path":"0.1.0","value":true}}
```

For a Function node, the input pin is used to pass parameters to invoke the function from the ember server.
The message payload types are:
- msg.payload.args takes a javascript object containing the list of args for the function
```
[{"type":1,"value":17},{"type":1,"value":88}]
```
- msg.payload takes a JSON object string that will be parsed to create the javascript object.
The string should be similar to the obejct above.

The argument types are
```
1 integer
2 real
3 string
```

![Inject a function](https://github.com/dufourgilles/node-red-contrib-emberplus/blob/master/images/function_inject.png)

![Inject a parameter](https://github.com/dufourgilles/node-red-contrib-emberplus/blob/master/images/parameter_inject.png)

### Output Pin
The data format of the output pin can be configured:
- plain: msg.payload contains the plain value from the Ember+ object
- contents: msg.payload.contents contains the contents Ember+ object from the underlying node-emberplus client 
- full: msg.payload.full contains the full Ember+ object from the underlying node-emberplus (including the device path)
- json: msg.payload contains a JSON object representing the ember element.

For function element, the output is always the response to the invokation of the function.
```
{"invocationId":2,"success":true,"result":[{"type":"integer","value":105}]}
```

If needed the inital state may be sent by clicking a corresponding checkbox.

## To Be Done

- [ ] Setting matrix connections
- [ ] Ember+ server
