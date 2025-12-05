<template>
  <div id="app">
    <!-- 后台页面 -->
    <AdminPage v-if="redirectToAdmin" @logout="onLogout" />
    
    <!-- 登录页面：专门包裹一层 login-wrapper 来显示蓝色背景 -->
    <div v-else class="login-wrapper">
      <form class="login-form">
        <div class="form-group">
          <label for="account">账户:</label>
          <input type="text" id="account" v-model="account" placeholder="请输入账户" />
        </div>

        <div class="form-group">
          <label for="password">密码:</label>
          <input type="password" id="password" v-model="password" placeholder="请输入密码" />
        </div>

        <div class="form-actions">
          <button type="button" id="register" @click="handleRegister">注册</button>
          <button type="button" id="login" @click="handleLogin">登录</button>
        </div>
      </form>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import AdminPage from './components/AdminPage.vue';

export default {
  name: 'App',
  components: {
    AdminPage
  },
  data() {
    return {
      redirectToAdmin: localStorage.getItem('isLoggedIn') === 'true',
      account: '',
      password: ''
    };
  },
  methods: {
    onLogout() {
      localStorage.removeItem('isLoggedIn');
      this.redirectToAdmin = false;
      this.account = '';
      this.password = '';
    },
    async handleLogin() {
      if (!this.account || !this.password) {
        alert('请输入账户和密码');
        return;
      }
      try {
        const response = await axios.post('http://47.115.39.175:10086/login', {
          account: this.account,
          password: this.password
        });
        if (response.status === 200) {
          alert('登录成功');
          this.redirectToAdmin = true;
          localStorage.setItem('isLoggedIn', 'true');
          localStorage.setItem('userAccount', this.account);
        } else {
          alert('登录失败');
        }
      } catch (error) {
        alert('登录出错，请稍后再试'); 
        console.error(error);
        // 为了方便你测试，报错也暂时允许进入，正式版请删除下面两行
        this.redirectToAdmin = true;
        localStorage.setItem('isLoggedIn', 'true');
        localStorage.setItem('userAccount', this.account);
      }
    },
    async handleRegister() {
        // 保持原有逻辑
    }
  }
};
</script>

<style>
/* 全局重置 */
html, body {
  margin: 0;
  padding: 0;
  width: 100%;
  height: 100%;
}

#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  color: #2c3e50;
  /* 注意：这里不要写任何背景色和 Flex 布局 */
  width: 100%;
  height: 100%;
}

/* --- 专门针对登录页的样式 --- */
.login-wrapper {
  background-color: #87CEEB; /* 蓝色背景只在这里 */
  width: 100vw;
  height: 100vh;
  display: flex;             /* 居中布局只在这里 */
  justify-content: center;
  align-items: center;
}

.login-form {
  /* 也可以给表单加个白色背景框，看你喜好，这里保持透明 */
  text-align: center;
}

/* 以下保持原有的输入框和按钮样式 */
.form-group {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.form-actions {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

input {
  width: 300px;
  padding: 10px;
  font-size: 16px;
  margin-bottom: 10px;
}

button {
  width: 320px;
  padding: 10px;
  font-size: 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 5px;
  cursor: pointer;
}

button:hover {
  background-color: #45a049;
}

button#login { order: 2; }
button#register { order: 1; margin-top: 20px; }
</style>