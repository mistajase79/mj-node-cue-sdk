mj-node-cue-sdk
========
***
### Node.js Corsair Cue SDK wrapper
`node-cue-sdk` is a Node.js addon for loading and and using the Cue SDK in
pure JavaScript.

This is an update for the node-cue-sdk wrapper orginal created by Yannick de Jong

Added support for iCue SDKv3 - including Lighting Node Pro and Commander Pro.
Also made other updates to fit my own needs.

I currently dont own any corsair keyboards - so i'm unable to test some functionality - however everything i needed works ( so now i can play CS:GO with reactive RGB lighting! ) 

If Corsair could provide me with a new keyboard or any other RGB accessory I'll be happy to add support for it!


### <a href="https://github.com/Yannicked/node-cue-sdk/wiki/Documentation">Original Documentation</a>

Example with asynchonous functions
-------

``` js
var CueSDK = require('mj-cue-sdk-node');

var cue = new CueSDK.CueSDK();

// The CueSDK.set function can also work asynchonously, just add a function to the arguments and it'll be asynchonous
cue.set('A', 255, 255, 0, () => { // This is the function which get called after completion
    console.log('Lights set!');
});

cue.set([
    ['A', 255, 0, 0],
    ['S', 0 , 255, 0],
    ['D', 0, 0, 255]
], () => { // This is the function which get called after completion
    console.log('Three lights set!');
}); // Set A to red, S to green, and D to blue

// fade from black [0, 0, 0] to cyan [0, 255, 255] in 1000ms
cue.fade('Logo', [0, 0, 0], [0, 255, 255], 1000, () {
    console.log('This will run when the fading has completed!');
});

```
***
Example with synchonous functions
-------

``` js
var CueSDK = require('mj-cue-sdk-node');

var cue = new CueSDK.CueSDK();

cue.set('W', 255, 255, 255); // Set the W key to #FFFFFF aka white

// You can set multiple colors at the time!
cue.set([
    ['A', 255, 0, 0],
    ['S', 0 , 255, 0],
    ['D', 0, 0, 255]
]); // Set A to red, S to green, and D to blue

// Special keys/lights are also supported!
cue.set('Logo', 255, 255, 0); // Make the Corsair logo yellow

// To turn off all leds
cue.clear();

```

***
Example for DIY-devices - Lighting Node and Commander Pro
-------

``` js
var CueSDK = require('mj-cue-sdk-node');
var cue = new CueSDK.CueSDK();

var deviceCount = cue.getCorsairDeviceCount();
console.log('deviceCount = '+deviceCount);

var ledIdsArray = [];

//loop through devices
for (i = 0; i<deviceCount; i++) {
  var deviceCheck = cue.getCorsairDeviceType(i);
  console.log(deviceCheck.model);

  if(deviceCheck.model == 'Lighting Node Pro'){
    var leds = cue.getCorsairPositionsByDeviceIndex(i);
    for (var i in leds) {
      if(leds[i].ledId != undefined){
        ledIdsArray.push(leds[i].ledId);
      }
    }
  }
  if(deviceCheck.model == 'Commander Pro'){
    var leds = cue.getCorsairPositionsByDeviceIndex(i);
    for (var i in leds) {
      if(leds[i].ledId != undefined){
        ledIdsArray.push(leds[i].ledId);
      }
    }
  }
}

cue.fadeSingleColour(ledIdsArray, [255, 0, 0], [0, 0, 0], 3000);

```

Requirements
------------

 * Windows (Linux and Mac OSX are currently not supported by the CueSDK)
 * Node.js ```5.0.0``` or higher

Installation
------------

Make sure you've installed all the [necessary build
tools](https://github.com/TooTallNate/node-gyp#installation),
then run this command in the source directory:

``` bash
$ npm install mj-cue-sdk-node
```
