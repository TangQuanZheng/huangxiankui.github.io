#Web Apps
本文整理、翻译自[飞奔的蜗牛](https://huangxiankui.github.io/blog/index.html?diaries/index.md)——转载请加以说明
###1.Web Apps支持不同的应用程序
###2.在webview中建立Web Apps
###3.调试Web Apps
###4.迁移到webView（Android4.4）
两种实现应用程序的方法:
1.客户端应用程序（C/S架构）
2.基于浏览器的web应用程序（B/S架构），也就是本文的主旨——Web Apps。
##1.Web Apps支持不同的应用程序
####the viewport
视口矩形区域,为web页面提供了一个可移动区域。可以指定多个视窗属性,例如它的大小和最初的规模。最重要的是视窗宽度,从web页面的点(可用CSS像素的数量)定义了适合水平像素的总数
####屏幕密度
WebView类和大多数web浏览器把 CSS像素值转换为密度独立像素值,所以你的网页出现在相同的可感知的大小中密度屏幕(约160dpi)，然而,如果web设计的图形是一个重要的因素,那么密切关注发生在不同密度的比例。因为300px-wide的图像在320 dpi屏幕上的像素会被按比例扩大。
####1.指定viewport属性
viewport有自己的像素密度来适应web page.
虽然设备屏幕可能物理宽度为480像素,视窗的宽度可以800像素。当视窗比例是1.0时，允许一个网页设计在800像素宽完全在屏幕上可见。
**NOTE**当在webview中渲染你的页面，，它没有默认使用大窗口模式（wide viewport mode)，可以用setUseWideViewPort（）启用大视窗模式。

支持的视口属性和values类型
```xml
<meta name="viewport"
      content="
          height = [pixel_value | "device-height"] ,
          width = [pixel_value | "device-width"] ,
          initial-scale = float_value ,
          minimum-scale = float_value ,
          maximum-scale = float_value ,
          user-scalable = ["yes" | "no"]
          " />
```

```xml
<head>
    <title>Example</title>
    <meta name="viewport" content="width=device-width, user-scalable=no" />
</head>
```
**NOTE**你应该禁止用户缩放，只有当你确定你的web页面布局是灵活的以及内容适合小屏幕的宽度。
###2.针对设备用CSS
Android浏览器和WebView支持CSS的媒体功能,允许您创建特殊的的风格。-webkit-device-pixel-ratio CSS 媒体特征可以是"0.75", "1", 或 "1.5"
可以为每一个密度创建独立的样式表

```xml
<link rel="stylesheet" media="screen and (-webkit-device-pixel-ratio: 1.5)" href="hdpi.css" />
<link rel="stylesheet" media="screen and (-webkit-device-pixel-ratio: 1.0)" href="mdpi.css" />
```
指定一个样式表的不同风格

```css
#header {
    background:url(medium-density-image.png);
}

@media screen and (-webkit-device-pixel-ratio: 1.5) {
    /* CSS for high-density screens */
    #header {
        background:url(high-density-image.png);
    }
}

@media screen and (-webkit-device-pixel-ratio: 0.75) {
    /* CSS for low-density screens */
    #header {
        background:url(low-density-image.png);
    }
}
```
###3.针对设备密度使用JavaScript
window.devicePixelRatio  
1.0   中等密度设备，默认无扩展
1.5   高等密度设备，默认扩展1.5倍
0.75 低等密度设备，默认缩小0.75倍
```JavaScript
if (window.devicePixelRatio == 1.5) {
  alert("This is a high-density screen");
} else if (window.devicePixelRatio == 0.75) {
  alert("This is a low-density screen");
}
```
##2.在WebView中建立Web Apps
###1.添加WebView
####1.界面
```xml
<?xml version="1.0" encoding="utf-8"?>
<WebView  xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/webview"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent"
/>
```
####2.加载webview

```java
WebView myWebView = (WebView) findViewById(R.id.webview);
myWebView.loadUrl("http://www.example.com");
```
####3.添加权限

```xml
<manifest ... >
    <uses-permission android:name="android.permission.INTERNET" />
    ...
</manifest>
```
###2.在WebView中使用javascript
####1.enable Javascript

```xml
WebView myWebView = (WebView) findViewById(R.id.webview);
WebSettings webSettings = myWebView.getSettings();
webSettings.setJavaScriptEnabled(true);
```
####2.绑定javascript到android
**注意**
如果你设置你的targetsdkVersion在17或以上，必须添加@JavascriptInterface

```java
public class WebAppInterface {
    Context mContext;

    /** Instantiate the interface and set the context */
    WebAppInterface(Context c) {
        mContext = c;
    }

    /** Show a toast from the web page */
    @JavascriptInterface
    public void showToast(String toast) {
        Toast.makeText(mContext, toast, Toast.LENGTH_SHORT).show();
    }
}
```
绑定这个类到JavaScript用addJavaScriptInterface（）来运行运WebView和叫做android的接口。

```java
WebView webView = (WebView) findViewById(R.id.webview);
webView.addJavascriptInterface(new WebAppInterface(this), "Android");
```

```javascript
<input type="button" value="Say hello" onClick="showAndroidToast('Hello Android!')" />

<script type="text/javascript">
    function showAndroidToast(toast) {
        Android.showToast(toast);
    }
</script>
```
不需要从javascript实例化android接口，webview自动适配web页面，当点击按钮时，showAndroidToast（）使用android接口来调用WebAppInterface.showToast().
**NOTE**这个对象绑定到javascript运行的另一个线程上，而不是构造的线程上。
**注意**用addJavascriptInterface（）存在一个危险的安全问题，最好全部自己写出现在webview中的html页面等。
###3.处理页面导航
用户点击打开链接

```java
WebView myWebView = (WebView) findViewById(R.id.webview);
myWebView.setWebViewClient(new WebViewClient());
```
如果想更多控制加载链接，创建自己的WebViewVlient来覆盖

```java
private class MyWebViewClient extends WebViewClient {
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
}
```

```java
WebView myWebView = (WebView) findViewById(R.id.webview);
myWebView.setWebViewClient(new MyWebViewClient());

```
###4.浏览网页的历史
goback（）和goforward（）来导航前进和后退的网页记录
canGoForward（）、canGoBack（）来判断网页是否可以前进或后退

```java
@Override
public boolean onKeyDown(int keyCode, KeyEvent event) {
    // Check if the key event was the Back button and if there's history
    if ((keyCode == KeyEvent.KEYCODE_BACK) && myWebView.canGoBack()) {
        myWebView.goBack();
        return true;
    }
    // If it wasn't the Back key or there's no web page history, bubble up to the default
    // system behavior (probably exit the activity)
    return super.onKeyDown(keyCode, event);
}
```
##3.调试Web Apps
###1.在Android浏览器使用控制台api
web页面执行JavaScript

```javascript
console.log("Hello World");
```

###2.在webview使用控制台api
API level7

```java
WebView myWebView = (WebView) findViewById(R.id.webview);
myWebView.setWebChromeClient(new WebChromeClient() {
  public void onConsoleMessage(String message, int lineNumber, String sourceID) {
    Log.d("MyApplication", message + " -- From line "
                         + lineNumber + " of "
                         + sourceID);
  }
});
```
API level 8或者更高

```java
WebView myWebView = (WebView) findViewById(R.id.webview);
myWebView.setWebChromeClient(new WebChromeClient() {
  public boolean onConsoleMessage(ConsoleMessage cm) {
    Log.d("MyApplication", cm.message() + " -- From line "
                         + cm.lineNumber() + " of "
                         + cm.sourceId() );
    return true;
  }
});
```
无论你是在为Android 2.1(API Level 7) 或 Android 2.2 (API Level 8 或更高)开发，你都需要实现WebChromeClient 并覆盖onConsoleMessage() 回调方法。然后，使用setWebChromeClient()将WebChromeClient应用到你的WebView 中。
##4.迁移到Android4.4的webView

android4.4介绍新版本的基于Chromium的webview，升级webview性能和标准来支持HTML5，CSS3、Javascript来适配最新的web浏览器。当运行在Android 4.4和更高版本时任何应用程序使用WebView将继承特性并升级。
###1.用户代理更改
如果你的WebView内容是基于用户代理的，你应该发现用户代码的内容已经发生了轻微的变化，现在的版本已经包含了Chrome版本:

```javascript
Mozilla/5.0(Linux; Android 4.4; Nexus 4 Build/KRT16H) AppleWebKit/537.36
(KHTML,like Gecko) Version/4.0 Chrome/30.0.0.0 Mobile Safari/537.36
```
如果你需要检索查询用户代理，但是不需要存储或者不想要实例化WebView,那么你应该使用静态方法,getDefaultUserAgent()。 但是如果你需要修改WebView的用户代理，那么你应该去使用getUserAgentString()方法。
###2.多线程和线程阻塞
使用runOnUiThread()方法,确保代码在UI线程上执行。

```java
runOnUiThread(new Runnable() {
    @Override
    public void run() {
        // Code for WebView goes here
    }
});
```
等待javascript回调时常用的错误代码，容易导致阻塞UI线程。

```java
// This code is BAD and will block the UI thread
webView.loadUrl("javascript:fn()");
while(result == null) {
  Thread.sleep(100);
}
```
用 evaluateJavascript()方法，异步运行JavaScript。
###3.自定义URL处理
当我们请求资源以及处理自定义URL结构链接的时候，新的WebView也提供额外的限制。例如:如果我们实现shouldOverrideUrlLoading()以及shouldInterceptRequest()这些回调方法，这样可以保证WebView只会回调合法的URLs。如果你横在使用一个自定义URL结构或者基于URL，有时候在Android4.4中接收加载数据失败。
###4.视窗变化
1.视窗target-densitydpi属性不再支持
2.视窗小时放大
3.不支持多个视窗标签
4.默认缩放弃用
###5.风格变化
####1.CSS速记覆盖background-size背景
####2.用CSS中像素,而不是屏幕上的像素
window.outerWidth和window.outerHeighet返回宽高属性是屏幕像素大小值。在新的WebView中，该返回是CSS样式中定义的像素值大小
####3.窄列以及单列不在被支持
在新的WebView中WebSettings.LayoutAlgorithm的NARROW_COLUMNS值将不再被支持。
[注]:在Android4.4或者更高的版本中这些APIs将不会被支持，即时你的targetSdkVersion设置成18或者更低，也不会起作用了。
通过以下方面可以处理这种变化:
1.改变程序风格
如果你控制编写页面的HTML和CSS样式文件，改变设计内容可能是最有效的方法。例如:如果你没有为你的页面定义viewport属性，这个设置将是特别有用的。
2.使用新的文本自动大小缩放布局算法:如果你使用narrow columns来让一个桌面网页在移动设备上面更具可读性并且不能改变HTML的内容，那么是哦那个TEXT_AUTOSIZING algorithm可能更加适合而不是使用narrow columns方法。
此外之前弃用的SINGLE_COLUMNS在新版本WebView中也不支持使用了。
####4.在JavaScript处理触摸事件
如果你的网页在WebView中直接处理触摸事件,并且确定也处理touchcancel事件。但是有些场景touchcance调用也会出现一些问题。
1.当控件元素被触摸或者页面滚动的时候，touchcancel可能会抛出异常。
2.当控件元素被触摸但是event.preventDefault()不被调用之前,touchcancel可能会抛出异常(WebView假定你不想使用触摸事件)。

 






