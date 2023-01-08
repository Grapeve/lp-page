<script setup>
import { RouterLink, RouterView } from "vue-router";
import { ref } from "vue";
const dialogVisible = ref(true);
</script>

<template>
  <el-dialog
    v-model="dialogVisible"
    title="🚗欢迎来到车牌识别页面 ~"
    width="30%"
  >
    <div style="margin-top: -15px">
      <span
        >✨点击
        <span style="font-size: 24px; font-weight: 800">+</span>
        图标即可上传图片</span
      >
    </div>
    <br />
    <span>🎉车牌识别到页面显示可能会花费2~4s，小小等一下哦 ~</span>
    <br /><br />
    <div style="margin-top: 0px">🔎预处理图片点击图片即可放大预览 ~</div>
    <template #footer>
      <span class="dialog-footer">
        <el-button type="primary" @click="dialogVisible = false">
          确认
        </el-button>
      </span>
    </template>
  </el-dialog>
  <div class="wrapper">
    <nav>
      <RouterLink
        to="/"
        style="
          font-size: 32px;
          letter-spacing: 1.5px;
          color: hsla(160, 100%, 37%, 1);
        "
        >🚗欢迎来到车牌自动识别页☀️</RouterLink
      >
      <!-- <RouterLink to="/about">About</RouterLink> -->
    </nav>
  </div>

  <RouterView />
</template>

<style scoped>
.logo {
  display: block;
  margin: 0 auto 2rem;
}

nav {
  width: 100%;
  font-size: 18px;
  text-align: center;
  margin-top: 0.5rem;
}

nav a.router-link-exact-active {
  color: var(--color-text);
}

nav a.router-link-exact-active:hover {
  background-color: transparent;
}

nav a {
  display: inline-block;
  padding: 0 1rem;
  border-left: 1px solid var(--color-border);
}

nav a:first-of-type {
  border: 0;
}
</style>
