<template>
<!-- 图片效果显示区域 -->
  <view class="content">

    <view class="canvas">
      <canvas canvas-id="gameCanvas" :hidden="hiddeCanves" ></canvas>
          <!-- 模板 -->
      <view class="container">
        <view class="show-company-text" >
        {{company}}
      </view>
      <!-- 姓名显示 -->
      <view class="edit-canvas">
      
        <view class="show-name-text">
          <text>{{name}}</text>
        </view>
      </view>
    </view>
    </view>
    
    <!-- 编辑区域 -->
      <view class="edit-tips">编辑：</view>
      <form class="form-container">
      <view class="show-company">
         <label class="company-name-label">公司名字</label>
         <input type="text" v-model="company" placeholder="请输入公司名字" class="input" />
      </view>
      <view class="line"></view>
      <view class="show-name">
        <label class="name-label">姓名</label>
        <input type="text" v-model="name" placeholder="请输入姓名" class="input" />
      </view>
    </form>
    
    <!-- 按钮 -->
    <view class="choose">
      <button class="a-side">A面</button>
      <button class="a-side b-side">B面</button>
    </view>
    <button class="create-btn" @click="createImage"> 确认生成 </button>


  </view>
</template>

<script>

export default {
  data () {
    return {
      company: 'HELIDA 科技有限公司',
      name: '李小道',
      imagePath: "./assets/images/bg.png",  
      imageWidth:'',
      imageHeight:'',
      hiddeCanvas: true,
    }
  },

  components: {},

  onReady: function(){
    let _this = this,
    deviceWidth = 0;
     //获取设备宽度，用于求所需画在画布上的图片的高度
    wx.getSystemInfo({      
      success:function(res){
        deviceWidth = res.windowWidth;   //获取设备宽度
        wx.getImageInfo({    //获取图片信息
          src: _this.data.imagePath,
          success: function(res){
             let imageWidth = deviceWidth,
              imageHeight = deviceWidth/res.width*res.height;  //求所需画在画布上的图片的高度
              _this.setData({
                'imageWidth': imageWidth,
                'imageHeight':imageHeight
             }); 
 
            const ctx = wx.createCanvasContext('gameCanvas');  //创建画布对象  
            ctx.setFillStyle('red');
            ctx.drawImage(_this.data.imagePath, 0, 0, imageWidth, imageHeight);  //添加图片
            ctx.setFontSize(16);      //设置字体大小
            ctx.setFillStyle('blue');   //设置字体颜色
            ctx.fillText(this.company, 0,0);  //设置字体内容、坐标
            ctx.fillText(this.name,300,150);
            ctx.setFillStyle('blue'); 
            ctx.draw();     //开始绘画
          }
        })
      }
    });
  },

  methods: {
      createImage: function(){
      imageWidth = this.data.imageWidth,
      imageHeight = this.data.imageHeight;
 
    wx.canvasToTempFilePath({     //将canvas生成图片
      canvasId: 'gameCanvas',
      x: 0,
      y: 0,
      width: imageWidth,
      height: imageHeight,
      destWidth: imageWidth,     //截取canvas的宽度
      destHeight: imageHeight,   //截取canvas的高度
      success: function (res) {
        wx.saveImageToPhotosAlbum({  //保存图片到相册
          filePath: res.tempFilePath,
          success: function () {
            wx.showToast({
              title: "保存成功！",
              duration: 2000
            })
          }
        })
      }
    })
  },
  },

  created () {
    
  }
}
</script>

<style scoped>
.content {
  background:#eee;
  height:100%;
}
.canvas {
  position:relative;
}
.container{
  position:absolute;
  top:-200rpx;
  left:0rpx;
}

.show-name-text{
  font-size:92px;
  color:#fff;
  text-align:center;
}
.choose {
  width:100%;
  display:flex;
}
.edit-tips{
  height:160rpx;
  background:#eee;
  padding-left:30rpx;
  font-size:24rpx;
  color:#969696;
  line-height:240rpx;
  margin-top:100rpx;
}
.a-side{
  margin:160rpx auto;
  width:180rpx;
  height:80rpx;
  line-height:80rpx;
  background:#fff;
  float:left;
  border:2rpx solid #00ff00;
}
.b-side{
  border:1rpx solid #E6E6E6;
}
.edit-canvas {
  width:750rpx; 
  height:300rpx;
  text-align:center;
  line-height:300rpx;
   background:#F80101;
}
.line {
  width:100%;
  height:1rpx;
}
.company-text{
  line-height:60rpx;
  height:60rpx;
}
.show-company-text{
  line-height:60rpx;
  height:60rpx;
  background:#fff;
  width:750rpx;
  text-align:left;
  font-size:40rpx;
}
.input{
  width: 440rpx;
  height: 88rpx;
  border-radius: 44rpx;
  /* background: #f5f5f5; */
  display: inline-block;
}
.company-name-label {
  float:left;
  margin-right:30rpx;
  color:#00ff00;
}
.name-label {
  float:left;
  color:#00ff00;
  width:160rpx;
}
.show-company{
  background:#fff;
  height:80rpx;
  padding-left:30rpx;
  line-height:80rpx;
}
.show-name{
  padding-left:30rpx;
  background:#fff;
  height:80rpx;
  line-height:80rpx;
}
.create-btn{
  width:100%;
  height:80rpx;
  text-align:center;
  font-size:32rpx;
  line-height:80rpx;
  color:#fff;
  border-radius:0rpx;
  background:#29CD29;
  position:bottom;
}

</style>
