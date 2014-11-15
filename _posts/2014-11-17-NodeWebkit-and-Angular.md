---
layout: post
title: NodeWebkit – Node and Angular.js for rich native Apps
---

I have to admit: I am stunned. Not that I haven't expected this. Every developer who has written a decent fat JavaScript client has stumbled upon it: How to use native or OS-level features with my application. And there have been answers. Google approached it with their chromium extensions. Flash always was capable of doing it in some limited ways. But what Intel has pulled of with the node community is stunning. And far to less people know about it. Hell, I'm gonna hypothesis it

> NodeWebkit is a game changer for native applications

Don't believe me? Well, than let my try to convince you.

## Superior UI models
The web is a vibrant place and the last years have been absolutely wild. Technologies come on a daily bases, decent ones on a monthly. The jungle is full of live and everyone is trying to find his corner in the ecosystem until the next great thing comes up and snatches it away.

> NodeWebkit is one of those technologies that doesn't snatch away a corner - it opens up a new one

Developing with nodeWebkit means to have you fronted based on the DOM, use your favorite javascript libraries / frameworks and show it in a native frame. The clue though is that you can run your node-source code just side by side. Written down in the same source file it allows you to mix the typical "client side / server side" code. It does not require you to host the files via express, just open them up locally (expect to need the ```node-remote:"somesource"``` flag).

> Without jugging the application structure that will grow from this, the possibility to run native node code and DOM UI code is what I will, with certainty call an "enabler" of innovation. 

## Rapid prototyping
With the unique ecosystem of modules, libraries and packages that exist in the JS  world many applications can be ported easily over to native apps. But it's nit only the porting. It enables to rapidly build a new class of applications that make heavy use of the web, its technologies, its design thinking etc. 

## The gaming industry knew
We have seen this shift a couple of years ago. A small company named "..." developed a decent html5 binding for c and c++ that allowed to integrate the DOM, CSS and what not into games. Suddenly those heavy applications hat these beautiful interfaces that everyone around could modify, iterate over or extend. Even though blizzard didn't catch up with the trend they surely tried something similar in World of Warcraft. 

Not everyone will agree with me on this. They'll say "oh yeah ? Facebook tried to bring HTML into their app. Didn't work. Users want native apps". Meh, I don't agree with this argument. It's a lie developers tell themselves to not get out of their comfort zone. Your users may want native as long as they can tell apart native from non-native. For your users, native is not much more than the color and shape of the controls. Your user base will not give a damn about your "believe" that you want them to want native applications. 

> But my point is not that I assume that this will replace native applications - it enables to write applications that have a far better UI concept and a vast and vibrant ecosystem of modules.

This is not a manifesto against native controls. I'm not saying the king is dead.

# An example

Now what does one need to set up an application that runs node and client side JavaScript in the same window. Well, you need

* node-webkit and
* a package.json

That's it. 

Create your favorite JS file structure. I typically start out with angular.

```sh
$ yo angular
```

Then go ahead and open up your ```package.json```. Everything you need to add is

```json
{
  "main": "app/index.html",
  "node-remote": "somesource",
  "window": {
    "show": false,
    "toolbar": false,
    "frame": true,
    "position": "center",
    "width": 360,
    "height": 300,
    "min_width": 220,
    "min_height": 220
  },
  "name": "nwapp",
  "version": "0.0.0"
}
```

Thats all. Note that the ```main``` property points to your index file and that ```node-remote:"somesource"``` disables chromes security constraint to not load local files with JavaScript. To test it out just run NodeWebkit and point to your current JavaScript project

```sh
$ /path/to/node-webkit.app/Contents/MacOS/node-webkit .
```
You will notice that this doesn't work since bower has betrayed you, as it assumes that you host your application on a web server. Now, that means you need to reference you JS and CSS files via a filesystem-path from your index.html. If you have 

```javascript
|-- package.json
|-+ app
| |-- index.html
|
|-+ some_js_components
  |-- some_js.js
```

you will need to reference it by

```html
<script src="../some_js_components/some_js.js"></script>
```

To make angular run, make sure that you referenced controllers via their relative path to the index as well, not via webserver routes.

```javascript
[...]
.when('/', {
    templateUrl: 'views/main.html',
    controller: 'MainCtrl'
  })
[...]
```

Now fire up the window with the ```onload`` function.

```javascript
var gui = require("nw.gui");
onload = function() {
    gui.Window.get().show();
  }
```

And we are already done. If you reference your files in the correct way, NodeWebkit will run them. And you are free to write things like 

```javascript
// Get the fs module ready
var fs = require("fs");
// Fire up the chrome devtools to debug the running application
gui.Window.get().showDevTools();
```

in your JS files.

## Links
* [bracket.io](http://www.bracket.io)
* [a template using node.js, code mirror, markdown (node.js and browser version) and local file access](https://bitbucket.org/ins0m/nw-template.git
)

## Notes
* Yes, this does allow you to access files of the local hard drive in JavaScript. Yes, it allows you to instantly work with the fs, native dialogs, the window object, processes etc. It gives you node.js
* No, you do NOT need express or any competitor to run such an app. Dont map REST interfaces to your frontend.


