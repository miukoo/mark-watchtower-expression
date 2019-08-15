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

<script>
  import GIF from 'gif.js'
  import html2canvas from 'html2canvas'
  import bg_0 from './assets/images/bg_0.png'
  import bg_1 from './assets/images/bg_1.png'
  import bg_2 from './assets/images/bg_2.png'
  import bg_3 from './assets/images/bg_3.png'
  import bg_4 from './assets/images/bg_4.png'
  import bg_5 from './assets/images/bg_5.png'
  import bg_6 from './assets/images/bg_6.png'
  import bg_7 from './assets/images/bg_7.png'
  import bg_8 from './assets/images/bg_8.png'
  import bg_9 from './assets/images/bg_9.png'
  import bg_10 from './assets/images/bg_10.png'
  import bg_11 from './assets/images/bg_11.png'
export default {
  name: 'App',
  data(){
    return {
      timer:null,
      text:'',
      showText:'骚等信号接收中...',
      chars:[],
      canvas:[],
      timeStep:500,
      outScale:0.6,
      outTitle:'你收到望楼传讯：',
      outText:"你那是喜欢吗，你是馋她的身体",
      list:[bg_0,bg_1,bg_2,bg_3,bg_4,bg_5,bg_6,bg_7,bg_8,bg_9,bg_10,bg_11],
      blag:[true,true,true,true,true,true,true,true,true,true,true,true]
    }
  },
  created(){
    if(window['outTitle']){
      this.outTitle=window['outTitle']
    }
    if(window['outText']){
      this.outText=window['outText']
    }
    if(window['outScale']){
      try {
        this.outScale = parseFloat( window['outScale'])
      }catch (e) {
        this.outScale =0.6
      }
    }
  },
  mounted(){
    let str = this.outText+" "
    let temp = str.split('')
    for(let i=0;i<temp.length;i++){
      this.chars.push(temp[i])
    }
    this.timeStep = 1800/str.length
    this.start()
  },
  methods:{
    // 启动定时器
    start:function(){
      let _this = this
      this.timer = setInterval(function(){_this.showBlag()},this.timeStep)
    },
    // 定时器关闭
    stop : function(){
      if(this.timer!=null){
        clearInterval(this.timer)
      }
    },
    // 生成每张图片的样式
    className : function(index){
      return 'bg_'+index;
    },
    // 获取unicode 二进制编码
    getUnicode : function(char){
      return parseInt(char.charCodeAt(0),10).toString(2);
    },
    // 使用gif生成gif图片
    createGif : function(){
        var gif = new GIF({
          workers: 2,
          quality: 5,
          workerScript:'/static/gif.worker.js'
        });
        for (let i = 0; i <this.canvas.length ; i++) {
          // 最后一帧演出一些
          if(i==this.canvas.length-1){
            gif.addFrame(this.canvas[i],{delay:2000,copy: true});
          }else
            gif.addFrame(this.canvas[i],{delay:this.timeStep,copy: true});
        }
        gif.on('finished', function(blob) {
          window.open(URL.createObjectURL(blob));
        });
        gif.render();
        this.stop()
    },
    // 记录每帧数据
    createCanvas : function() {
      let _this = this;
      _this.stop()
      html2canvas(document.getElementById('gif-body'),{scale:0.6}).then(canvas => {
        canvas.crossorigin="anonymous"
        _this.canvas.push(canvas)
        _this.start()
      });
    },
    // 依据字符串长度，渲染每帧的显示数据
    showBlag : function() {
      if(this.showText.indexOf("\.")!=-1){
        this.showText=this.showText.substring(0,this.showText.length-3)
      }else{
        this.showText+="..."
      }
      let cha = this.chars.shift()
      if (cha) {
        cha = cha.toUpperCase()
        let uni = this.getUnicode(cha)
        this.text+=cha
        let bs = uni.split('');
        for(let i=0;i<this.blag.length;i++){
          if(bs[i]=='1'){
            this.$set(this.blag,i,true)
          }else{
            this.$set(this.blag,i,false)
          }
        }
        if(this.chars.length==1){
          this.showText = this.text
        }
        // 创建can
        this.createCanvas();
      }
      else {
        this.createGif()
      }
    }
  }
}
</script>

<style>
  html,body{height:100%;}
  *{
    margin: 0px;
    padding: 0px;
    vertical-align: middle;
    box-sizing: border-box;
  }
.wapper {
  width:100%;
  height: 100%;
  overflow: hidden;
  text-align: center;
}
.wapper div{
    width: 640px;
    height: 326px;
    clear: both;
    position: absolute;
    top: 50%;
    left:50%;
    margin-top: -163px;
    margin-left: -320px;
    text-align: left;
}
  .text{
    position:relative;
    font-size: 36px;
    color: #ff6666;
    text-wrap: revert;
  }
</style>
