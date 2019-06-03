<template>
  <!-- <div> -->
    <ul class="pages">
        <li
          class="paper"
          data-left
          @touchstart="leftTouchHandler($event)"
          @touchmove="leftMoveHandler($event)"
          @touchend="leftEndHandler($event)">
            <div class="page page-1-back">
                <img class="img_img" src="../assets/5.png" alt>
            </div>
            <div class="page page-1">
                <img class="img_img" src="../assets/6.png" alt>
            </div>
        </li>
        <!-- <li class="paper" data-right v-for="(item,index) in imgList2" :key="index">
            <div class="page page-2">
                <img class="img_img" :src="item.url" alt>
            </div>
            <div class="page page-2-back">
                <img class="img_img" :src="item.url" alt>
            </div>
        </li> -->
        <li
          class="paper"
          data-right
          @touchstart="leftTouchHandler($event)"
          @touchmove="rightMoveHandler($event)"
          @touchend="leftEndHandler($event)">
            <div class="page page-2">
                <img class="img_img" src="../assets/1.png" alt>
            </div>
            <div class="page page-2-back">
                <img class="img_img" src="../assets/2.png" alt>
            </div>
        </li>
        <!-- <li class="paper" data-right>
            <div class="page page-2">
                <img class="img_img" src="../assets/7.png" alt>
            </div>
            <div class="page page-2-back">
                <img class="img_img" src="../assets/8.png" alt>
            </div>
        </li> -->
    </ul>
  <!-- </div> -->
</template>

<script>
export default {
  data () {
    return {
      imgList1: [
        {
          url: require('../assets/1.png'),
        },
        {
          url: require('../assets/2.png'),
        },
        {
          url: require('../assets/3.png'),
        },
        {
          url: require('../assets/4.png'),
        }
      ],
      imgList2: [
        {
          url: require('../assets/1.png'),
        },
        {
          url: require('../assets/2.png'),
        },
        {
          url: require('../assets/7.png'),
        },
        {
          url: require('../assets/8.png'),
        },
        {
          url: require('../assets/6.png'),
        },
        {
          url: require('../assets/5.png'),
        }
      ]
    }
  },
  mounted () {
    this.goToNextPage()
  },
  methods: {
    // $('#next-page').addEventListener('click', goToNextPage);
    // const flipAnimateTime = 1000;
    goToNextPage () {
      let currentPage = 1;
      let $ = document.querySelector.bind(document);
        // 触发CSS动画
        // $('.paper[data-right]').setAttribute('data-begin-animate', true);
        setTimeout(() => {
          // data-right变成data-left
          let $rightPaper = $('.paper[data-right]'),
              $leftPaper = $('.paper[data-left]');
          $rightPaper.removeAttribute('data-right');
          $rightPaper.setAttribute('data-left', true);
          // data-left没有了
          // $leftPaper.removeAttribute('data-left');
          // $leftPaper.querySelector('.page-1').classList.remove('page-1');
          // $leftPaper.querySelector('.page-1-back').classList.remove('page-1-back');
          // 重新设置类的关系
          $leftPaper = $rightPaper;
          $rightPaper = $leftPaper.nextElementSibling;
          $leftPaper.querySelector('.page').classList.add('page-1-back');
          $leftPaper.querySelector('.page').classList.add('page-1');
          // 如果还有下一页
          if ($rightPaper) {
            $rightPaper.setAttribute('data-right', true);
            $rightPaper.querySelector('.page').classList.add('page-2');
            $rightPaper.querySelector('.page').classList.add('page-2-back');
          }
          currentPage++;
        }, 1000);
    },
    leftTouchHandler (e) {
      console.log(e)
    },
    leftMoveHandler (e) {
      let $ = document.querySelector.bind(document)
      $('.paper[data-left]').style.transformOrigin = 'left center'
      $('.paper[data-left]').style.animation = 'flip-to-left 2s ease-in-out forwards'
      console.log(e)
      // this.goToNextPage()
    },
    leftEndHandler (e) {
      console.log(e)
    },
    rightTouchHandler (e) {
      console.log(e)
    },
    rightMoveHandler (e) {
      console.log(e)
      this.goToNextPage()
    },
    rightEndHandler (e) {
      console.log(e)
    }
  }
}
</script>

<style scoped>
.img_img {
  width: 180px;
  height: 200px;
}
.page-1-back,
.page-2-back {
  transform: scale(-1, 1);
}
.page-1,
.page-2 {
  z-index: 1;
  transform: translateZ(1px);
}
.paper {
  display: none;
  position: absolute;
  transform-style: preserve-3d;
  position: absolute;
  left: 335px;
}
.page {
  position: absolute;
}
.paper[data-left],
.paper[data-right] {
  display: block;
  z-index: 1;
}
.paper[data-left] {
  z-index: 2;
}
.paper[data-right] + .paper {
  display: block;
}
@keyframes flip-to-left {
  from {
    transform: perspective(1000px) rotateY(0);
  }
  to {
    transform: perspective(1000px) rotateY(-180deg);
  }
}
.paper[data-left] {
  transform-origin: left center;
  animation: flip-to-left 2s ease-in-out forwards;
}
@keyframes flip-to-right {
  from {
    transform: perspective(1000px) rotateY(-180deg);
  }
  to {
    transform: perspective(1000px) rotateY(0deg);
  }
}
/* .paper[data-left] {
    transform-origin: left center;
    animation: flip-to-right 2s ease-in-out forwards;
} */
</style>
