# Vuejs项目规范

一、首先全局安装 cloud-cli

> npm install -g cloud-cli

检测是否安装成功，有版本号输出则说明安装成功（这里是大写的V） 

> cloud -V


二、下载vue项目模板

> cloud init vue-template <my-project>


三、项目目录结构

```
├─build                  # webpack相关配置
│  └─config       
└─src                    # 源码目录
    ├─assets             # 静态资源文件
    │  ├─fonts           # font字体，这里默认放置iconfont.cn生成的全部文件
    │  ├─images   
    │  │  ├─copyfiles    # 目录下的文件并不会被webpack处理，它们会直接复制到最终的打包目录（默认是dist/assets/images/copyfiles），而其他的静态资源都会被webpack处理解析为模块依赖
    │  │  └─sprites      # 如果icon不能生成字体，则放在这里最终生成雪碧图（_sprites.png）
    │  ├─js              # 不通过npm下载的第三方js文件      
    │  └─less            # 样式文件
    └─modules            # 项目的功能模块及业务模块
        ├─component      # 组件，统一采用大驼峰或用中划线连接，如SendCode或send-code
        │  └─SendCode
        ├─filters        # 过滤器
        ├─lang           # 国际化或者用于文字提示
        ├─router  
        ├─services       # 服务请求（针对于axios的封装）
        └─views          # 页面page，统一采用小驼峰（切记），helloWorld
            └─helloWorld               
```

四、项目模板`package.json dependencies`内置常用的工具集

- [magicless](https://github.com/cklwblove/magicless) less的mixins集合

- [cloud-utils](https://github.com/cloud-templates/cloud-utils) 常用的js工具类函数集，详见[api具体说明文档](https://cloud-templates.github.io/cloud-utils/)

五、项目中尽量用**绝对路径**，这里搭配webapck自带的**路径别名**及**文件别名**可以很容易实现这一点，并且写法很简单。具体如下（webpack.base.js）：

```
  alias: {
        'vue$': 'vue/dist/vue.esm.js',
        '@': utils.resolve('src'),
        '@assets': utils.resolve('src/assets'),
        '@less': utils.resolve('src/assets/less'),
        '@js': utils.resolve('src/assets/js'),
        '@components': utils.resolve('src/modules/components'),
        '@mixins': utils.resolve('src/modules/mixins'),
        '@views': utils.resolve('src/modules/views'),
  
        // 项目公用
        'services': utils.resolve('src/modules/services'),
        'lang': utils.resolve('src/modules/lang/zh-cn'),
        'variable': utils.resolve('src/assets/less/variable.less'),
        'utils': utils.resolve('node_modules/cloud-utils/dist/cloud-utils.min'),
        'mixins': utils.resolve('node_modules/magicless/magicless.less')
      }
```

- 在**vue**文件的用法：

```
<template>
  <div class="page page-hello">
    <!-- 静态资源路径写法事例 -->
    <img src="~@assets/images/copyfiles/logo.png">
    <p></p>
    <!-- 组件用法 -->
    <send-code class="button button-default" v-model="start" @click.native="handleSendCode"></send-code>
  </div>
</template>

<script>
  /**
   * 以下仅为事例代码，可以随意扩展修改
   */

  // 工具类
  import {formatTime} from 'utils';
  // 组件
  import SendCode from '@components/SendCode';
  // 服务
  import {login} from 'services';

  export default {
    data() {
      return {
        msg: 'Welcome to Your Vue.js App',
        start: false
      }
    },
    methods: {
      handleLogin() {
        login({
          username: 'demo',
          password: '123456'
        }).then((res) => {
          this.message = formatTime(res && res.login_time);
        });
      },
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



