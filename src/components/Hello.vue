<template>
  <div>
    {{ msg }}
    <ul>
      <li v-for="x in features" :key="x">{{ x }}</li>
      <li>
        <input type="text" placeholder="请输入" @keyup.enter="addFeatures" />
      </li>
      <li>{{ count }}</li>
      <li>{{ cube }}</li>
    </ul>
    <ul>
      <li v-for="(item, index) in list" :key="index">{{ item.name }}</li>
    </ul>
    <button @click="addList">增加</button>
  </div>
</template>

<script lang="ts">
import { Component, Prop, Vue, Emit, Watch } from "vue-property-decorator";

// 接口中只需定义结构，无需初始化
interface List {
  id: number;
  name: string;
}

@Component({
  // 等同于 @Prop（）
  props: {
    name: {
      type: String,
      default: "匿名"
    }
  }
})
export default class Hello extends Vue {
  // required:true,告诉的是vue这里一定要传参数，！是告诉TS的 目标对象不一样，都要写
  // TS 的 灵活性 msg = "bala" => 类型不一定声明，自动得知类型 类型推论
  // 类型检测:type:String : string 本质上是相互独立的,各写各的, 真正的传值检测是 vue 在进行 type:String 是js构造函数
  @Prop({ required: true, type: String }) private msg!: string;

  // ! 声明特性数组 -> !相当于 data 中的值
  // ! private -> 私有访问; protected -> 继承的类访问; public -> 都能访问; rendonly -> 只读属性,创建之初修改其值; 默认public
  private features = ["类", "空", "类型注解"]; //Array<string>  :string[]
  private list: List[] = [
    {
      id: 1,
      name: "数组",
    }
  ];
  // ! 方法声明 -> 以便当作回调函数使用
  // 对 ajax 的 入参返回 可做约束 更加清晰
  private addFeatures(e: any) {
    this.features.push(e.target.value);
    e.target.value = "";
  }

  // 相当于 计算属性
  get count() {
    return this.features.length;
  }
  get cube() {
    return this.features[1];
  }

  // !生命周期
  private created() {
    setTimeout(() => {
      this.features.push("我是creacted插入的");
    }, 3000);
  }
  // 派发事件
  //  `未指定事件名则函数名作为事件名 （addList -> add-list）
  @Emit()
  private addList(e: any){ //若无返回值形参将作为事件参数
    const item = {id: this.list.length+1,name: "字符串"}
    this.list.push(item)
    return item //返回值作为事件参数
  }
  // 监听
  @Watch('msg')
  private onValChange(newVal: string, oldVal: any){
    console.log("我装好了监听器"+ oldVal + "->" + newVal)
  }
  @Watch('list')
  private onValChange2(newVal: string, oldVal: any){
    console.log("我装好了监听器"+ oldVal + "->" + newVal)
  }
}
</script>
