<template>
  <div class='match-team-style'>
    <div class='base-data'>
      <p>第一屏：基础数据</p>
      <div class='1'>
        <ul ref='u1' :style='movedist'>
          <li v-for='n in 100'>{{n}}</li>
        </ul>
      </div>
      <div class='2'>
        <ul>
          <li v-for='n in 100'>{{n}}</li>
        </ul>
      </div>
      <div class='3'>
        <ul ref='u3' :style='movedist3'>
          <li v-for='n in 100'>{{n}}</li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  components: {},
  data () {
    return {
      movedist: '',
      movedist3: ''
    }
  },
  mounted () {
    this.$nextTick(() => {
      this.moveDiv(this.$refs.u1, this.$refs.u3)
    })
  },
  methods: {
    moveDiv (dom1, dom2) {
      // 滑动dom1,滑动dom2,dom1和dom2有连锁滑动效果
      // self.movedist、self.movedist3是dom1和2的style变量
      let startY1 = ''
      let moveY1 = ''
      let movedis = 0
      let translates = 0
      let self = this
      dom1.addEventListener('touchstart', function (e) {
        startY1 = e.changedTouches[0].pageY  // 开始滑动的位置
      })
      dom1.addEventListener('touchmove', function (e) {
        moveY1 = e.changedTouches[0].pageY  // 移动时的实时位置
        movedis = translates + (moveY1 - startY1) // 移动的位移
        if (movedis > 0) {
          movedis = 0  // 当滑到顶部时，不能滑动
        } else if (movedis < -(dom1.offsetHeight - 300)) {
          movedis = -(dom1.offsetHeight - 300) // 当滑到底部时，不在滑动
        }
        self.movedist = `transform: translateY(${movedis}px)`
        self.movedist3 = `transform: translateY(${movedis}px)`
      })
      dom1.addEventListener('touchend', function (e) {
        translates = movedis  // 记录上一次移动的距离，用在下一次移动
      })
      let startY3 = ''
      let moveY3 = ''
      dom2.addEventListener('touchstart', function (e) {
        startY3 = e.changedTouches[0].pageY
      })
      dom2.addEventListener('touchmove', function (e) {
        moveY3 = e.changedTouches[0].pageY
        movedis = translates + moveY3 - startY3
        if (movedis > 0) {
          movedis = 0
        } else if (movedis < -(dom1.offsetHeight - 300)) {
          movedis = -(dom1.offsetHeight - 300)
        }
        self.movedist = `transform: translateY(${movedis}px)`
        self.movedist3 = `transform: translateY(${movedis}px)`
      })
      dom2.addEventListener('touchend', function (e) {
        translates = movedis
      })
    }
  }
}
</script>
<style lang='less'>
	.match-team-style{
     border:1px solid red;
     width: 100%;
     height: 100%;
     >div{
      border:1px solid yellow;
      height: 100%;
      >div{
        margin-top: 100px;
        width:80px;
        border:1px solid blue;
        display: inline-block;
        height: 300px;
        overflow: hidden;
        ul{
          border: 1px solid red;
        }
      }
     }
	}
</style>
