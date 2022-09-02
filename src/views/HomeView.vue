<template>
  <div class="home">
      <el-input id="line4targeturl" @keydown.enter="short()"  v-model="input" placeholder="Please input targetUrl" />
      <el-input id="line4newurl" @keydown.enter="postShort()"  v-model="newUrl" placeholder="Please input wanted shortUrl" />
  </div>
  <el-link v-if="promision" @click="redirct()" type="success">{{newUrl}}</el-link>
  <el-button id="but" @click="changeColor()" key="success" type="success" circle>Change bkColor</el-button>
</template>

<script>
import {Check} from '@element-plus/icons-vue'
import axios from 'axios'
import {ElNotification} from "element-plus"
export default {
  name: 'HomeView',
  components:{
    Check,
  },
  data(){
    return{
      input:'',
      newUrl:'',
      promision:false,
    }
  },
  methods:{
    async short(){
      await axios.get('https://node-api-zeta.vercel.app/get',{params:{
        url: this.input
      }}).then((res)=>{
        this.newUrl=res.data.newurl
      })
      this.$copyText(this.newUrl).then(function (e) {
        ElNotification({
          title: 'Success',
          message: 'Copy success',
          type: 'success',
        })
        }, function (e) {
         ElNotification({
          title: 'error',
          message: 'Copy error', 
          type: 'error',
        })
        })
    },
    redirct(){
      window.open("http://"+this.newUrl)
    },
    async postShort(){
      const postUrl = 'https://node-api-zeta.vercel.app/post'
      // const postUrl = 'http://127.0.0.1/post'
      await axios.post(postUrl,{
        "url": this.input,
        "newurl": this.newUrl
      }).then((res)=>{
        console.log(res.data);
        this.promision = res.data.promision
        if(this.promision==true){
          this.newUrl=res.data.newurl
          console.log("this wanted url is available");
        }
        })
    },
    changeColor(){
      const color = ["#FF66FF","#CC33CC","#CC00FF","#FF33FF","#CC99FF","#9900CC","#FF00FF","#CC66FF","#CC33FF","#CC99CC","#990066","#993399","#CC66CC","#CC00CC","#663366","#CC0099","#990099"]
      //随机
      const fir = Math.floor(Math.random()*color.length+1)
      const sec = Math.floor(Math.random()*color.length+1)
      console.log(`linear-gradient(to bottom right, ${color[fir]}, ${color[sec]})`);
      document.getElementsByTagName("body")[0].style.backgroundImage=`linear-gradient(to bottom right, ${color[fir]}, ${color[sec]})`
      
    }
  }
}
</script>
<style scoped>
.home{
  display: flex;
  flex-direction:column;
}

.el-input{
  width: 300px;
}

#but{
  position: absolute;
  bottom: 0px;
  right:0px;
  margin: 30px;
}

input{
  opacity: 0;
  border:none
}
</style>
