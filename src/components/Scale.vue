<template>
  <div class="scale">
    <div class="scale-num">{{scale_num}} {{unit}}</div>
    <scroll-view scroll-x scroll-with-animation :scroll-left="scroll_left" @scroll="onScroll">
      <div class="scale-wrapper" :style="{width:parentWidth+(max-min)*100+'px'}">
        <div class="space" :style="{width:parentWidth/2+'px'}"></div>
        <div class="scale-body" :style="{width:(max-min)*100+'px'}">
          <div class="scale-groups">
            <div class="scale-group" v-for="(i,index) in (max-min)" :key="index">
              <div
                class="scale-group-item"
                v-for="(j,jndex) in 10"
                :key="jndex"
                style="width:10px;"
              >
                <div class="dot"></div>
                <div class="num">{{i+min}}</div>
              </div>
            </div>
            <div class="scale-group">
              <div class="scale-group-item" style="width:10px;">
                <div class="dot"></div>
                <div class="num">{{max}}</div>
              </div>
            </div>
          </div>
        </div>
        <div class="space" :style="{width:parentWidth/2+'px'}"></div>
      </div>
    </scroll-view>
    <div class="ind-container" :style="{left:parentWidth/2+'px'}">
      <div class="ind"></div>
      <div class="ind-circle"></div>
    </div>
  </div>
</template>
<script>
export default {
  props: {
    parentWidth: Number,
    min: Number,
    max: Number,
    unit: String,
    targetNum: Number
  },
  data() {
    return {
      scale_space: 10,
      target: 0,
      scroll_left: 0,
      alginIndicateFunc: null,
      is_algining: false,
      px_peer_rpx: 1
    };
  },
  onLoad() {
    this.alginIndicateFunc = this.debounce(this.alginIndicate, 200);
    this.scroll_left = this.targetNum * 100;
  },
  computed: {
    scale_num() {
      return (this.scroll_left / 100 + this.min).toFixed(1);
    }
  },
  methods: {
    onScroll(e) {
      if (this.is_algining) {
        this.is_algining = false;
        return;
      }
      this.alginIndicateFunc(e.mp.detail.scrollLeft);
    },
    alginIndicate(left) {
      let a = left % this.scale_space;
      if (a >= this.scale_space / 2) {
        this.scroll_left = left + this.scale_space - a;
      } else {
        this.scroll_left = left - a;
      }
      this.is_algining = true;
      // 发送给父组件
      setTimeout(() => {
        this.$emit("selectValue", this.scale_num);
        this.$emit("selectWeight", this.scale_num);
      }, 1000);
    },
    debounce(func, wait, immediate) {
      let timeout, result;

      return function() {
        let context = this;
        let args = arguments;

        if (timeout) clearTimeout(timeout);
        if (immediate) {
          // 如果已经执行过，不再执行
          let callNow = !timeout;
          timeout = setTimeout(function() {
            timeout = null;
          }, wait);
          if (callNow) result = func.apply(context, args);
        } else {
          timeout = setTimeout(function() {
            func.apply(context, args);
          }, wait);
        }
        return result;
      };
    }
  }
};
</script>
<style lang="less" scoped>
.scale {
  width: 100%;
  margin-top: 20rpx;
  position: relative;
  .scale-num {
    margin-bottom: 30rpx;
    width: 100%;
    height: 40rpx;
    text-align: center;
    font-size: 32rpx;
    font-weight: bold;
    color: rgba(50, 87, 129, 1);
  }
  .scale-wrapper {
    height: 120rpx;
    background-color: #f3fbff;
    position: relative;
    .space {
      height: 120rpx;
      background-color: #f3fbff;
      display: inline-block;
    }
    .scale-body {
      display: inline-block;
      height: 120rpx;
      .scale-groups {
        position: absolute;
        top: 50%;
        display: flex;
        .scale-group {
          height: 100%;
          display: flex;
          .scale-group-item {
            box-sizing: border-box;
            text-align: left;
            position: relative;
            &:first-child {
              .dot {
                width: 6rpx;
                height: 6rpx;
                border-radius: 50%;
                background-color: #4B81BF;
                position: absolute;
              }
              .num {
                display: inline-block;
              }
            }
            .dot {
              width: 4rpx;
              height: 4rpx;
              background-color: #BFD6E8;
              border-radius: 50%;
              position: absolute;
              transform: translate(-50%, -50%);
            }
            .num {
              position: absoulte;
              display: none;
              top: 45px;
              transform: translateX(-50%);
              font-size: 20rpx;
              color: #4b81bf;
            }
          }
        }
      }
    }
  }
  .ind-container {
    width: 0;
    top: -120rpx;
    position: relative;
    height: 120rpx;
  }
  .ind {
    height: 120rpx;
    position: absolute;
    top: 0;
    z-index: 100;
    width: 5rpx;
    background: rgba(75, 129, 191, 1);
    border-radius: 3px;
  }
  .ind-circle {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    margin-left: 2rpx;
    width: 10rpx;
    height: 10rpx;
    background: rgba(75, 129, 191, 1);
    border: 2px solid rgba(206, 219, 231, 1);
    border-radius: 50%;
    z-index: 101;
  }
}
</style>