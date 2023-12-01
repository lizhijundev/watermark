<script setup lang="ts">
 import {ref} from "vue";

 const icp = ref('粤ICP备18126829号-1')
 const year = ref(new Date().getFullYear())

</script>

<template>
  <footer class="footer">
    <div>© 2015 - {{year}} All Rights Reserved. 荔枝君</div>
    <a href="http://beian.miit.gov.cn/" target="_blank">{{icp}}</a>
  </footer>
</template>

<style scoped>
.footer{
  padding: 20px 0 10px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.footer a{
  color: var(--el-color-primary);
  text-decoration: none;
}
</style>
