



<template>
  <div>
    <Head title="注册" :show="true"></Head>
    <div class="register">
      <van-cell-group>
        <van-field
          placeholder="请输入用户名"
          label="用户名"
          left-icon="contact"
          v-model="registerInfo.username"
          name="username"
          v-validate="'required|name'"
          :error="errors.has('username')"
          :error-message="errors.first('username')"
        />
      </van-cell-group>
      <van-cell-group>
        <van-field
          placeholder="请输入密码"
          type="password"
          label="密码"
          left-icon="browsing-history-o"
          v-model="registerInfo.password"
          name="password"
          v-validate="'required|pwd'"
          :error="errors.has('password')"
          :error-message="errors.first('password')"
        />
      </van-cell-group>
      <van-cell-group>
        <van-field
          placeholder="请输入手机号"
          label="手机号"
          left-icon="phone-o"
          v-model="registerInfo.tel"
          name="phone"
          v-validate="'required|mobile'"
          :error="errors.has('phone')"
          :error-message="errors.first('phone')"
        />
      </van-cell-group>
      <van-button type="danger" round size="large" class="btn" @click="register()">注册</van-button>
    </div>
  </div>
</template>

<script>
import { Validator } from "vee-validate";
import { Toast } from "vant";
export default {
  data() {
    return {
      registerInfo: {},
      selected: "",
    };
  },
  methods: {
    register() {
      this.$validator.validateAll().then(required => {
        if (required) {
          this.$axios.post("/vue/register", this.registerInfo).then(res => {
            console.log(res.data);
            if (!!res.data.type) {
              window.sessionStorage.userInfo = JSON.stringify({
                token: res.data.token
              });
            }
          });
          this.$router.push({ name: "login" });
          this.$store.state.username = this.registerInfo.username;
          localStorage.username = this.registerInfo.username;
        }
    });
  },
}
}
</script>

<style scoped>
.register {
  background-color: #ffffff;
  width: 100%;
  height: 90%;
  position: absolute;
  left: 0;
  top: 1rem;
  right: 0;
  bottom: 0;
}
.col {
  line-height: 0.8rem;
  color: red;
}
</style>
