# JUMP Android Upgrade Guide

This guide describes the steps required to upgrade from different versions of the library.

## Generalized Upgrade Process

1. Remove **Jump** or **JREngage** modules from your project.
2. Follow the steps in **Jump_Integration_Guide.md**

       **Note:** Be sure to update your `AndroidManifest.xml`.

### Generalized Solutions

* **java: package com.janrain.android.engage.R does not exist**

    Use `R` instead.

* **java: package com.janrain.android.engage.utils does not exist**

    Use `com.janrain.android.utils` instead.

* **java: method createConnection in class com.janrain.android.engage.net.JRConnectionManager cannot be applied to given types;
      required: java.lang.String,com.janrain.android.engage.net.JRConnectionManagerDelegate,java.lang.Object,java.util.List<org.apache.http.NameValuePair>,byte[],boolean
      found: java.lang.String,<anonymous com.janrain.android.engage.net.JRConnectionManagerDelegate.SimpleJRConnectionManagerDelegate>,<nulltype>
      reason: actual and formal argument lists differ in length**

    The signature of `createConnection` has changed to `createConnection(String requestUrl,
    JRConnectionManagerDelegate delegate, Object tag, List<NameValuePair> requestHeaders, byte[] postData,
    boolean followRedirects)`. `requestHeaders` and `postData` are both optional (can be `null).

* **java: cannot find symbol
      symbol:   method logd(java.lang.String,java.lang.String)
      location: class com.janrain.android.engage.JREngage**

    Use `com.janrain.android.utils.LogUtils` instead.

###  Upgrading to v6.0.0
* **IMPORTANT:** This is the last release of this form of the Janrain Android Mobile Libraries.  Other than major bug fixes or compatibility updates no further implementations will be released.  A new Android Sample Application will be written using more modern Android tools and libraries.
* The only IDE that this release supports and has been tested with is the Android Studio IDE.
* The Android Mobile Libraries have removed all inter-dependencies on the Google, Facebook, and Twitter SDK's and Libraries.  The SimpleDemoNative app has been created to demonstrate how to integrate native provider logon for these providers using their SDK's and Libraries.  Please refer to the "Native Authentication Guide" for more information.
* **NOTE:**  Google Play/Sign-On libraries newer than version 8.1 are NOT supported.  Google has changed the oAuth access token provisioning as of version 8.3 and it is no longer compatible with Janrain's API's at the time of this release.  Janrain will be updating their API's to support Google's re-architecture in the future.
* Resolved an issue with the CaptureJsonUtils.java file where customers with large amounts of user records would overflow the Integer data type.
* If you want to use the Janrain Mobile Libraries and Sample Code with the latest Android API levels there is now has dependencies on the deprecated org.apache.http.legacy.jar.  This file is included in the Github repo in the libs folder.  Additional information on this can be found in the build.gradle file.

### Upgrading to v5.0.1

The Janrain Mobile Libraries and Sample Code now has dependencies on OKHttp. Follow the steps in **Eclipse_Import_Guide.md** or
**IntelliJ_Import_Guide.md** no add the necessary Jars to your project.

### Solutions when upgrading to v4.7.0

* **When linking accounts my implementation of `jrAuthenticationDidSucceedForLinkAccount` is never called**

    `jrAuthenticationDidSucceedForLinkAccount` was moved from the `JREngageDelegate` to the
    `Jump.CaptureLinkAccountHandler` to prevent Social Sign-in only applications from having to implement
    unnecessary methods. To fix this add `Jump.CaptureLinkAccountHandler` to the list of interfaces that your
    delegate implements. For example:

        private class MyEngageDelegate implements JREngageDelegate, Jump.CaptureLinkAccountHandler {


## Upgrading v2.0.1-v3.1.0 to v4.2.0

1. Remove the **JREngage** module from your project.
2. Follow the steps in **Engage_Only_Integration_Guide.md** (v2.0.1 through v3.1.0 did not support Capture.)

       **Note:** Be sure to update your `AndroidManifest.xml`.


### Solutions for upgrading from v2.0.1-v2.0.12

* **java: package com.janrain.android.engage.utils does not exist**

    Import `com.janrain.android.utils.PrefUtils` and change references to `Prefs` to `PrefUtils`.

* **java: reference to showAuthenticationDialog is ambiguous, both method
    showAuthenticationDialog(android.app.Activity,java.lang.String) in com.janrain.android.engage.JREngage and
    method showAuthenticationDialog(java.lang.Boolean,java.lang.String) in com.janrain.android.engage.JREngage
    match**

    `showAuthenticationDialg(Boolean, String)` has been deprecated, use
    `showAuthenticationDialog(Activity, String)` instead and pass in the Activity that is launching the
     authentication as the Activity.

     For example in the old version of our SimpleDemo we had
     `mEngage.showAuthenticationDialog(null, "facebook")` in `MainActivity`'s `onCreate`. To upgrade we
     replaced it with `mEngage.showAuthenticationDialog(MainActivity.this, "facebook")`.


### Solutions for upgrading from v3.0.0-v3.1.0

* **java: cannot find symbol
      symbol:   class JRCustomUiConfiguration
      location: package com.janrain.android.engage.ui**

    Use `com.janrain.android.engage.ui.JRCustomInterfaceConfiguration` instead.

* **java: cannot find symbol
      symbol:   class JRCustomUiView
      location: package com.janrain.android.engage.ui**

    Use `com.janrain.android.engage.ui.JRCustomInterfaceView` instead.

* **java: cannot find symbol
      symbol:   method showBetaDirectShareDialog(com.janrain.android.simpledemo.MainActivity,com.janrain.android.engage.types.JRActivityObject)
      location: variable mEngage of type com.janrain.android.engage.JREngage**

    `showBetaDirectShareDialog` has been removed.

* **java: method does not override or implement a method from a supertype** in reference to a subclass of
    SimpleJRConnectionManagerDelegate

    The signature of `connectionDidFail` has changed to `connectionDidFail(Exception ex,
    HttpResponseHeaders responseHeaders, byte[] payload, String requestUrl, Object tag)`.
