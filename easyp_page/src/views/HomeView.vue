<template>
  <div class="home">
    <h1>easyP 约饭速P</h1>
    <h3>前端没有图像处理功能，图片调整好之后手动截图保存</h3>

    <!-- <el-button @click="test_button">test</el-button> -->


    <!-- 调节滑块 -->

    <div class="slider-box">
      <div class="slider-item">
        <span>尺寸-基底宽度</span>
        <el-slider v-model="base_width" />
      </div>
      <div class="slider-item">
        <span>尺寸-子元宽度</span>
        <el-slider v-model="item_width" />
      </div>
      <div class="slider-item">
        <span>尺寸-子元高度</span>
        <el-slider v-model="item_height" />
      </div>
      <div class="slider-item">
        <span>位置-子元top</span>
        <el-slider v-model="item_top" />
      </div>
      <div class="slider-item">
        <span>位置-子元left</span>
        <el-slider v-model="item_left" />
      </div>
    </div>


    <!-- 顶部成品图 -->

    <div class="result">
      <img class="base" :src="'base/' + base_select" alt="" :style="{ width: base_width + '%' }">
      <div class="item-box"
        :style="{ width: item_width + '%', height: item_height + '%', top: item_top + '%', left: item_left + '%' }">
        <img :src="item_select" alt="">
      </div>
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
      <h6>2.前端直接生成合成后的图片</h6>
      <h6>3.开一个图床存放更多的图</h6>
      <h6>4.上传自定义图片</h6>

      <div>
        umas@2023
        <a href="https://github.com/umas2022/easyP">Visit GitHub</a>
      </div>
    </div>

  </div>
</template>


<script lang="ts" setup>
import { ref, Ref, onMounted } from "vue"
import { ElMessage } from 'element-plus'


import index_base from "@/assets/index_base.json"
import index_item_eat from "@/assets/index_item_eat.json"
import index_item_play from "@/assets/index_item_play.json"
import index_item_other from "@/assets/index_item_other.json"


// 显示模式切换(横屏/竖屏)
const ishandy = ref(false)
// 初始化图片宽度
onMounted(() => {
  console.log(window.innerWidth)
  if (window.innerWidth < 800) {
    ishandy.value = true
    let silder_box = window.document.querySelector(".slider-box");
    (silder_box as HTMLElement).style.width = "90%"
    let result_box = window.document.querySelector(".result");
    (result_box as HTMLElement).style.width = "90%"
    let collapse_box = window.document.querySelector(".el-collapse");
    (collapse_box as HTMLElement).style.width = "90%"
  }
})


// 选中的图号
const base_select = ref("001.jpg")
const item_select = ref("item_eat/001.jpg")
const set_base = (value:string)=>{
  base_select.value = value
  let msg = "set"+JSON.stringify(value)
  ElMessage.success(msg)
}
const set_item = (value:string)=>{
  item_select.value = value
  let msg = "set"+JSON.stringify(value)
  ElMessage.success(msg)
}


// 宽度条件
const base_width = ref(80)
const item_width = ref(30)
const item_height = ref(30)
// 位置条件
const item_top = ref(5)
const item_left = ref(20)


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

  div.slider-item {
    display: flex;
  }

  .el-slider {
    padding-left: 20px;
    width: calc(100% - 150px);
  }
}

// 顶部成品图
div.result {
  position: relative;
  border: solid 2px rgba(0, 0, 0, 0.2);
  padding: 30px;
  width: 40%;
  text-align: center;

  .base {
    position: relative;
    width: 100%;
  }

  div.item-box {
    position: absolute;
    height: 40%;
    width: 40%;
    z-index: 99;

    img {
      width: 100%;
      height: 100%;
    }
  }
}


// 折叠栏图元选择

.el-collapse {
  width: 40%;
  padding-top: 30px;
  padding-bottom: 200px;
  padding-left: 30px;
  padding-right: 30px;
  border-left: solid 1px rgba(0, 0, 0, 0.234);
  border-right: solid 1px rgba(0, 0, 0, 0.234);
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