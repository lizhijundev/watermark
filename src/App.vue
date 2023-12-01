<script setup lang="ts">
import CommonHeader from './components/CommonHeader.vue'
import CommonFooter from './components/CommonFooter.vue'
import WatermarkForm from './components/WatermarkForm.vue'
import {reactive, ref, watch} from "vue";

const form = reactive({
  name: '仅用于XXX使用,勿作他用',
  color: '#141414',
  opacity: 20,
  fontSize: 14,
  padding: 3
})
watch(form, () => {
  drawImage()
})


const imgFile = ref<File|undefined>(undefined)
const canvasDom = ref<HTMLCanvasElement|undefined>(undefined)

function onImage(file:File) {
  imgFile.value = file
  drawImage()
}


function drawImage() {
  if (imgFile.value == null) { return }
  const ctx = canvasDom.value.getContext('2d')
  const img = new Image()
  img.src = URL.createObjectURL(imgFile.value)
  img.onload = function () {
    canvasDom.value.width = img.width
    canvasDom.value.height = img.height
    ctx.drawImage(img, 0, 0)
    if (!form.name) {
      return
    }
    drawText(canvasDom.value, ctx)
  }
}

function drawText(canvasDom: HTMLCanvasElement | undefined, textCtx:CanvasRenderingContext2D | undefined) {
  if (!canvasDom) return;
  if (!textCtx) return;
  if (!form.name) return;

  textCtx.rotate(45 * Math.PI / 180);
  textCtx.globalAlpha = Number(form.opacity) / 100
  // textCtx.fillStyle = makeStyle();
  textCtx.font = `bold ${form.fontSize}px -apple-system,"Helvetica Neue",Helvetica,Arial,"PingFang SC","Hiragino Sans GB","WenQuanYi Micro Hei",sans-serif`;

  // 文字宽度
  const width = textCtx.measureText(form.name).width;
  // 画布对角线长度
  const step = Math.sqrt(Math.pow(canvasDom.width, 2) + Math.pow(canvasDom.height, 2));
  // 文字间隔
  const margin = textCtx.measureText('啊').width;
  // 画布最多能容纳多少行文字
  const x = Math.ceil(step / (width + margin));
  // 画布最多能容纳多少列文字
  const y = Math.ceil(step / (form.fontSize + form.padding));
  if (x === Infinity || y === Infinity) return;


  for (let i = 0; i <= x; i++) {
    for (let j = -y; j <= y; j++) {
      textCtx.fillText(form.name, (width + margin) * i, form.padding * form.fontSize * j);
    }
  }
}

function download() {
  if (!canvasDom.value) return
  const a = document.createElement('a')
  a.href = canvasDom.value.toDataURL()
  a.download = 'watermark.png'
  a.click()
}

function reset() {
  if (!canvasDom.value) return
  const ctx = canvasDom.value.getContext('2d')
  ctx.clearRect(0, 0, canvasDom.value.width, canvasDom.value.height)
  imgFile.value = undefined
}

</script>

<template>

  <div class="common-layout">
    <common-header title="图片水印" />
    <div class="main-container">
      <el-alert title="安全地为你的图片加水印，无任何网络请求，特别适合各种敏感证件（身份证，驾照，护照等）。" type="success" />
      <div class="flex-container">
        <div class="form-container">
          <watermark-form v-model:form="form" @onImage="onImage" />
        </div>
        <div class="canvas-container">
          <div class="canvas-box">
            <canvas ref="canvasDom" class="canvas"/>
            <div v-if="imgFile" class="canvas-op">
              <el-button type="success" @click="download">下载</el-button>
              <el-button type="danger" @click="reset">清空</el-button>
            </div>
          </div>
          <el-alert v-if="imgFile" title="长按图片 或 右键点击图片选择【图片存储为...】保存" type="success" />
        </div>
      </div>
    </div>
    <common-footer />
  </div>
</template>

<style scoped>
.main-container {
  width: var(--container-width);
  margin: 10px auto ;
}
.flex-container{
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  margin: 10px 0;
}
.form-container {
  width: 300px;
  flex-shrink: 0;
  padding-right: 10px;
}
.canvas-container {
  width: 100%;
  padding-left: 10px;
}
.canvas-container .canvas-box {
  width: 100%;
  border: 1px solid #eee;
  position: relative;
  margin-bottom: 10px;
}
.canvas-container .canvas-box .canvas {
  width: 100%;
}
.canvas-container .canvas-box .canvas-op {
  width: 100%;
  position: absolute;
  left: 0;
  bottom: 0;
  display: flex;
  flex-direction: row;
  justify-content: center;
  align-items: center;
  padding: 10px 0;
}

@media screen and (max-width: 960px) {
  .main-container {
    width: 100%;
    padding: 0 10px;
  }
  .flex-container {
    flex-direction: column;
  }
  .form-container {
    width: 100%;
  }
  .canvas-container {
    width: 100%;
    padding-left: 0;
  }
}
</style>
