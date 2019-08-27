<template>
<div class="wrapper">
  <div class="scale-wrapper">
    <scale v-if="parentWidth!=0" :parentWidth="parentWidth" :min='100' :max="210" :targetNum="targetNum-100" :unit="'厘米'" @selectValue="selectValue"></scale>
    <scale v-if="parentWidth!=0" :parentWidth="parentWidth" :min='0' :max="100" :targetNum="weight" :unit="'公斤'" @selectWeight="selectWeight"></scale>

  </div>
</div>
</template>

<script>
import Scale from '@/components/Scale'

export default {
  data () {
    return {
      parentWidth:0,
      targetNum:165,
      weight:50
    }
  },

  components: {
    Scale
  },
  onLoad(){
    let that=this;
    wx.createSelectorQuery().select(".scale-wrapper")
      .boundingClientRect(function(rect) {
        that.parentWidth = rect.width;
      })
      .exec();
  },
  methods: {
    selectValue(e){
      console.log(e,'selectHeight')
    },
    selectWeight(e){
      console.log(e,'selectWeight')
    }
  },

}
</script>

<style lang="less" scoped>
.wrapper{
  width: 100%;
  padding: 20rpx;
  box-sizing: border-box;
  .scale-wrapper{
    background-color: #fff;
    width: 100%;
    height: 800rpx;
    border:1px solid #ddd;
    border-radius: 10rpx;
    box-shadow: 3px 2px 2px 2px #f1f1f1;
  }
}
</style>