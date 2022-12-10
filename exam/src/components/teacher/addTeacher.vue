<!-- 添加教师 -->
<template>
  <section class="add">
    <el-form ref="form" :model="form" :rules="rules" label-width="80px">
      <el-form-item label="姓名" prop="teacherName">
        <el-input v-model="form.teacherName"></el-input>
      </el-form-item>
      <el-form-item label="学院" prop="institute">
        <el-input v-model="form.institute"></el-input>
      </el-form-item>
      <el-form-item label="性别" prop="sex">
        <el-select v-model="form.sex" placeholder="请选择性别">
          <el-option label="男" value="男"></el-option>
          <el-option label="女" value="女"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="电话号码" prop="tel">
        <el-input v-model="form.tel"></el-input>
      </el-form-item>
      <el-form-item label="密码" prop="pwd">
        <el-input placeholder="密码为6-16位字母和数字组合" v-model="form.pwd" show-password></el-input>
      </el-form-item>
      <el-form-item label="身份证号" prop="cardId">
        <el-input v-model="form.cardId"></el-input>
      </el-form-item>
      <el-form-item label="职称" prop="type">
        <el-select v-model="form.type" placeholder="请选择职称">
          <el-option label="教授" value="教授"></el-option>
          <el-option label="副教授" value="副教授"></el-option>
          <el-option label="讲师" value="讲师"></el-option>
          <el-option label="助教" value="助教"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item>
        <el-button type="primary" @click="onSubmit()">立即创建</el-button>
        <el-button type="text" @click="cancel()">取消</el-button>
      </el-form-item>
    </el-form>
  </section>
</template>

<script>
export default {
  data() {
    return {
      form: { //表单数据初始化
        studentName: null,
        grade: null,
        major: null,
        clazz: null,
        institute: null,
        tel: null,
        email: null,
        pwd: null,
        cardId: null,
        sex: null,
        role: 2
      },
      rules:{
        teacherName:[{ required: true, message: '姓名不能为空', trigger: 'blur' },
          { min: 1, max: 10, message: '长度在 1 到 10 个字符', trigger: 'blur' }
        ],
        institute:[
          {required: true, message: '学院不能为空', trigger: 'blur'}
        ],
        sex:[
          {required: true, message: '性别不能为空', trigger: 'blur'}
        ],
        tel:[
          {required: true, message: '电话不能为空', trigger: 'blur'},
          {pattern: /^((0\d{2,3}-\d{7,8})|(1[34578]\d{9}))$/, message: '请输入正确的电话格式', trigger: ['blur','change']}
        ],
        cardId:[
          { required: true, message: '请输入身份证号', trigger: 'blur' },
          { pattern: /(^\d{15}$)|(^\d{18}$)|(^\d{17}(\d|X|x)$)/, message: '请输入正确的身份证号', trigger: ['blur','change']}
        ],
        pwd:[
          { required: true, message: '密码不能为空', trigger: 'blur' },
          {pattern: /^(?![0-9]+$)(?![a-zA-Z]+$)[0-9A-Za-z]{6,16}$/,message: '密码格式不正确', trigger: ['blur','change'] }
        ],
        type:[
          {required: true, message: '职称不能为空', trigger: 'blur'}
        ]
      },
    };
  },

  methods: {
    onSubmit(form) { //数据提交

      this.$refs.form.validate((valid) => {
        if (valid) {
          this.loading=true;
          alert('submit!');

          this.$axios({
            url: '/api/teacher',
            method: 'post',
            data: {
              ...this.form
            }
          }).then(res => {
            if(res.data.code == 200) {
              this.$message({
                message: '数据添加成功',
                type: 'success'
              })
              this.$router.push({path: '/teacherManage'})
            }
          })
        } else {
          console.log('error submit!!');
          return false;
        }
      })

    },
    cancel() { //取消按钮
      this.$refs.form.resetFields();
    },

  }
};
</script>
<style lang="less" scoped>
.add {
  padding: 0px 40px;
  width: 400px;
}
</style>

