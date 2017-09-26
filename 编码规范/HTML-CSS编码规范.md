# HTML/CSS编码规范
标签： 云纪网络IFS工作室前端开发小组

----------

##<a name='html'>HTML编码规范</a>
内容列表</br>
 - [标签](#21)</br>
 - [命名](#22)</br>
 - [属性](#23)</br>
 - [Head](#24)</br>
 - [图片](#25)</br>
 - [表单](#26)</br>
 - [其他](#27)</br>
 
###<a name='21'>标签</a>

 - `html`标签必须全部小写。
 - 所有元素必须正确嵌套。
 - 双标签必须闭合。
 - 对于无需自闭合的标签，不允许自闭合。
 - 对 `HTML5` 中规定允许省略的闭合标签，不允许省略闭合标签。
 - `HTML` 标签的使用应该遵循标签的语义。 <br>

常见语义化标签：

- `p` - 段落
- `h1`,`h2`,`h3`,`h4`,`h5`,`h6` - 层级标题
- `strong`,`em` - 强调
- `ins` - 插入
- `del` - 删除
- `ul` - 无序列表
- `ol` - 有序列表
- `dl`,`dt`,`dd` - 定义列表

`Html5`语义化标签：

- `header` - 页面或者`section`的页眉区域
- `footer` - 页面或者`section`的页脚区域
- `section` - 元素代表文档中的“节”或“段”，或者一个模块
- `aside` - 页面中的附属信息部分
- `article` - 页面中的文章部分

###<a name='22'>命名</a>

- `class`必须单词全字母小写，单词间以 - 分隔。且应注重命名的语义化，推荐使用英文语义命名，不推荐使用中文拼音命名。

例如:

```
<button class="btn-cancel">取消</button>
```

- `id`命名需以驼峰式命名，命名规则与`class`相同。

例如：

```
<button id="cancelButton">取消</button>  
```

- 命名是推荐使用`class`进行命名，以`id`命名时，该`id`必须保证页面唯一。

###<a nmae='23'>属性</a>

 - 属性名必须使用小写字母。
 - 属性值必须用双引号包围。
 - 自定义属性建议以 `xxx-` 为前缀，推荐使用 `data-`，自定义属性需按照语义化命名

###<a name='24'>Head</a>

 - 页面必须使用精简形式，明确指定字符编码。指定字符编码的`meta` 必须是`head`的第一个直接子元素。
 - 页面必须包含`title`标签声明标题。`title`必须作为`head` 的直接子元素，并紧随`charset`声明之后。
 - 使用`favicon`设置页面的小图标时，必须保证`favicon`地址可访问。
 - 若页面欲对移动设备友好，需指定页面的`viewport`。
 - 引入`CSS`时必须指明 `rel="stylesheet"`。
 - `JavaScript` 应当放在页面末尾，或采用异步加载。

###<a name='25'>图片</a>

 - 禁止`img`的`src`取值为空。延迟加载的图片也要增加默认的`src`，`src`取值为空，会导致部分浏览器重新加载一次当前页面。
 - 避免为`img`添加不必要的`title`属性，多余的`title` 影响看图体验，并且增加了页面尺寸。
 - 为重要图片添加`alt`属性，可以提高图片加载失败时的用户体验。
 - 添加`width`和`height`属性，以避免页面抖动。
 - 有下载需求的图片采用`img`标签实现，无下载需求的图片采用`CSS` 背景图实现。
 - 图片命名规范：
   - 下划线写法：前缀+描述
      - 背景 bg 
      - 按钮 btn 
      - 图标 icon 
      - 广告 ad 
      - 标志 logo</br>

 例如：`bg_videoPlay.png`

###<a nmae='26'>表单</a>

 - 有文本标题的控件推荐使用`label`标签将其与其标题相关联。
 - 使用`button`元素时必须指明`type`属性值。
 - 在针对移动设备开发的页面时，根据内容类型指定输入框的`type`属性。
 
###<a name='27'>其他</a>

 - `DOCTYPE`推荐设置为`html5`规范，及`<!DOCTYPE html>`
 - 页面中禁止使用空格符调整样式的空格。

----------

##<a name='css'>CSS/Less编码规范</a>
说明：本文档制定的目标是使`CSS`编码风格和编码规范保持一致，提高代码效率和性能，便于团队协作和项目维护。
虽然本文档是针对`CSS`制定的，但是在使用各种`CSS`的预编译器(如`less`、`sass`、`stylus`等)时，适用的部分也应尽量遵循本文档的约定。

内容列表</br>
 - [命名规则](#31)</br>
 - [格式化](#32)</br>
 - [编码规范](#33)</br>
 - [图片处理](#34)</br>
 - [less](#35)</br>

###<a name='31'>命名规则</a>
####1. 命名组成
- 命名必须由单词和中划线组成。所有命名都使用小写英文字母，使用中划线 `-` 作为连接字符，而不是下划线 `_`。
- 不使用汉语拼音来作为类名，尤其是缩写的拼音、拼音与英文的混合；

####2. 命名单词
- 不以表现形式来命名，比如：`.left`， `.right` ，`.center`， `.red`， `.black`。而是根据内容来命名。
- 类名命名应`见名知意`，根据内容来命名，推荐使用功能和内容相关词汇的命名。
  比如：`.header`，`.news-list`。

####3. 命名空间
说明：为了避免类名污染，造成样式冲突及后期维护困难，需要有一套合理有序的命名方式。通过命名空间来对类名进行划分和隔离，每个类名只作用在特定的命名空间下，互不干扰冲突，同时便于样式的快速定位和后期维护。

- 命名空间根据网页布局、页面、内容模块和组件来定义：


<table border="1">
<tr>
<td>前缀</td>
<td>说明</td>
<td>示例</td>
</tr>
<tr>
<td>g- </td>
<td>全局通用样式命名</td>
<td>g-hide,g-fl</td>
</tr>
<tr>
<td>page- </td>
<td>根据页面命名，作为每个页面的顶级类名</td>
<td>.page-home,.page-about</td>
</tr>
<tr>
<td>m- </td>
<td>模块命名，通常对多处共用的模块进行统一命名，已便于代码复用和功能扩展</td>
<td>m-product,m-search,m-news</td>
</tr>
<tr>
<td>ui- </td>
<td>组件命名</td>
<td>ui-table,ui-form</td>
</tr>
<tr>
<td>js-</td>
<td>用于纯交互的命名，不涉及任何样式规则</td>
<td>js-back</td>
</tr>
</table>

- 不允许对类似`.info`， `.active`，`.img`，`.tip` 等的选择器进行样式定义，以避免造成全局冲突。应在相应的模块下或顶级类名下进行样式定义。

//good
```
.login .tip {}
.info-box .tip {}
.nav-bar li .active {}
.news-list li .active {}
.page-index .btn-box {}
.page-about .btn-box {}
```	
//bad
```
.tip {}
.active {}
```
###<a name='32'>格式化</a>
####1. 字符编码格式
- 每一个`css`文件的字符编码格式应选择`无BOM的UTF-8编码方式`。

####2. 缩进
- 使用 4个空格做为一个缩进层级，不允许使用 2 个空格或tab字符。

####3. 空格
- `选择器` 与 ` { ` 之间必须包含空格。

```
.selector {
}
```
- `属性名` 与之后的 `: ` 之间不允许包含空格， `: ` 与 `属性值` 之间必须包含空格。

```
margin: 0;
```
- `列表型属性值` 书写在单行时，`, ` 后必须跟一个空格。

```
 font-family: Aria, sans-serif;
 box-shadow:5px 5px 5px rgba(0, 0, 0, .6); 
```

####4. 选择器
- 当一条样式规则包含多个选择器时，每个选择器声明必须独占一行。

//good
```
.post,
.page,
.comment {
    line-height: 1.5;
}
```
//bad
```
.post, .page, .comment {
    line-height: 1.5;
}
```		
- `>`、`+`、`~` 选择器的两边各保留一个空格。

//good
```
main > nav {
    padding: 10px;
}
label + input {
    margin-left: 5px;
}
input:checked ~ button {
    background-color: #ccc;
}
```
//bad
```
main>nav {
    padding: 10px;
}
label+input {
    margin-left: 5px;
}
input:checked~button {
    background-color: #ccc;
}
```
- 属性选择器中的值必须用双引号包围。不允许使用单引号，不允许不使用引号。

//good
```
article[character="juliet"] {
    voice-family: "Vivien Leigh", victoria, female
}
```
//bad
```
article[character='juliet'] {
    voice-family: "Vivien Leigh", victoria, female
}
```
####5. 属性

- 每一条属性定义必须另起一行。

//good
```
.selector {
    margin: 0;
    padding: 0;
}
```
//bad
```
.selector { margin: 0; padding: 0; }
```

- 每一条属性定义后必须以分号结尾。

//good
```
.selector {
    margin: 0;
}
```
//bad
```
.selector { 
	margin: 0
}
```		
###<a name='33'>编码规范</a>

####1. 通用

- 如非特殊需要，不使用 `行内样式` 和 `style 标签`，应该使用`外联CSS文件`

- 选择器，如无必要，不得为 `id`、`class` 选择器添加 `类型选择器` 进行限定。

//good
```
#error,
.danger-message {
    font-color: #c00;
}
```
//bad
```
div#error,
p.danger-message {
    font-color: #c00;
}
```
- 选择器的嵌套层级最多不大于 `3` 级，扩展类嵌套层级最多不大于 `4` 级，位置靠后的限定条件应尽可能精确。
`注：CSS匹配规则是从右往左，应此需要避免将效率低的选择器写在右侧`

//good
```
#username input {}
.comment .avatar {}
```
//bad
```
.page .header .login #username input {}
.comment div * {}
```
- 避免使用通用选择器 `*`。

- 属性缩写在可以使用缩写的情况下，尽量使用属性缩写。

//good
```
.post {
    font: 12px/1.5 arial, sans-serif;
}
```
//bad
```
.post {
    font-family: arial, sans-serif;
    font-size: 12px;
    line-height: 1.5;
}
```
说明： 由于 `border`/ `margin` / `padding` 等缩写会同时设置多个属性的值，容易覆盖不需要覆盖的值，如某些方向需要继承其他声明的值。使用 `border`/ `margin` / `padding` 等缩写时，应注意隐含值对实际数值的影响，确实需要设置多个方向的值时才使用缩写，否则应每个属性分开设置。

//good
```
.page {
    margin-right: auto;
    margin-left: auto;
}
.featured {
    border-color: #ccc;
}
```
//bad
```
.page {
    margin: 5px auto; 
}
.featured {
    border: 1px solid #ccc; 
}
```

- 属性书写顺序
   同一规则下的属性在书写时，应按功能进行分组，并以 `Formatting Model（布局方式、位置）` > `Box Model（尺寸）` > `Typographic（文本相关）` > `Visual（视觉效果）` 的顺序书写，以提高代码的可读性。
 - Formatting Model 相关属性包括：`position` / `top` / `right` / `bottom` / `left` / `float` / `display` / `overflow` 等

 - Box Model 相关属性包括：`border` / `margin` / `padding` / `width` / `height` 等
 - Typographic 相关属性包括：`font` / `line-height` / `text-align` / `word-wrap`等
 
 - Visual 相关属性包括：`background` / `color` / `transition` / `list-style`等

示例：
```
 .sidebar {
 
/* formatting model: positioning schemes / offsets / z-indexes / display / ...  */
    position: absolute;
    top: 50px;
    left: 0;
    overflow-x: hidden;
    
/* box model: sizes / margins / paddings / borders / ...  */
    width: 200px;
    padding: 5px;
    border: 1px solid #ddd;
    
/* typographic: font / aligns / text styles / ... */
    font-size: 14px;
    line-height: 20px;
    
/* visual: colors / shadows / gradients / ... */
    background: #f5f5f5;
    color: #333;
    -webkit-transition: color 1s;
    -moz-transition: color 1s;
    transition: color 1s;
}
```
- （清除浮动）当内部元素浮动父元素需要具有高度以避免网页塌陷时，需通过对伪类设置 `clear` 属性或触发 `BFC` 的方式来清除浮动。不使用增加空标签及其他方式来清除浮动。

 触发 BFC 的方式很多，常见的有：
	- `float` 非 `none`
	- `position` 非 `static`
	- `overflow` 非 `visibl`

	对已经触发 BFC 的元素不需要再清除浮动。

- 尽量不使用 `!important `进行声明。当需要强制指定样式且不允许任何场景覆盖时，通过标签内联和 !important 定义样式。

- 将 `z-index` 进行分层，对文档流外绝对定位元素的视觉层级关系进行管理。同层的多个元素，如多个由用户输入触发的 Dialog，在该层级内使用相同的 `z-index` 或递增 `z-index`。

	`注：在可控环境下，期望显示在最上层的元素，z-index指定为 999999。`


####2. 值和单位

- 文本</br>
文本内容必须用双引号包围。

//good
```
html[lang|="zh"] q:before {
    font-family: "Microsoft YaHei", sans-serif;
    content: "“";
}
html[lang|="zh"] q:after {
    font-family: "Microsoft YaHei", sans-serif;
    content: "”";
}
```
//bad
```
html[lang|=zh] q:before {
    font-family: 'Microsoft YaHei', sans-serif;
    content: '“';
}
html[lang|=zh] q:after {
    font-family: "Microsoft YaHei", sans-serif;
    content: "”";
}
```
- 数值</br>
当数值为 0 - 1 之间的小数时，省略整数部分的 0 。

//good
```
.panel {
    opacity: .8;
}
```
//bad
```
.panel {
    opacity: 0.8;
}
```
- 长度</br>
当长度为 0 时须省略单位。 (也只有长度单位可以省略)

//good
    
```
body {
    padding: 0 5px;
}
```
    
//bad
```
body {
    padding: 0px 5px;
}
```
- 颜色</br>
	RGB颜色值必须使用十六进制记号形式 `#rrggbb`。不允许使用 `rgb()`。
	仅当带有 `alpha` 的颜色信息时才可以使用 rgba()。使用 rgba() 时每个逗号后必须保留一个空格。
	
//good
```
.success {
    box-shadow: 0 0 2px rgba(0, 128, 0, .3);
    border-color: #008000;
}
```
//bad
```
.success {
    box-shadow: 0 0 2px rgba(0,128,0,.3);
    border-color: rgb(0, 128, 0);
}
```
- 当颜色值可以缩写时，必须使用缩写形式。

//good
```
.success {
    background-color: #aca;
}
```
//bad
```
.success {
    background-color: #aaccaa;
}
```
- 颜色值不允许使用命名色值。
	
//good
```
.success {
    color: #90ee90;
}
```
//bad
```
.success {
    color: lightgreen;
}
```
- 颜色值中的英文字符采用小写。
	  
//good
```
.success {
    background-color: #aca;
    color: #90ee90;
    }
```
//bad
```
.success {
    background-color: #ACA;
    color: #90EE90;
}
```	
- 2D 位置</br>
2D 位置初始值为 0% 0%，但在只有一个方向的值时，另一个方向的值会被解析为 `center`。为避免理解上的困扰，应同时给出两个方向的值。
参见：[background-position属性值的定义](https://www.w3.org/TR/CSS21/colors.html#propdef-background-position)

//good
```
body {
    background-position: center top; /* 50% 0% */
}
```
//bad
```
body {
    background-position: top; /* 50% 0% */
}
```
####3. 文本编排
- 字体族
`font-family `属性中的字体族名称应使用字体的英文` Family Name`，其中如有空格，须放置在引号中。
常见的 Family Name如下：</br>

    <table border="1">
        <tr>
        <td>字体</td>
        <td>操作系统</td>
        <td>Family Name</td>
        </tr>
        <tr>
        <td>宋体 (中易宋体) </td>
        <td>Windows</td>
        <td>SimSun</td>
        </tr>
        <tr>
        <td>黑体 (中易黑体)</td>
        <td>Windows</td>
        <td>SimHei</td>
        </tr>
        <tr>
        <td>微软雅黑</td>
        <td>Windows</td>
        <td>Microsoft YaHei</td>
        </tr>
        <tr>
        <td>华文黑体</td>
        <td>Mac/iOS</td>
        <td>STHeiti</td>
        </tr>
        <tr>
        <td>冬青黑体</td>
        <td>Mac/iOS</td>
        <td>Hiragino Sans GB</td>
        </tr>
        </table>


- `font-family ` 按“西文字体在前、中文字体在后”、“效果佳 (质量高/更能满足需求) 的字体在前、效果一般的字体在后”的顺序编写，最后必须指定一个通用字体族( `serif`  /  `sans-serif` )。
更详细说明可参考[本文](http://www.zhihu.com/question/19911793/answer/13329819)。


    

```
/* Display according to platform */
.article {
    font-family: Arial, sans-serif;
}
	
/* Specific for most platforms */
h1 {
    font-family: "Helvetica Neue", Arial, "Hiragino Sans GB", "WenQuanYi Micro Hei", "Microsoft YaHei", sans-serif;
}
```
- `font-family` 不区分大小写，但在同一个项目中，同样的 Family Name 大小写必须统一。

//good
```
body {
    font-family: Arial, sans-serif;
}
h1 {
    font-family: Arial, "Microsoft YaHei", sans-serif;
}
```
//bad
```
body {
    font-family: arial, sans-serif;
}
h1 {
    font-family: Arial, "Microsoft YaHei", sans-serif;
}
```
- 字号</br>
需要在 Windows 平台显示的中文内容，其字号应不小于 `12px`。
注：由于 Windows 的字体渲染机制，小于 12px 的文字显示效果极差、难以辨认。

- 字体风格</br>
需要在 Windows 平台显示的中文内容，不要使用除 `normal` 外的 `font-style`。其他平台也应慎用。
- 字重</br>
`font-weight` 属性必须使用数值方式描述。
`CSS` 的字重分 100 – 900 共九档，但目前受字体本身质量和浏览器的限制，实际上支持 400 和 700 两档，分别等价于关键词 `normal` 和 `bold`。浏览器本身使用一系列[启发式规则](https://www.w3.org/TR/CSS21/fonts.html#propdef-font-weight)来进行匹配，在 <700 时一般匹配字体的 `Regular` 字重，>=700 时匹配 `Bold`字重。

//good
```
h1 {
    font-weight: 700;
}
```
//bad
```
h1 {
    font-weight: bold;
}
```
- 行高</br>
`line-height` 在定义文本段落时，应使用数值。

注：将 `line-height` 设置为数值，浏览器会基于当前元素设置的 `font-size` 进行再次计算。在不同字号的文本段落组合中，能达到较为舒适的行间间隔效果，`font-size` 都需要设置 `line-height`。
当 `line-height` 用于控制垂直居中时，还是应该设置成与容器高度一致。

示例：
```
.container {
    line-height: 1.5;
}
```	
####4.  变换与动画
- 使用 transition 时应指定 `transition-property`。
	
//good
```
.box {
    transition: color 1s, border-color 1s;
}
```
//bad
```
.box {
    transition: all 1s;
}
```	
- 尽可能在浏览器能高效实现的属性上添加过渡和动画，增强用户体验。在可能的情况下应选择这样四种变换：

	- transform: translate(npx, npx);
	- transform: scale(n);
	- transform: rotate(ndeg);
	- opacity: 0..1;
	
	典型的，可以使用 translate 来代替 left 作为动画属性。

//good
```
.box {
    transition: transform 1s;
}
.box:hover {
    transform: translate(20px); /* move right for 20px */
}
```
//bad
```
.box {
    left: 0;
    transition: left 1s;
}
.box:hover {
    left: 20px; /* move right for 20px */
}
```
####5. 响应式
`Media Query` 不得单独编写，必须与相关的规则一起定义。

//good
```
/* header styles */
@media (...) {
    /* header styles */
}
/* main styles */
@media (...) {
    /* main styles */
}
/* footer styles */
@media (...) {
    /* footer styles */
}
```
//bad
```
/* header styles */
/* main styles */
/* footer styles */
@media (...) {
    /* header styles */
    /* main styles */
    /* footer styles */
}
```
###<a name='34'>图片处理</a>
- 纯色图标优先使用字体图标。字体图标通过 `CSS` 定义，不在 `html` 里定义，

例如:</br>
  
```
<i class="iconfont>#xe624;</i>
```

示例：

```
.icon-search:before { 
	content: "\e624";
}
```

- 应该使用合适尺寸的图片并进行压缩，在质量和尺寸间合理取舍。禁止直接使用未压缩的原图或尺寸明显超过需要的图片
- 对于多处共用的小图标，应该使用 `CSS Sprite` ，通过 `CSS` 属性引用。

- 图标样式的定义：图标样式的定义应尽量保持图标的独立性，以便在不同的地方使用。通常图标样式只需要定义`display` / `width` / `height` / `background`四个属性。其余和网页上下文有关的属性应在相应的地方进行定义。

//good

```
.icon {
    display:inline-block;
    width:30px;
    height:30px;
    background: url(bg.png);
}
.header .icon {
    vertical-align: middle;
}
.news .icon {
     float:left;
     margin-left:3px;
}
```

//bad

```
.header .icon {
    display:inline-block;
    width:30px;
    height:30px;
    background: url(bg.png);
    vertical-align: middle;
}

.news .icon {
    float:left;
    width:30px;
    height:30px;
    margin-left:3px;
    background: url(bg.png);
}
```
###<a name='35'>less</a>
说明：模块化能很好的避免CSS类名冲突及其造成的很多问题。通过less的`@import`语法能够非常方便的实现CSS模块化开发和管理。
###1. 项目构建

├── source</br>
│   ├── less</br>
│   │    ├── components</br>
│   │    ├── modules</br>
│   │    └── pages</br>
│   ├── css</br>
│   │    ├── app.less</br>
│   │    ├── home.less</br>
│   │    └── ...</br>
│   ├── images</br>

建议根据以上文件结构来构建项目的CSS，通过`@import` 按照模块来引入每个页面所需的less文件。
```
// Config
@import "mixin";
@import "config";

// Base
"normalize";
@import "global";
@import "base";
@import "header";
@import "footer";

// Components
@import "components/btn";
@import "components/form";
@import "components/icon";
@import "components/table";

// Modules
@import "modules/search";
@import "modules/aside-nav";
@import "modules/pagination";
@import "modules/article";
...

// Pages
@import "pages/home";
@import "pages/about";
@import "pages/contact";
...
```
###2.  less编码规范
- Less 代码的基本规范和原则与 CSS 编码规范 保持一致。

####1.代码组织

- @import
- 变量声明
- 样式声明

示例：
```
@import "est/all.less";
@default-text-color: #333;
.page {
    width: 960px;
    margin: 0 auto;
}
```
####2.@import 
- `@import `</br>
语句引用的文件必须写在一对引号内，.less 后缀不得省略（与引入 CSS 文件时的路径格式一致）。引号使用 ' 和 " 均可，但在同一项目内必须统一。

//good
```
@import "est/all.less";
@import "my/mixins.less";
```
//bad
```
@import "est/all.less";
@import "my/mixins.less";
```
####3.空格
- 属性、变量
`选择器`和 `{` 之间必须保留一个空格。
`属性名`后的冒号 `:` 与`属性值`之间必须保留一个空格，冒号前不得保留空格。
定义变量时冒号  `:` 与`变量值`之间必须保留一个空格，冒号前不得保留空格。
在用 `,` 分隔的列表（Less 函数参数列表、以 , 分隔的属性值等）中，`,`后必须保留一个空格，`,`前不得保留空格。

//good

```
.box {
    @w: 50px;
    @h: 30px;
    width: @w;
    height: @h;
    transition: width 1s, height 3s;
}
```
//bad
```
.box{
    @w:50px;
    @h :30px;
    width:@w;
    height :@h;
    color: rgba(255,255,255,.3);
    transition: width 1s,height 3s;
}
```
- 运算
`+` / `-` / `*` /	`/` 四个运算符两侧必须保留一个空格。`+` / `-` 两侧的操作数必须有相同的单位，如果其中一个是变量，另一个数值必须书写单位。

//good
```
@a: 200px;
@b: (@a + 100px) * 2;
```
//bad
```
@a: 200px;
@b: (@a+100)*2;
```
- 混入（mixin）
`mixin` 和后面的空格之间不得包含空格。在给 `mixin` 传递参数时，在参数分隔符（`,` /`;`）后必须保留一个空格：

//good
```
.box {
    .size(30px, 20px);
    .clearfix();
}
```
//bad
```
.box {
    .size(30px,20px);
    .clearfix ();
}
```
####4. 嵌套
- 嵌套的声明块前必须增加一次缩进，有多个声明块共享命名空间时尽量嵌套书写，避免选择器的重复。
但是需注意的是，尽量仅在必须区分上下文时才引入嵌套关系（在嵌套书写前先考虑如果不能嵌套，会如何书写选择器）。

//good

```
.main {
    .title {
        font-weight: 700;
    }
    .content {
        line-height: 1.5;
    }
    .warning {
        font-weight: 700;
    }
}
#comment:invalid {
    color: #f00;
}

```
//bad

```
.main .title {
  font-weight: 700;
}
.main .content {
  line-height: 1.5;
}
.main {
.warning {
  font-weight: 700;
}
.comment-form {
    #comment:invalid {
      color: #f00;
    }
  }
}
```
####5. 变量
- Less 的变量值总是以同一作用域下最后一个同名变量为准，务必注意后面的设定会覆盖所有之前的设定。

- 变量命名和类名一样，采用单词和`-`中划线作为连接符的形式。

//good

```
@sidebar-width: 200px;
@width: 800px;
```

//bad

```
@sidebarWidth: 200px;
@width:800px;
```

####6. 继承
- 使用继承时，如果在声明块内书写 `:extend` 语句，必须写在开头：

//good

```
.sub {
    &:extend(.mod all);
    color: #f00;
}
```
//bad

```
.sub {
    color: #f00;
    &:extend(.mod all);
}
```
####7. 混入（mixin）
- 在定义 `mixin` 时，如果 `mixin` 名称不是一个需要使用的 `className`，必须加上括号，否则即使不被调用也会输出到 CSS 中。

//good

```
.big-text() {
    font-size: 1.5;
}
h3 {
    .big-text();
}
```
//bad

```
.big-text {
    font-size: 1.5;
}
h3 {
    .big-text;
}
```
- 如果混入的是本身不输出内容的 `mixin`，必须在 `mixin` 后添加括号（即使不传参数），以区分这是否是一个 className（修改以后是否会影响 HTML）。

//good
```
.box {
    .clearfix();
    .size(20px);
}
```
//bad
```
.box {
    .clearfix;
    .size (20px);
}
```
####8. 命名空间
`变量` 和 `mixin` 在命名时必须遵循如下原则：

- 一个项目只能引入一个无命名前缀的基础样式库（如 global.less）
- 业务代码和其他被引入的样式代码中，`变量` 和 `mixin` 必须有项目或库的前缀

`mixin` 的参数分隔符使用 `,` 和 `;` 均可，但在同一项目中必须保持统一。

####9. 注释
- 单行注释尽量使用 `//` 方式。

- 属性声明
首先列出除去 @include 和嵌套选择器之外的所有属性声明，紧随后面的是 @include，这样可以使得整个选择器的可读性更高。

示例：
```
.btn-red {
  background-color: #f00;
  font-weight: 700;
  @include transition(background 0.5s ease);
  // ...
}
```

----------

##云纪网络前端开发小组内部文稿（禁止上传或转发）


