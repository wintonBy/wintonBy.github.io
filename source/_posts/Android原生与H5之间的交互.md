---
title: Android原生与H5之间的交互
categories: Android
tags: [android,H5,WebView]
date: 2016-4-13 14:33:23

---

------

## 引言
　　应用开发中经常会遇到原生应用与H5交互的问题，作为开发者需要掌握这方面的知识，才能很好的解决问题，作为要开始加速的新司机，这里做一番总结。
![fun_img](http://news.k618.cn/roll/201706/W020170607727364801229.png)
## Js调用Android原生的方法
　　就是指我们在html页面调用Andorid本身或者我们自己写的Java方法(好想画个图)，这一块相对来说是比较简单的，具体的步骤和方法，我罗列在下面。
### 创建接口对象


### 开启WebView支持
　　这一步就是让WebView支持js调用Java本地的方法，示例代码如下：

```java
mWebView.getSettings().setJavaScriptEnabled(true);
```
### 编写本地方法用于js调用
```java
public class JavaScriptObject {

    private Context mContext;

    public JavaScriptObject(Context context){
       mContext = context;
    }
    @JavascriptInterface
    public boolean hasBindBox(){
        boolean result = false ;
        return result;
    }
}
```
### 注册方法对象
　　在对WebView进行初始化时，通过下面的方法将持有方法的对象进行注册，这里相当于localFunc就是一个JavaScriptObjecte类的对象。
```java
mWebView.addJavascriptInterface(new JavaScriptObject(this),"localFunc");

```
### 调用
　　在需要调用相关方法的js中可以直接调用，实现js与原生的交互。
```JavaScript
localFunc.hasBindBox();
```
![fun_img](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1523541251728&di=2051e876e48ad8fac8481816ce5746ae&imgtype=0&src=http%3A%2F%2Fwanzao2.b0.upaiyun.com%2Fsystem%2Fpictures%2F37317948%2Foriginal%2F1468993411_600x600.png)

## 原生调用Js的方法
　　这个就更简单了，说出来你可能不信，只需要一行代码：
```java
mWebView.loadUrl("javascript:setText('hello world')");
```

　　其中setText()方法就是我们当前html页面的一个js方法，如下：
```JavaScript
function setText (text) {
        var tx = document.getElementById("tx");
        tx.value=text;
      }
```
　　是不是炒鸡简单，哈哈哈！！！
![go](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1523542320679&di=531d67c8743d7677948b47db88805314&imgtype=0&src=http%3A%2F%2Fwww.xz7.com%2Fup%2F2017-8%2F20178995340.jpg)
