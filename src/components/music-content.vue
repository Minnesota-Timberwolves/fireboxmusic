<!--  -->
<template>
  <div class="music-content">
    <div class="music-cnotent-left">
      <div class="content-left-list">
        <div class="left-list-item" v-for="item in songList" :key="item.id">
          <div class="list-item-left" @click="getSong(item.id)"></div>
          <div class="list-item-mid" v-text="item.name"></div>
          <div
            :class="[
              'list-item-right',
              { hidden: item.mvid === 0 ? true : false },
            ]"
            @click="getMV(item.mvid)"
          ></div>
        </div>
      </div>
    </div>
    <div class="music-content-mid">
      <img
        src="../assets/player_bar.png"
        alt=""
        :class="['player-bar', { playing: isAudioPlaying }]"
      />
      <img
        :src="imgUrl"
        alt=""
        class="cover"
      />
      <img
        src="../assets/disc.png"
        alt=""
        :class="['disc', { autorotate: isAudioPlaying }]"
      />
    </div>
    <div class="music-content-right">
      <h1>热门评论</h1>
      <div class="comment-list">
        <div
          class="comment-list-item"
          v-for="comment in hotComments"
          :key="comment.commentId"
        >
          <div class="list-item-left">
            <img :src="comment.user.avatarUrl" alt="" />
          </div>
          <div class="list-item-right">
            <div class="item-right-top">
              {{ comment.user.nikename }}
            </div>
            <div class="item-right-bottom">
              {{ comment.content }}
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
//这里可以导入其他文件（比如：组件，工具js，第三方插件js，json文件，图片文件等等）
//例如：import 《组件名称》 from '《组件路径》';

export default {
  //import引入的组件需要注入到对象中才能使用
  props: {
    songList: {
      type: Array,
      default: function () {
        return [];
      },
    },
    isAudioPlaying: {
      type: Boolean,
      default: false,
    },
  },
  components: {},
  data() {
    //这里存放数据
    return {
    //   AudioPlaying: this.isAudioPlaying,
      hotComments: [],
      imgUrl:''
    };
  },
  //监听属性 类似于data概念
  computed: {},
  //监控data中的数据变化
  watch: {},
  //方法集合
  methods: {
    getSong: async function (id) {
    //   this.AudioPlaying = true;
    
      let res = await this.$http.get("song/url", {
        params: {
          id,
        },
      });
      //   .then(res=>{
      //     //   console.log(res.data.data[0].url)
      //       let songUrl=res.data.data[0].url
      //       this.$emit('geturl',songUrl)
      //   })
      let songUrl = res.data.data[0].url;
      this.$emit("geturl", songUrl);
      //  第二次请求
      let res2 = await this.$http.get("comment/hot", {
        params: {
          type: 0,
          id: id,
        },
      });
      this.hotComments=res2.data.hotComments;
        
      let res3 =await this.$http.get("song/detail",{
          params:{
              ids:id
          }
      })
      this.imgUrl= res3.data.songs[0].al.picUrl
    
    

    },
    getMV: async function (mvid) {
      let res = await this.$http.get("mv/url", {
        params: {
          id: mvid,
        },
      });
      // console.log(res)
      let mvUrl = res.data.data.url;
      this.$emit("getmv", mvUrl);
    },
  },
  //生命周期 - 创建完成（可以访问当前this实例）
  created() {},
  //生命周期 - 挂载完成（可以访问DOM元素）
  mounted() {},
  beforeCreate() {}, //生命周期 - 创建之前
  beforeMount() {}, //生命周期 - 挂载之前
  beforeUpdate() {}, //生命周期 - 更新之前
  updated() {}, //生命周期 - 更新之后
  beforeDestroy() {}, //生命周期 - 销毁之前
  destroyed() {}, //生命周期 - 销毁完成
  activated() {}, //如果页面有keep-alive缓存功能，这个函数会触发
};
</script>
<style lang=less scoped>
@keyframes running {
  from {
    transform: rotate(0deg);
  }
  to {
    transform: rotate(360deg);
  }
}
.music-content {
  display: flex;
  .music-cnotent-left {
    width: 200px;
    border-right: 1px dotted var(--themeColor);
    height: 480px;
    .content-left-list {
      overflow-y: scroll;
      height: 480px;
      .left-list-item:nth-child(odd) {
        background: rgba(224, 217, 217, 0.5);
      }
      .left-list-item {
        display: flex;
        justify-content: space-between;
        align-items: center;
        .list-item-left {
          width: 25px;
          height: 25px;
          background: url(../assets/table.png) -18px -18px;
        }
        .list-item-mid {
          overflow: hidden;
          text-overflow: ellipsis;
          width: 150px;
          height: 30px;
          color: rgb(166, 44, 40);
          line-height: 30px;
          text-align: center;
        }
        .hidden {
          visibility: hidden;
        }
        .list-item-right {
          width: 30px;
          height: 20px;
          background: url(../assets/table.png) left -45px;
        }
      }
    }
  }

  .music-content-mid {
    width: 400px;
    border-right: 1px dotted var(--themeColor);
    height: 480px;
    position: relative;
    .cover {
      top: 156px;
      left: 156px;
      position: absolute;
      width: 150px;
      height: 150px;
    }
    .player-bar {
      left: 200px;
      position: absolute;
      z-index: 8;
      transform: rotate(-25deg);
      transform-origin: 12px 12px;
      transition: all 2s;
    }
    .disc {
      position: absolute;
      left: 100px;
      top: 100px;
    }
    .playing {
      transform: rotate(0);
    }
    .autorotate {
      animation-name: running;
      animation-duration: 5s;
      animation-timing-function: linear;
      animation-iteration-count: infinite;
    }
  }

  .music-content-right {
    width: 200px;
    height: 480px;
    h1 {
        font-size: 16px;
        font-weight: 500;   
    }
    .comment-list {
        height: 480px;
        overflow-y: scroll;
      .comment-list-item {
          display: flex;
        .list-item-left {
          img {
              width: 30px;
              height: 30px;
              border-radius: 50%;

          }
        }

        .list-item-right {
            padding: 5px;
          .item-right-top {
              font-size: 16px;

          }

          .item-right-bottom {
              color:white;
              font-size: 12px;
          }
        }
      }
    }
  }
}
</style>