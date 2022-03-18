<template>
  <div class="columns py-6">
    <div class="column is-half is-offset-one-quarter">
      <el-card shadow="never">
        <div slot="header" class="has-text-centered has-text-weight-bold">
          用户登录
        </div>
         <!-- 使用v-loading在接口为请求到数据之前，显示加载中，直到请求到数据后消失,防止多次注入数据 -->
         <!-- v-model:通常用于表单上双向数据的绑定，如果除了表单其他组件使用时，起不到任何效果。它还可以实现子组件到父组件的双向数据动态绑定。 -->
         <!-- :model是v-bind:model的缩写。只是将父组件的值传递给和子组件，但是并未实现子组件 -->
                <!-- 和父组件之间的双向数据绑定，当然引用类型除外，子组件改变了引用类型的数据的话，父组件的数据也会跟着改变。 -->
        <div>
          <el-form
            v-loading="loading"   
            :model="ruleForm"
            status-icon
            :rules="rules"
            ref="ruleForm"
            label-width="100px"
            class="demo-ruleForm"
          >
            <el-form-item label="账号" prop="name">
              <el-input v-model="ruleForm.name"></el-input>
            </el-form-item>

            <el-form-item label="密码" prop="pass">
              <el-input
                type="password"
                v-model="ruleForm.pass"
                autocomplete="off"
              ></el-input>
            </el-form-item>

            <el-form-item label="记住" prop="delivery">
              <el-switch v-model="ruleForm.rememberMe"></el-switch>
            </el-form-item>

            <el-form-item>
              <el-button type="primary" @click="submitForm('ruleForm')"
                >提交</el-button
              >
              <el-button @click="resetForm('ruleForm')">重置</el-button>
            </el-form-item>
          </el-form>
        </div>
      </el-card>
    </div>
  </div>
</template>

<script>
export default {
  name: "Login",
  data() {
    return {
      redirect: undefined,
      loading: false,
      ruleForm: {
        name: "",
        pass: "",
        rememberMe: true,
      },
      rules: {
        name: [
          { required: true, message: "请输入账号", trigger: "blur" },
          {
            min: 2,
            max: 15,
            message: "长度在 2 到 15 个字符",
            trigger: "blur",
          },
        ],
        pass: [
          { required: true, message: "请输入密码", trigger: "blur" },
          {
            min: 6,
            max: 20,
            message: "长度在 6 到 20 个字符",
            trigger: "blur",
          },
        ],
      },
    };
  },
  methods: {
    submitForm(formName) {
      this.$refs[formName].validate((valid) => {
        if (valid) {
          this.loading = true;
          this.$store
            .dispatch("user/login", this.ruleForm)
            .then(() => {
              this.$message({
                message: "恭喜你，登录成功",
                type: "success",
                duration: 2000,
              });
              setTimeout(() => {
                this.loading = false;
                this.$router.push({ path: this.redirect || "/" });
              }, 0.1 * 1000);
            })
            .catch(() => {
              this.loading = false;
            });
        } else {
          return false;
        }
      });
    },
    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
  },
};
</script>

<style scoped>
</style> 