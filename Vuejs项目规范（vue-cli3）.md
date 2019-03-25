# Vue项目规范(vue-cli3)

**注意：此项目规范适用于 `vue-cli3`，使用的项目脚手架是 [vue-preset](https://github.com/cloud-templates/vue-preset)**

一、首先全局安装 vue-cli

> npm install -g @vue/cli

检测是否安装成功，有版本号输出则说明安装成功

> vue --version

二、下载vue项目模板

> vue create --preset cloud-templates/vue-preset my-project

三、

```
.
├── build                # webpack 及 node 命令行工具相关配置
├── public               # 目录下的文件不会被webpack处理，而其他的静态资源都会被webpack处理解析为模块依赖
└── src
    ├── assets           # 静态资源文件
    │   ├── fonts        # font字体，这里默认放置iconfont.cn生成的全部文件
    │   ├── img
    │   └── less
    ├── components      # 统一采用大驼峰连接，如SendCode
    │   ├── SendCode    # tree shaking 
    │   └── global      # 全局组件
    ├── filters
    ├── icons           # vue-svgicon
    │   ├── components
    │   └── svg
    ├── router          # 过滤器
    ├── services        # 服务请求（针对于axios的封装）
    └── views           # 页面page，统一采用小驼峰（切记），helloWorld
        └── hello
```

四、项目模板`package.json dependencies`内置常用的工具集

- [magicless](https://github.com/cklwblove/magicless) less的mixins集合
- [cloud-utils](https://github.com/cloud-templates/cloud-utils) 常用的js工具类函数集，详见[api具体说明文档](https://cloud-templates.github.io/cloud-utils/)

五、项目中尽量用**绝对路径**，这里搭配webapck自带的**路径别名**及**文件别名**可以很容易实现这一点，并且写法很简单。具体如下（`vue.config.js`）：

```
  alias: {
    // 目录别名
    '@': utils.resolve('src'),
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

六、开发指南

##### 组件数据

组件的 data 必须是一个函数。

```js
// bad
export default {
  data: {
    foo: 'bar'
  }
}

// good
export default {
  data () {
    return {
      foo: 'bar'
    }
  }
}
```

#### 单文件组件文件名称

单文件组件的文件名应该要么始终是单词大写开头 (PascalCase)，要么始终是横线连接 (kebab-case)。

```
// bad
mycomponent.vue
myComponent.vue

// good
my-component.vue
MyComponent.vue
```

#### 紧密耦合的组件名

和父组件紧密耦合的子组件应该以父组件名作为前缀命名。

```
// bad
components/
|- TodoList.vue
|- TodoItem.vue
└─ TodoButton.vue

// good
components/
|- TodoList.vue
|- TodoListItem.vue
└─ TodoListItemButton.vue
```

#### 自闭合组件

在单文件组件中没有内容的组件应该是自闭合的。

```html
<!-- bad -->
<my-component></my-component>

<!-- good -->
<my-component />
```

#### Prop 名大小写

在声明 prop 的时候，其命名应该始终使用 camelCase，而在模板中应该始终使用 kebab-case。

```js
// bad
export default {
  props: {
    'greeting-text': String
  }
};

// good
export default {
  props: {
    greetingText: String
  }
}
```

```html
<!-- bad -->
<welcome-message greetingText="hi" />

<!-- good -->
<welcome-message greeting-text="hi" />
```

#### Props 换行

多个 Props 的元素应该分多行撰写，每个 Props 一行，闭合标签单起一行。

```html
<!-- bad -->
<my-component foo="a" bar="b" baz="c" />

<!-- good -->
<my-component
  foo="a"
  bar="b"
  baz="c"
/>
```

#### 指令缩写

指令缩写，用 `:` 表示 `v-bind:` ，用 `@` 表示 `v-on:`

```html
<!-- bad -->
<input
  v-bind:value="value"
  v-on:input="onInput"
>

<!-- good -->
<input
  :value="value"
  @input="onInput"
>
```

#### Props 顺序

标签的 Props 应该有统一的顺序，依次为指令、属性和事件。

```html
<my-component
  v-if="if"
  v-show="show"
  v-model="value"
  ref="ref"
  :key="key"
  :text="text"
  @input="onInput"
  @change="onChange"
/>
```

#### 组件选项的顺序

组件选项应该有统一的顺序。

```js
export default {
  name: '',

  mixins: [],

  components: {},

  props: {},

  data() {},

  computed: {},

  watch: {},

  created() {},

  mounted() {},

  destroyed() {},

  methods: {}
};
```

#### 组件选项中的空行

组件选项较多时，建议在属性之间添加空行。

```js
export default {
  computed: {
    formattedValue() {
      // ...
    },

    styles() {
      // ...
    }
  },

  methods: {
    onInput() {
      // ...
    },

    onChange() {
      // ...
    }
  }
};
```

#### 单文件组件顶级标签的顺序

单文件组件应该总是让顶级标签的顺序保持一致，且标签之间留有空行。

```html
<template>
...
</template>

<script>
/* ... */
</script>

<style>
/* ... */
</style>
```

**Note**: [Vuejs官方编码指南](https://cn.vuejs.org/v2/style-guide/index.html)

