# WebView APP Example

This is a template project for Android Studio that allows you to create an android webview application in minutes. You can use it to create a simple app for your website or as a starting point for your HTML5 based android app.

### Getting started

1. [Install Android Studio](http://developer.android.com/sdk/index.html), make sure that the [Android SDK Tools](http://developer.android.com/sdk/index.html#Other) are properly installed and install the [appropriate packages](http://developer.android.com/sdk/installing/adding-packages.html) for the platforms you want to target.

2. [Download](https://github.com/rony-freitas/webview/archive/master.zip) or clone this repository and import it into Android Studio.

### Explaining the code

These are the basic settings of a good WEB APP with WebView

######## Getting WebView Settings (**line 49**)

	```
	WebSettings webSettings = webView.getSettings();
	```
  
######## Enabling JavaScript (**line 52**)

	```
	webSettings.setJavaScriptEnabled(true);
	```
  
########. Disable Support Zoom (**line 55**)

	```
	webSettings.setSupportZoom(false);
	```
