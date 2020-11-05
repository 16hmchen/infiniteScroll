<template>
  <div class="main">
    <div class="scroll-container">
      <div
        v-for="index in 8"
        :key="index"
        class="scroll-item"
        :style="styleFormatter(index - 1)"
        ref="scrollItem"
      >
        {{ index }}
      </div>
    </div>
    <div class="controller">
      <button class="btn" @click="toggleClick">{{ isScrolling ? '暂停滚动' : '开始滚动' }}</button>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      colorList: [
        'gray',
        'antiquewhite',
        'aquamarine',
        'cadetblue',
        'darkgoldenrod',
        'mediumaquamarine',
        'violet',
        'royalblue'
      ],

      // 每个元素的宽度
      width: 200,

      // 滚动速度
      speed: 100,

      isScrolling: false
    }
  },
  mounted () {
    this.initListener()
  },
  methods: {
    toggleClick () {
      if (this.isScrolling) {
        this.isScrolling = false
        this.pause()
      } else {
        this.isScrolling = true
        this.scroll()
      }
    },

    // 根据索引计算当前元素所在位置
    styleFormatter (index) {
      return {
        backgroundColor: this.colorList[index],
        left: `${index * this.width}px`
      }
    },

    // 控制滚动开始
    scroll () {
      const elementList = this.$refs.scrollItem
      elementList.forEach(ele => {
        ele.style.transitionDuration = `${(ele.offsetLeft + this.width) / this.speed}s`
        ele.style.left = `-${this.width}px`
      })
    },

    // 暂停滚动
    pause () {
      const elementList = this.$refs.scrollItem
      elementList.forEach(ele => {
        const styles = getComputedStyle(ele, null)
        console.log(styles.left)
        ele.style.transitionDuration = '0s'
        ele.style.left = `${styles.left}`
      })
    },

    // 初始化listener，监听滚动动画
    // 当元素完全滚动至显示范围外时，则将该元素插入队尾
    initListener () {
      const elementList = this.$refs.scrollItem

      elementList.forEach(ele => {
        // 当动画结束后会触发transitionend事件
        ele.addEventListener('transitionend', () => {
          // 计算队尾位置
          const lastestElementPosition = this.lastestElementPosition()
          ele.style.transitionDuration = `0s`
          ele.style.left = `${lastestElementPosition + this.width}px`

          // 渲染完毕之后开始滚动
          this.$nextTick(() => {
            ele.style.transitionDuration = `${(ele.offsetLeft + 200) / this.speed}s`
            ele.style.left = `-${this.width}px`
          })
        })
      })
    },

    // 计算最后一个元素的位置
    lastestElementPosition () {
      const elementList = this.$refs.scrollItem
      return Math.max(...elementList.map(ele => {
        // 注意，这里不能直接读取ele.style.left，这样只能读到终点位置的偏移量，而我们需要的是当前元素的实际位置，需要通过getComputedStyle来获取
        const styles = getComputedStyle(ele, null)
        return parseFloat(styles.left)
      }))
    }
  }
}
</script>

<style scoped>
  .scroll-container {
    width: 100%;
    height: calc(100% - 50px);
    display: flex;
    flex-direction: row;
    position: relative;
    overflow: hidden;
  }

  .scroll-item {
    display: flex;
    flex-shrink: 0;
    width: 200px;
    height: 100%;
    position: absolute;
    transition-property: left;
    transition-timing-function: linear;
  }

  .btn {
    margin-top: 20px;
    height: 30px;
    line-height: 30px;
  }
</style>
