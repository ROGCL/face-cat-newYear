<template>
  <div id="app">
    <!-- 固定头部 -->
    <div class="header"></div>
    <!-- 标题随轮播图动态切换 -->
    <div class="title">
      <img src="" alt="" style="width: 100%; height: 100%" id="imgTitle" />
    </div>
    <!-- 轮播图 -->
    <div class="swiper-container">
      <div class="swiper-wrapper">
        <template v-if="system == 'android'">
          <div
            class="swiper-slide"
            v-for="item in androidImgList"
            :key="item.id"
            @click.native="turnNew(item.id)"
          >
            <img
              :src="item.imgs"
              alt=""
              style="
                width: 7.92rem;
                height: 100%;
                object-fit: scale-down;
                background-repeat: no-repeat;
                background-size: 100% 100%;
                margin-left: 40px;
                border-radius: 20px;
              "
              @click="turnNew(item.id)"
            />
          </div>
        </template>
        <template v-else>
          <div class="swiper-slide" v-for="items in iosImgList" :key="items.id">
            <img
              :src="items.imgs"
              alt=""
              style="
                width: 7.92rem;
                height: 100%;
                object-fit: scale-down;
                background-repeat: no-repeat;
                background-size: 100% 100%;
                margin-left: 40px;
                border-radius: 20px;
              "
            />
          </div>
        </template>
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
import { sendMessage, getDevice } from "@/util/initChat";
// import vconsole from "vconsole";
export default {
  data() {
    return {
      //android轮播图
      androidImgList: [
        {
          id: 1,
          imgs: "http://cdn.fwyouni.com/media/default/2301/12/1673510437_b366CsAG84.png",
        },
        {
          id: 2,
          imgs: "http://cdn.fwyouni.com/media/default/2301/12/1673510420_b2hTejGKCJ.png",
        },
        {
          id: 3,
          imgs: "http://cdn.fwyouni.com/media/default/2301/12/1673510447_e3tRdC3hYF.gif",
        },
      ],
      //ios轮播图
      iosImgList: [
        {
          id: 1,
          imgs: "http://cdn.fwyouni.com/media/default/2301/12/1673510420_b2hTejGKCJ.png",
        },
        {
          id: 2,
          imgs: "http://cdn.fwyouni.com/media/default/2301/12/1673510437_b366CsAG84.png",
        },
      ],
      system: "", // 设备型号
    };
  },
  mounted() {
    // new vconsole();
    this.device(); //提前获取设备型号判定IOS || Android
    //异步获取，防止未获取到设备型号后就直接执行轮播图了
    setTimeout(() => {
      this.swiper(); //swiper配置项
    }, 100);
  },
  methods: {
    //获取设备型号
    device() {
      this.system = getDevice().system;
    },
    //swiper轮播图配置项
    swiper() {
      new Swiper(".swiper-container", {
        direction: "horizontal", // 垂直切换选项
        loop: true, // 循环模式选项
        autoplay: {
          disableOnInteraction: false,
          delay: 5000,
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
            let system = getDevice().system;
            let text = document.getElementById("btnText");
            let title = document.getElementById("imgTitle");
            if (system == "android") {
              if (this.activeIndex == 3 || this.activeIndex == 0) {
                text.innerText = "一键变身 >";
                title.src = require("./img/10.png");
              } else if (this.activeIndex == 2) {
                text.innerText = "一键解锁 >";
                title.src = require("./img/13.png");
              } else if (this.activeIndex == 1 || this.activeIndex == 4) {
                text.innerText = "晒一晒 >";
                title.src = require("./img/2.png");
              }
            } else if (system == "ios") {
              if (this.activeIndex == 1 || this.activeIndex == 3) {
                text.innerText = "一键解锁 >";
                title.src = require("./img/13.png");
              } else {
                text.innerText = "晒一晒 >";
                title.src = require("./img/2.png");
              }
            }
          },
          //为避免拖动轮播图后不生效的情况,设置touchEnd方法(在触摸的最后阶段在获取一次当前的索引)以避免
          touchEnd: function () {
            let text = document.getElementById("btnText");
            let title = document.getElementById("imgTitle");
            let system = getDevice().system;
            if (system == "android") {
              if (this.activeIndex == 3 || this.activeIndex == 0) {
                text.innerText = "一键变身 >";
                title.src = require("./img/10.png");
              } else if (this.activeIndex == 2) {
                text.innerText = "一键解锁 >";
                title.src = require("./img/13.png");
              } else {
                text.innerText = "晒一晒 >";
                title.src = require("./img/2.png");
              }
            } else {
              if (this.activeIndex == 1 || this.activeIndex == 3) {
                text.innerText = "一键解锁 >";
                title.src = require("./img/13.png");
              } else if (this.activeIndex == 2 || this.activeIndex == 0) {
                text.innerText = "晒一晒 >";
                title.src = require("./img/2.png");
              }
            }
          },
          click: function () {
            let system = getDevice().system;
            if (system == "android") {
              if (this.activeIndex == 3 || this.activeIndex == 0) {
                window.location.href =
                  "http://isubtitle.oss-cn-hangzhou.aliyuncs.com/media/page/painting_cat/index.html";
              } else if (this.activeIndex == 2) {
                sendMessage("openTemplateGroup", {
                  id: "49",
                  name: "姓氏头像",
                });
              } else {
                sendMessage("openTemplateGroup", {
                  id: "48",
                  name: "全家福",
                });
              }
            } else {
              if (this.activeIndex == 1 || this.activeIndex == 3) {
                sendMessage("openTemplateGroup", {
                  id: "49",
                  name: "姓氏头像",
                });
              } else if (this.activeIndex == 2 || this.activeIndex == 0) {
                sendMessage("openTemplateGroup", {
                  id: "48",
                  name: "全家福",
                });
              }
            }
          },
        },
      });
    },
    //按钮点击跳转
    skip() {
      let text = document.getElementById("btnText").innerText;
      if (text == "一键变身 >") {
        window.location.href =
          "http://isubtitle.oss-cn-hangzhou.aliyuncs.com/media/page/painting_cat/index.html";
        //  sendMessage("openTemplateGroup",{
        //   id:"43",
        //   name:"ai绘画"
        //  })
      } else if (text == "一键解锁 >") {
        sendMessage("openTemplateGroup", {
          id: "49",
          name: "姓氏头像",
        });
      } else {
        sendMessage("openTemplateGroup", {
          id: "48",
          name: "全家福",
        });
      }
    },
    //跳转新页面
    turnNew(id) {
      console.log(id);
    },
  },
};
</script>

<style scoped>
#app {
  width: 100%;
  height: 23.1633rem;
  /* height: 100vh; */
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
  margin-top: 0.2667rem;
}
</style>
