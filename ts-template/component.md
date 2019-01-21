### 示例代码

```js
<!--Component-->
<script lang="ts">
  import { Component, Prop, Vue } from 'vue-property-decorator';

  @Component
  export default class MyComponent extends Vue {

    // prop
    @Prop({ type: String, default: '' })
    name: string;

    @Prop({ default: 0 })
    propA: number;

    @Prop({ type: [ Number, String ], default: 16 })
    size: number | string;

    // data
    title: string = '您好';
    
    dataA: string = 'test';
  }
</script>
```
