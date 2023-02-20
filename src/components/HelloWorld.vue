<template>
  <div class="hello">
    <div id="gundong" @scroll="scroll($event)" class="messageBox">
      <li :key="index" v-for="(item, index) in messagelist">
        <p>{{ ForTime(item,index) }}</p>
        <span v-if="item.loading">上传中</span>
        消息:{{ item.content }}
      
      </li>
    </div>
    <input type="file" name="image" accept="image/*" @change="upload($event)">
    <button @click="send()">发送</button>
  </div>
</template>

<script>
import {messageData,newMessage} from './data'
import {checkShowRule } from './js'
export default {
  name: 'HelloWorld',
  data() {
    return {
      messagelist: messageData,
      lastScrollHeight: 0
    }
  },
  mounted() {
    
    this.$nextTick(() => {
      let msg = document.getElementById('gundong') // 获取对象
      this.lastScrollHeight = msg.scrollHeight
      msg.scrollTop = msg.scrollHeight // 滚动高度
    })
  },
  methods: {
    upload(event) {
      var e = window.event || event;
      console.log(e)
      // 获取当前选中的文件
      var File = e.target.files[0];
      console.log(File);//打印值看下面图片,简单点的话我们直接把这个数据给后台处理就可以了
      const obj={
        file:File,
        type:'file',
        loading:true
      }
      setTimeout(() => {
        obj.loading=false;
        console.log(this.messagelist);
        this.$forceUpdate();
      }, 10000);
      console.log(this.messagelist);
      this.messagelist.push(obj)
      this.$nextTick(() => {
        let msg = document.getElementById('gundong') // 获取对象
        this.lastScrollHeight = msg.scrollHeight
        msg.scrollTop = msg.scrollHeight // 滚动高度
      })
    },

    send() {
      console.log(3333);
      this.messagelist = [...this.messagelist, '111']
      this.$nextTick(() => {
        let msg = document.getElementById('gundong') // 获取对象
        this.lastScrollHeight = msg.scrollHeight
        msg.scrollTop = msg.scrollHeight // 滚动高度
      })
    },
    scroll(event) {
      if (event.target.scrollTop == 0) {
        let msg = document.getElementById('gundong')

        const newdata = Array(20).fill('1');
        this.messagelist = [...newdata, ...this.messagelist]

        this.$nextTick(() => {
          console.log(msg.scrollHeight, this.lastScrollHeight);
          let scrollTop = msg.scrollHeight - this.lastScrollHeight
          msg.scrollTop = scrollTop
          this.lastScrollHeight = msg.scrollHeight
        })



      }
    },
    ForTime(item,index){
      console.log(checkShowRule(item,this.messagelist,'sendTime',index));
     return  checkShowRule(item,this.messagelist,'sendTime',index)
      // if(index!==0){
      //   const thisTime=new Date(item.sendTime);
      //   const lastTime=new Date(this.messagelist[index-1].sendTime);
      //   // console.log(thisTime-lastTime);
      // }else{
      //   return item.sendTime
      // }
    }
  }
}
</script>


<style scoped>
li{
  list-style: none;
}
.messageBox {
  width: 300px;
  height: 300px;
  border: 1px solid #000;
  overflow: auto;
}
</style>
