<template>
  <div class="login">
    <div class="container">
      <a href="/#/index">
        <img src="/imgs/login-logo.png" alt />
      </a>
    </div>
    <div class="wrapper">
      <div class="container">
        <div class="login-form">
          <h3>
            <span class="checked">账号登陆</span>
            <span class="sep-line">|</span>
            <span class="checked">扫码登陆</span>
          </h3>
          <div class="input">
            <input type="text" placeholder="请输入账号" v-model="username" />
          </div>
          <div class="input">
            <input type="password" placeholder="请输入密码" v-model="password" />
          </div>
          <div class="btn-box">
            <a href="javascript:;" class="btn" @click="login">登录</a>
          </div>
          <div class="tips">
            <div class="sms" @click="register">手机短信登录/注册</div>
            <div class="reg">
              立即注册
              <span>|</span>忘记密码?
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="footer">
      <div class="footer-link">
        <a href="https://www.mi.com/index.html" target="_blank">小米商城</a>
        <span>|</span>
        <a href="https://www.miui.com" target="_blank">MIUI</a>
        <span>|</span>
        <a href="https://home.mi.com/index.html" target="_blank">米家</a>
        <span>|</span>
        <a href="www.duokan.com" target="_blank">多看</a>
        <span>|</span>
        <a href="https://xiaomi.tmall.com" target="_blank">小米天猫店</a>
        <span>|</span>
        <a href="https://miwifi.com" target="_bhome.mi.com/index.htmllank">路由器</a>
      </div>
      <div class="copyright">
        京ICP证110507号 京ICP备10046444号
        <span class="domain">京公网安备11010802020134号</span> 京网文[2020]0276-042号
      </div>
    </div>
  </div>
</template>

<script>
import { mapActions } from "vuex";
import {Message} from 'element-ui';
export default {
  name: "login",
  data() {
    return {
      username: "",
      password: "",
      userId: ""
    };
  },
  methods: {
    login() {
      let { username, password } = this;
      this.axios
        .post("/user/login", {
          username,
          password
        })
        .then(res => {
          this.$cookie.set("userId", res.id, { expires: "Session" });
          // to-do 保存用户名  dispatch  派发 saveUserName 这个方法
          //  this.$store.dispatch('saveUserName', res.username);
          this.saveUserName(res.username);
          this.$router.push("/index");
          this.getCartCount();
        });
    },
    ...mapActions(["saveUserName"]),
    register() {
      this.axios
        .post("/user/register", {
          username: "chenng22",
          password: "chenng22",
          email: "chenng22@163.com"
        })
        .then(() => {
          this.$message.success("注册成功");
          // Message.success("注册成功");
        });
    },
    getCartCount() {
      this.axios.get("/carts/products/sum").then((res = 0) => {
        // to-do 保存到vuex里面
        this.$store.dispatch("saveCartCount", res);
      });
    }
  }
};
</script>

<style lang="scss">
.login {
  & > .container {
    height: 113px;
    img {
      width: auto;
      height: 100%;
    }
  }
  .wrapper {
    background: url("/imgs/login-bg.jpg") no-repeat center;
    .container {
      height: 576px;
      .login-form {
        box-sizing: border-box;
        padding: 0 31px;
        width: 410px;
        height: 510px;
        background-color: #ffffff;
        position: absolute;
        bottom: 29px;
        right: 0;
        h3 {
          line-height: 23px;
          font-size: 24px;
          text-align: center;
          margin: 40px auto 49px;
          .checked {
            color: EFF660;
          }
          .sep-line {
            margin: 0 32px;
          }
        }
        .input {
          display: inline-block;
          width: 348px;
          height: 50px;
          border: 1px solid #e5e5e5;
          margin-bottom: 20px;
          input {
            width: 100%;
            height: 100%;
            border: none;
            padding-left: 18px;
          }
        }
        .btn {
          width: 100%;
          line-height: 50px;
          margin-top: 10px;
          font-size: 16px;
        }
        .tips {
          margin-top: 14px;
          display: flex;
          justify-content: space-between;
          font-size: 14px;
          cursor: pointer;
          .sms {
            color: #ff6600;
          }
          .reg {
            color: #999999;
            span {
              margin: 0 7px;
            }
          }
        }
      }
    }
  }
  .footer {
    height: 100px;
    padding-top: 60px;
    color: #999999;
    font-size: 16px;
    text-align: center;
    .footer-link {
      a {
        color: #999999;
        display: inline-block;
      }
      span {
        margin: 0 10px;
      }
    }
    .copyright {
      margin-top: 13px;
    }
  }
}
</style>