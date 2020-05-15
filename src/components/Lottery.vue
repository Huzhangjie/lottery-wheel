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
      <div 
        class="lottery-content__item"
        v-for="(item, index) in prizeList"
        :key="index"
      >
        <span>{{item.name}}</span>
      </div>
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
      prizeList: [
        {name: "小熊饼干", weight: 80},
        {name: "干脆面", weight: 100},
        {name: "冰淇淋", weight: 20},
        {name: "奶茶", weight: 50},
        {name: "小龙虾", weight: 20},
        {name: "海底捞", weight: 50},
      ]
    }
  },
  methods: {
    clickLottery() {
      this.$emit('lotteryClick')
    }
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
  transform: skewY(30deg);
  transform-origin: 100% 100%;
}
.lottery-content__item span{
    /* transform: rotate(-45deg); */
    transform: rotate(-45deg) skew(-30deg,30deg);
}
.lottery-content__item:first-child {
  background-color: red;
  transform: rotate(60deg) skewY(30deg);
}
.lottery-content__item:nth-child(2) {
  background-color: yellow;
  transform: rotate(120deg) skewY(30deg);
}
.lottery-content__item:nth-child(3) {
  background-color: antiquewhite;
  transform: rotate(180deg) skewY(30deg);
}
.lottery-content__item:nth-child(4) {
  background-color: darkblue;
  transform: rotate(240deg) skewY(30deg);
}
.lottery-content__item:last-child {
  background-color: blue;
  transform: rotate(300deg) skewY(30deg);
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
