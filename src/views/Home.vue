<template>
  <div class="home">
    <!-- <img alt="Vue logo" src="../assets/logo.png" /> -->
    <!-- <HelloWorld msg="Welcome to Your Vue.js + TypeScript App" /> -->
    <Hello :msg="msg" @add-list="onAddList"></Hello>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import HelloWorld from "@/components/HelloWorld.vue"; // @ is an alias to /src
import Hello from "@/components/Hello.vue";

@Component({
  components: {
    HelloWorld,
    Hello
  }
})
export default class Home extends Vue {
  msg = "巴拉巴拉小魔仙"
  private onAddList(item: any){
    console.log(item.name)
    this.msg = "我被监听了"
  }
}
</script>
