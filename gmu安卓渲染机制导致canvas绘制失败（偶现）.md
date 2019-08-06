# gmu安卓渲染机制导致canvas绘制失败（偶现）

## 背景
在做`离线包`的时候，有的时候用户环境会遇到*应用首页*绘制图表失败的问题，用的图表库包含
`echarts,F2等`。

## 解决方案

>容器加载机制导致的，
webview里的内容是后台渲染的以降低白屏时间，导致body的width获取为0，rem就全为0了

在 `index.html` 里添加如下代码：
```js
// index.html
<script>
    (function(doc, win) {
      var docEl = doc.documentElement,
          resizeEvt = 'orientationchange' in window ? 'orientationchange' : 'resize',
          recalc = function() {
              var clientWidth = screen.width;
              if (!clientWidth) return;
              docEl.style.fontSize = (clientWidth / 10) + 'px';
          };
      if (!doc.addEventListener) return;
      win.addEventListener(resizeEvt, recalc, false);
      doc.addEventListener('DOMContentLoaded', recalc, false);
    })(document, window);
  </script>
```

