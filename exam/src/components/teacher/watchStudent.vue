<template>
  <section class="description">
    <p class="title">监考管理</p >
    <div v-if="isExist">
      <p class="content">当前无考试异常信息</p>
    </div>
    <div class="exceptionStu" v-else>
      <p class="title">异常考试信息！</p>
      <p class="content2">当前存在考试异常信息</p>
      <img :src="src" alt="没出来">
    </div>
    <el-button @click="check">哈哈哈哈</el-button>
  </section>
</template>

<script>
export default {
  name: "checkExam",
  data(){
    return{
      isExist: true,
      src:require("../../assets/img/good1.jpg")
    }
  },
  methods: {
    check(){
      this.isExist = !this.isExist
    }
  }
}
</script>

<style lang="less" scoped>
.description {
  margin-left: 40px;
  .title {
    font-size: 22px;
    font-weight: 400;
    color: rgb(31, 47, 61);
  }
  .content {
    width: 600px;
    background-color: rgb(236, 248, 255);
    padding: 16px 32px;
    border-radius: 4px;
    border-left: 5px solid rgb(80, 191, 255);
    margin: 20px 0px;
  }
  .content2 {
    width: 600px;
    background-color: rgb(236, 248, 255);
    padding: 16px 32px;
    border-radius: 4px;
    border-left: 5px solid rgb(255, 80, 135);
    margin: 20px 0px;
  }
}
</style>
