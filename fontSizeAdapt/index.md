## font-size 设置 rem

项目的跟目录的index中运行js代码，根据屏幕的宽度动态改变font-size

```
(function (doc, win) {
    var docEl = doc.documentElement,
        resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
        recalc = function () {
            var clientWidth = docEl.clientWidth;
            docEl.style.fontSize = 100 * (clientWidth / 750) + 'px';
        };
    if (!doc.addEventListener) {
        return;
    }
    win.addEventListener(resizeEvt, recalc, false);
    doc.addEventListener('DOMContentLoaded', recalc, false);
    recalc();
}(document, window));

```

在页面放进了安卓的快应用的时候，发现如果用户的系统字体大小进行了修改，那么上述的修改的方案会让字体显示也偏大。

本来认为是无解的，但是经过查看[快应用的官网](https://m.quickapp.cn)发现好像有更好的适配方案。

## 代码如下
```
function adapt(designWidth, rem2px) {
    var d = window.document.createElement('div');
    d.style.width = '1rem';
    d.style.display = "none";
    var head = window.document.getElementsByTagName('head')[0];
    head.appendChild(d);
    var defaultFontSize = parseFloat(window.getComputedStyle(d, null).getPropertyValue('width'));
    d.remove();
    document.documentElement.style.fontSize = window.innerWidth / designWidth * rem2px / defaultFontSize * 100 + '%';
    var st = document.createElement('style');
    var portrait =
        "@media screen and (min-width: " + window.innerWidth + "px){html{ font-size:" + ((window.innerWidth / (designWidth / rem2px) / defaultFontSize) * 100) + "%; } }";
    var landscape = "@media screen and (min-width: " + window.innerHeight + "px) { html{ font-size:" + ((window.innerHeight / (designWidth / rem2px) / defaultFontSize) * 100) + "%; } }"
    st.innerHTML = portrait + landscape;
    head.appendChild(st);
    return defaultFontSize
};
var defaultFontSize = adapt(750, 100);
```

## 两个适配方案的区别

1. 可以认为，都是为了修改网页根字体的大小。
2. 方案一，当视窗确定，系统字体大小被修改的时候，通过查看computed得知，设定的1rem不再是单纯的根字体的px大小，而是重新计算过的px，其中具体的原理可以再研究。
3. 在方案二中，先查看未修正过1rem字体的大小，再根据视窗宽来搞，可以保证页面基本不会再根据系统字体大小来修改
4. 就像app软件一样，用户设定了系统字体大小，但app仍可以有自己规定但比例来建设页面。也可以说，跟随系统字体大小。所以方案各有利弊。
5. 如果说想要能跟着系统字体大小变化，注意页面布局要更加但适配性。要求会更高，也是更应该推荐但方案。