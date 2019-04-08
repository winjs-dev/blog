# `JSN`开发总结

## 样式部分

1：扩展字体图标

使用iconfont导出fonts

构建自定义font-family

```js
// app.js 引用
// 字体图标扩展
const domModule = App.requireModule('dom');
domModule.addRule('fontFace', {
  'fontFamily': 'iconfont',
  'src': 'url(\'./fonts/iconfont.ttf\')'
});
```

使用`text`标签展示图标

```
<text class="icon">&#xe604;</text>
```

`css`定义如下:

```
.icon {
    font-family: iconfont; // 这里不带引号
  }
```

2：文字过长…省略在`H5`和`JSN`两端下的同步实现

```css
lines: 1; // JSN
width: 280px;
white-space: nowrap;
overflow: hidden;
text-overflow: ellipsis;
```

3：元素宽度的计算

`JSN`开发过程中所有屏幕宽度都为**750px**，因此具体宽度可通过比例计算获得

4：`JSN`开发中元素过多页面滚动展示

一定要使用带有滚动功能的标签如`scroller`、`list`等

5：`JSN`不能使用`css`背景图，只能用`image`标签

6：**z-index**在`JSN`中无效，如果想层级高一些，则将`DOM`元素放在`DOM`结构下部

7：`JSN`中绑定`class`只能用数组不能用对象

8：**文字内容**必须只能放在`text`标签内，因为后续会编译成`native`的对应元素

9：所有父级元素默认均带`display:flex;`属性

10：`JSN`部分所有`dom`元素后续都会编译成`native`的元素，因此`DOM`层级越少性能越好，尽量减少嵌套

11：不支持**less**等预编译语法

## 框架部分

1：`JSN`的native网络请求使用stream，ajax暂时只支持`H5`部分，axios也不行

2：release可同时编译出`H5`和`JSN`代码，-p命令后直接访问index.html为`H5`入口，app.native.js为`JSN`入口

3：`project.json`中plugin去掉`jsn即`H5`开发模式

4：`JSN`框架打包出的 `H5`包，start会监听壳子device ready事件，\_startNow不会监听此事件，因此现场有个现象是用start方法在壳子上访问时白屏，改成\_startNow后就正常了，但后续了解到，如果正常情况来讲，壳子访问`H5`都会走桥接层的deviceready事件的，start按理说也没问题，具体原因需要他们去检查下，我们后续还用start就好了

5：`light`对象是唯一实例，组件内可以通过`this.$light`来访

6：引入js插件时可直接放到根目录下，这样打包出来文件还在，如果单独创建文件夹引入的话则打包后目录中则不存在了，与打包的文件引用机制有关吧。

7：依赖的安装要在`lib`文件夹下执行,用咱们自己的[light-jsn-template](<https://github.com/cloud-templates/light-jsn-template>),则不存在这种差异

8：根目录下新增的文件都需要重新`release`后才能引用到

9：`JSN`中不能使用`window`对象，在`app.js`文件中，如果使用`window`对象会导致`lightView`页面加载失败

10：获取`DOM`模块，可以通过 `light.requireModel('dom')`

11：`JSN`项目用`lightView`预览时，如果确实是同一个局域网，还是提示网络异常，原因是由于代码在环境里报错了，**先从自身代码排查**。

## demo示例

[*https://git01.hundsun.com/light/light-demo-os/tree/master/lighting-ui-demoApp*](https://git01.hundsun.com/light/light-demo-os/tree/master/lighting-ui-demoApp)

https://git01.hundsun.com/liwb/jsn-xy-poc/ （这次POC的项目，用[light-jsn-template](<https://github.com/cloud-templates/light-jsn-template>)重构之后的）

## 内测版文档

*http://3aonb8o53.lightyy.com/zh-cn/docs/*

## 联机调试步骤

[*https://document.lightyy.com/app\_dev\_`JSN`/content/shi\_yong\_lightView\_she\_bei\_tiao\_shi.html*](https://document.lightyy.com/app_dev_`JSN`/content/shi_yong_lightView_she_bei_tiao_shi.html)

注意：在调试完成之后，记得关闭lightView的联机调试功能，否则再次扫描二维码时不会展示页面，此时lightView认为还在调试功能中。

## 使用`JSN`框架工程直接改为纯`H5`的相关问题

实际开发中涉及到了用`JSN`框架临时改为纯`H5`的情况（与`JSN`打包出的`H5`部分不同，不涉及框架转换一层的过程，访问速度上快了20%），只需将`project.json`中plugin的`JSN`ative去掉即可，但后续改动样式的工作量较多

1：标签不同，如`text、image、list`以及相应动态绑定的事件

2：样式写法不同，所有px单位的值需要重新转换，而且需要单独设置适配，另外所有父元素的flex样式已经没有默认display:flex;的属性

3：背景图和字体图标的使用要修改

4：接口请求服务要修改

## 其他

1：他们所说的web容器即webview所在controller做一些优化处理

2：他们所说的js桥即Cordova那种，只不过Cordova属于自己重量级工具，他们的桥相对轻量级

3：他们`JSN`的演示是通过自己的壳子，带有light相关内容的壳子，跑`JSN`打包出的代码（已被转换成native部分）

4：他们`H5`的演示是通过自己的壳子带有light相关内容的壳子，跑`JSN`打包出的代码（已被转换成`H5`的部分）

5：图标如果用iconfont整体加载速度较慢，如果用压缩后的png整体加载速度较快，后续我们可以单独测试下

6：`lightView`客户端和`light`平台不是同步更新的，所以有可能存在异常隐患。

7：在弱网环境下`H5`的数据访问不到，此时建议`H5`部分做数据缓存，后续`light`会提供相应接口，将数据缓存到客户端本地，而不是缓存到`webview`中，这样对持久性和数据量都有较好的保障

8：顾老师建议我们将使用过程中的不良体验整理文档发给他们，他们会及时修复和更新
