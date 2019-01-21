### 示例代码
[Vue官网相关介绍](https://vuefe.cn/v2/guide/typescript.html)

```js
<!--view-->
<script lang="ts">
  import {Component, Vue, Prop, Watch} from 'vue-property-decorator';
  // 组件
  import Test from '@/components/test.vue';
  // mixins
  import Emitter from '@/mixins/emitter.ts';

  // 组件引用，mixins，filters 等放在 @Component 里面
  @Component({
    components: {
      Test
    },
    mixins: [ Emitter ]
  });
  export default class MyView extends Vue {
    // data
    // 用户名称
    nickName: string = '';

    // computed
    get fullName() {
      return 'Li' + this.nickName;
    }

    // watcher
    @Watch('child')
    onChildChanged (val: string, oldVal: string) {}

    // 生命周期钩子函数
    created(): void {
      console.log(`created~`);
    }

    mounted(): void {
      console.log(`mounted~`);
    }

    // method
    initData() {
      console.log('method~');
    }
    
  }
</script>

<!--解析后对应的js代码-->
<script>
  // 组件
  import Test from '@/components/test.vue';
  // mixins
  import Emitter from '@/mixins/emitter';

  export default {
    mixins: [Emitter],
    
    components: {
      Test
    },

    data() {
      return {
        nickName: ''
      }
    },

    computed: {
      fullName() {
        return 'Li' + this.nickName;
      }
    },

    watch: {
      'child': {
        handler: 'onChildChanged',
        immediate: false,
        deep: false
      }
    },

    created() {
      console.log(`created~`);
    },

    mounted() {
      console.log(`mounted~`);
    },

    methods: {
      initData() {
        console.log('method~');
      },
      onChildChanged(val, oldVal) {
        console.log('onChildChanged~');
      }
    }
  }
</script>

```
