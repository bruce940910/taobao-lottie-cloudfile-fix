
# lottie

lottie 动画库适配小程序的版本。

> lottie 的相关介绍与动画生成方法等请参考 [官方说明](https://github.com/airbnb/lottie-web)
> 依赖手淘版本 >= 9.7.0 的环境

## 使用

可参考该[代码片段](https://gw.alicdn.com/bao/uploaded/TB1In6gDxD1gK0jSZFsXXbldVXa.zip?spm=a1z3i.a4.0.0.cf98eb1dV4rJNg&file=TB1In6gDxD1gK0jSZFsXXbldVXa.zip)，大致步骤如下：<br />

1. 通过 npm 安装：

```
npm install --save taobao-lottie-cloudfile-fix
```

2. 传入 canvas 对象用于适配

```
<canvas id="canvas"  type="2d" onReady="onCanvasReady" style="width: 300px; height: 500px"></canvas>

```

```javascript
import lottie from "taobao-lottie-cloudfile-fix";
import animationData from "../../json/catrim"; // 实际文件路径

Page({
  onReady() {},
  onCanvasReady() {
    this.canvas = my.createCanvas({
      id: "canvas",
      success: (canvas) => {
        ar systemInfo = my.getSystemInfoSync();
        canvas.width = 300 * systemInfo.pixelRatio;
        canvas.height = 500 * systemInfo.pixelRatio;
        this.canvas = canvas;
        console.log("canvas=====", canvas);
        lottie.setup(canvas);
        this.ani = lottie.loadAnimation({
          loop: true,
          autoplay: true,
          // path:'https://gw.alipayobjects.com/os/lottie-asset/coupon-tip/data.json/data-80154.json',
          animationData,
          rendererSettings: {
            context: canvas.getContext("2d"),
          },
        });
      },
    });
  },
  play() {
    this.ani.play();
  },
  pause() {
    this.ani.pause();
  },
});
```

3. 使用 lottie 接口

```
lottie.setup(canvas)
lottie.loadAnimation({
  ...
})
```

## 接口

目前提供两个接口：<br />**lottie.setup(canvas)**<br />需要在任何 lottie 接口调用之前调用，传入 canvas 对象<br />**lottie.loadAnimation(options)**<br />与原来的 [loadAnimation](https://github.com/airbnb/lottie-web/wiki/loadAnimation-options) 有些不同，支持的参数有：

- loop
- autoplay
- animationData
- path （只支持网络地址）
- rendererSettings.context （必填）

## 说明

- 本项目是以 npm 的方式依赖原 lottie-web 项目，若原项目有新版本，可直接改变依赖的版本号。
- 本项目依赖手淘  9.7.0 里性能更好的 canvas 实现，由于还有些小问题没有正式开放，但目前用在此处暂无发现问题。
- 由于小程序本身不支持动态执行脚本，因此 lottie 的 expression 功能也是不支持的。
- 本项目基于@tbminiapp/lottie-miniapp, 与原项目保持一致，只是修复一些影响使用的问题

## 修复

- 修复淘宝小程序不能使用云存储json文件的问题
- 修复淘宝小部件环境下报错导致不能使用的问题
