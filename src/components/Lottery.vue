<!--
 * @Author: HuZhangjie
 * @Date: 2020-05-15 11:22:38
 * @LastEditors: HuZhangjie
 * @LastEditTime: 2020-05-15 18:36:42
 * @Description: 转盘组件
--> 
<template>
  <div class="lottery-wraper">
    <!-- 转盘背景 -->
    <div class="lottery_bg">
      <img :src="lotterybg">
    </div>
    <div class="lottery-content">
      <template v-for="(item, index) in prizeList">
        <div 
          class="lottery-content__item"
          :key="index"
          :style="{transform: `rotate(${itemSelfAngle * index + wheelAngle}deg) skew(${skewAngle}deg, ${skewAngle}deg)`}"
        >
          <div class="prize-item__name">{{item.name}}</div>
        </div>
      </template>
    </div>

    <!-- 转盘指针 -->
    <div class="lottery_pointer" @click="clickLottery">
      <img :src="pointerbg">
    </div>
  </div>
</template>

<script>
export default {
  name: 'Lottery',
  props: {
    msg: String,
    lotteryBg: {
      type: String,
      default: ''
    },
    pointerBg: {
      type: String,
      required: true
    },
    lotteryStart: {
      type: Number,
      default: 0
    },
  },
  watch: {
    lotteryStart () {
      if (this.lotteryStart >= 1) {
        this.startRotate()
      }
    }
  },
  data() {
    return {
      lotterybg: this.lotteryBg, // 外圈背景
      pointerbg: this.pointerBg, // 指针背景
      wheelAngle: 0,
      prizeList: [
        {name: "小熊饼干", weight: 80},
        {name: "干脆面", weight: 100},
        {name: "冰淇淋", weight: 20},
        {name: "奶茶", weight: 50},
        {name: "小龙虾", weight: 20},
        {name: "海底捞", weight: 50},
      ],
      // 中奖后奖品的下边
      prizeNo: 0,
    }
  },
  computed: {
    itemSelfAngle(){
      return 360 / this.prizeList.length
    },
    skewAngle() {
      return (90 - this.itemSelfAngle) / 2
    },
    // 中奖奖品的rotate的角度
    priceAngel () {
      const priceNum = this.prizeList.length
      return (this.prizeNo / priceNum) * 360 - 360 / priceNum / 2
    },
  },
  methods: {
    clickLottery() {
      this.$emit('lotteryClick')
      this.startRotate()
    },
    startRotate() {
      const self = this
      let speed = 0
      const acceleration = 5
      const racceleration = 1
      const firstTimes = 48 // 旋转48次到达最高次数
      const secondTimes = 10 // 已最高速度转10次
      const highSpeed = firstTimes * acceleration // 最高速度
      const thirdTimes = highSpeed / racceleration // 执行three次 速度从highSpeed减速到0
      // 等差数列求和公式
      const firstAngle = this.wheelAngle + firstTimes * (firstTimes - 1) / 2 * acceleration
      console.log("startRotate -> highs", highSpeed, secondTimes)
      const secondAngle = firstAngle + highSpeed * secondTimes
      const thirdAngle = secondAngle + thirdTimes * (thirdTimes - 1) / 2 * racceleration
      console.log("startRotate -> thirdAngle", firstAngle, secondAngle, thirdAngle)
      console.log("startRotate -> this.priceAngel", this.priceAngel)
      function cicle () {
        if(self.wheelAngle < firstAngle) {
          speed += acceleration
          self.wheelAngle += speed
        } else if(self.wheelAngle >= firstAngle && self.wheelAngle < secondAngle) {
          self.wheelAngle += speed  
        } else if(self.wheelAngle > secondAngle && self.sheelAngle <= thirdAngle) {
          console.log("cicle -> speed", speed)
          speed -= racceleration
          if(speed <= 0) {
            speed = 0
            self.wheelAngle = self.wheelAngle % 360
            return false
          }
          self.angle += speed
        }

        requestAnimationFrame(cicle)
      }
      cicle()
    }
    // 计算得出每个抽奖项的 rotate和 skew角度
    // getContentTransformStyle(index) {
    //   const itemSelfAngle = 360 / this.prizeList.length
    //   // skew需要的角度
    //   const skewAngle = (90 - itemSelfAngle) / 2
    //   return {
    //     transform: `rotate(${itemSelfAngle * index + this.wheelAngle}) skew(${skewAngle}deg, ${skewAngle}deg)`
    //   }
    // }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.lottery-wraper{
  position: relative;
  max-width: 500px;

}
.lottery-wraper img{
  width: 100%;
}
.lottery-content{
  position: absolute;
  top: 50%;
  left: 50%;
  width: 85%;
  height: 85%;
  transform: translate(-50%,-50%);
  -webkit-transform: translate(-50%,-50%);
  border-radius: 50%;
  overflow: hidden;
  /* border-radius: 50%;
  overflow: hidden; */
}
.lottery-content__item {
  display: flex;
  justify-content: center;
  align-items: center;
  position: absolute;
  width: 50%;
  height: 50%;
  background-color: aqua;
  /* transform: skew(30deg, 30deg); */
  transform-origin: 100% 100%;
}
.lottery-content__item .prize-item__name{
    /* transform: rotate(-45deg); */
    transform: rotate(-45deg);
}
.lottery-content__item:first-child {
  background-color: red;
  /* transform: rotate(60deg) skew(15deg, 15deg); */
}
.lottery-content__item:nth-child(2) {
  background-color: yellow;
  /* transform: rotate(120deg) skew(15deg, 15deg); */
}
.lottery-content__item:nth-child(3) {
  background-color: antiquewhite;
  /* transform: rotate(180deg) skew(15deg, 15deg); */
}
.lottery-content__item:nth-child(4) {
  background-color: azure;
  /* transform: rotate(240deg) skew(15deg, 15deg); */
}
.lottery-content__item:nth-child(5) {
  background-color: darkblue;
  /* transform: rotate(300deg) skew(15deg, 15deg); */
}
.lottery-content__item:last-child {
  background-color: blue;
  /* transform: rotate(0deg) skew(15deg, 15deg); */
}
.lottery_pointer{
  position: absolute;
  top: 48%;
  left: 50%;
  width: 35%;
  transform: translate(-50%,-50%);
  -webkit-transform: translate(-50%,-50%);
}
</style>
