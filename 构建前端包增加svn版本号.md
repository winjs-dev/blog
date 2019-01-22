> 为每个出去的前端包增加 `svn` 版本号，防止出现异常 bug，便于追踪，溯源。

### 依赖

- [svn-info](https://github.com/jtrussell/node-svn-info): 用于获取当前的 `svn` 版本信息
- [html-webpack-inline-code-plugin](https://github.com/cklwblove/html-webpack-inline-code-plugin): 将获取的 svn 版本号,以内联 `script` 形式插入到 `html` 文档中

### 具体代码实现

> 可以参照**小组件**，详见 `build/utils.js` 中的 `getSvnInfo()`,及 `genMultiHtmlPlugins()`

```js
const svnInfo = require('svn-info')
const HtmlWebpackInlineCodePlugin  = require('html-webpack-inline-code-plugin')

// 获取当前SVN版本信息
exports.getSvnInfo = function() {
  const svnUrl = 'https://192.168.57.168/BrokerNet/iSeeRobotAdvisor/trunk/Sources/web/h5-isee-financial-component';
  const info = svnInfo.sync(svnUrl, 'HEAD');

  return info;
}

// script 代码插入到 html 中
new HtmlWebpackInlineCodePlugin  ({
  headTags: [{
    tagName: 'script',
    tagCode: `window.__version=${exports.getSvnInfo().lastChangedRev}`
}]
```

**最终可以在控制台里，打印出来版本信息。(console.log(window.__version))**
