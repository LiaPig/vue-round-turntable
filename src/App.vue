<template>
  <div id="app">
    <h1>剩余抽奖次数：{{ num }}</h1>
    <roundTurntable ref="roundTurntable" :prizeData="prizeData" @endRotation="endRotation"/>
    <div @click="startRotation" class="pointer"></div>
  </div>
</template>

<script>
import roundTurntable from './components/roundTurntable'

export default {
  name: 'App',
  components: {
    roundTurntable
  },
  data() {
    return {
      num: 2,
      prizeData: [
        {
          id: 1,
          prizeName: '2000元京东券',
          prizeImg: 'http://demo.sc.chinaz.net/Files/DownLoad/webjs1/201803/jiaoben5789/images/1.png',
        },
        {
          id: 2,
          prizeName: '300元京东券',
          prizeImg: 'http://demo.sc.chinaz.net/Files/DownLoad/webjs1/201803/jiaoben5789/images/7.png',
        },
        {
          id: 3,
          prizeName: '50个比特币',
          prizeImg: 'http://demo.sc.chinaz.net/Files/DownLoad/webjs1/201803/jiaoben5789/images/3.png',
        },
        {
          id: 4,
          prizeName: '50元话费券',
          prizeImg: 'http://demo.sc.chinaz.net/Files/DownLoad/webjs1/201803/jiaoben5789/images/4.png',
        },
        {
          id: 5,
          prizeName: '100元话费券',
          prizeImg: 'http://demo.sc.chinaz.net/Files/DownLoad/webjs1/201803/jiaoben5789/images/5.png',
        },
        {
          id: 6,
          prizeName: '100个比特币',
          prizeImg: 'http://demo.sc.chinaz.net/Files/DownLoad/webjs1/201803/jiaoben5789/images/6.png',
        },
      ],
      // 用来锁定转盘，避免同时多次点击转动
      isLocking: false,
      // 中奖的奖品的index
      prizeIndex: -1,
    }
  },
  methods: {
    startRotation() {
      if (this.num > 0) {
        // 如果正在转动着
        if (this.isLocking) {
          return false;
        }
        // 开始转动
        // 先上锁
        this.isLocking = true;
        // 设置在哪里停下，应该与后台交互，这里随机抽取0~5
        const index = Math.floor(Math.random() * 6);
        // 成功后次数减少一次
        this.num--;
        this.prizeIndex = index;
        // 告诉子组件，开始转动了
        this.$refs.roundTurntable.rotate(index);
      } else {
        alert('已经木有次数了！');
      }
    },
    // 已经转动完转盘触发的函数
    endRotation() {
      // 提示中奖
      alert(`恭喜您获奖啦,您的奖品是：${this.prizeData[this.prizeIndex].prizeName}`);
      // 解锁
      this.isLocking = false;
    },
  }
}
</script>

<style lang="scss">
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin: 0;
  padding: 0;
  .pointer {
    position: absolute;
    left: calc(50% - 46px);
    top: calc(50% - 50px);
    width: 100px;
    height: 100px;
    background-image: url("http://demo.sc.chinaz.net/Files/DownLoad/webjs1/201803/jiaoben5789/images/start.png");
    background-size: contain;
    background-repeat: no-repeat;
  }
}
</style>
