<script setup>
import { ref } from 'vue'
import { QuestionCircleOutlined, PlusOutlined, PauseOutlined,QuestionOutlined } from '@ant-design/icons-vue';
defineProps({
  msg: String
})

const count = ref(0)

</script>

<template>
  <h2>{{ msg }}</h2>
  <div class="card">
    <question-circle-outlined />
    <plus-outlined />
    <question-circle-outlined />
    <pause-outlined rotate="90" />
    <question-outlined />
  </div>
  <a-button type="primary" @click="handleReset">Change</a-button>
  <div class="card">
    <span>time cost: {{count}} seconds</span>
    <a-row :gutter="[8,8]">
      <a-col v-for="(item,i) in list" :key="i" :span="12">
        {{item.a}}  +  {{item.b}} =
        <a-input-number v-model:value="item.d" :data-idx="i" @change="handleChange"></a-input-number>
        <a-popover v-model="item.popover" title="Tips" trigger="click">
          <template #content>
            <p v-for="s in item.tips">{{s}}</p>
          </template>
          <a-alert :hidden="item.d == null" :message="item.c == item.d? 'Success':'Error'" :type="item.c == item.d? 'success':'error'" show-icon style="display: inline-flex;height: 24pt;whiteSpace: 'pre-wrap';"/>
        </a-popover>
      </a-col>
    </a-row>
    
  </div>
  <p>

  </p>
</template>
<script>

export default {
  data(){
    return{
      list:[],
      n:222,
    }
  },
  filters:{
    fmtTips(val){
      return val.join('\r\n')
    },
  },
  mounted(){
    this.init()
    this.intvl = setInterval(()=>{
      this.count ++
    }, 1000)
  },
  methods:{
    init(){
      let data =[]
      for (let i = 0; i < 10; i++) {
        let a = Number.parseInt(Math.random() * 10)
        let b = Number.parseInt(Math.random() * 10)
        let c = a + b
        let tips = this.makeTip(a,b)
        data.push({a:a,b:b,c:c,d:null,tips:tips,popover:false})
      }
      this.list = data
    },
    makeTip(a, b){
      if(a+b>10){
        if(a>b){
          let x = 10 - a
          let y = b - x
          let s1 = `${a} + ${x} = 10`
          let s2 = `${b} = ${x} + ${y}`
          let s3 = `${a} + ${x} + ${y} = 10 + ${y} = ${a + b}`
          return [s1, s2, s3]
        }else if(a<b){
          let x = 10 - b
          let y = a - x
          let s1 = `${b} + ${x} = 10`
          let s2 = `${a} = ${x} + ${y}`
          let s3 = `${b} + ${x} + ${y} = 10 + ${y} = ${a + b}`
          return [s1, s2, s3]
        }else{
          return [`${a} × 2 = ${a*2}`]
        }
      }else{
        return [`It's too sample! You can fix it.`]
      }
    },
    handleBlur(evt){
      let idx = evt.target.dataset.idx
      let item = this.list[idx]
      console.log(idx, item)
      if (item.a + item.b == item.d) {
        //this.$notification.success({message:'yes'})
      }else{
        if(item.tips && item.c > 10){
          this.$notification.info({message:`Tips`,description:item.tips.join('\r\n'),style:{whiteSpace:'pre-wrap'}})
        }
      }
    },
    handleChange(val){
      console.log(val)
    },
    handleReset(){
      this.count = 0
      this.init()
    },
  }
}
</script>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>