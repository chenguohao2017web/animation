<template>
  <div class="home" ref="home">
    <div class="content">
      <div class="item" v-for="(item,index) of listArr">
        <div class="imgWrap">
          <img :src="item.imgUrl" alt="商品图片" class="img">
        </div>
        <button class="btn" @click="handleAddToShopCar(item.imgUrl)">加入购物车</button>
      </div>
    </div>
    <transition name="drop">
      <div class="shopWrapper" v-show="shopWrapperIsShow" ref="shopWrap" :class="{active:shopWrapperIsShow}"></div>
    </transition>
    <div class="footer">
      <span class="shopCarBg" v-show="shopCarNumsIsShow" ref="shopCarBg">{{filterShopCarNums}}</span>
    </div>
    <div class="buyBtn" @click="handleTack" v-show="canTack">结账</div>
    <transition>
    <div class="loading" v-show="loadingIsShow">loading....</div>
    </transition>
  </div>
</template>
<script>
export default {
  name: "Home",
  data() {
    return {
      shopWrapperIsShow: false,
      listArr: [
        {
          imgUrl: require("../../images/1.jpg")
        },
        {
          imgUrl: require("../../images/2.jpg")
        },
        {
          imgUrl: require("../../images/3.jpg")
        }
      ],
      shopCarNums: 0,
      canBuy: true,
      canTack: false,
      loadingIsShow: false,
      timer: null
    };
  },
  mounted() {
    // console.log("网页可见区域高", document.body.clientHeight);
    // console.log("网页可见区域高 (包括边线的高) ", document.body.offsetHeight);
    // console.log("网页正文全文高 ", document.body.scrollHeight);
    // console.log("屏幕分辨率的高 ", window.screen.height);
    // console.log("屏幕可用工作区宽度 ", window.screen.availHeight);
    window.addEventListener("scroll", () => {
      clearTimeout(this.timer);
      let wScrollY = window.scrollY;
      let wInnerH = window.innerHeight;
      let bScrollH = document.body.scrollHeight;
      if (wScrollY + wInnerH >= bScrollH) {
        this.loadingIsShow = true;
        this.timer = setTimeout(() => {
          this.loadingIsShow = false;
          this.listArr.push({
            imgUrl: require("../../images/3.jpg")
          });
          console.log("add how");
          console.log("succ");
        }, 2000);
      }
    });
  },
  computed: {
    shopCarNumsIsShow() {
      return this.shopCarNums > 0;
    },
    filterShopCarNums() {
      return this.shopCarNums + "0" + "%";
    }
  },
  methods: {
    handleAddToShopCar(imgUrl) {
      if (this.canBuy) {
        this.shopCarNums++;
        this.shopWrapperIsShow = true;
        const el = this.$refs.shopWrap;
        el.style.backgroundImage = `url(${imgUrl})`;
        el.style.backgroundClip = "content-box";
        el.style.backgroundSize = "100px 50px";
        this.canBuy = false;
        el.addEventListener("animationend", e => {
          this.shopWrapperIsShow = false;
          this.canBuy = true;
        });
      }
    },
    handleTack() {
      alert("进入结账界面");
    }
  },
  watch: {
    shopCarNums() {
      if (this.shopCarNums >= 10) {
        this.canTack = true;
      } else {
        this.canTack = false;
      }
    }
  }
};
</script>
<style lang="less" scoped>
.v-enter,
.v-leave-to {
  opacity: 0;
}
.v-enter-active,
.v-leave-active {
  transition: opacity 0.5s;
}
.loading {
  height: 50px;
  line-height: 50px;
  background: green;
  color: white;
}
.home:after {
  content: "";
  display: block;
  height: 50px;
}
.content {
  position: relative;
  .item {
    padding: 30px;
    .imgWrap {
      width: 100%;
      .img {
        width: 100%;
      }
    }
    .btn {
      padding: 10px;
    }
  }
}
.shopWrapper {
  position: fixed;
  width: 100px;
  height: 50px;
  top: 50%;
  left: 50%;
  transform: translate3d(-50%, -50%, 0);
  // background: red;
  background-clip: content-box;
  border: 2px solid green;
  padding: 10px;
  &.active {
    animation: drop 1s ease;
  }
  @keyframes drop {
    from {
      opacity: 1;
      transform: translate3d(-50%, -50%, 0) scale(1);
    }
    to {
      opacity: 0;
      transform: translate3d(-50%, 300%, 0) scale(0.5);
    }
  }
}
.footer {
  position: fixed;
  bottom: 0;
  width: 100%;
  height: 50px;
  background: #ccc;
  text-align: center;
  line-height: 50px;
}
.buyBtn {
  position: fixed;
  bottom: 0;
  width: 100px;
  height: 50px;
  background: yellow;
  transform: translateX(-50%);
  left: 50%;
}
</style>

