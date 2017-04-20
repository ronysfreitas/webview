# WebView APP Example

This is a template project for Android Studio that allows you to create an android webview application in minutes. You can use it to create a simple app for your website or as a starting point for your HTML5 based android app.

[![alt tag](https://github.com/rony-freitas/webview/tree/master/docs/en-US.png)](https://github.com/rony-freitas/webview) | Documentation en-US

[![alt tag](https://github.com/rony-freitas/webview/tree/master/docs/pt-BR.png)](docs/pt-BR.md) | Documentation pt-BR

## Getting started

1. [Install Android Studio](http://developer.android.com/sdk/index.html), make sure that the [Android SDK Tools](http://developer.android.com/sdk/index.html#Other) are properly installed and install the [appropriate packages](http://developer.android.com/sdk/installing/adding-packages.html) for the platforms you want to target.

2. [Download](https://github.com/rony-freitas/webview/archive/master.zip) or clone this repository and import it into Android Studio.

## Explaining the code

These are the basic settings of a good WEB APP with WebView:

### AndroidManifest

* You must add the INTERNET permissions to your Android Manifest File
	
	This must be a child of the <manifest> element.

	```xml
	<uses-permission android:name="android.permission.INTERNET" />
	```

### MainActivity

* Getting WebView Settings (**line 49**)

	```java
	WebSettings webSettings = webView.getSettings();
	```
  
* Enabling JavaScript (**line 52**)

	```java
	webSettings.setJavaScriptEnabled(true);
	```
  
* Disable Support Zoom (**line 55**)

	```java
	webSettings.setSupportZoom(false);
	```

* Settings to enable/disable cache in WebView (**line 58**)

	```java
	webView.setDrawingCacheEnabled(true);
	```

* Settings to disable text selection in WebView (**start in line 61**)

	```java
	webView.setOnLongClickListener(new View.OnLongClickListener() {
        @Override
        public boolean onLongClick(View v) {
            return true;
        }
    });
	```

* Settings to enable Cookie in WebView (**start in line 69**)

	```java
	CookieManager cookieManager = CookieManager.getInstance();
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.LOLLIPOP) {
            CookieSyncManager.createInstance(this);
        }
    cookieManager.setAcceptCookie(true);
	```

* Set Up WebViewClient (**start in line 75**)

	```java
	webView.setWebViewClient(new WebViewClient() {
        @Override
        public boolean shouldOverrideUrlLoading(WebView view, String url) {
            if (Uri.parse(url).getHost().equals("www.example.com")) {
                // This is my web site, so do not override; let my WebView load the page
                return false;
            }
            // Otherwise, the link is not for a page on my site, so launch another Activity that handles URLs
            Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
            startActivity(intent);
            return true;
        }

        @Override
        public void onPageStarted(WebView view, String url, Bitmap favicon) {
            // Page Started
            webView.setVisibility(View.INVISIBLE);
            progressBar.setVisibility(View.VISIBLE);
        }

        @Override
        public void onPageFinished(WebView view, String url) {
            // Page Finished
            progressBar.setVisibility(View.INVISIBLE);
            webView.setVisibility(View.VISIBLE);
        }

        @Override
        public void onReceivedError(WebView view, WebResourceRequest request, WebResourceError error) {
            // In case of error
        }
    });
	```

* Load resource (**start in line 109**)

	```java
	// Use remote resource
    webView.loadUrl("http://www.example.com");

    // Use local resource
    // webview.loadUrl("file:///android_asset/www/index.html");
	```

* Function for reload WebView (**start in line 116**)
	
	```java
	private void webViewReload() {
        webView.reload();
    }
	```

* Reload WebView on click item menu

    ```java
	@Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int id = item.getItemId();

        if (id == R.id.reload) {
            webViewReload();
            return true;
        }

        return super.onOptionsItemSelected(item);
    }
	```

* Navigating web page history

	```java
	@Override
    public void onBackPressed() {
        if (webView.canGoBack()) {
            webView.goBack();
        } else {
            super.onBackPressed();
        }
    }
    ```