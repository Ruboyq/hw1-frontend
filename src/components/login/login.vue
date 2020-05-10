<template>
  <div class="container" v-loading.fullscreen.lock="fullscreenLoading">
    <h1 class="slogan">树莓派连接平台</h1>
    <div class="background"></div>
    <el-card class="login" id="login">
      <div class="logindiv">
        <div class="username">
          <i class="el-icon-message guide-icon"></i>
          <label class="guide">账号：</label>
        </div>
        <el-input class="username" v-model="username" placeholder="请输入用户名"></el-input>
        <div class="username">
          <i class="el-icon-edit guide-icon"></i>
          <label class="guide">密码：</label>
        </div>
        <el-input
          @keyup.enter.native="login"
          type="password"
          class="password"
          v-model="password"
          placeholder="请输入密码"
        ></el-input>
        <div class="button" style="margin-top:30px;">
          <el-button round class="login-button" id="username_login" type="success" @click="login">登录</el-button>
        </div>
      </div>
    </el-card>
  </div>
</template>

<script>
export default {
  name: "Login",
  components: {},
  data() {
    return {
      username: "",
      password: "",
      fullscreenLoading: false
    };
  },
  mounted() {},
  methods: {
    error(val) {
      this.$message({
        message: val,
        type: "error"
      });
    },
    login() {
      if (this.username == null || this.username == "") {
        this.error("用户名不能为空");
        return;
      }
      if (this.password == null || this.password == "") {
        this.error("密码不能为空");
        return;
      }
      console.log(this.username);
      this.axios
        .post(
          this.GLOBAL.BASE_URL + "/user/login",
          {
            userName: this.username,
            password: this.password
          },
          {
            headers: {
              "content-type": "application/json;charset=utf-8"
            }
          }
        )
        .then(response => {
          var result = response.data;
          console.log(result);
          if (result.status == "200") {
            this.$router.push({
              path: "home"
            });
          } else {
            this.error(result.msg);
          }
        })
        .catch(error => {
          console.log(error);
          this.$message({
            type: "error",
            message: "服务器响应超时"
          });
        });
    }
  }
};
</script>

<style scoped>
.container {
  font-family: "Open Sans", sans-serif;
  color: #4a4a4a;
  text-align: center;
  overflow: hidden;
  position: relative;
  min-width: 1150px;
}
.background {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-image: url("../../assets/bg.jpg");
  background-size: cover;
  filter: blur(2px);
}
.slogan {
  color: white;
  font-size: 50px;
  font-weight: 400;
  font-style: bold;
  position: absolute;
  z-index: 999;
  top: 10%;
  left: 0;
  right: 0;
  margin: auto;
}

.login-button {
  float: left;
  width: 100%;
}
#login {
  position: absolute;
  left: 0;
  right: 0;
  bottom: 30%;
  width: 320px;
  height: 300px;
  margin: auto;
  text-align: center;
  background-color: #f5f5f5;
  z-index: 1000;
}
.button,
.username,
.password {
  width: 250px;
  display: inline-block;
  margin-top: 10px;
  text-align: center;
}
.guide {
  font-family: "PingFang SC";
  font-size: Medium;
  margin-left: 5px;
  float: left;
  vertical-align: top;
}
.guide-icon {
  float: left;
  vertical-align: top;
  margin-top: 3px;
}
.el-button--success {
  color: #fff;
  background-color: #3fc36c;
  border-color: #3fc36c;
}

.el-button--success:focus,
.el-button--success:hover {
  background: #43d574;
  border-color: #43d574;
  color: #fff;
}
</style>
