<template>
  <div id="carousel" class="carousel" @mouseover="isCarousel=true" @mouseout="isCarousel=false">
    <div class="carousel-inner">
      <transition
        name="carousel-animates"
        enter-class="carousel-animate-enter"
        leave-class="carousel-animate-leave"
        :leave-active-class="leaveToClass"
      >
        <template>
          <div class="item" :key="active">
            <a href="javascript:;" title="">
              <img :src="carousel[active] || carousel[active].imgUrl" alt>
							<!-- {{carousel[active].image}} -->
            </a>
          </div>
        </template>
      </transition>
    </div>
    <ol class="carousel-indicators">
      <li
        @click="move(active>index?-1:1,index)"
        v-for="(item,index) in carousel"
        :key="index"
        :class="index===active?'item active':'item'"
      >
      <!-- {{index+1}} -->
      </li>
    </ol>
    <!-- <a @click="move(-1)" class="left carousel-control">
      <i class="iconfont icon-zuo"></i>
    </a>
    <a @click="move(1)" class="right carousel-control">
      <i class="iconfont icon-you"></i>
    </a> -->
  </div>
</template>
<script>
export default {
  name: "swiper",
  data() {
    return {
      //轮播内容
      active: -1, //当前轮播图位置
			//carousel: this.carousels, //轮播内容
      times: null, //定时器
      isCarousel: false, //是否鼠标移入暂停轮播
      leaveToClass: "", //轮播图片离开时的动画，不同方向，动画不同
      interval: 3000 //每张图片的间隔空隙
    };
	},
	props: {
		carousel: {
			type: Array,
			default() {
				return []
			}
		}
	},
	computed: {
    imgLen: function () {
      return this.carousel.length
    }
  },
  created: function() {
    //创建实例之后获取

    var images = [];
    for (var i = 0; i < this.imgLen; i++) {
      //预加载图片
      images[i] = new Image();
      images[i].src = this.carousel[i].imgURl;
    }

    if (
      this.carousel === undefined ||
      Object.prototype.toString.call(this.carousel) !== "[object Array]"
    ) {
      console.error("请正确设置您的轮播内容");
    } else if (this.imgLen < 1) {
      console.warn("轮播图少于一张，无法轮播");
    } else {
      this.carouselAnimate(); //开始轮播
    }
	},
	mounted() {
		console.log(this.imgLen)
	},
  methods: {
    carouselAnimate: function() {
      //轮播动画
      if (!this.isCarousel) {
        this.move(1);
      } //如果没有鼠标移入暂停轮播，则轮播下一张图
      setTimeout(this.carouselAnimate.bind(this), this.interval);
    },
    move: function(direction, index) {
      //direction为轮播方向，正数为右，负数为左。index为当前轮播图
      var num = this.active;
      num = index !== undefined ? index : num + direction;

      num =
        num >= this.imgLen
          ? 0
          : num < 0
          ? this.imgLen - 1
          : num;

      this.active = num;
      this.leaveToClass =
        direction > 0
          ? "carousel-animate-leave-to-left"
          : "carousel-animate-leave-to-right";
    }
  },
  destroyed: function() {
    //销毁组件时清除定时器
    clearTimeout(this.times);
  }
};
</script>
<style scoped>
.carousel {
  width: 100%;
  position: relative;
  overflow: hidden;
}
.carousel:hover .carousel-control.left {
  -webkit-transform: translateX(100%);
  -moz-transform: translateX(100%);
  -ms-transform: translateX(100%);
  -o-transform: translateX(100%);
  transform: translateX(100%);
}
.carousel:hover .carousel-control.right {
  -webkit-transform: translateX(-100%);
  -moz-transform: translateX(-100%);
  -ms-transform: translateX(-100%);
  -o-transform: translateX(-100%);
  transform: translateX(-100%);
}
.carousel-inner {
  position: relative;
  overflow: hidden;
  width: 100%;
  font-size: 0;
}
.carousel-inner > .item {
  position: relative;
  -webkit-transition: all 0.5s ease-in-out;
  -o-transition: all 0.5s ease-in-out;
  transition: all 0.5s ease-in-out;
}
.carousel-inner > .item > img,
.carousel-inner > .item > a > img {
  width: 100%;
  height: auto;
}
.carousel-indicators {
  position: absolute;
  bottom: 10px;
  left: 50%;
  z-index: 15;
  list-style: none;
  text-align: right;
  transform: translateX(-50%);
  -ms-transform: translateX(-50%);
}
.carousel-indicators li {
  display: inline-block;
  width: 8px;
  height: 8px;
  /* line-height: 8px; */
  margin: 0 5px;
  font-size: 0;
  /* color: #fff; */
  background-color: #000 \9;
  background-color: rgba(255, 255, 255, 0.2);
  /* border: 1px solid rgba(0, 0, 0, 0.6); */
  cursor: pointer;
  /* text-align: center; */
  -webkit-transition: all 0.5s ease-in-out;
  -o-transition: all 0.5s ease-in-out;
  transition: all 0.5s ease-in-out;
  -moz-user-select: none;
  -webkit-user-select: none;
  -ms-user-select: none;
  -khtml-user-select: none;
  user-select: none;
}
.carousel-indicators li.active {
  background-color: rgba(255, 255, 255, 0.9);
}
.carousel-control {
  position: absolute;
  top: 50%;
  width: 80px;
  height: 160px;
  line-height: 160px;
  margin-top: -40px;
  color: #fff;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.6);
  background: rgba(0, 0, 0, 0.4);
  cursor: pointer;
  text-align: center;
  -webkit-transition: all 0.5s ease-in-out;
  -o-transition: all 0.5s ease-in-out;
  transition: all 0.5s ease-in-out;
}
.carousel-control .iconfont {
  font-size: 50px;
}
.carousel-control.left {
  left: -80px;
}
.carousel-control.right {
  right: -80px;
}
.carousel-control:hover,
.carousel-control:focus {
  outline: 0;
  color: #fff;
  text-shadow: 0 1px 2px rgba(0, 0, 0, 0.8);
  text-decoration: none;
  background: rgba(0, 0, 0, 0.6);
}
.carousel-animate-enter {
  opacity: 0;
  -webkit-transform: scale(0.8, 0.8);
  -moz-transform: scale(0.8, 0.8);
  -ms-transform: scale(0.8, 0.8);
  -o-transform: scale(0.8, 0.8);
  transform: scale(0.8, 0.8);
}
.carousel-animate-leave {
  position: absolute!important;
  top: 0;
  width: 100%;
}
.carousel-animate-leave-to-left,
.carousel-animate-leave-to-right {
  position: absolute!important;
  top: 0;
  width: 100%;
  z-index: 10;
}
.carousel-animate-leave-to-left {
  -webkit-transform: translateX(-100%) scale(0.8, 0.8);
  -moz-transform: translateX(-100%) scale(0.8, 0.8);
  -ms-transform: translateX(-100%) scale(0.8, 0.8);
  -o-transform: translateX(-100%) scale(0.8, 0.8);
  transform: translateX(-100%) scale(0.8, 0.8);
}
.carousel-animate-leave-to-right {
  -webkit-transform: translateX(100%) scale(0.8, 0.8);
  -moz-transform: translateX(100%) scale(0.8, 0.8);
  -ms-transform: translateX(100%) scale(0.8, 0.8);
  -o-transform: translateX(100%) scale(0.8, 0.8);
  transform: translateX(100%) scale(0.8, 0.8);
}
</style>
