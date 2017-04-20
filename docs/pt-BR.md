# Exemplo de APP do WebView

Este é um projeto modelo para o Android Studio que permite criar um aplicativo de webview do Android em questão de minutos. Você pode usá-lo para criar um aplicativo simples para o seu site ou como um ponto de partida para o seu aplicativo baseado em HTML5 Android.

## Começando

1. [Instalar o Android Studio](http://developer.android.com/sdk/index.html), certifique-se de que o [Android SDK Tools](http://developer.android.com/sdk/index.html#Other) está instalado corretamente e instale os [pacotes apropriados](http://developer.android.com/sdk/installing/adding-packages.html) para as plataformas que você deseja segmentar.

2. [Download](https://github.com/rony-freitas/webview/archive/master.zip) ou clone este repositório e importe-o no Android Studio.

## Explicando o código

Estas são as configurações básicas de um bom WEB APP com WebView:

### AndroidManifest

* Você deve adicionar as permissões de INTERNET ao seu arquivo AndroidManifest
	
	Esta deve ser um filho do elemento <manifesto>.

	```xml
	<uses-permission android:name="android.permission.INTERNET" />
	```

### MainActivity

* Como obter configurações do WebView (**linha 49**)

	```java
	WebSettings webSettings = webView.getSettings();
	```
  
* Habilitando o JavaScript (**line 52**)

	```java
	webSettings.setJavaScriptEnabled(true);
	```
  
* Desabilitando suporte ao zoom (**recomendação**) (**linha 55**)

	```java
	webSettings.setSupportZoom(false);
	```

* Configurações para habilitar/desabilitar cache no WebView (**linha 58**)

	```java
	webView.setDrawingCacheEnabled(true);
	```

* Configuraçĩes para desabilitar a seleção de texto no WebView (**começo na linha 61**)

	```java
	webView.setOnLongClickListener(new View.OnLongClickListener() {
        @Override
        public boolean onLongClick(View v) {
            return true;
        }
    });
	```

* Configurações para habilitar Cookie no WebView (**começo na linha 69**)

	```java
	CookieManager cookieManager = CookieManager.getInstance();
        if (Build.VERSION.SDK_INT < Build.VERSION_CODES.LOLLIPOP) {
            CookieSyncManager.createInstance(this);
        }
    cookieManager.setAcceptCookie(true);
	```

* Configurando o WebViewClient (**começo na linha 75**)

	```java
	webView.setWebViewClient(new WebViewClient() {
        @Override
        public boolean shouldOverrideUrlLoading(WebView view, String url) {
            if (Uri.parse(url).getHost().equals("www.example.com")) {
                // Este é o meu site, portanto, não substitua; Deixe o meu WebView carregar a página
                return false;
            }
            // Caso contrário, o link não é para uma página no meu site, então inicie outra atividade que manipula URLs
            Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
            startActivity(intent);
            return true;
        }

        @Override
        public void onPageStarted(WebView view, String url, Bitmap favicon) {
            // Página iniciada
            webView.setVisibility(View.INVISIBLE);
            progressBar.setVisibility(View.VISIBLE);
        }

        @Override
        public void onPageFinished(WebView view, String url) {
            // Página finalizada
            progressBar.setVisibility(View.INVISIBLE);
            webView.setVisibility(View.VISIBLE);
        }

        @Override
        public void onReceivedError(WebView view, WebResourceRequest request, WebResourceError error) {
            // Em caso de erro
        }
    });
	```

* Carregar conteúdo (**começo na linha 109**)

	```java
	// Usar recursos remotos
    webView.loadUrl("http://www.example.com");

    // Usar recursos locais
    // webview.loadUrl("file:///android_asset/www/index.html");
	```

* Função para recarregar o WebView (**começo na linha 116**)
	
	```java
	private void webViewReload() {
        webView.reload();
    }
	```

* Recarregar WebView com o clique de o item do menu

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

* Navegando no histórico da página da web

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