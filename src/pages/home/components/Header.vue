<template>
  <div class="header">
    <div class="header-left"><i class="el-icon-share"></i></div>
    <div class="header-right">
      <template v-if="login">
        <el-button type="primary" plain>登录</el-button>
        <el-button type="primary" plain>注册</el-button>
      </template>
      <template v-else>
        <el-button type="primary" plain>退出登录</el-button>
        <el-button type="danger" plain>购物车</el-button>
      </template>
    </div>
  </div>
</template>
<script>
export default {
  name:'HomeHeader',
  data () {
    return {
      login: true
    }
  }
}
</script>
<style lang="less" scoped>
.header {
  height: 50px;
  display: flex;
  justify-content: space-between;
  &-left {
    margin-left: 10px;
    line-height: 50px;
    width: 100px;
    font-size:40px;
    color:blue;
  }
  &-right {
    line-height: 50px;
    margin-right:10px;
    min-width:100px;
  }
}
</style>
