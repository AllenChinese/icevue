<template>
  <div class="user-login">
    <!-- :style="{'background-image':`url(${backgroundImage})`}" -->
    <div class="user-login-bg" :style="{'background-image':`url(${backgroundImage})`}"/>
    <div id="svgContainer" class="user-login-bg--lottie"/>
    <div class="content-wrapper">
      <h2 class="slogan">欢迎使用
        <br>MARK 小程序管理系统
      </h2>
      <div class="form-container">
        <h4 class="form-title">登录</h4>
        <el-form ref="form" :model="user" label-width="0">
          <div class="form-items">
            <el-row class="form-item">
              <el-col>
                <el-form-item :rules="[ { required: true, message: '手机号不能为空'}]" prop="username">
                  <div class="form-line">
                    <i class="el-icon-edit-outline input-icon"/>
                    <el-input
                      v-model="user.username"
                      class="form-input"
                      style="border:none;"
                      placeholder="手机号"
                    />
                  </div>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row class="form-item">
              <el-col>
                <el-form-item :rules="[ { required: true, message: '密码不能为空'}]" prop="password">
                  <div class="form-line">
                    <i class="el-icon-service input-icon"/>
                    <el-input
                      v-model="user.password"
                      type="password"
                      class="form-input"
                      placeholder="密码"
                    />
                  </div>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row class="form-item">
              <el-col>
                <el-form-item>
                  <el-checkbox class="checkbox">记住账号</el-checkbox>
                </el-form-item>
              </el-col>
            </el-row>
            <el-row class="form-item">
              <el-button
                :loading="isLoading"
                type="primary"
                class="submit-btn"
                size="small"
                @click="submitBtn"
              >登 录</el-button>
            </el-row>
          </div>
          <el-row class="tips">
            <a href="/" class="link">立即注册</a>
            <span class="line">|</span>
            <a href="/" class="link">忘记密码</a>
          </el-row>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
import BasicContainer from '@vue-materials/basic-container';
import { getToken } from '@/http/inderface';
import Lottie from 'lottie-web'
const backgroundImage = ''
  // 'https://img.alicdn.com/tfs/TB1zsNhXTtYBeNjy1XdXXXXyVXa-2252-1500.png';
export default {
  name: 'UserLogin',
  components: { BasicContainer },

  data() {
    return {
      backgroundImage: require('../../../../assets/bg_2.png'),
      isLoading: false,
      user: {
        username: '',
        password: '',
      },
    };
  },

  created() {

  },

  mounted () {
    var animItem = Lottie.loadAnimation({
      container: document.getElementById('svgContainer'),
      renderer: 'svg',
      loop: true,
      autoplay: true,
      path: 'https://assets.lottiefiles.com/datafiles/ghTW1F7fewycr7V/data.json'
    });
  },

  methods: {
     submitBtn() {
      this.$refs['form'].validate(async (valid) => {
        if (valid) {
          this.isLoading = true
          window.localStorage.isLogined = true

          let getTokenResult = await getToken(this.user)
          console.log(getTokenResult)
          if (getTokenResult.code === 0) {
            setTimeout(() => {
              this.isLoading = false
              this.$router.push({path: '/'}) // 一定要写？
            }, 2000)
          }
        }
      });
    },
  },
};
</script>

<style lang="scss">
@import "./UserLogin.scss";
.form-input {
  .el-input__inner {
    border: 1px solid #fff !important;
  }
}
</style>
