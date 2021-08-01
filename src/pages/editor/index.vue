<template>

    <view >
      <view style="width:100%;height:260rpx;background:url('./assets/images/bg.png'); dixplay:flex;">
        <view>
          <label>名称：</label>
        <input placeholder='输入朋友的验证码' class='input' bindinput='bindKeyInput'/>
        </view>
        <view> 
          <label>名字：</label>
          <input type="text"
             v-model="title"
             class='input'
             placeholder="标题（5-15字以内）"  maxlength="15"/>
          </view>
      </view>

    <view @click="formSubmit">
      <button style="background-color: #00ff00" class="btn">确定并保存</button></button>
    </view>

    <view class='imagePathBox' :hidden="maskHidden">
      <image :src="imagePath" @click="previewImage" class='shengcheng'></image>
      <button class='baocun' @click='baocun'>重新生成</button>
    </view>
    <view :hidden="maskHidden" class="mask"></view>
    <view class="canvas-box">
      <canvas style="width: 375px;height: 667px;position:fixed;" canvas-id="mycanvas" />
    </view>


  </view>

</template>

<script>
export default {
  data () {
    return {
      count: '',
      title: '',
      maskHidden: true,
    }
  },

  components: {
  },
  onLoad: function(){
    let that = this;
    that.bgImg();
  },

  methods: {
    bgImg(){
      const ctx = wx.createCanvasContext('c');
      let width = 500;
      let height = 300;
      let bgPic = "./assets/images/bg.png";
     // ctx.drawImage(bgPic,0,0,width,height);
      ctx.setFillStyle("#000");
      ctx.fillRect(0, 100, 375, 567);
      ctx.draw();// 绘制背景图片
    },
    //生成画布
    createNewImg() {
      var that = this;
      var context = wx.createCanvasContext('mycanvas');
      context.setFillStyle("#fff")
      context.fillRect(0, 100, 375, 567)
      var path = "https://always520.cn/images/ydbj2.png";
      //将模板图片绘制到canvas,在开发工具中drawImage()函数有问题，不显示图片
      //不知道是什么原因，手机环境能正常显示
      context.drawImage(that.mysrc, 0,100, 375,567);
      //不知道是什么原因，手机环境能正常显示
      context.save();
      // 保存当前context的状态
      //console.log(this.count, "a", this.title)
      //var title = this.title;
      //绘制标题
      context.setFontSize(24);
      context.setFillStyle('#333333');
      context.setTextAlign('center');

      context.fillText(this.title, 185, 260);
      context.stroke();
      //绘制祝福语
      var text  = this.count;
      var chr = text.split("");
      var temp = "";
      var row = [];
      for (var a = 0; a < chr.length; a++) {
        if (context.measureText(temp).width < 300) {
          temp += chr[a];
        }
        else {
          a--; //这里添加了a-- 是为了防止字符丢失，效果图中有对比
          row.push(temp);
          temp = "";
        }
      }
      row.push(temp);
      console.log("a0", temp)
      context.setFontSize(18);
      context.setFillStyle('#333333');
      context.setTextAlign('center');
      for (var b = 0; b < row.length; b++) {
        context.fillText(row[b], 195, 300 + b * 30);
      }
      context.stroke();
      //绘制验证码背景
      //context.drawImage(path3, 48, 390, 280, 84);
      //绘制右下角扫码提示语
      //context.drawImage(path5, 248, 578, 90, 25);
      context.setFontSize(18);
      context.setFillStyle('red');
      context.setTextAlign('center');
      context.fillText("长按保存或分享", 195, 440);
      
      context.setFontSize(18);
      context.setFillStyle('red');
      context.setTextAlign('center');
      context.fillText("长按识别二维码，快点来生", 195, 465);
    
      context.setFontSize(18);
      context.setFillStyle('red');
      context.setTextAlign('center');
      context.fillText("成自己独特的元旦贺卡吧！", 195, 485);
      context.draw();
      
      //将生成好的图片保存到本地，需要延迟一会，绘制期间耗时
      setTimeout(function () {
        wx.canvasToTempFilePath({
          canvasId: 'mycanvas',
          success: (res) => {
            var tempFilePath = res.tempFilePath;
           
            that.imagePath = tempFilePath,
              that.imglist.push(tempFilePath),
              that.xs = false;
            this.canvasHidden=true
          },
          fail(res) {
            console.log(res);
          }
        });
      }, 200);
 //点击返回重新生成
  },
  baocun() {
      var that = this;
      that.maskHidden = true;
      that.xs = true;
      //保存图片
      //wx.saveImageToPhotosAlbum({
      //  filePath: this.imagePath,
      //  success(res) {
      //    wx.showModal({
      //      content: '图片已保存到相册，赶紧晒一下吧~',
      //      showCancel: false,
      //      confirmText: '好的',
      //      confirmColor: '#333',
      //      success: function (res) {
      //        if (res.confirm) {
      //          that.maskHidden = true
      //        }
      //      }, fail: function (res) {
      //        console.log(11111)
      //      }
      //    })
      //  }
      //})
    },
    //预览
    previewImage (e) {
        wx.previewImage({
          current: this.imglist[0], // 当前显示图片的http链接   
          urls: this.imglist // 需要预览的图片http链接列表   
        })
      },
    getInputText(inputText) {
      console.log(inputText.detail.value)
      
    },
  
  },

  created () {
    
    
  }
}
</script>

<style scoped>
  page{
  height: 100%;
  min-height: 100%;
  
}
.input{
  text-align: center;
  width: 600rpx;
  height: 50rpx;
  background: #f5f5f5;
  font-size: 32rpx;
  display: inline-block;
}
.btn{
  width: 600rpx;
  height: 88rpx;
  border-radius: 44rpx;
  background:linear-gradient(90deg,rgba(255,226,0,1),rgba(255,200,11,1));
  box-shadow: 0px 4px 8px 0px rgba(255,200,11,0.5); 
  color:#333;
  font-size: 32rpx;
  display: inline-block;
  line-height: 88rpx;
  margin-left: 40rpx;
}
  .imagePathBox {
    width: 100%;
    height: 100%;
    background: rgba(0,0,0,0.7);
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 10;
  }
  .shengcheng {
    width: 80%;
    height: 80%;
    position: fixed;
    top: 50rpx;
    left: 50%;
    margin-left: -40%;
    z-index: 10;
  }
  .baocun {
    display: block;
    width: 80%;
    height: 80rpx;
    padding: 0;
    line-height: 80rpx;
    text-align: center;
    position: fixed;
    bottom: 50rpx;
    left: 10%;
    background: #ff6f3f;
    color: #fff;
    font-size: 32rpx;
    border-radius: 44rpx;
  }

  button[class="baocun"]::after {
    border: 0;
  }

</style>
