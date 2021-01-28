<!--  -->
<template>
<div class='music-header'>
    <div class="music-header-left">
        火线音乐盒
    </div>
    <div class="music-header-right">
        <input type="text" v-model="inputVal" @keydown.enter="handleSave">
    </div>
</div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
//import引入的组件需要注入到对象中才能使用
components: {},
data() {
//这里存放数据
return {
 inputVal:''
};
},
//监听属性 类似于data概念
computed: {},
//监控data中的数据变化
watch: {},
//方法集合
methods: {
 handleSave:async function(){
     if(!this.inputVal){
         return false
     }
    //  this.$http.get("https://apimusic.linweiqin.com/search",{
    //      params:{
    //          keywords:this.inputVal
    //      }
    //  }).then(res=>{
    //      console.log(res)
    //  })
     
     let res=await this.$http.get("search",{
         params:{
             keywords:this.inputVal
         }
     })
    this.$emit('getlist',res.data.result.songs)
 }
},
//生命周期 - 创建完成（可以访问当前this实例）
created() {

},
//生命周期 - 挂载完成（可以访问DOM元素）
mounted() {
    // window.onload=function(){
    //     this.handleSave()
    // }

},
beforeCreate() {}, //生命周期 - 创建之前
beforeMount() {}, //生命周期 - 挂载之前
beforeUpdate() {}, //生命周期 - 更新之前
updated() {}, //生命周期 - 更新之后
beforeDestroy() {}, //生命周期 - 销毁之前
destroyed() {}, //生命周期 - 销毁完成
activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
}
</script>
<style lang="less" scoped>
.music-header {
    display: flex;
    justify-content: space-between;
    padding: 5px;
    align-items: center;
    height: 60px;
  .music-header-left {
     color: white;
  }

  .music-header-right {
    input {
        width: 300px;
        height: 30px;
        border: 0px;
        border-radius: 20px;
        text-indent: 15px;
        background: url(../assets/zoom.png) 270px center no-repeat white;
    }
  }
}
</style>