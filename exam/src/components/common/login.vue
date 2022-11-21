<!-- 用户登录界面 -->
<template>
  <div id="login">
    <div class="bg"></div>
    <el-row class="main-container">
      <div class="pic-frame">
    <div class="shell">
        <ul class="images">
            <li class="img"></li>
            <li class="img"></li>
            <li class="img"></li>
            <li class="img"></li>
        </ul>
        <ul class="min-images">
            <lin class="min"></lin>
            <lin class="min"></lin>
            <lin class="min"></lin>
            <lin class="min"></lin>
        </ul>
        <div class="button">
            <div class="button-left"><</div>
            <div class="button-right">></div>
        </div>
    </div>
</div>
      <el-col :lg="8" :xs="16" :md="10" :span="10">
        <div class="top">
          <i class="iconfont icon-kaoshi"></i><span class="title">在线考试系统</span>
        </div>
        <div class="bottom">
          <div class="container">
            <p class="title">账号登录</p>
            <el-form :label-position="labelPosition" label-width="80px" :model="formLabelAlign">
              <el-form-item label="用户名">
                <el-input v-model.number="formLabelAlign.username" placeholder="请输入用户名"></el-input>
              </el-form-item>
              <el-form-item label="密码">
                <el-input v-model="formLabelAlign.password" placeholder="请输入密码" type='password'></el-input>
              </el-form-item>
              <div class="submit">
                <el-button type="primary" class="row-login" @click="login()">登录</el-button>
              </div>
              <!-- <div class="options">
                <p class="find"><a href="javascript:;">找回密码</a></p>
                <div class="register">
                  <span>没有账号?</span>
                  <span><a href="javascript:;">去注册</a></span>
                </div>
              </div> -->
            </el-form>
          </div>
        </div>
      </el-col>
    </el-row>
    <el-row class="footer">
      <el-col>
        <p class="msg2">版权所有 ©2022 战神小队保留所有权利</p>
      </el-col>
    </el-row>
    <section class="remind">
      <span>管理员账号：9527</span>
      <span>教师账号：20081001</span>
      <span>密码都是：123456</span>
    </section>
  </div>
</template>
<script>
import {mapState} from 'vuex'
export default {
  name: "login",
  data() {
    return {
      role: 2,
      labelPosition: 'left',
      formLabelAlign: {
        username: '20154084',
        password: '123456'
      }
    }
  },
  methods: {
    //用户登录请求后台处理
    login() {
      console.log("登录操作执行-------");
      this.$axios({
        url: `/api/login`,
        method: 'post',
        data: {
          ...this.formLabelAlign
        }
      }).then(res=>{
        let resData = res.data.data
        if(resData != null) {
          switch(resData.role) {
            case "0":  //管理员
              this.$cookies.set("cname", resData.adminName)
              this.$cookies.set("cid", resData.adminId)
              this.$cookies.set("role", 0)
              this.$router.push({path: '/index' }) //跳转到首页
              break
            case "1": //教师
              this.$cookies.set("cname", resData.teacherName)
              this.$cookies.set("cid", resData.teacherId)
              this.$cookies.set("role", 1)
              this.$router.push({path: '/index' }) //跳转到教师用户
              break
            case "2": //学生
              this.$cookies.set("cname", resData.studentName)
              this.$cookies.set("cid", resData.studentId)
              this.$router.push({path: '/student'})
              break
          }
        }
        if(resData == null) { //错误提示
          this.$message({
            showClose: true,
            type: 'error',
            message: '用户名或者密码错误'
          })
        }
      })
    },
    clickTag(key) {
      this.role = key
    }
  },
  computed: mapState(["userInfo"]),
  mounted() {
    let left = document.querySelector('.button-left')
    let right = document.querySelector('.button-right')
    let min = document.querySelectorAll('.min')
    let images = document.querySelector('.images')
    let index = 0
    let time

    function position(){
        images.style.left = (index * -100)+"%"
    }
    function add(){
        if(index>=min.length-1){
            index = 0;
        }else {
            index++
        }
    }
    function desc(){
        if(index<1){
            index = min.length-1;
        }else {
            index--
        }
    }
    function timer(){
        time = setInterval(()=>{
            index++
            desc()
            add()
            position()
        },3000)
    }
    left.addEventListener('click',()=>{
        desc()
        position()
        clearInterval(time)
        timer()
    })
    right.addEventListener('click',()=>{
        add()
        position()
        clearInterval(time)
        timer()
    })
  }
}
</script>

<style lang="less" scoped>
.remind {
  border-radius: 4px;
  padding: 10px 20px;
  display: flex;
  position: fixed;
  right: 20px;
  bottom: 50%;
  flex-direction: column;
  color: #606266;
  background-color: #fff;
  border-left: 4px solid #409eff;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19)
}
.container {
  margin-bottom: 32px;
}
.container .el-radio-group {
  margin: 30px 0px;
}
a:link {
  color:#ff962a;
  text-decoration:none;
}
#login {
  font-size: 14px;
  color: #000;
  background-color: #fff;
}
#login .bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  overflow-y: auto;
  height: 100%;
  background: url('../../assets/img/loginbg.png')center top / cover no-repeat;
  background-color: #b6bccdd1 !important;
}
#login .main-container {
  display: flex;
  justify-content: center;
  align-items: center;
}
#login .main-container .top {
  margin-top: 100px;
  font-size: 30px;
  color: #ff962a;
  display: flex;
  justify-content: center;
}
#login .top .icon-kaoshi {
  font-size: 80px;
}
#login .top .title {
  margin-top: 20px;
}
#login .bottom {
  display:flex;
  justify-content: center;
  background-color:#fff;
  border-radius: 5px;
  box-shadow: 0 4px 8px 0 rgba(0, 0, 0, 0.2), 0 6px 20px 0 rgba(0, 0, 0, 0.19);
}
#login .bottom .title {
  text-align: center;
  font-size: 30px;
}
.bottom .container .title {
  margin: 30px 0px;;
}
.bottom .submit .row-login {
  width: 100%;
  background-color: #04468b;
  border-color: #04468b;
  margin: 20px 0px 10px 0px;
  padding: 15px 20px;
}
.bottom .submit {
  display: flex;
  justify-content: center;
}
.footer {
  text-align: center;
}
.footer .msg1 {
  font-size: 18px;
  color: #fff;
  margin-bottom: 15px;
}
.footer .msg2 {
  font-size: 14px;
  color: #e3e3e3;
  margin-top: 70px;
}
.bottom .options {
  margin-bottom: 40px;
  color: #ff962a;
  display: flex;
  justify-content: space-between;
}
.bottom .options > a {
  color: #ff962a;
}
.bottom .options .register span:nth-child(1) {
  color: #8C8C8C;
}
body{
    display: flex;
    height: 100vh;
    align-items: center;
    justify-content: center;
    background-color: rgb(170,190,250);
}
ul li{
    margin-top: 0px;
    padding-left: 0px;
    list-style: none;
}
.pic-frame{
    margin-top: 150px;
    margin-right: 50px;
}
.login-frame{
    width: 500px;
    height: 400px;
    border-radius: 5px;
    border: 10px #fff solid;
    box-shadow: 20px 30px 20px rgba(0,0,0,.5);
}
.shell{
    width: 500px;
    height:400px;
    position: relative;
    overflow-x: hidden;
    border-radius: 5px;
    border: 10px #fff solid;
    box-shadow: 20px 30px 20px rgba(0,0,0,.5);
}
.images{
    width:400%;
    height:100%;
    display: flex;
    position: absolute;
    padding: 0px;
    margin: 0px;
    transition: .2s;
}
.img{
    width: 100%;
    background-size:cover ;
}
.img:nth-child(1){
    background-image: url("../../assets/img/cry1.gif");
}
.img:nth-child(2){
    background-image: url("../../assets/img/cry2.jpg");
}
.img:nth-child(3){
    background-image: url("../../assets/img/good1.jpg");
}
.img:nth-child(4){
    background-image: url("../../assets/img/userimg.png");
}
.min-images{
    display: flex;
    justify-content: space-evenly;
    position: absolute;
    bottom: 20px;
    width: 40%;
    z-index:999 ;
    right: 10%;
}
.min{
    width: 30px;
    height: 30px;
    cursor:pointer;
    border-radius: 50%;
    background-size: cover;
    border: solid 5px rgba(255,255,255,0.5);
    background-position: -20px 0;
}
.min:nth-child(1){
    background-image: url("../../assets/img/cry1.gif");
}
.min:nth-child(2){
    background-image: url("../../assets/img/cry2.jpg");
}
.min:nth-child(3){
    background-image: url("../../assets/img/good1.jpg");
}
.min:nth-child(4){
    background-image: url("../../assets/img/userimg.png");
}
.button{
    width: 100%;
    height: 100%;
    position: absolute;
    display: flex;
    justify-content: space-between;
    user-select: none;
}
.button-left,.button-right{
    font-size: 50px;
    background-color: rgba(100,190,255,0.7);
    padding: 30px 20px;
    line-height: 300px;
    cursor: pointer;
}

</style>
