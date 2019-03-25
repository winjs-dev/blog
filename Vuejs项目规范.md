# Vuejs项目规范

**注意：此项目规范适用于 `vue-cli2`，使用的项目脚手架是 [vue-template](https://github.com/cloud-templates/vue-template)**

一、首先全局安装 vue-cli

> npm install -g vue-cli

检测是否安装成功，有版本号输出则说明安装成功（这里是大写的V）

> vue -V


二、下载vue项目模板

> vue init cloud-templates/vue-template <my-project>


三、项目目录结构

```

├─build
│  └─config    # webpack相关配置
├─config       # 基于node-config配置的服务请求地址
├─src          # 源码目录
│  ├─assets    # 静态资源文件
│  │  ├─fonts  # font字体，这里默认放置iconfont.cn生成的全部文件
│  │  ├─images
│  │  │  └─sprites # 如果icon不能生成字体，则放在这里最终生成雪碧图（_sprites.png）
│  │  ├─js         # 不通过npm下载的第三方js文件
│  │  └─less       # 样式文件
│  │      └─widget
│  ├─components     # 组件，统一采用大驼峰连接，如SendCode
│  │  └─SendCode
│  ├─filters        # 过滤器
│  ├─router         # 路由文件
│  ├─services       # 服务请求（针对于axios的封装）
│  └─views          # 页面page，统一采用小驼峰（切记），helloWorld
│      └─hello
└─static        # 目录下的文件不会被webpack处理，而其他的静态资源都会被webpack处理解析为模块依赖


```

四、项目模板`package.json dependencies`内置常用的工具集

- [magicless](https://github.com/cklwblove/magicless) less的mixins集合

- [cloud-utils](https://github.com/cloud-templates/cloud-utils) 常用的js工具类函数集，详见[api具体说明文档](https://cloud-templates.github.io/cloud-utils/)

五、项目中尽量用**绝对路径**，这里搭配webapck自带的**路径别名**及**文件别名**可以很容易实现这一点，并且写法很简单。具体如下（webpack.base.js）：

```
  alias: {
    // 目录别名
    '@': utils.resolve('src'),
    '@static': utils.resolve('static'),
    '@assets': utils.resolve('src/assets'),
    '@less': utils.resolve('src/assets/less'),
    '@js': utils.resolve('src/assets/js'),
    '@components': utils.resolve('src/components'),
    '@mixins': utils.resolve('src/mixins'),
    '@filters': utils.resolve('src/filters'),
    '@store': utils.resolve('src/store'),
    '@views': utils.resolve('src/views'),

    // 文件别名
    'services': utils.resolve('src/services'),
    'variable': utils.resolve('src/assets/less/variable.less'),
    'utils': utils.resolve('node_modules/cloud-utils/dist/cloud-utils.esm'),
    'mixins': utils.resolve('node_modules/magicless/magicless.less')

```

- 在**vue**文件的用法：

```
<template>
  <div class="page page-hello">
    <div class="page-content">
      <!-- 静态资源路径写法事例 -->
      <img src="~@assets/images/logo.png">
      <h1 v-text="msg"></h1>
      <h2 v-text="message"></h2>
      <div class="demo">
        <h3>方法示例</h3>
        <pre>
          &lt;template&gt;
            &lt;div class=&quot;page page-hello&quot;&gt;
              &lt;!-- 静态资源路径写法事例 --&gt;
              &lt;img src=&quot;~@assets/images/copyfiles/logo.png&quot;&gt;
              &lt;p v-text=&quot;msg&quot;&gt;&lt;/p&gt;
              &lt;!-- 组件用法 --&gt;
              &lt;send-code class=&quot;button button-default&quot; v-model=&quot;start&quot; @click.native=&quot;handleSendCode&quot;&gt;&lt;/send-code&gt;
            &lt;/div&gt;
          &lt;/template&gt;
          &lt;script&gt;
            /**
            * 以下仅为事例代码，可以随意扩展修改
            */

            // 工具类
            import {formatDate} from &#x27;utils&#x27;;
            // 组件
            import SendCode from &#x27;@components/SendCode&#x27;;

            export default {
              data() {
                return {
                  msg: &#x27;Welcome to Your Vue.js App&#x27;,
                  start: false
                }
              },
              created() {
                this.getTenantInfo();
              },
              methods: {
                getTenantInfo() {
                  // 接口请求示例
                  const data = {
                    tenant_key: '06db342e571d46da8867b79d7e8a47ea'
                  };
                  this.$services.funcTenantInfoGet({
                    data
                  }).then((res) =&gt; {
                    console.log(&#x27;接口请求成功：&#x27; + JSON.stringify(res, null, 2));
                    this.message = formatDate(Date.now());
                  });
                },
                handleSendCode() {
                  setTimeout(() =&gt; {
                    this.start = true;
                  }, 1000);
                }
              },
              components: {
                &#x27;send-code&#x27;: SendCode
              }
            }
          &lt;/script&gt;

          &lt;style lang=&quot;less&quot; rel=&quot;stylesheet/less&quot;&gt;
            @import &quot;./style.less&quot;;
          &lt;/style&gt;
        </pre>
      </div>
    </div>
  </div>
</template>

<script>
  /**
   * 以下仅为事例代码，可以随意扩展修改
   */

  // 工具类
  import {formatDate} from 'utils';
  // 组件
  import {SendCode} from '@components';

  export default {
    data() {
      return {
        msg: 'Welcome to Your Vue.js App123',
        message: '现在时间是：' + formatDate(Date.now()),
        start: false
      }
    },
    methods: {
      handleSendCode() {
        setTimeout(() => {
          this.start = true;
        }, 1000);
      }
    },
    components: {
      'send-code': SendCode
    }
  }
</script>

<style lang="less" rel="stylesheet/less">
  @import "./style.less";
</style>


```

**Note**: [Vuejs官方编码指南](https://cn.vuejs.org/v2/style-guide/index.html)



