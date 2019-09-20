# vue-round-turntable

> Vue中可配置的圆形转盘组件（适配移动端）；具体代码在`/src/App.vue`中，已抽离组件`/src/components/roundTurntable.vue`


## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

## 正式内容：

### 一、整个抽奖流程的思路分析：


1. 点击了转盘正中间的指针（即开始抽奖按钮），判断是否可以转动（具体逻辑封装在`canBeRotated()`里--①当前拥有的抽奖次数是否大于0②现在是否正在转动着（被锁着）），判断通过则进行下一步, 否则弹出相应提示。


2. 设置转盘在哪里停下，应该与后台交互，这里随机抽取0~5。


3. 当前抽奖次数减少一次。


4. 告诉转盘组件，开始转动了，并且动画结束后停在步骤2设置的地方。


5. 转盘转动，停在步骤3设置的地方后再提示中奖，解锁。


### 二、圆形抽奖转盘组件需要做的事情


1. 可以自定义每一格转盘的背景颜色，外边框的颜色。（`turntableStyleOption`属性）


2. 转盘的大小与位置。（在调用时，给组件加个class，代码里为`turntable`）


3. 自定义这个转盘运转起来要转过的圈数。（`rotateCircle`属性）


4. 可以自定义运转动画的时长。（`duringTime`属性）


5. 通过接收到父组件传递过来的奖品信息（`prizeData`），显示在每一格转盘的位置。（计算要根据圆心旋转的角度`getRotateAngle()`方法）


6. 被父组件调用后就开始转动，并在指定位置停下的方法（`rotate`），结束动画后告诉父组件已停下。


7. 奖品的名称和图片可以自定义样式。



### 三、页面预览


![](https://upload-images.jianshu.io/upload_images/7016617-97d36471f2a65d07.gif?imageMogr2/auto-orient/strip)


### 四、基础用法


1. 引用


```
import roundTurntable from './components/roundTurntable';
```



2. 声明


```
components: {
  roundTurntable
},
```



3. 调用


```html
<round-turntable
  ref="roundTurntable"
  :prizeData="prizeData"
  :rotateCircle="rotateCircle"
  :duringTime="duringTime"
  :turntableStyleOption="turntableStyleOption"
  @endRotation="endRotation"
  class="turntable">
    <template slot="item" slot-scope="scope">
      <div class="turntable-name">{{ scope.item.prizeName }}</div>
      <div class="turntable-img">
        <img :src="scope.item.prizeImg">
      </div>
    </template>
</round-turntable>
```


```javascript
data() {
  return {
    // 转盘上的奖品数据
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
   }
  ],
  // 转动的圈数
  rotateCircle: 6,
  // 转动需要持续的时间（s）
  duringTime: 4.5,
  // 转盘样式的选项
  turntableStyleOption: {
    // 背景色
    prizeBgColors: ['#AE3EFF', '#4D3FFF', '#FC262C', '#3A8BFF', '#EE7602', '#FE339F'],
    // 转盘的外边框颜色
    borderColor: '#199301',
  },
 }
},
methods: {
  // 已经转动完转盘触发的函数
   endRotation() {
      // 提示中奖
      alert(`恭喜您获奖啦,您的奖品是：${this.prizeData[this.prizeIndex].prizeName}`);
   },
},
```


```sass
.turntable {
  position: absolute;
  left: calc(50% - 200px);
  top: calc(50% - 200px);
  width: 400px;
  height: 400px;
}
.turntable-name {
  /*background: pink;*/
  position: absolute;
  left: 10px;
  top: 20px;
  width: calc(100% - 20px);
  font-size: 26px;
  text-align: center;
  color: #fff;
}
  .turntable-img {
  position: absolute;
  /*要居中就要50% - 宽度 / 2*/
  left: calc(50% - 80px / 2);
  top: 60px;
  width: 80px;
  height: 80px;
  img {
    display: inline-block;
    width: 100%;
    height: 100%;
  }
}
```


### 五、roundTurntable组件的属性说明

| 参数 | 说明 | 类型 | 默认值 |
|- | - | - | - |
|ref | 获取这组件的dom节点，调用子组件的开始转动动画方法要用到`this.$refs[refName].rotate(index)`| string | — |
|prizeData | 显示在转盘上的奖品数据 | array | — |
|rotateCircle | 转盘要转过的圈数 | number | `6` |
|duringTime| 转动需要持续的时间（单位为秒`s`） | number | `4.5` |
|turntableStyleOption | 转盘的样式选项（背景色、外边框颜色） | object | `{ prizeBgColors: ['#AE3EFF', '#4D3FFF', '#FC262C', '#3A8BFF', '#EE7602', '#FE339F'], borderColor: '#199301' }` |
|class | 用来定义转盘位置和大小的样式 | string | — |




### 六、roundTurntable组件的事件说明

| 事件名称 | 说明 | 回调参数 |
|- | - | - |
| endRotation | 转盘停下来后触发的事件回调  | — |



### 使用到的奖品图片和指针图片均来自：
[http://sc.chinaz.com/jiaobendemo.aspx?downloadid=12018113053246](http://sc.chinaz.com/jiaobendemo.aspx?downloadid=12018113053246)
