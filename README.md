# com.intel.xdk.inappbrowserplus

InAppBrowserPlus is based on a copy of the Android portion of Apache Cordova InAppBrowser 1.4.0.
This plugin extends the Cordova In App Browser plugin to add a new API for Android.
The resource block is commented out in plugin.xml in order to avoid colliding with the resources from InAppBrowser.
The java package name and the Cordova feature name have been changed to avoid colliding with InAppBrowser.
To see other differences, compare this plugin to Apache Cordova InAppBrowser 1.3.0.

## window.openplus

This is a copy of the InAppBrowser API window.open, but it opens an InAppBrowserPlus window instead of an InAppBrowser window.
See Cordova InAppBrowser 1.4.0 docs for more information.

## executeScriptWithConditionalCallback

> Injects JavaScript code into the `InAppBrowserPlus` window with deferred callback

    ref.executeScriptWithConditionalCallback(details, callback, callbackCondition);

- __ref__: reference to the `InAppBrowserPlus` window. _(InAppBrowserPlus)_

- __injectDetails__: details of the script to run, must be a`code` key. _(Object)_
  - __code__: Text of the script to inject.

- __callback__: the function that executes after the JavaScript code is injected.
    - If the injected script is of type `code`, the callback executes
      with a single parameter, which is the return value of the
      script, wrapped in an `Array`. For multi-line scripts, this is
      the return value of the last statement, or the last expression
      evaluated.

- __callbackCondition__: Text of the callback condition
   - The callback condition is evaluated every 250ms.
      When the callback condition evaluates to true, the callback will be called.

### Supported Platforms

- Android

### Quick Example

    var ref = cordova.InAppBrowser.openplus('http://apache.org', '_blank', 'location=yes');
   var myCallback = function(res) { console.log(JSON.stringify(res); }

   ref.addEventListener('loadstop', function() {
      ref.executeScriptWithConditionalCallback(
         { code: "localStorage.getItem( 'name' )" },
         myCallback,
         "localStorage.getItem( 'name' )"
      );
   });


I tried several approaches to extending the plugin, but in the end making a copy and adding the new API was the safest, most easily maintainable way.

Other approaches attempted
1. directly modify original plugin 
problem: too easy to lose changes with upgrade

2. extend native side, replace javascript side with wrapper
problem: InAppBrowser prototype is a closure, so is inaccesible

3. extend native side, provide new javascript API that wraps original API
problem: javascript wrapper not possible due to call to native being inline)