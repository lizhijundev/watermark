<script setup lang="ts">
import {defineEmits, defineProps} from "vue";

const emit = defineEmits(['onImage', 'update:form'])


const props = defineProps({
  form: {
    type: Object,
    default: () => ({
      name: '仅用于XXX使用,勿作他用',
      color: '#141414',
      opacity: 20,
      fontSize: 14,
      padding: 3
    })
  }
})

function onChoosePic(){
  const input = document.createElement('input')
  input.type = 'file'
  input.accept = 'image/*'
  input.onchange = handleFileChange
  input.click()
}


function handleFileChange(event: any) {
  const file = event.target.files[0];
  if (file) {
    emit('onImage', file)
  }
}

function onFormChange() {
  emit('update:form', props.form)
}

</script>

<template>
  <el-form :model="form" label-width="120px">
    <el-form-item label="">
      <el-button type="primary" @click="onChoosePic">选择图片</el-button>
    </el-form-item>
    <el-form-item label="水印">
      <el-input v-model="form.name" type="textarea" @blur="onFormChange" />
    </el-form-item>
    <el-form-item label="颜色">
      <el-color-picker v-model="form.color" @change="onFormChange"/>
    </el-form-item>
    <el-form-item label="透明度">
      <el-slider v-model="form.opacity" :min="20" :max="100" @change="onFormChange"/>
    </el-form-item>
    <el-form-item label="文字大小">
      <el-slider v-model="form.fontSize" :min="22" :max="36" @change="onFormChange"/>
    </el-form-item>
    <el-form-item label="文字间隔">
      <el-slider v-model="form.padding" :min="3" :max="4.5" :step="0.2" @change="onFormChange"/>
    </el-form-item>
  </el-form>
</template>

<style scoped>
</style>
