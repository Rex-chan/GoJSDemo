<template>
  <div id="app">
    <el-container style="height: 100%; border: 1px solid #eee; padding: 50px">
      <el-aside width="200px">
        <el-menu default-active="1" class="el-menu-vertical-demo" router>
          <el-menu-item index="/">
            <span slot="title">demo1</span>
          </el-menu-item>
          <el-menu-item index="/workflow2">
            <span slot="title">demo2</span>
          </el-menu-item>
          <el-menu-item index="/tree_layout">
            <span slot="title">demo3</span>
          </el-menu-item>
        </el-menu>
      </el-aside>
      <el-container>
        <el-main height="100%">
          <router-view />
        </el-main>
      </el-container>
    </el-container>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {};
  },
  methods: {},
};
</script>

<style >
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  height: 100%;
}
</style>
