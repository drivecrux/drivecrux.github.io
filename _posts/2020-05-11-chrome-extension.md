---
layout: post
title: Chrome-extension 
date: 2020-05-11 17:15
summary: Build your own chrome extension in just 10 minutes to skip Ads on youtube.
categories: jekyll pixyll
---

We all love to customize our browsers and extensions are just perfect for doing that. Time after time we wonder, what if there is an easier and faster way of doing something while browsing. <br>
From finding discount codes to reading in a dark mode, there are a whole plethora of extensions that we could use. And let me tell you, they do help in increasing our productivity while browsing.
So, let us try to build a simple chrome extension for skipping Ads on youtube. After this tutorial you can even build your own chrome extension whenver required.

## Creating directory for files
The first step is to create a folder for storing our extension files. 
* Let us make a folder as <span class="red">my-extension</span> and move into it.
```javascript
$ mkdir my-extension
$ cd my-extension
```
 
## manifest.json:
Every extension needs a <span class="red">manifest.json</span> file that contains all the information about the app. 
> You could read about it [here](https://developer.chrome.com/extensions/manifest).

* Create manifest.json file and add the following fields into it.
```javascript
$ nano manifest.json
{
  "manifest_version": 2,
  "name": "Ad skipper",
  "description": "Skips Ads on Youtube",
  "version": "1.0"
}
```
> <span class="red">manifest_version</span>, <span class="red">name</span> and <span class="red">version</span> fields are required, rest you could add according to your program.

## Adding Logo
Next up we will add logo for our extension. 
> You could create your own or just use [this](https://raw.githubusercontent.com/drivecrux/chrome-extension/master/icon.png) instead.

* Add the <span class="red">.png</span> file into our extension folder and add it into our manifest file.
```javascript
{
  "manifest_version": 2,
  "name": "Ad skipper",
  "description": "Skips Ads on Youtube",
  "version": "1.0",
  “icons”: {“128”: “icon.png”} 
}
```

## Skipping the Ads:
* Now, we will create a file <span class="red">App.js</span> for defining the functions for skipping the Ads.
```javascript
$ nano App.js
```
* For skipping Ads on youtube, we just have to look for the skip button on Ads and trigger it by <span class="red">click()</span>. We could also add buttons for closing the banner Ads. 
* Our App.js file will look something like this:
```javascript
const click = (clazz) => {
  const buttons = document.getElementsByClassName(clazz);
  for (const button of buttons) {
    button.click();
  }
}

setInterval(() => {
  click("ytp-ad-skip-button-text");
  click("ytp-ad-overlay-close-button");
}, 300);
```

>"ytp-ad-skip-button-text": This is for video Ads(<span class="red">Skip Ad</span>)

>"ytp-ad-overlay-close-button": This is for banner Ads(<span class="red">X</span>)

## Content Script:
* The <span class="red">content script</span> is the file that can directly access the contents of a web page. This file will perform tasks on wherever we insert it. <br>
* The <span class="red">matches</span> field specifies which domains to run the content script on, in our case we want to run it on Youtube.
* We only have to add some lines in our manifest.json file:
```javascript
{
  "manifest_version": 2,
  "name": "Ad Skipper",
  "description": "Skips Ads on Youtube.",
  "version": "1.0",
  "icons": { “128”: "icon.png" },
  "content_scripts": [
    {
      "matches": ["https://*.youtube.com/*"],
      "js": ["App.js"]
    }
  ]
}
```

## Installing the extension:
* Open [chrome-extensions](chrome://extensions/) on Google chrome and toggle the developer mode. 
* Click on <span class="red">Load unpacked</span> and navigate to the extension folder(my-extension) to select it
* When the extension is loaded we will see the following:
<p style="text-align:center;"><img src="https://raw.githubusercontent.com/drivecrux/chrome-extension/master/screenshot.PNG" alt="Logo"></p>

## Testing of extension:
* Open Youtube and disable other Youtube Adblockers, if installed. 
* To check that our script is inserted, you could Inspect and open sources to see the script.
* Open some Youtube videos and see for yourself that Ads are getting closed automatically.

Now, you have got your own working chrome extension and also know how to create more. <br>

This is just a basic tutorial to get you to know about making chrome extensions. You could create much more complex ones for your purpose.

Source code is available [here](https://github.com/drivecrux/chrome-extension/tree/master). 





