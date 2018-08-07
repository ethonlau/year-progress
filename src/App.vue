<template>
  <div id="app">
    <div class="switch">
      <a :class="[ 'item', { 'active': item.status } ]"
         v-for="(item, index) in list"
         :key="index"
         @click="switchStatus(index)">
        {{ item.name }}
      </a>
    </div>
    <div v-if="list[0].status" class="bar">
      <div class="box"><div id="js-progress" class="progress"></div></div>
    </div>
    <div v-else-if="list[1].status" class="ring">
      <div class="percent">{{ percent }}<span>%</span></div>
      <svg width="240" height="240" class="_3l2Da" viewBox="25 25 50 50">
        <circle class="bg" cx="50" cy="50" r="20" fill="none"></circle>
      </svg>
      <svg width="240" height="240" class="_3l2Da" viewBox="25 25 50 50">
        <circle id="js-circle" class="active" cx="50" cy="50" r="20" fill="none"></circle>
      </svg>
    </div>
    <div v-else-if="list[2].status" class="dot">
      <span v-for="(item, index) in totalNum"
            :key="index"
            :style="{ animationDuration: `.${index%35 + 1}s` }"
            :class="{'active' : index < num}">
      </span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      list: [{
        name: 'BAR',
        status: false
      },
      {
        name: 'RING',
        status: false
      },
      {
        name: 'DOT',
        status: true
      }]
    }
  },
  computed: {
    num () {
      return Math.ceil((new Date() - new Date(new Date().getFullYear().toString()))/(24*60*60*1000));
    },
    totalNum () {
      if (new Date().getFullYear()%4 === 0) {
        return 366
      } else {
        return 365
      }
    },
    percent () {
      return Math.floor((this.num/this.totalNum)*100)
    }
  },
  methods: {
    switchStatus (index) {
      this.list[0].status = false
      this.list[1].status = false
      this.list[2].status = false
      this.list[index].status = true

      if (index === 0) {
        this.$nextTick(() => {
          setTimeout(() => {
            document.getElementById("js-progress").style.width = this.percent + '%'
          }, 100)
        })
      }

      if (index === 1) {
        this.$nextTick(() => {
          setTimeout(() => {
            document.getElementById("js-circle").style.strokeDasharray = this.num/this.totalNum * 126 + ',126'
          }, 100)
        })
      }
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;

  $main-color: #FF6565;
  $bg-color: #ddd;

  .switch {
    max-width: 420px;
    margin: 50px auto;
    text-align: center;

    .item {
      position: relative;
      display: inline-block;
      margin: 0 -1px;
      padding: 10px 0;
      width: 30%;
      border: 2px solid $bg-color;
      cursor: pointer;
      transition: .2s;
      z-index: 1;

      &.active {
        color: white;
        background-color: $main-color !important;
        border-color: $main-color;
        z-index: 2;
      }

      &:hover {
        background-color: #f7f7f7;
      }

      &:first-child {
        border-radius: 8px 0 0 8px;
      }

      &:last-child {
        border-radius: 0 8px 8px 0;
      }
    }
  }

  .bar, .ring, .dot {
    position: relative;
    margin: 0 auto;
    height: 280px;
    font-size: 0;
  }

  .bar {
    max-width: 420px;

    .box {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      width: 100%;
      height: 60px;
      border-radius: 8px;
      overflow: hidden;

      &:after {
        content: '';
        position: absolute;
        top: 0;
        left: 0;
        bottom: 0;
        right: 0;
        border: 2px solid $bg-color;
        border-radius: 8px;
        z-index: -1;
      }

      &:before, .progress:before {
        content: 'Year Progress';
        position: absolute;
        top: 0;
        left: 0;
        bottom: 0;
        width: calc(100vw - 16px);
        max-width: 420px;
        text-align: center;
        font-size: 24px;
        line-height: 60px;
      }

      &:before {
        color: #999;
      }

      .progress {
        position: absolute;
        top: 0;
        left: 0;
        bottom: 0;
        width: 5px;
        background-color: $main-color;
        transition: .5s;
        overflow: hidden;

        &:before {
          color: white;
        }
      }
    }
  }

  .ring {
    width: 280px;

    .percent {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%,-50%);
      font-size: 60px;

      span {
        font-size: 24px;
      }
    }

    svg {
      position: absolute;
      top: 50%;
      left: 50%;
      margin: -120px 0 0 -120px;
      transform: rotate(-90deg);

      circle {
        stroke-width: 2px;
        stroke-miterlimit: 10;
        stroke-linecap: round;
        will-change: stroke-dasharray;

        &.bg {
          stroke: $bg-color;
        }

        &.active {
          stroke: $main-color;
          stroke-dasharray: 1,126;
          transition: .5s ease;
        }
      }
    }
  }

  .dot {
    width: 280px;

    span {
      display: inline-block;
      margin: 0 4px 4px 0;
      width: 10px;
      height: 10px;
      background-color: $bg-color;
      animation-name: dot;

      @keyframes dot {
        0% {
          opacity: 0;
        }
        100% {
          opacity: 1;
        }
      }

      &.active {
        background-color: $main-color;
        border-color: $main-color;
      }
    }
  }
}
</style>
