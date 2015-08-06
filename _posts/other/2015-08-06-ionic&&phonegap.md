---
layout: post
title: "ionic"
description: "ionic使用"
category: "other"
tags: [ionic]
---

## 概念
Ionic提供了一个免费且开源的移动优化HTML、CSS和JS 组件库，来构建高交互性应用。基于Sass构建和AngularJS 优化。

## How to use

- 1、 安装ionic

  npm install -g cordova ionic

- 2、初始化项目

  ionic start hosjoy

- 3、更改config.xml

  路径：ionic/hosjoy/platforms/ios/HelloCordova/config.xml
      <content src="index.html" />
      改为 <content src="http://m.ttmai.com" />

- 4、构建APP

  ionic build ios

- 5、模拟器打开APP

  ionic emulate ios

- 6、部署到设备

  ionic run ios

##ionic VS PhoneGap

####PhoneGap
Cordova是PhoneGap贡献给Apache后的开源项目，是从PhoneGap中抽离出的核心代码，是驱动PhoneGap的核心引擎。有点类似Webkit和Google Chrome的关系。渊源就是：早在2011年10月，Adobe收购了Nitobi Software和它的PhoneGap产品，然后宣布这个移动Web开发框架将会继续开源，并把它提交到Apache Incubator，以便完全接受ASF的管治。当然，由于Adobe拥有了PhoneGap商标，所以开源组织>的这个PhoneGap v2.0版产品就更名为Apache Cordova。（目前Adobe PhoneGap <===>Apache Cordova，似乎只是包名不一样而已，未来会有多大变化与改变，拭目以待吧！！）

####ionic
你可以先理解是一个webapp的框架，通俗的说就是html、js、css写出来的一个h5页面，
然后它又集成了angular和sass便于开发维护，本质上还是老三样（html、js、css），
再然后它为了方便或者为了以后盈利，又集成了cordova的功能，安装依赖后可以直接用
ionic的命令来调用cordova的创建、编译、打包等功能。
目前来看ionic后期可能收费:Pricing 路 Ionic.io Documentation，所以你可以选择只把ionic当做ui框架来用，舍弃它集成的cordova功能。
