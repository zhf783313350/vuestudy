<template>
  <div id="app">
    <!-- 后台页面 -->
    <AdminPage v-if="redirectToAdmin" @logout="onLogout" ref="adminPage" />
    
    <!-- 登录页面：专门包裹一层 login-wrapper 来显示蓝色背景 -->
    <div v-else class="login-wrapper">
      <h1 class="welcome-message">欢迎来到后台管理系统</h1>
      <form class="login-form">
        <div class="form-group">
          <label for="name">账户:</label>
          <input type="text" id="name" v-model="name" placeholder="请输入账户" />
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
      name: localStorage.getItem('name') || '', // 自动填充账号
      password: localStorage.getItem('password') || '' // 自动填充密码
    };
  },
  methods: {
    onLogout() {
      localStorage.removeItem('isLoggedIn');
      this.redirectToAdmin = false;
      this.name = '';
      this.password = '';
    },
    async handleLogin() {
      if (!this.name || !this.password) {
        alert('请输入账户和密码');
        return;
      }
      try {
        const response = await axios.post('http://47.115.39.175:8080/login', {
          name: this.name,
          password: this.password
        });
        console.log('完整的响应数据:', response);
        if (response.status === 200) {
          this.redirectToAdmin = true;
          localStorage.setItem('isLoggedIn', 'true');
          localStorage.setItem('name', this.name);
          localStorage.setItem('password', this.password);
          const classID = response.data.data.classid; // 修正路径，确保正确获取班级ID
          alert('登录成功,班级ID为: ' + classID); // 显示班级ID
          console.log('后端返回的数据:', response.data.data);
          if (classID) {
            localStorage.setItem('classID', classID.toString()); // 确保存储为字符串
          } else {
            console.error('后端未返回有效的班级ID');
          }
        } else {
          alert('登录失败');
        }
      } catch (error) {
        alert('登录出错，请稍后再试'); 
        console.error('登录请求出错:', error);
      }
    },
    async handleRegister() {
      if (!this.name || !this.password) {
        alert('请输入账户和密码');
        return;
      }
      const teacher = {
        name: this.name,
        password: this.password
      };
      try {
        const response = await axios.post('http://47.115.39.175:8080/register', teacher);
        if (response.status === 200) {
          alert('注册成功');
        } else {
          alert(`注册失败: ${response.data.message || '未知错误'}`);
        }
      } catch (error) {
        alert(`注册出错: ${error.response?.data?.message || '请稍后再试'}`);
        console.error(error);
      }
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
  flex-direction: column;    /* 让欢迎语和表单垂直排列 */
}

.login-form {
  /* 也可以给表单加个白色背景框，看你喜好，这里保持透明 */
  text-align: center;
}

/* 新增欢迎语样式 */
.welcome-message {
  font-size: 24px;
  margin-bottom: 20px;
  color: #333; /* 欢迎语颜色 */
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