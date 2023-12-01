<script setup lang="ts">
defineProps<{
  title: string
}>()
</script>

<template>
  <div class="nav">

    <div class="nav-flex" >
      <img class="avatar" src="https://www.lizhijun.com.cn/images/lzj_avatar.png" alt="">
      <h1>{{ title }}</h1>
    </div>
  </div>
</template>

<style scoped>
.nav {
  width: 100%;
  background-color: #fff;
  box-shadow: 0 1px 3px 0 rgb(0 0 0 / 0.1);
}
.nav-flex {
  width: var(--container-width);
  margin: 0 auto;
  display: flex;
  flex-direction: row;
  align-items: center;
  padding: 8px;
}
.nav .avatar {
  width: 40px;
  height: 40px;
  border-radius: 20px;
  margin-right: 6px;
}
.nav h1 {
  font-size: 1.125rem;
  line-height: 1.75rem;
  font-weight: 400;
  margin-left: 0.25rem;
  margin-right: 0.25rem;
}
</style>
