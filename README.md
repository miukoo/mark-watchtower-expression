# mark-watchtower-expression

> <b style='color:#0a6a;font-size:24px'>长安十二时望楼表情生成程序</b>
>
> 最近《长安十二时辰》电视剧火了，其中望楼传递信息在技术圈也掀起了一阵算法浪潮。该项目就是踩着这个热点结合技术可定制化生成个人偏好的望楼表情程序。
>
> <span style='color:#ffaa11'>— 喜欢的请点星，或者微信搜索'技术记号'</span>



### 此程序支持哪些人玩

- 不会前端的你，也可以玩
- 会前端的，希望你玩出B格

### 支持的功能

- 输入的字符自动转成望楼编码

- 依据输入字符长度，自动调节动画时长

- 生成的动画可直接右键保存为gif图片

- 支持非程序员修改表情文字

  

### 效果演示

![](G:\00_自媒体空间\VUE\mark-watchtower-expression\md\2.gif)

![](G:\00_自媒体空间\VUE\mark-watchtower-expression\md\3.gif)

## A. 你不会程序，可以这样玩

- 第一步：下载a文件夹
- 第二步：把a文件夹放在一个没有中文路径的地方
- 第三步：双击a文件夹下的nginx.exe
- 第四步：在浏览器中访问http://localhost:9898

```javascript
//  ======================================================
//  如果你想修改文字，可以修改a/html/index.html文件中的以下几项:
//  ======================================================
// 表情的缩放比例
window['outScale']=1;
// 表情的红色文字，建议不要超过8个汉字
window['outTitle']='';
// 表情的黑色文字，建议不要超过12个汉字
window['outText']='';
```

## B. 你会程序，可以这么玩

### 环境依赖

- nodejs
- webstorm
- webpack
- vue

### 安装

``` bash
# 控制终端输入以下命令，安装相关依赖库
cnpm i
```

### 运行

```bash
# 控制终端输入以下命令，安装相关依赖库
cnpm run dev
```

### 高级玩家进阶-源码说明

>程序核心源码在App.vue中，实现的思路是：
>
>- 准备1张望楼图片，其中方格处背景透明
>- 准备12张红色块图片，每一张对应望楼的12个其中一个方格
>- 把字符转成unicode二进制编码，共计16位
>- 截取16位的低12位，每一位控制一格望楼方格，如果是1则显示，反正不显示

注：常用汉字3500左右，12位能容纳4096个汉字，赶兴趣的老铁可以自行映射，该项目暂时截断高位字节处理。

#### Model

```java
	  timer:null,		//定时器
      text:'',			//计算临时变量
      showText:'骚等信号接收中...',	//默认显示的等待提示语
      chars:[],			//outText的字符数组
      canvas:[],		//截取的gif每贞数据
      timeStep:500,		//默认滚动时间步长
      outScale:0.6,		//gif的缩放比例
      outTitle:'你收到望楼传讯：',		//红色字的内容
      outText:"语文老师语文不及格",		//黑色字的内容
      list:[bg_0,bg_1,bg_2,bg_3,bg_4,bg_5,bg_6,bg_7,bg_8,bg_9,bg_10,bg_11],	//红色块图片
      blag:[true,true,true,true,true,true,true,true,true,true,true,true]//红色块显示控制
```

#### View

视图代码较为简单，在渲染图片时，通过循环数组输出，注意13张图片需要重叠在一起，并且注意层的顺序。

```html
<template>
  <div class="wapper">
    <div id="gif-body">
    <div class="bg">
      <div class="text" style="width: 150px;height: 30px;margin:-150px -80px;left: 100px;">{{outTitle}}</div>
      <div class="text" style="width: 150px;height: 100px;margin:-50px -20px;left: 50px;color:#000;">{{showText}}</div>
      <!-- 望楼-楼框样式 -->
      <img src="./assets/images/bg.png"></div>
      <!-- 循环输出12位对应的图片,如果隐藏则不输出 -->
      <template v-for="(item,index) in list">
        <div v-if="blag[index]" :key="index" :class="className(index)">
          <img :src="item">
        </div>
      </template>
    </div>
  </div>
</template>
```

### VM

VM实现较为复杂，但只要理解一下实现技巧，实现起来依很简单：

- 启动定时器，定时改变页面VIEW的内容（详见：showBlag方法）
- 每一次修改了VIEW内容之后，创建一个canvas并缓存起来（详见：createCanvas方法）
- 当字符输出完之后，合并所有的canvas元素，输出成gif动画图片（详见：createGif方法）