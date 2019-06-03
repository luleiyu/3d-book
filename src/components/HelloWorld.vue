  <template>
  <div id="wraper">
    <div id="content">
      <div class="page" id="page1">
        page1
      </div>
      <div class="page" id="page2">
        page2
      </div>
      <!-- <div class="page" id="page3">
        page3
      </div> -->
    </div>
  </div>
</template>
<script>
export default {
  mounted () {
    // this.moveRight()
    var page1 = document.getElementById('page1'),//滑动item
        page2 = document.getElementById('page2'),
        page3 = document.getElementById('page3'),
        wraper = document.getElementById('wraper'),
        content = document.getElementById('content');//滑动容器
    var itemHeight = wraper.offsetWidth;//滑动item高度
    console.log(itemHeight)
    var index = 0;//滑动计数
    var moveX,//滑动距离
        endX,//滑动停止的Y坐标
        startX;//滑动开始的Y坐标
    // content.style.height = (itemHeight * 3) + 'px'
    // 触摸开始
    function boxTouchStart(e) {
      console.log(e)
      var touch = e.touches[0];
      console.log(touch)
      startX = touch.pageX;
      endX = content.style.webkitTransform;
      console.log(endX,'endX')
      if(!endX){
        endX = 0;
      }else{
        endX = parseInt(endX.replace('translateX(',''))
      }
    }

     // 触摸结束
    function boxTouchEnd(e) {
      console.log(e)
    }
    // 触摸移动
    function boxTouchMove(e) {
      var touch = e.touches[0];
      moveX = touch.pageX - startX;
      index = Number(e.target.id.split('page')[1])

        //下一页
      // if(moveX < 0) {
      //   if(index == 3) {
      //     return false;
      //   }
      //   content.style.webkitTransform = 'translateX(-'+(itemHeight*index)+'px)'
      // }
      // //上一页
      // else if(moveX > 0) {
      //   if(index == 1) {
      //     return false;
      //   }
      //   content.style.webkitTransform = 'translateX('+(itemHeight+endX)+'px)'
      // }

    }
    content.addEventListener('touchstart', boxTouchStart, false )
    content.addEventListener('touchmove', boxTouchMove, false)
    content.addEventListener('touchend', boxTouchEnd, false)
  },
  methods: {
    moveRight () {
      var ele = document.getElementsByClassName("img-box")[0];
        var beginX, beginY, endX, endX, swipeLeft, swipeRight;
        ele.addEventListener('touchstart', function (event) {
            event.stopPropagation();
            event.preventDefault();
            beginX = event.targetTouches[0].screenX;
            beginY = event.targetTouches[0].screenY;
            swipeLeft = false, swipeRight = false;
        });

        ele.addEventListener('touchmove', function (event) {
            event.stopPropagation();
            event.preventDefault();
            endX = event.targetTouches[0].screenX;
            endX = event.targetTouches[0].screenY;
            // 左右滑动
            if (Math.abs(endX - beginX) - Math.abs(endX - beginY) > 0) {
                /*向右滑动*/
                if (endX - beginX > 0) {
                    swipeRight = true;
                    swipeLeft = false;
                }
                /*向左滑动*/
                else {
                    swipeLeft = true;
                    swipeRight = false;
                }
            }
            else if(Math.abs(endX - beginX) - Math.abs(endX - beginY) < 0)
            {
                // 上下滑动
            }
        });
        ele.addEventListener('touchend', function (event) {
            event.stopPropagation();
            event.preventDefault();

            if (Math.abs(endX - beginX) - Math.abs(endX - beginY) > 0) {
                event.stopPropagation();
                event.preventDefault();if (swipeRight) {
                    swipeRight = !swipeRight;
                    /*向右滑动*/
                }
                if(swipeLeft) {
                    swipeLeft = !swipeLeft;
                    /*向左滑动*/
                }
            }
        });
    }
  }
}
</script>

<style scoped>
  html,body,h2 {
    margin: 0;
    padding: 0;
  }
  html,body,#wraper,#content {
    width: 100%;
    height: 100%;
  }
  #wraper {
    overflow: hidden;
  }
  #content {
    transition: all 1s linear;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    width: 100%;
  }
  .page {
    width: 300px;
    height: 350px;
  }
  #page1 {
    background: #f00
  }
  #page2 {
    background: #0f0;
    transform-style: preserve-3d;
    perspective: 1px;
    transform-origin: left top;
    transform:rotate3d(0,180,0,100deg);
  }
  #page3 {
    background: #00f
  }
</style>
