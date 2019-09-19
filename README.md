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

1. 点击了转盘正中间的指针（即开始抽奖按钮），判断当前拥有的抽奖次数是否大于0。如果没有，则提示没有抽奖次数了；反之，下一步骤。


2. 判断现在是否正在转动着（被锁着），如果不是，则下一步骤。（这一步骤与第一步顺序可以互换）


3. 设置转盘在哪里停下，应该与后台交互，这里随机抽取0~5。


4. 当前抽奖次数减少一次。


5. 告诉转盘组件，开始转动了，并且动画结束后停在步骤3设置的地方。


6. 转盘转动，停在步骤3设置的地方后，提示中奖，解锁。

### 二、圆形抽奖转盘组件需要做的事情


1. 可以自定义每一格转盘的背景颜色，外边框的颜色。（vue组件里的`defaultOption`属性）


2. 转盘的大小与位置。（`css`中`turntable`类，`$diameter`）


3. 自定义这个转盘运转起来要转过的圈数。（vue组件里的`rotateCircle`属性）


4. 可以自定义运转动画的时长。（在vue组件中搜索`duringTime`）


5. 通过接收到父组件传递过来的奖品信息（`prizeData`），显示在每一格转盘的位置。（class为`prize-container`）


6. 被父组件调用后就开始转动，并在指定位置停下的方法（`rotate`），结束动画后告诉父组件已停下。



### 三、效果浏览


![](https://upload-images.jianshu.io/upload_images/7016617-f9157d4fdf34ce3f.gif?imageMogr2/auto-orient/strip)



### 使用到的奖品图片和指针图片均来自：


[http://sc.chinaz.com/jiaobendemo.aspx?downloadid=12018113053246](http://sc.chinaz.com/jiaobendemo.aspx?downloadid=12018113053246)
