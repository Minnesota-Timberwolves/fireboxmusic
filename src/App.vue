<template>
  <div id="app">
    <!-- <img alt="Vue logo" src="./assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/> -->
    <div class="app-player">
      
      <div class="app-play-header">
        <MusicHeader @getlist='handleGetMusic'></MusicHeader> 
      </div>
      <div class="app-play-content">
        <MusicContent :songList=songList @geturl='handleGetSongUrl' @getmv='handleGetMvUrl' :is-audio-playing='isAudioPlaying'></MusicContent>
      </div>
      <div class="app-play-foot">
         <MusicFooter :song-url='songUrl' @pause='handlePause' @play="palymiusc"></MusicFooter>
      </div>
    </div>
      <div :class="{hidden:hiddenVedio}">
    <div class="app-player-close" @click='handleCloseMV'>X</div>
    <div class="app-player-video">
      <video :src="mvUrl"
       ref="vedio" 
       controls="controls"
       loop="loop"
      autoplay="autoplay"></video>
    </div>
    <div class="app-mark"></div>
       </div>
  </div>
</template>

<script>
// import HelloWorld from './components/HelloWorld.vue'
import MusicHeader from './components/music-header'
import MusicContent from './components/music-content'
import MusicFooter from './components/music-footer'
export default {
  name: 'App',
  data:function(){
     return {
        songList:[],
        songUrl:"",
        mvUrl:'',
        hiddenVedio:true,
        isAudioPlaying:false
     }
  },
  watch:{
    isAudioPlaying(n){
      this.isAudioPlaying=n
    }
  },

  
  components: {
    // HelloWorld
    MusicHeader,
    MusicContent,
     MusicFooter

  },
  methods:{
    handleGetMusic:function(songList){
        this.songList=songList
    },
    handleGetSongUrl:function(songUrl){
      this.songUrl=songUrl
      this.isAudioPlaying= true

    },
  
    handleGetMvUrl:function(mvUrl){
      this.mvUrl=mvUrl
      this.hiddenVedio=false
    },
    handleCloseMV:function(){
      this.hiddenVedio=true
      this.$refs.vedio.pause()
    },
    handlePause:function(isAudioPlaying){
      this.isAudioPlaying=isAudioPlaying
    },
      palymiusc(isAudioPlaying){
      this.isAudioPlaying= isAudioPlaying
    },
  }
}
</script>

<style>
#app {
  /* font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px; */

      background:url(./assets/bg.png) no-repeat;
      width: 100vw;
      height: 100vh;
      background-size:100% 100% ;
      display: flex;
      align-items: center;
      justify-content: center;
      --themeColor:#C20C0C


}

*{
  padding: 0;
  margin: 0;
  box-sizing: border-box;
}
.app-player{
  width: 800px;
  height: 600px;
  border-radius: 10px;
}
  .app-play-header {
    height: 60px;
    width: 100%;
    background: var(--themeColor)
  }

  .app-play-content {
      height: 480px;
      width: 100%;
      display: flex;
      border-bottom: 1px solid var(--themeColor);
      background: rgba(255, 255, 255, 0.5);
  }

  .app-play-foot {
      background:white;
      height: 60px;
      widows: 100%;
  
}
  .app-mark{
    height: 100vh;
    width: 100vw;
    background: black;
    position: fixed;
    left: 0;
    top: 0;

  }
  .app-player-video{
    transform: translate(-50%,-50%);
    left: 50%;
    top: 50%;
    position: fixed;
    z-index: 1000;
    width: 600px;
    height: 800px;
    background: white;
  }
  video{
     width: 600px;
    height: 800px;
  }
  .app-player-close{
    position: fixed;
    z-index: 1001;
    top: 10px;
    right: 10px;
    color: white;
  }
 

  .hidden{
    display: none;
  }
</style>
