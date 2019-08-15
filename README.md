# mark-watchtower-expression

> <b style='color:#0a6a;font-size:24px'>长安十二时望楼表情生成程序</b>
>
> 最近《长安十二时辰》电视剧火了，其中望楼传递信息在技术圈也掀起了一阵算法浪潮。该项目就是踩着这个热点结合技术可定制化生成个人偏好的望楼表情程序。
>
> <span style='color:#ffaa11'>— 喜欢的请点星，或者微信搜索'技术记号'</span>
>
> ​	<span style='color:#9b9b9b;font-size:13px'>作者 | 罗学勇</span>
>
> ​	<span style='color:#9b9b9b;font-size:13px'>监制 | 向日葵</span>
>
> 项目地址：<https://github.com/miukoo/mark-watchtower-expression>



### <b style='color:red'>此程序支持哪些人玩</b>

- 不会前端的你，也可以玩
- 会前端的，希望你玩出B格

### <b style='color:red'>支持的功能</b>

- 输入的字符自动转成望楼编码

- 依据输入字符长度，自动调节动画时长

- 生成的动画可直接右键保存为gif图片

- 支持非程序员修改表情文字

  

### <b style='color:red'>效果演示</b>

<img src="https://github.com/miukoo/mark-watchtower-expression/blob/master/md/2.gif"/>

<img src="https://github.com/miukoo/mark-watchtower-expression/blob/master/md/3.gif"/>

## <b style='color:red'>A. 你不会程序，可以这样玩</b>

- 第一步：下载a文件夹
- 第二步：把a文件夹放在一个没有中文路径的地方
- 第三步：双击a文件夹下的nginx.exe
- 第四步：在chrome或firefox浏览器中访问http://localhost:9898

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

## <b style='color:red'>B. 你会程序，可以这么玩</b>

### <b style='color:red'>环境依赖</b>

- nodejs
- webstorm
- webpack
- vue

### <b style='color:red'>安装</b>

``` bash
# 控制终端输入以下命令，安装相关依赖库
cnpm i
```

### <b style='color:red'>运行</b>

```bash
# 控制终端输入以下命令，安装相关依赖库
cnpm run dev
```

### <b style='color:red'>高级玩家进阶-源码说明</b>

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

VM实现较为复杂，但只要理解以下实现技巧，实现起来依就简单：

- 启动定时器，每帧处理一个字符，定时改变页面VIEW的内容（详见：showBlag方法）
- 每一次修改了VIEW内容之后，创建一个canvas并缓存起来（详见：createCanvas方法）
- 当字符输出完之后，合并所有的canvas元素，输出成gif动画图片（详见：createGif方法）

```javascript
// 启动定时器
start:function(){ },
// 定时器关闭
stop : function(){},
// 生成每张图片的样式
className : function(index){ },
// 获取unicode 二进制编码
getUnicode : function(char){},
// 使用gif生成gif图片
createGif : function(){},
// 记录每帧数据
createCanvas : function() {},
// 依据字符串长度，渲染每帧的显示数据
showBlag : function() {}
```

## <b style='color:red'>彩蛋—常用表情</b>

>以下表情均由身边正经朋友提供，对此表示感谢!

| 【这事必须放颗原子弹庆祝】<br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E8%BF%99%E4%BA%8B%E5%BF%85%E9%A1%BB%E6%94%BE%E9%A2%97%E5%8E%9F%E5%AD%90%E5%BC%B9%E5%BA%86%E7%A5%9D.gif'/> | 【一个大叉，像个树杈，你是傻瓜】<br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E5%A4%A7%E5%8F%89%E6%A0%91%E6%9D%88%E5%82%BB%E7%93%9C.gif'/> |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| <b>【你那是喜欢吗，你是馋她的身体】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E4%BD%A0%E9%82%A3%E6%98%AF%E5%96%9C%E6%AC%A2%E5%90%97%EF%BC%8C%E4%BD%A0%E6%98%AF%E9%A6%8B%E5%A5%B9%E7%9A%84%E8%BA%AB%E4%BD%93.gif'/> | <b>【几个菜啊，喝成这样】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E5%87%A0%E4%B8%AA%E8%8F%9C%E5%95%8A%EF%BC%8C%E5%96%9D%E6%88%90%E8%BF%99%E6%A0%B7.gif'/> |
| <b>【只有聪明的猪才能看明白这信息】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E5%8F%AA%E6%9C%89%E8%81%AA%E6%98%8E%E7%9A%84%E7%8C%AA%E6%89%8D%E8%83%BD%E7%9C%8B%E6%98%8E%E7%99%BD%E8%BF%99%E4%BF%A1%E6%81%AF.gif'/> | <b>【帮弄楼上，可领个雪糕！】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E5%B8%AE%E5%BC%84%E6%A5%BC%E4%B8%8A%EF%BC%8C%E5%8F%AF%E9%A2%86%E4%B8%AA%E9%9B%AA%E7%B3%95%EF%BC%81.gif'/> |
| <b>【来喝酒、划拳、裸泳】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E6%9D%A5%E5%96%9D%E9%85%92%E3%80%81%E5%88%92%E6%8B%B3%E3%80%81%E8%A3%B8%E6%B3%B3.gif'/> | <b>【给主人发红包，才显示信息】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E7%BB%99%E4%B8%BB%E4%BA%BA%E5%8F%91%E7%BA%A2%E5%8C%85%EF%BC%8C%E6%89%8D%E6%98%BE%E7%A4%BA%E4%BF%A1%E6%81%AF.gif'/> |
| <b>【你是什么垃圾】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E4%BD%A0%E6%98%AF%E4%BB%80%E4%B9%88%E5%9E%83%E5%9C%BE.gif'/> | <b>【Test Test This's test】</b><br /><img src="https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/Test%20Test%20This's%20test.gif"/> |
| <b>【我今天晚上有空】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E6%88%91%E4%BB%8A%E5%A4%A9%E6%99%9A%E4%B8%8A%E6%9C%89%E7%A9%BA.gif'/> | <b>【你想我吗？】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E4%BD%A0%E6%83%B3%E6%88%91%E5%90%97%EF%BC%9F.gif'/> |
| <b>【悄悄告诉你，你很优秀！】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E6%82%84%E6%82%84%E5%91%8A%E8%AF%89%E4%BD%A0%EF%BC%8C%E4%BD%A0%E5%BE%88%E4%BC%98%E7%A7%80%EF%BC%81.gif'/> | <b>【明白了大哥】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E6%98%8E%E7%99%BD%E4%BA%86%E5%A4%A7%E5%93%A5.gif'/> |
| <b>【楼上这货，格杀勿论】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E6%98%8E%E7%99%BD%E4%BA%86%E5%A4%A7%E5%93%A5.gif'/> | <b>【来卡黑】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E6%9D%A5%E5%BC%80%E9%BB%91.gif'/> |
| <b>【楼下集合】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E6%A5%BC%E4%B8%8B%E9%9B%86%E5%90%88.gif'/> | <b>【求包养】</b><br /><img src='https://github.com/miukoo/mark-watchtower-expression/blob/master/%E7%83%AD%E7%82%B9%E8%A1%A8%E6%83%85%E5%8C%85/%E6%B1%82%E5%8C%85%E5%85%BB.gif'/> |

