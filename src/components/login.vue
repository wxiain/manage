<template>
  <div class="login">
    <section class="wrapper">
      <el-form :model="ruleForm" status-icon size="mini" :rules="rules" ref="ruleForm" label-width="70px" class="form">
        <el-form-item label="账号" prop="username">
          <el-input v-model.number="ruleForm.username"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="pass">
          <el-input type="password" v-model="ruleForm.pass" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" @click="submitForm('ruleForm')">登录</el-button>
          <el-button to="/res" @click="handleRegister">注册</el-button>
          <el-button @click="resetForm('ruleForm')">重置</el-button>
        </el-form-item>
      </el-form>
    </section>
  </div>
</template>

<script>
import { mapMutations } from 'vuex';
export default {
  name: 'login',
  data() {
    let checkUsername = (rule, value, callback) => {
      if (!value) {
        callback(new Error('账号不能为空'));
        return;
      }
      if (!/^\w{6,10}$/.test(value)) {
        callback(new Error('账号类型错误, 可以是数字字母, 6-10位'));
      } else {
        callback();
      }
    };
    let validatePass = (rule, value, callback) => {
      if (value === '') {
        callback(new Error('请输入密码'));
      } else {
        if (!/^\w{6,16}$/.test(value)) {
          callback(new Error('密码类型错误, 可以是数字或字母, 6-16位'));
          return false;
        }
        callback();
      }
    };
    return {
      ruleForm: {
        pass: '',
        username: ''
      },
      rules: {
        pass: [{ validator: validatePass, trigger: 'change' }],
        username: [{ validator: checkUsername, trigger: 'change' }]
      }
    };
  },
  methods: {
    ...mapMutations(['login']),
    submitForm(formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          // 具体使用可以参考src/component/res.vue页面的解释
          this.login({
            username: this.ruleForm.username
          });
          this.$message({
            message: '登录成功',
            type: 'success'
          });
          this.$router.replace('/');
        } else {
          this.$message({
            message: '登录失败, 请重试',
            type: 'warning'
          });
          return false;
        }
      });
    },
    handleRegister() {
      this.$router.push('/register');
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    }
  }
};
</script>

<style scoped lang="scss">
.login {
  height: 100%;
  .form {
    width: 400px;
    .is-username {
      text-align: right;
      line-height: 3.5;
      a {
        color: #5fadff;
      }
    }
  }
  .wrapper {
    display: flex;
    justify-content: center;
    align-items: center;
    height: 100%;
    text-align: center;
  }
}
</style>
