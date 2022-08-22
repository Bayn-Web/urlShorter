<template>
  <div class="home">
      <el-input id="line4targeturl" @keydown.enter="short()"  v-model="input" placeholder="Please input targetUrl" />
      <el-input id="line4newurl" @keydown.enter="postShort()"  v-model="newUrl" placeholder="Please input wanted shortUrl" />
  </div>
  <el-link v-if="promision" @click="redirct" type="success">{{newUrl}}</el-link>
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
    short(){
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
      axios.get('/api/get',{params:{
        url: this.input
      }}).then((res)=>{
        this.newUrl=res.data.newurl
      })
    },
    redirct(){
      window.open("http://"+this.newUrl)
    },
    postShort(){
      axios.post('/api/post',{
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
</style>
