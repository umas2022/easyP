<template>
  <div class="home">
    <h1>easyP 约饭速P</h1>

    <!-- <el-button @click="test_button">test</el-button> -->


    <!-- 调节滑块 -->

    <div class="slider-box">
      <el-form label-width="150px">
        <el-form-item label="整体宽度">
          <el-slider v-model="box_width_percent" :min="10" :max="90" />
        </el-form-item>
        <el-form-item label="尺寸-基底宽度">
          <el-slider v-model="base_width_percent" :min="10" :max="90" />
        </el-form-item>
        <el-form-item label="尺寸-子元宽度">
          <el-slider v-model="item_width_percent" :min="5" :max="50" />
        </el-form-item>
        <el-form-item label="位置-子元top">
          <el-slider v-model="item_top_persent" :min="0" :max="100" />
        </el-form-item>
        <el-form-item label="位置-子元left">
          <el-slider v-model="item_left_persent" :min="0" :max="100" />
        </el-form-item>
      </el-form>
    </div>


    <!-- 叠加成品图 -->
    <div class="result-box">
      <div class="img-box">
        <img class="base" :src="'base/' + base_select" alt="" :style="{ width: base_width + 'px' }">
        <img class="item" :src="item_select" alt=""
          :style="{ width: item_width + 'px', top: item_top + 'px', left: item_left + 'px' }">
      </div>
    </div>

    <el-button style="margin: 20px;" type="primary" @click="paint_canvas">绘图</el-button>

    <!-- canvas成品图 -->
    <div class="canvas-box">
      <canvas id="cv-res"></canvas>
    </div>


    <!-- 折叠栏图元选择 -->
    <el-collapse>

      <el-collapse-item name="1">
        <template #title>
          <h2>选择基底</h2>
        </template>
        <div class="img-box">
          <div class="img-each" v-for="(item, index) in index_base" :key="index">
            <img :src="'base/' + item.path" alt="" @click="set_base(item.path)">
            <h3>{{ item.title }}</h3>
          </div>
        </div>
      </el-collapse-item>

      <el-collapse-item name="2">
        <template #title>
          <h2>选择子元（吃喝）</h2>
        </template>
        <div class="img-box">
          <div class="img-each" v-for="(item, index) in index_item_eat" :key="index">
            <img :src="'item_eat/' + item.path" alt="" @click="set_item('item_eat/' + item.path)">
            <h3>{{ item.title }}</h3>
          </div>
        </div>
      </el-collapse-item>

      <el-collapse-item name="3">
        <template #title>
          <h2>选择子元（玩乐）</h2>
        </template>
        <div class="img-box">
          <div class="img-each" v-for="(item, index) in index_item_play" :key="index">
            <img :src="'item_play/' + item.path" alt="" @click="set_item('item_play/' + item.path)">
            <h3>{{ item.title }}</h3>
          </div>
        </div>
      </el-collapse-item>

      <el-collapse-item name="4">
        <template #title>
          <h2>选择子元（其他）</h2>
        </template>
        <div class="img-box">
          <div class="img-each" v-for="(item, index) in index_item_other" :key="index">
            <img :src="'item_other/' + item.path" alt="" @click="set_item('item_other/' + item.path)">
            <h3>{{ item.title }}</h3>
          </div>
        </div>
      </el-collapse-item>

    </el-collapse>


    <div>
      <h4>以下是一些以后可能会上线的功能（但更可能不会上线）</h4>
      <h6>1.鼠标拖动子图元调整位置和大小</h6>
      <h6>2.开一个图床存放更多的图</h6>
      <h6>3.上传自定义图片</h6>

      <div>
        umas@2023
        <a href="https://github.com/umas2022/easyP">Visit GitHub</a>
      </div>
    </div>

  </div>
</template>


<script lang="ts" setup>
import { ref, Ref, onMounted, computed } from "vue"
import { ElMessage } from 'element-plus'


import index_base from "@/assets/index_base.json"
import index_item_eat from "@/assets/index_item_eat.json"
import index_item_play from "@/assets/index_item_play.json"
import index_item_other from "@/assets/index_item_other.json"


// 显示模式切换(横屏/竖屏)
const ishandy = ref(false)
// 初始化图片宽度(横屏/竖屏)
onMounted(() => {
  if (window.innerWidth < 800) {
    ishandy.value = true
    let silder_box = window.document.querySelector(".slider-box");
    (silder_box as HTMLElement).style.width = "90%"

    let collapse_box = window.document.querySelector(".el-collapse");
    (collapse_box as HTMLElement).style.width = "90%"

    box_width_percent.value = 90
  }
})


// 选中的图号
const base_select = ref("001.jpg")
const item_select = ref("item_eat/001.jpg")
const set_base = (value: string) => {
  base_select.value = value
  let msg = "set" + JSON.stringify(value)
  get_aspect_ratio("base/" + base_select.value, base_ratio)
  ElMessage.success(msg)
}
const set_item = (value: string) => {
  item_select.value = value
  let msg = "set" + JSON.stringify(value)
  get_aspect_ratio("item/" + item_select.value, item_ratio)
  ElMessage.success(msg)
}



// 获取当前窗口的宽度，一切宽度都以此为基准计算出具体像素值（而不是百分比）
const windowWidth = ref(window.innerWidth)

// 整体宽度
const box_width_percent = ref(40)
const box_width = computed(() => Math.floor(windowWidth.value * box_width_percent.value / 100))

// 宽度条件
const base_width_percent = ref(50)
const base_width = computed(() => Math.floor(base_width_percent.value / 100 * box_width.value))
const item_width_percent = ref(20)
const item_width = computed(() => Math.floor(item_width_percent.value / 100 * box_width.value))


// 位置条件
const item_left_persent = ref(0)
const item_left = computed(() => Math.floor(item_left_persent.value / 100 * base_width.value))
const item_top_persent = ref(0)
const item_top = computed(() => Math.floor(item_top_persent.value / 100 * base_width.value * base_ratio.value))


// 长宽比（长/宽）
const base_ratio = ref(1)
const item_ratio = ref(1)
const get_aspect_ratio = (url: string, save_ref: Ref<any>) => {
  let image = new Image()
  image.src = url
  image.onload = function () {
    save_ref.value = image.height / image.width
  }
}


const paint_canvas = () => {

  const canvasBox = (document.querySelector(".canvas-box") as HTMLElement)
  canvasBox.style.display = "block"

  // 创建 Canvas 元素
  var canvas = (document.querySelector("#cv-res") as HTMLCanvasElement)
  var context = canvas!.getContext('2d');

  // 加载第一张图片
  var image1 = new Image();
  image1.src = 'base/' + base_select.value

  // 加载第二张图片
  var image2 = new Image();
  image2.src = item_select.value

  // 等待两张图片加载完成
  image1.onload = function () {
    image2.onload = function () {
      // 设置 Canvas 大小
      canvas!.width = base_width.value
      canvas!.height = Math.floor(base_width.value * base_ratio.value)

      // 在 Canvas 上绘制第一张图片
      context!.drawImage(image1, 0, 0, base_width.value, Math.floor(base_width.value * base_ratio.value));

      // 在 Canvas 上绘制第二张图片（覆盖在第一张图片上）
      context!.drawImage(image2, item_left.value, item_top.value, item_width.value, Math.floor(item_width.value * item_ratio.value));

    };
  };

}





// 测试按钮
const test_button = () => {
  console.log("test")


}


</script>


<style lang="scss" scoped>
div.home {
  width: 100%;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
}

// 滑块
div.slider-box {
  width: 40%;
  border: solid 2px rgba(0, 0, 0, 0.2);
  border-radius: 10px;
  margin: 30px;
  padding-top: 20px;

  div.slider-item {
    display: flex;
  }

  .el-slider {
    padding-left: 20px;
    width: calc(100% - 150px);
  }
}

// 顶部成品图
div.result-box {
  position: relative;
  border: solid 2px rgba(0, 0, 0, 0.2);
  border-radius: 10px;
  padding: 30px;
  text-align: center;
  margin: 30px;

  div.img-box {
    display: inline-block;
    position: relative;

    .base {
      position: relative;
      width: 100%;
    }

    .item {
      position: absolute;
      z-index: 99;
    }
  }


}


// canvas成品
div.canvas-box {
  position: relative;
  border: solid 2px rgba(0, 0, 0, 0.2);
  border-radius: 10px;
  padding: 30px;
  text-align: center;
  display: none;
  margin: 30px;
}

// 折叠栏图元选择

.el-collapse {
  width: 40%;
  padding: 20px;
  border: solid 2px rgba(0, 0, 0, 0.2);
  border-radius: 10px;
  margin: 30px;
}


div.img-box {
  display: flex;
  flex-wrap: wrap; // 换行

  div.img-each {
    text-align: center;
    padding: 10px;

    img {
      cursor: pointer;
      width: 100px;
    }
  }
}
</style>