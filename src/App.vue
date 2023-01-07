<script setup>
import { RouterLink, RouterView } from "vue-router";
</script>

<template>
  <div class="wrapper">
    <nav>
      <RouterLink to="/" style="font-size: 32px"
        >欢迎来到车牌自动识别主页</RouterLink
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
