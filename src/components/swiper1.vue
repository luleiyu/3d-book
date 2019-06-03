<template>
<div class="book">
  <div
    class="page-box-public"
    :class="'page-box-' + (index+1)"
    v-for="(item,index) in imgList"
    :key="index"
    :style="{zIndex: item.zIndex}"
    ref="zIndex">
    <div class="page-public"
      @touchstart="startHandler"
      @touchmove="moveHandler"
      @touchend="endHandler">
      <div class="page-ahead">
        <img :src="item.url" alt="">
        <p class="page-p" v-if="index > 0">{{item.title}}</p>
      </div>
      <div class="page-after">
        <img :src="item.url1" alt="">
        <p class="page-p" v-if="index > 0">{{item.title}}</p>
      </div>
    </div>
  </div>
</div>
</template>

<script>
export default {
  data () {
    return {
      imgList: [
        {
          url:require('../assets/1.png'),
          url1:require('../assets/7.png'),
          title: '越是熟的朋友对话就越粗鲁，越是熟的朋友行为就越猥琐。'
        },
        {
          url:require('../assets/4.png'),
          url1:require('../assets/2.png'),
          title: '也许，我喜欢你，就是因为你是我认识的人中，唯一不可归类的人。'
        },
        {
          url:require('../assets/3.png'),
          url1:require('../assets/2.png'),
          title: '彼岸花。开一千年，落一千年，花叶永不相见，生生想错。'
        },
        {
          url:require('../assets/2.png'),
          url1:require('../assets/4.png'),
          title: '也许，我喜欢你，就是因为你是我认识的人中，唯一不可归类的人。'
        },
        {
          url:require('../assets/5.png'),
          url1:require('../assets/2.png'),
          title: '彼岸花。开一千年，落一千年，花叶永不相见，生生想错。'
        },
        {
          url:require('../assets/2.png'),
          url1:require('../assets/6.png'),
          title: '也许，我喜欢你，就是因为你是我认识的人中，唯一不可归类的人。'
        },
        {
          url:require('../assets/7.png'),
          url1:require('../assets/2.png'),
          title: '彼岸花。开一千年，落一千年，花叶永不相见，生生想错。'
        },
        {
          url:require('../assets/2.png'),
          url1:require('../assets/8.png'),
          title: '越是熟的朋友对话就越粗鲁，越是熟的朋友行为就越猥琐。'
        }
      ],
      zIndex: 0
    }
  },
  created () {
    this.imgList.forEach((item,index) => {
      item.zIndex = (this.imgList.length - index)
    })
  },
  methods: {
    startHandler (e) {
      console.log(e,'startHandler')
      this.startLocation = e.changedTouches[0].clientX
    },
    moveHandler (e) {
      e.stopPropagation()
      e.preventDefault()
      this.imgDeg = parseInt((this.startLocation - e.changedTouches[0].clientX) / 2)
      this._imgDeg = this.imgDeg
      if (this.imgDeg >= 0) {
        this.imgDeg = Math.abs(this.imgDeg)
        if (this.imgDeg >= 0 && this.imgDeg <= 180) {
          this.publicRotate(e,this.imgDeg)
        }
      } else {
        if (this.imgDeg <= 0 && this.imgDeg >= -180) {
          this.imgDeg = this.imgDeg + 180
          this.publicRotate(e,this.imgDeg)
        }
      }
    },
    endHandler (e) {
      if (this._imgDeg < 0 && this.imgDeg <= 135) {
        this.selfSetInterval(1,1,e,this.imgDeg)
      } else if (this._imgDeg < 0 && this.imgDeg > 135) {
        this.selfSetInterval(0,0,e,this.imgDeg)
      } else if (this._imgDeg > 0 && this.imgDeg >= 45) {
        this.selfSetInterval(0,0,e,this.imgDeg)
      } else if (this._imgDeg > 0 && this.imgDeg < 45) {
        this.selfSetInterval(0,1,e,this.imgDeg)
      }
    },
    publicRotate (e,deg,computedType,zIndex) {
      /**
       * 通过不同的点击获取到具体的标签，开始找子元素或者父元素，最终都是让
       * page-box-public
       * 进行翻转
       */
      // 判断 e.target.classList.value 是否有值 有值说明含有类名（class），没值就是标签
      if (e.target.classList.value) {
        if (e.target.classList.value.length == 11) {
          // 控制 page-public
          e.target.parentNode.style.transform = "rotateY(-" + deg + "deg)"
          e.target.parentNode.style.zIndex = zIndex
        } else if (e.target.classList.value == 'book' && e.target.classList.value.length == 4) {
          // 控制 book
          e.target.childNodes[0].style.transform = "rotateY(-" + deg + "deg)"
          e.target.childNodes[0].style.zIndex = zIndex
        } else if (e.target.classList.value === 'page-p') {
          // 控制 page-p
          e.target.parentNode.parentNode.parentNode.style.transform = "rotateY(-" + deg + "deg)"
          e.target.parentNode.parentNode.parentNode.style.zIndex = zIndex
        } else if (e.target.classList.value.length == 10) {
          // 控制 page-ahead 和 page-after
          e.target.parentNode.parentNode.style.transform = "rotateY(-" + deg + "deg)"
          e.target.parentNode.parentNode.style.zIndex = zIndex
        } else {
          // 控制 page-box-public
          e.target.style.transform = "rotateY(-" + deg + "deg)"
          e.target.style.zIndex = zIndex
        }
      } else {
        // 控制img标签
        e.target.parentNode.parentNode.parentNode.style.transform = "rotateY(-" + deg + "deg)"
        e.target.parentNode.parentNode.parentNode.style.zIndex = zIndex
        if (computedType) {
          if (e.target.classList.value == 'page-ahead') {
            e.target.parentNode.style.zIndex = zIndex
          }
        } else {
          if (e.target.classList.value == 'page-ahead') {
            e.target.parentNode.style.zIndex = zIndex
          }
        }
      }
    },
    selfSetInterval (endFlag,computedType,e,imgDeg) {
      let timeId = setInterval(() => {
        if (endFlag) {
          if (imgDeg <= 0) {
            this.publicRotate(e,imgDeg,computedType,++this.zIndex)
            clearInterval(timeId)
            return
          }
        } else {
          if (imgDeg >= 179) {
            console.log(e)
            this.publicRotate(e,imgDeg,computedType,++this.zIndex)
            clearInterval(timeId)
            return
          }
        }
        /**
         * computedType 0打开 1合上
         * 区分每页是打开还是合上。
         * 小于45度：1合上
         * 大于45度：0打开
         * 小于135度：1打开（准确的说是，翻开了一页，但是合上的角度小，就又平躺下来了）
         * 大于135：0合上
         */
        if (computedType) {
          // 合上
          --imgDeg
        } else {
          // 打开
          ++imgDeg
        }
        // computedType传过去这个值，可以知道，用户是左翻还是右翻
        this.publicRotate(e,imgDeg,computedType)
      },10)
    }
  }
}
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
}
body {
  background: #212121;
  perspective: 1000px;
}
.book {
  width: 800px;
  margin: 0 auto;
  position: relative;
  transform: rotateX(30deg);
  transform-style: preserve-3d;
}
.page-box-public {
  width: 600px;
  height: 400px;
  position: absolute;
  left: 0px;
  top: 0px;
}
.page-public {
  width: 300px;
  height: 400px;
  position: absolute;
  left: 50%;
  background: #fff;
  border: 1px solid #96a36e;
}
.page-p {
  font-size: 18px;
  margin-top: 20px;
  padding: 30px;
  text-align: left;
}
.page-ahead {
  position:absolute;
  top:0px;
  left:50px;
  z-index:1;
}
.page-after {
  position:absolute;
  top:0px;
  left:50px;
  z-index:1;
  transform: scale(-1, 1);
}
</style>
