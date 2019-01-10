# Vue项目部署遇到的问题及解决方案

## 写在前面
`Vue-Router` 有两种模式，默认是 `hash` 模式，另外一种是 `history` 模式。

- **hash**：也就是地址栏里的 `#` 符号。比如 `http://www.example/#/hello`，hash 的值为 `#/hello`。特点：hash 虽然出现 URL 中，但不会被包含在 HTTP 请求中，对后端不会产生什么影响，改变 URL 不会重载页面。

- **history**：利用了 HTML5 History Interface 中新增的 `pushState()` 和 `replaceState()` 方法，来完成 URL 跳转而无须重新加载页面。（需要特定浏览器支持）

hash 和 history 两种模式都是基于**浏览器**自身的属性，`vue-router` 只是利用了这两个特性（底层还是浏览器提供的接口）来实现前端路由。

## 使用场景
一般来说，两种模式都是可以的。除非在意不太漂亮的 `#`，只能选择 history。

这两种模式在开发环境下都没有什么太大的问题，但是当部署到生产环境中后，两者有所不同。

hash 模式部署没有什么问题，只要访问到服务器上的 index.html，就可以访问网站了。 
history 模式下，前端的 URL 必须和实际向后端发起请求的 URL 一致，如 `http://www.example.com/user/id`。如果后端缺少对 `/user/id` 的路由处理，将返回 404 错误。
> 不过这种模式要玩好，还需要后台配置支持……所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。- [Vue-Router](https://router.vuejs.org/zh/guide/essentials/history-mode.html)

## 问题起因
在做「国盛年度账单」项目的时候，项目部署的时候，用的是 hash 模式。**国盛通安卓端**分享出去的链接对于 `#` 做了特殊处理，encode 转义成了 `%23`，所以**路由只能换成 history 的模式。**

因此，现把解决的思路总结下，虽然 [官网](https://router.vuejs.org/zh/guide/essentials/history-mode.html#%E5%90%8E%E7%AB%AF%E9%85%8D%E7%BD%AE%E4%BE%8B%E5%AD%90) 上给出了解决方案，但在实际的编码中也遇到了一些问题。

### 根目录下
当项目在根目录下部署的时候（如 `http://www.example.com/`）,`vue` 的相关文件默认不需要修改，修改的是后端，这里以 nginx 为例。

```bash
location / {
  try_files $uri $uri/ /index.html;
}
```
`$uri` 就是访问的 url，不包含 `域名` 和 `querystring`。例如 `/test/hello` 当访问 `$uri` 时，如果存在，则访问 `$uri/`, 不存在就访问 `/index.html` 这样配置好，访问 `http://example.com/` 时就可以访问到网站了，进入多级目录后刷新页面也不会存在问题。

### 子级目录下
这里涉及到修改 `vue` 项目几个配置文件。
先定义几个环境

- 部署的域名：`http://fulldev.yjifs.com:8080/`
- nginx 的 root 目录：`home/web/`
- vue 的部署路径：`home/web/h5-year-bill/`
- vue 项目的链接：`http://fulldev.yjifs.com:8080/h5-year-bill/`
- vue 项目的静态资源路径：`http://fulldev.yjifs.com:8080/h5-year-bill/static/`

**1. 打包后的静态资源路径需要修改**

找到 `build/config/index.js`，代码如下：

```diff
...
build: {
...
-    assetsPublicPath
     // 访问路径，修改成绝对路径
+    assetsPublicPath: '/h5-year-bill/'
}
```

**2. 路由文件**
`Vue-Router` 有一个 `base` 属性, [传送门](https://router.vuejs.org/zh/api/#base)
```
base
类型: string
默认值: "/"
应用的基路径。例如，如果整个单页应用服务在 /app/ 下，然后 base 就应该设为 "/app/"
```
因此，找到 `src/router/index.js`，代码如下：
```js
// 不影响本地开发，兼容性做了处理
const isHistoryMode = process.env.NODE_ENV === 'production' ? {
  mode: 'history',
  base: '/h5-year-bill/'
} : {
  mode: 'hash'
};

const router = new Router({
  ...isHistoryMode,
  routes
});
```

**至此，打包配置的相关修改已全部完成，项目也能够正常访问。**

**但还是会有一个问题，跳转到某个路由后，刷新页面，就会出现页面空白，或者路由不通，此时就要修改 nginx 的配置了。**

**3. nginx 配置相关修改**

`nginx部署路径/conf/nginx.conf`，修改如下：

```
#h5-year-bill
location ^~ /h5-year-bill {
    root   /home/web;
    index  index.html;
    try_files $uri $uri/ /h5-year-bill/index.html last;
}
```

`/h5-year-bill/` 就是部署的网站目录。

这样几项配置后，就可以在子目录下访问网站，刷新也没有问题。
