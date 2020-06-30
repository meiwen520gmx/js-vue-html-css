<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
    <!-- 一个自定义组件，渲染后包含两个输入框，如果需要让这个自定义组件支持v-model='value'指令实现双向数据绑定，
    绑定到value的值是2个文本框的输入文本，通过使用一个空格拼接起来的字符串，如果定义该组件？ 解法如下：-->
    <!-- :value="text"
        @input="text=$event.target.value"
        v-model="text"相当于传递给当前组件一个叫做value的属性
        给当前组件设置了一个自定义事件 input  $emit("input", "xxx") -->
    <my-input v-model="text"></my-input>
    <h2>{{text}}</h2>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'
import MyInput from '@/components/Input'

export default {
  name: 'Home',
  data() {
    return {
      text: "高级前端 哈哈哈哈"
    }
  },
  mounted () {
    setTimeout(() => {
      this.text = "哈哈哈 嘻嘻嘻"
    },1000);
  },
  components: {
    HelloWorld,
    MyInput
  }
}
</script>
