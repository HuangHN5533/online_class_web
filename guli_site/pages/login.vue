<template>
  <div class="main">
    <div class="title">
      <a class="active" href="/login">登录</a>
      <span>·</span>
      <a href="/register">注册</a>
    </div>

    <div class="sign-up-container">
      <form action="register">
        <div class="input-prepend restyle">
          <input
            v-model="user.mobile"
            type="text"
            placeholder="手机号">
          <i class="iconfont icon-phone"/>
        </div>
        <div class="input-prepend">
          <input
            v-model="user.password"
            type="password"
            placeholder="密码">
          <i class="iconfont icon-password"/>
        </div>
        <div class="btn">
          <input
            type="button"
            class="sign-in-button"
            value="登录"
            @click="submitLogin()">
        </div>
      </form>
      <!-- 更多登录方式 -->
      <div class="more-sign">
        <h6>社交帐号登录</h6>
        <ul>
          <li>
            <a href="http://localhost:9110/api/ucenter/gitee/login">
              <img
                style="heigth:25px; width:25px; margin:5px 25px 0 0"
                src="https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1596117028589&di=852a2db7528a3dc8e63d5154da2349fc&imgtype=0&src=http%3A%2F%2Fgit.oschina.net%2Fuploads%2F1%2F1_oschina-org.png%3F1524465517">
            </a>
          </li>
          <li><a id="weixin" class="weixin" href="http://localhost:8150/api/ucenter/wx/login"><i class="iconfont icon-weixin"/></a></li>
          <li><a id="qq" class="qq" target="_blank" href="#"><i class="iconfont icon-qq"/></a></li>
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import '~/assets/css/sign.css'
import '~/assets/css/iconfont.css'
import cookie from 'js-cookie'
import loginApi from '~/api/login'

export default {
  layout: 'sign',

  data() {
    return {
      user: {
        mobile: '',
        password: ''
      }
    }
  },

  methods: {
    // 登录
    submitLogin() {
      // 调用登录接口
      loginApi.submitLogin(this.user).then(response => {
        // token存到cookie
        cookie.set('guli_jwt_token', response.data.token, { domain: 'localhost' })

        // 跳转网站首页
        // window.location.href = '/'
        if (document.referrer.indexOf('register') !== -1) { // 如果上一页是注册页
          window.location.href = '/'
        } else {
          // 跳转到上一个访问的页面
          history.go(-1)
        }
      })
    }
  }
}
</script>
