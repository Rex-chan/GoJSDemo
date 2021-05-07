<template>
  <el-menu default-active="1" class="el-menu-vertical-demo" router>
    <el-menu-item index="/">
      <i class="el-icon-menu"></i>
      <span slot="title">导航1</span>
    </el-menu-item>
    <el-menu-item index="/workflow2">
      <i class="el-icon-menu"></i>
      <span slot="title">导航二</span>
    </el-menu-item>
  </el-menu>
</template>

<script>
export default {
  name: "el-menu",
};
</script>

<style>
</style>
