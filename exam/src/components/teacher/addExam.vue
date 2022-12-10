<!-- 添加考试 -->
<template>
  <section class="add">
    <el-form ref="form" :model="form" :rules="rules" label-width="80px">
      <el-form-item label="试卷名称" prop="source">
        <el-input v-model="form.source"></el-input>
      </el-form-item>
      <el-form-item label="介绍" prop="description">
        <el-input placeholder="请说明考试类型"  v-model="form.description"></el-input>
      </el-form-item>
      <el-form-item label="所属学院" prop="institute">
        <el-input v-model="form.institute"></el-input>
      </el-form-item>
      <el-form-item label="所属专业" prop="major">
        <el-input v-model="form.major"></el-input>
      </el-form-item>
      <el-form-item label="年级" prop="grade">
        <el-select v-model="form.grade">
          <el-option label="2019" value="2019"></el-option>
          <el-option label="2020" value="2020"></el-option>
          <el-option label="2021" value="2021"></el-option>
          <el-option label="2022" value="2022"></el-option>
        </el-select>
      </el-form-item>
      <el-form-item label="考试日期" prop="examDate">
        <el-col :span="11">
          <el-date-picker placeholder="选择日期" v-model="form.examDate" style="width: 100%;"></el-date-picker>
        </el-col>
      </el-form-item>
      <el-form-item label="持续时间" prop="totalTime">
        <el-input v-model="form.totalTime"></el-input>
      </el-form-item>
      <el-form-item label="总分" prop="totalScore">
        <el-input v-model="form.totalScore"></el-input>
      </el-form-item>
      <el-form-item label="考试类型" prop="type">
        <el-input v-model="form.type"></el-input>
      </el-form-item>
      <el-form-item label="考生提示">
        <el-input  type="textarea" v-model="form.tips"></el-input>
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
        source: null,
        description: null,
        institute: null,
        major: null,
        grade: null,
        examDate: null,
        totalTime: null,
        totalScore: null,
        type: null,
        tips: null,
        paperId: null,
      },
      rules:{
        source: [
          { required: true, message: '试卷名称不能为空', trigger: 'blur' }
        ],
        institute:[
          {required: true, message: '学院不能为空', trigger: 'blur'}
        ],
        description:[
          {required: true, message: '介绍不能为空', trigger: 'blur'}
        ],
        major:[
          {required: true, message: '专业不能为空', trigger: 'blur'}
        ],
        grade:[
          {required: true, message: '年级不能为空', trigger: 'blur'}
        ],
        examDate:[
          {required: true, message: '日期不能为空', trigger: 'blur'}
        ],
        totalTime:[
          {required: true, message: '时间不能为空', trigger: 'blur'},
          {pattern: /^([1-9]|[1-9][0-9]|[1-9][0-9][0-9])$/, message: '请输入1~999的数字', trigger: ['blur','change']}
        ],
        totalScore:[
          {required: true, message: '总分不能为空', trigger: 'blur'},
          {pattern: /^([1-9]|[1-9][0-9]|[1-9][0][0])$/, message: '请输入1~100的数字', trigger: ['blur','change']}
        ],
        type:[
          {required: true, message: '类型不能为空', trigger: 'blur'}
        ]
      }
    };
  },
  methods: {
    formatTime(date) { //日期格式化
      let year = date.getFullYear()
      let month= date.getMonth()+ 1 < 10 ? "0" + (date.getMonth() + 1) : date.getMonth() + 1;
      let day=date.getDate() < 10 ? "0" + date.getDate() : date.getDate();
      let hours=date.getHours() < 10 ? "0" + date.getHours() : date.getHours();
      let minutes=date.getMinutes() < 10 ? "0" + date.getMinutes() : date.getMinutes();
      let seconds=date.getSeconds() < 10 ? "0" + date.getSeconds() : date.getSeconds();
      // 拼接
      return year+"-"+month+"-"+day+" "+hours+":"+minutes+":"+seconds;
    },
    onSubmit() {
      this.$refs.form.validate((valid) => {
        if (valid) {
          this.loading=true;
          alert('submit!');
          let examDate = this.formatTime(this.form.examDate)
          this.form.examDate = examDate.substr(0,10)
          this.$axios(`/api/examManagePaperId`).then(res => {
            this.form.paperId = res.data.data.paperId + 1 //实现paperId自增1
            this.$axios({
              url: '/api/exam',
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
                this.$router.push({path: '/selectExam'})
              }
            })
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

