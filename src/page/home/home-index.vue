<template>
  <div id="app">
    <!-- 固定头部 -->
    <div class="header"></div>
    <!-- 标题随轮播图动态切换 -->
    <div class="title">
      <img src="" alt="" style="width: 100%; height: 100%"  id="imgTitle"/>
    </div>
    <!-- 轮播图 -->
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <div class="swiper-slide" v-for="item in imgList" :key="item.id">
          <img
            :src="item.img"
            alt=""
            style="
              width: 7.92rem;
              object-fit: cover;
              margin-left: 40px;
              border-radius: 0.4rem;
            "
          />
        </div>
      </div>
      <!-- 如果需要导航按钮 -->
      <div class="left-button swiper-button-prev"></div>
      <div class="right-button swiper-button-next"></div>
    </div>
    <!-- 如果需要分页器 -->
    <div class="swiper-pagination"></div>
    <!-- 按钮 -->
    <div class="button" @click="skip">
      <span id="btnText"></span>
    </div>
  </div>
</template>

<script>
import Swiper from "swiper";
import "swiper/css/swiper.min.css";
import {sendMessage} from "@/util/initChat";
import vconsole from 'vconsole'
export default {
  data() {
    return {
      //轮播图的图片列表
      imgList: [
        {
          id: 1,
          img: require("./img/19.png"),
        },
        {
          id: 2,
          img: require("./img/14.png"),
        },
        {
          id: 3,
          img:require("./img/20.gif")
        },
      ],
    };
  },
  mounted() {
    new vconsole()
    console.log("测试1")
    this.swiper() //swiper配置项
  },
  methods: {
    //swiper轮播图配置项
    swiper(){
      new Swiper(".swiper-container", {
      direction: "horizontal", // 垂直切换选项
      loop: true, // 循环模式选项
      autoplay: {
        disableOnInteraction: false,
        delay:5000
      },
      parallax: true,
      navigation: {
        prevEl: ".swiper-button-prev",
        nextEl: ".swiper-button-next",
      },
      pagination: {
        el: ".swiper-pagination",
        clickable: true,
      },
      on: {
        //拖动完成时触发
        slideChangeTransitionEnd: function () {
          let text = document.getElementById("btnText");
          let title = document.getElementById("imgTitle")
          if (this.activeIndex == 3 || this.activeIndex == 0) {
            text.innerText = "一键变身 >";
            title.src = require("./img/10.png")
          } else if (this.activeIndex == 2) {
            text.innerText = "一键解锁 >";
            title.src = require("./img/13.png")
          } else {
            text.innerText = "晒一晒 >";
            title.src = require("./img/2.png")
          }
        },
        //为避免拖动轮播图后不生效的情况,设置touchEnd方法(在触摸的最后阶段在获取一次当前的索引)以避免
        touchEnd:function(){
          let text = document.getElementById("btnText");
          let title = document.getElementById("imgTitle")
          if (this.activeIndex == 3 || this.activeIndex == 0) {
            text.innerText = "一键变身 >";
            title.src = require("./img/10.png")
          } else if (this.activeIndex == 2) {
            text.innerText = "一键解锁 >";
            title.src = require("./img/13.png")
          } else {
            text.innerText = "晒一晒 >";
            title.src = require("./img/2.png")
          }
        }
      },
    });
    },
    //按钮点击跳转
    skip() {
        let text = document.getElementById("btnText").innerText
        if(text == "一键变身 >"){
           sendMessage("openTemplateGroup",{
            id:"43",
            name:"ai绘画"
           })
        }else if(text == "一键解锁 >"){
           sendMessage("openTemplateGroup",{
            id:"49",
            name:"姓氏头像"
           })
        }else{
          sendMessage("openTemplateGroup",{
            id:"48",
            name:"全家福"
          })
        } 
    },
  },
};
</script>

<style scoped>
#app {
  width: 100%;
  height: 23.1633rem;
  background-image: url("./img/5.png");
  background-size: 100% 100%;
  background-repeat: no-repeat;
}
.header {
  width: 7.32rem;
  height: 2.7067rem;
  background-image: url("./img/11.png");
  background-repeat: no-repeat;
  background-size: 100% 100%;
  margin: 0 auto;
  transform: translateY(1.4667rem);
}
.title {
  width: 6.64rem;
  height: 1.92rem;
  margin: 1.3333rem auto;
}
#imgReview {
  width: 100%;
  height: 100%;
}
.swiper-container {
  width: 100%;
  height: 10.4667rem;
}
.left-button {
  width: 1.04rem;
  height: 1.24rem;
  background-image: url("./img/8.png");
  background-repeat: no-repeat;
  background-size: 100% 100%;
}
.right-button {
  width: 1.04rem;
  height: 1.24rem;
  background-image: url("./img/7.png");
  background-repeat: no-repeat;
  background-size: 100% 100%;
}
.button {
  width: 8.3333rem;
  height: 3.04rem;
  background-image: url("./img/1.png");
  background-repeat: no-repeat;
  background-size: 100% 100%;
  margin: 0 auto;
  text-align: center;
  line-height: 2.85rem;
  font-size: 0.5333rem;
  color: #272727;
  font-weight: bold;
  animation: transform infinite 3s;
}
@keyframes transform {
  0% {
    transform: translateY(0);
  }
  50% {
    transform: translateY(0.2667rem);
  }
  100% {
    transform: translateY(0);
  }
}
</style>

<style>
* {
  margin: 0;
  padding: 0;
  list-style: none;
}
body {
  width: 100%;
  height: 100%;
  position: relative;
}
.swiper-button-prev:after {
  content: "";
}
.swiper-button-next:after {
  content: "";
}
.swiper-button-next {
  right: -5px;
}
.swiper-button-prev {
  left: -5px;
}
.swiper-pagination-bullet {
  width: 0.32rem;
  height: 0.32rem;
  margin-left: 0.2133rem;
}
.swiper-pagination-bullet-active {
  background: #fff;
}
.swiper-pagination {
  position: relative;
  margin-top: 0.1333rem;
}
</style>
