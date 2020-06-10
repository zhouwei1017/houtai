<template>
  <el-row
    type='flex'
    align='middle'
    justify='center'
  >
    <el-col :span="8">
      <el-form
        :model="loginForm"
        :rules="rules"
        ref="loginForm"
        class="demo-ruleForm"
      >
        <el-form-item
          label="用户名"
          prop="username"
        >
          <el-input v-model="loginForm.username"></el-input>
        </el-form-item>
        <el-form-item
          label="密码"
          prop="password"
        >
          <el-input v-model="loginForm.password"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button
            type="primary"
            @click="startLogin('loginForm')"
          >登陆</el-button>
          <el-button @click="resetForm('loginForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </el-col>
  </el-row>
</template>

<script>
import axios from 'axios'
export default {
  data () {
    return {
      loginForm: {
        username: 'admin',
        password: '123456'
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在 3 到 5 个字符', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 6, max: 10, message: '请输入6-10字符', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    startLogin () {
      this.$refs.loginForm.validate((valid) => {
        if (!valid) {
          this.$message({
            message: '登陆失败',
            type: 'warning'
          })
        }

        axios.post('http://localhost:8888/api/private/v1/login', this.loginForm).then(res => {
          if (res.data.meta.status === 200) {
            this.$message({
              message: '登陆成功',
              type: 'success'
            })
            this.$router.push('/home')
          }
        })
      })
    },
    resetForm () {
      this.$refs.loginForm.resetFields()
    }
  }
}
</script>

<style lang='less' scoped>
* {
  margin: 0;
  padding: 0;
}
html,
body,
#app {
  height: 100%;
}
.el-row {
  height: 100%;
  background-color: blue;
  .el-form {
    padding: 10px 20px;
    background-color: #fff;
    border-radius: 20px;
  }
}
</style>
