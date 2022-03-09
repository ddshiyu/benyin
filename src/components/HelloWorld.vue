<template>
  <div class="hello">
    <header>
      <img height="40" src="../assets/logo.png" alt="" />
      <div class="desc">
        <h3>本音研究室</h3>
        <h5>小空间声学处理研究|分享</h5>
      </div>
    </header>
    <nav>
      <el-carousel :interval="4000" height="300px">
        <el-carousel-item v-for="item in 6" :key="item">
          <div
            class="bg"
            :style="`background-image:url(https://blog.jdqiong.cn/banner/banner${item}.jpg)`"
          ></div>
          <!-- <img :src="`https://blog.jdqiong.cn/banner/banner${item}.jpg`" alt=""> -->
        </el-carousel-item>
      </el-carousel>
    </nav>
    <main>
      <h1>房间混响时间计算工具</h1>
      <p>
        控制房间混响是录音棚、混音室、HIFI等房间声学处理的重点。这个网站为需要声学设计的人提供了一个工具，用于估算房间混响时间（T60），以及进行声学处理所需要吸声板的面积。
        你需要输入你的房间尺寸，并选择吸声板的尺寸，网站会自动计算出进行声学处理前后的混响时间，及声学处理所需要的吸声板数量。
      </p>
      <div class="box">
        <el-card class="box-card">
          <el-form
            :inline="true"
            :model="formInline"
            size="default"
            class="demo-form-inline"
          >
            <h2>房间尺寸（m）</h2>
            <el-form-item label="长">
              <el-input
                v-model="formInline.leng"
                placeholder="请输入房间长"
                @input="changeNum"
              ></el-input>
            </el-form-item>
            <el-form-item label="宽">
              <el-input
                v-model="formInline.width"
                placeholder="请输入房间宽"
                @input="changeNum"
              ></el-input>
            </el-form-item>
            <el-form-item label="高">
              <el-input
                v-model="formInline.height"
                placeholder="请输入房间高"
                @input="changeNum"
              ></el-input>
            </el-form-item>
            <h2>选择吸声板尺寸（cm）</h2>
            <el-form-item label="">
              <el-select v-model="formInline.size" placeholder="请选择尺寸" @change="changeNum">
                <!-- <el-option label="60 * 60" value="0.36"></el-option> -->
                <el-option label="30 * 120" value="0.36"></el-option>
                <el-option label="60 * 120" value="0.72"></el-option>
              </el-select>
            </el-form-item>
            <h2>混响时间计算</h2>
            <div class="reverberation">
              <div class="child">
                <div>混响时间（原始)</div>
                <div class="num">{{result.orginTime}}</div>
              </div>
              <div class="child">
                <div>混响时间（语言声)</div>
                <div class="num">0.3</div>
              </div>
              <div class="child">
                <div>混响时间（音乐)</div>
                <div class="num">0.5</div>
              </div>
            </div>
            <h2>声学处理的吸声板数量</h2>
            <div class="reverberation">
              <div class="child">
                <div></div>
                <div class="num"></div>
              </div>
              <div class="child">
                <div>吸声板数量（语言声）</div>
                <div class="num">{{result.langNum}}</div>
              </div>
              <div class="child">
                <div>吸声板数量（音乐声）</div>
                <div class="num">{{result.musicNum}}</div>
              </div>
            </div>
          </el-form>
        </el-card>
        <p style="color: #999;">
          注：计算混响时间结果均为500-1000Hz中频混响时间，原始房间默认为大多数为硬质不吸声材料，需要了解混响时间频谱、低频驻波等更多声学处理细节欢迎在网站底部与我联系，相互交流。
        </p>
        <h2>吸声板图片</h2>
        <el-card class="box-card">
          <img src="../assets/pic1.png" alt="">
          <img src="../assets/pic2.png" alt="">
          <img src="../assets/pic3.png" alt="">
          <img src="../assets/pic4.png" alt="">
        </el-card>
        <h2>联系方式</h2>
        WECHAT：zjacoustic
        <div style="text-align:center;">
          <img src="../assets/peoson.png" alt="">
          <p>本音研究室|小空间声学处理研究分享</p>
        </div>
      </div>
    </main>
  </div>
</template>

<script lang="ts" setup>
import { reactive } from "vue";

const formInline = reactive({
  leng: "",
  width: '',
  height: '',
  size: "0.36",
});
const result = reactive({
  orginTime: 0,
  langNum: 0,
  musicNum: 0
})

const changeNum = () => {
  if (formInline.height && formInline.leng && formInline.width) {
    result.orginTime = +((0.161 * +formInline.leng * +formInline.width * +formInline.height) / (0.1 * (+formInline.leng * +formInline.width *2 + +formInline.width * +formInline.height * 2 + +formInline.width * +formInline.leng * 2))).toFixed(2)
    result.langNum = +(((0.161 * +formInline.leng * +formInline.width * +formInline.height) / 0.3 - (0.1 * (+formInline.leng * +formInline.width *2 + +formInline.width * +formInline.height * 2 + +formInline.width * +formInline.leng * 2))) / (+formInline.size * 0.8)).toFixed(2)
    result.musicNum = +(((0.161 * +formInline.leng * +formInline.width * +formInline.height) / 0.5 - (0.1 * (+formInline.leng * +formInline.width *2 + +formInline.width * +formInline.height * 2 + +formInline.width * +formInline.leng * 2))) / (+formInline.size * 0.8)).toFixed(2)
  } else {
    result.orginTime = 0
    result.langNum = 0
    result.musicNum = 0
  }
}
const onSubmit = () => {
  console.log("submit!");
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
header {
  width: 100%;
  height: 60px;
  padding: 0 50px;
  background: #fff;
  display: flex;
  align-items: center;
  justify-content: start;
}
.desc {
  margin-left: 10px;
}
.desc h3, .desc h5{
  margin: 0;
}
.carousel-img {
  width: 100%;
  height: 440px;
  object-fit: cover;
}
main {
  width: 60%;
  margin: 20px auto;
}
@media (min-width: 320px) and (max-width: 720px) {
  main {
    width: 90%;
    margin: 20px auto;
  }
}
.el-carousel__item h3 {
  color: #475669;
  font-size: 14px;
  opacity: 0.75;
  line-height: 200px;
  margin: 0;
  text-align: center;
}

.el-carousel__item:nth-child(2n) {
  background-color: #99a9bf;
}

.el-carousel__item:nth-child(2n + 1) {
  background-color: #d3dce6;
}
.bg {
  width: 100%;
  height: 100%;
  background-size: cover;
}
.reverberation {
  display: flex;
  justify-content: space-between;
  text-align: center;
}
.num {
  margin-top: 20px;
  font-size: 20px;
}
.child {
  width: 33.3%;
}
</style>
