---
title: Inappbrowser
description: Open an in-app browser window based on this plugin (https://github.com/apache/cordova-plugin-inappbrowser), We just change it for android only to support open it in (X, Y, Width, Height).
---
<!--
# license: Licensed to the Apache Software Foundation (ASF) under one
#         or more contributor license agreements.  See the NOTICE file
#         distributed with this work for additional information
#         regarding copyright ownership.  The ASF licenses this file
#         to you under the Apache License, Version 2.0 (the
#         "License"); you may not use this file except in compliance
#         with the License.  You may obtain a copy of the License at
#
#           http://www.apache.org/licenses/LICENSE-2.0
#
#         Unless required by applicable law or agreed to in writing,
#         software distributed under the License is distributed on an
#         "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
#         KIND, either express or implied.  See the License for the
#         specific language governing permissions and limitations
#         under the License.
-->

# cordova-plugin-inappbrowser

You can show helpful articles, videos, and web resources inside of your app. Users can view web pages without leaving your app.


This plugin provides a web browser view that displays when calling `cordova.InAppBrowser.open()`.

    var ref = cordova.InAppBrowser.open('http://apache.org', '_blank', 'location=yes', X, Y, Width, Height);

The `cordova.InAppBrowser.open()` function is defined to be a drop-in replacement
for the `window.open()` function.  Existing `window.open()` calls can use the
InAppBrowser window, by replacing window.open:

    window.open = cordova.InAppBrowser.open;

The InAppBrowser window behaves like a standard web browser,
and can't access Cordova APIs. For this reason, the InAppBrowser is recommended
if you need to load third-party (untrusted) content, instead of loading that
into the main Cordova webview. The InAppBrowser is not subject to the
whitelist, nor is opening links in the system browser.

The InAppBrowser provides by default its own GUI controls for the user (back,
forward, done).

For backwards compatibility, this plugin also hooks `window.open`.
However, the plugin-installed hook of `window.open` can have unintended side
effects (especially if this plugin is included only as a dependency of another
plugin).  The hook of `window.open` will be removed in a future major release.
Until the hook is removed from the plugin, apps can manually restore the default
behaviour:

    delete window.open // Reverts the call back to its prototype's default

Although `window.open` is in the global scope, InAppBrowser is not available until after the `deviceready` event.

    document.addEventListener("deviceready", onDeviceReady, false);
    function onDeviceReady() {
        console.log("window.open works well");
    }


## <a id="reference">Reference</a>
## Installation

    cordova plugin add https://github.com/ahmedelrefaiy/InAppBrowserWidthHeight


## cordova.InAppBrowser.open

Opens a URL in a new `InAppBrowser` instance, the current browser
instance, or the system browser.

    var ref = cordova.InAppBrowser.open(url, target, options, X, Y, Width, Height);

- __ref__: Reference to the `InAppBrowser` window when the target is set to `'_blank'`. _(InAppBrowser)_

- __url__: The URL to load _(String)_. Call `encodeURI()` on this if the URL contains Unicode characters.

- __target__: The target in which to load the URL, an optional parameter that defaults to `_self`. _(String)_

    - `_self`: Opens in the Cordova WebView if the URL is in the white list, otherwise it opens in the `InAppBrowser`.
    - `_blank`: Opens in the `InAppBrowser`.
    - `_system`: Opens in the system's web browser.

- __X__: The starting point at X coordinate to open InAppBrowser. _(Number)_
- __Y__: The starting point at Y coordinate to open InAppBrowser. _(Number)_
- __Width__: The width of opened InAppBrowser. _(Number)_
- __Height__: The height of opened InAppBrowser. _(Number)_

- __options__: Options for the `InAppBrowser`. Optional, defaulting to: `location=yes`. _(String)_

    The `options` string must not contain any blank space, and each feature's name/value pairs must be separated by a comma. Feature names are case insensitive.

    All platforms support:

    - __location__: Set to `yes` or `no` to turn the `InAppBrowser`'s location bar on or off.

    Android supports these additional options:

    - __hidden__: set to `yes` to create the browser and load the page, but not show it. The loadstop event fires when loading is complete. Omit or set to `no` (default) to have the browser open and load normally.
    - __clearcache__: set to `yes` to have the browser's cookie cache cleared before the new window is opened
    - __clearsessioncache__: set to `yes` to have the session cookie cache cleared before the new window is opened
    - __closebuttoncaption__: set to a string to use as the close button's caption instead of a X. Note that you need to localize this value yourself.
    - __closebuttoncolor__: set to a valid hex color string, for example: `#00ff00`, and it will change the
    close button color from default, regardless of being a text or default X. Only has effect if user has location set to `yes`.
    - __footer__: set to `yes` to show a close button in the footer similar to the iOS __Done__ button. 
    The close button will appear the same as for the header hence use __closebuttoncaption__ and __closebuttoncolor__ to set its properties.
    - __footercolor__: set to a valid hex color string, for example `#00ff00` or `#CC00ff00` (`#aarrggbb`) , and it will change the footer color from default.
    Only has effect if user has __footer__ set to `yes`.
    - __hardwareback__: set to `yes` to use the hardware back button to navigate backwards through the `InAppBrowser`'s history. If there is no previous page, the `InAppBrowser` will close.  The default value is `yes`, so you must set it to `no` if you want the back button to simply close the InAppBrowser.
    - __hidenavigationbuttons__: set to `yes` to hide the navigation buttons on the location toolbar, only has effect if user has location set to `yes`. The default value is `no`.
    - __hideurlbar__: set to `yes` to hide the url bar on the location toolbar, only has effect if user has location set to `yes`. The default value is `no`.
    - __navigationbuttoncolor__: set to a valid hex color string, for example: `#00ff00`, and it will change the color of both navigation buttons from default. Only has effect if user has location set to `yes` and not hidenavigationbuttons set to `yes`.
    - __toolbarcolor__: set to a valid hex color string, for example: `#00ff00`, and it will change the color the toolbar from default. Only has effect if user has location set to `yes`.
    - __zoom__: set to `yes` to show Android browser's zoom controls, set to `no` to hide them.  Default value is `yes`.
    - __mediaPlaybackRequiresUserAction__: Set to `yes` to prevent HTML5 audio or video from autoplaying (defaults to `no`).
    - __shouldPauseOnSuspend__: Set to `yes` to make InAppBrowser WebView to pause/resume with the app to stop background audio (this may be required to avoid Google Play issues like described in [CB-11013](https://issues.apache.org/jira/browse/CB-11013)).
    - __useWideViewPort__: Sets whether the WebView should enable support for the "viewport" HTML meta tag or should use a wide viewport. When the value of the setting is `no`, the layout width is always set to the width of the WebView control in device-independent (CSS) pixels. When the value is `yes` and the page contains the viewport meta tag, the value of the width specified in the tag is used. If the page does not contain the tag or does not provide a width, then a wide viewport will be used. (defaults to `yes`).

    iOS supports these additional options:

    - __hidden__: set to `yes` to create the browser and load the page, but not show it. The loadstop event fires when loading is complete. Omit or set to `no` (default) to have the browser open and load normally.
    - __clearcache__: set to `yes` to have the browser's cookie cache cleared before the new window is opened
    - __clearsessioncache__: set to `yes` to have the session cookie cache cleared before the new window is opened    
    - __closebuttoncolor__: set as a valid hex color string, for example: `#00ff00`, to change from the default __Done__ button's color. Only applicable if toolbar is not disabled.
    - __closebuttoncaption__: set to a string to use as the __Done__ button's caption. Note that you need to localize this value yourself.
    - __disallowoverscroll__: Set to `yes` or `no` (default is `no`). Turns on/off the UIWebViewBounce property.
    - __hidenavigationbuttons__:  set to `yes` or `no` to turn the toolbar navigation buttons on or off (defaults to `no`). Only applicable if toolbar is not disabled.
    - __navigationbuttoncolor__:  set as a valid hex color string, for example: `#00ff00`, to change from the default color. Only applicable if navigation buttons are visible.
    - __toolbar__:  set to `yes` or `no` to turn the toolbar on or off for the InAppBrowser (defaults to `yes`)
    - __toolbarcolor__: set as a valid hex color string, for example: `#00ff00`, to change from the default color of the toolbar. Only applicable if toolbar is not disabled.
    - __toolbartranslucent__:  set to `yes` or `no` to make the toolbar translucent(semi-transparent)  (defaults to `yes`). Only applicable if toolbar is not disabled.
    - __enableViewportScale__:  Set to `yes` or `no` to prevent viewport scaling through a meta tag (defaults to `no`).
    - __mediaPlaybackRequiresUserAction__: Set to `yes` to prevent HTML5 audio or video from autoplaying (defaults to `no`).
    - __allowInlineMediaPlayback__: Set to `yes` or `no` to allow in-line HTML5 media playback, displaying within the browser window rather than a device-specific playback interface. The HTML's `video` element must also include the `webkit-playsinline` attribute (defaults to `no`)
    - __keyboardDisplayRequiresUserAction__: Set to `yes` or `no` to open the keyboard when form elements receive focus via JavaScript's `focus()` call (defaults to `yes`).
    - __suppressesIncrementalRendering__: Set to `yes` or `no` to wait until all new view content is received before being rendered (defaults to `no`).
    - __presentationstyle__:  Set to `pagesheet`, `formsheet` or `fullscreen` to set the [presentation style](http://developer.apple.com/library/ios/documentation/UIKit/Reference/UIViewController_Class/Reference/Reference.html#//apple_ref/occ/instp/UIViewController/modalPresentationStyle) (defaults to `fullscreen`).
    - __transitionstyle__: Set to `fliphorizontal`, `crossdissolve` or `coververtical` to set the [transition style](http://developer.apple.com/library/ios/#documentation/UIKit/Reference/UIViewController_Class/Reference/Reference.html#//apple_ref/occ/instp/UIViewController/modalTransitionStyle) (defaults to `coververtical`).
    - __toolbarposition__: Set to `top` or `bottom` (default is `bottom`). Causes the toolbar to be at the top or bottom of the window.
    - __hidespinner__: Set to `yes` or `no` to change the visibility of the loading indicator (defaults to `no`).

    Windows supports these additional options:

    - __hidden__: set to `yes` to create the browser and load the page, but not show it. The loadstop event fires when loading is complete. Omit or set to `no` (default) to have the browser open and load normally.
    - __hardwareback__: works the same way as on Android platform.
    - __fullscreen__: set to `yes` to create the browser control without a border around it. Please note that if __location=no__ is also specified, there will be no control presented to user to close IAB window.


### Supported Platforms

- Android
- Browser
- iOS
- OSX
- Windows

### Example

	var ref = cordova.InAppBrowser.open("http://apache.org", '_self', 'location=no,zoom=no,hardwareback=yes', 0, 0, 50, 100);

## Remaining Features

Start reading the remaining of the plugin features from the [original plugin](https://github.com/apache/cordova-plugin-inappbrowser) as it is the same.
Remember only to add (X, Y, Width, Height) after `options` parameter

## Limitations

- The plugin don't support the resizing during the execution of the app.
- You must predefined your app will be portrait or landscape from the begining (and disable user from changing this during the execution of the app)
- (X, Y, Width, Height) will be applied for android only.
