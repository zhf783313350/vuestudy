<template>
  <div id="admin-page">
    <div class="layout">
      <!-- 左侧菜单 -->
      <nav class="left-menu">
        <ul>
          
          <!-- === 菜单 1：推广管理 === -->
          <li>
            <div class="menu-title" @click="toggleSubMenu">
              班级管理
              <span class="arrow">{{ showSubMenu ? '▼' : '▶' }}</span>
            </div>
            
            <ul v-if="showSubMenu" class="sub-menu">
              <li 
                :class="{ 'active': activeItem === 'account' }" 
                @click="() => { selectMenu('account'); fetchStudentsList(); }">
                学生列表
              </li>
            
            </ul>
          </li>

          <!-- === 菜单 2：控制台 === -->
          <li class="menu-item-spacing">
            <div class="menu-title" @click="toggleConsoleMenu">
              控制台
              <span class="arrow">{{ showConsoleMenu ? '▼' : '▶' }}</span>
            </div>
        
            <ul v-if="showConsoleMenu" class="sub-menu">
              <li 
                :class="{ 'active': activeItem === 'permission' }" 
                @click="selectMenu('permission')"
              >
                权限管理
              </li>
              <li 
                :class="{ 'active': activeItem === 'role' }" 
                @click="selectMenu('role')"
              >
                角色管理
              </li>
              <li 
                :class="{ 'active': activeItem === 'user' }" 
                @click="selectMenu('user')"
              >
                用户管理
              </li>
            </ul>
          </li>

          <!-- === 菜单 3：客户端管理 (新增) === -->
          <li class="menu-item-spacing">
            <div class="menu-title" @click="toggleClientMenu">
              客户端管理
              <span class="arrow">{{ showClientMenu ? '▼' : '▶' }}</span>
            </div>
            
            <ul v-if="showClientMenu" class="sub-menu">
              <li 
                :class="{ 'active': activeItem === 'clientList' }" 
                @click="selectMenu('clientList')"
              >
                客户端列表
              </li>
            </ul>
          </li>

          <li class="menu-item-FeeManage">
            <div class="menu-title" @click="toggleFeeManagementMenu">
              收费管理
              <span class="arrow">{{ showFeeManagementMenu ? '▼' : '▶' }}</span>
            </div>
            <ul v-if="showFeeManagementMenu" class="sub-menu">
              <li 
                :class="{ 'active': activeItem === 'clientUserList' }" 
                @click="selectMenu('clientUserList')"
              >
                客户列表
              </li>
            </ul>
        <ul v-if="showFeeManagementMenu" class="sub-menu">
              <li 
                :class="{ 'active': activeItem === 'clientRechargeList' }" 
                @click="selectMenu('clientRechargeList')"
              >
                充值列表
              </li>
            </ul>
          <ul v-if="showFeeManagementMenu" class="sub-menu">
              <li 
                :class="{ 'active': activeItem === 'clientPackageList' }" 
                @click="selectMenu('clientPackageList')"
              >
                套餐列表
              </li>
            </ul>
          </li>
        </ul>
      </nav>

      <!-- 右侧内容区域 -->
      <div class="content">
        <!-- 学生信息展示区域 -->
        <div v-if="students.length > 0">
          <table>
            <thead>
              <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Age</th>
                <th>Email</th>
                <th>Address</th>
                <th>ClassID</th>
                <th>Time</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(student, index) in students" :key="index">
                <td>{{ index + 1 }}</td>
                <td>{{ student.name }}</td>
                <td>{{ student.age }}</td>
                <td>{{ student.email }}</td>
                <td>{{ student.address }}</td>
                <td>{{ student.classid }}</td>
                <td>{{ student.time }}</td>
              </tr>
            </tbody>
          </table>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
import StudentList from './StudentList.vue';

export default {
  name: 'AdminPage',
  components: {
    StudentList
  },
  data() {
    return {
      // 各个一级菜单的折叠状态
      showSubMenu: true,      // 推广管理
      showConsoleMenu: false, // 控制台
      showClientMenu: false,  // 客户端管理 (新增)
      showFeeManagementMenu: false, // 收费管理
      // 当前选中的菜单项ID
      activeItem: 'account' ,
        userAccount: '' ,
      students: [] // 存储学生信息的数组
    };
  },
  mounted(){
    this.userAccount = localStorage.getItem('userAccount') || '';
    this.fetchStudentsList(); // 示例班级ID，实际应用中应根据情况动态获取
  },
  methods: {
    // 切换 推广管理
    
    toggleSubMenu() {
      this.showSubMenu = !this.showSubMenu;
    },
    // 切换 控制台
    toggleConsoleMenu() {
      this.showConsoleMenu = !this.showConsoleMenu;
    },
    // 新增：切换 客户端管理
    toggleClientMenu() {
      this.showClientMenu = !this.showClientMenu;
    },
        toggleFeeManagementMenu() {
      this.showFeeManagementMenu = !this.showFeeManagementMenu;
    },
    // 统一的选择菜单逻辑
    selectMenu(itemName) {
      this.activeItem = itemName;
      console.log('当前选中:', itemName);
      if(itemName=="班级管理"){
        this.fetchStudentsList();
      }
    },
    
    handleLogout() {
      this.$emit('logout');
    },
    async fetchStudentsList() {
  const rawClassID = localStorage.getItem('classID');
  console.log('从 localStorage 获取的原始 classID:', rawClassID);

  if (!rawClassID) {
    console.error('班级ID未找到，请先登录');
    return;
  }

  const classID = parseInt(rawClassID, 10); // 转换为整数
  if (isNaN(classID) || classID <= 0) {
    console.error('班级ID无效，请检查后端返回的数据');
    return;
  }

  try {
    const response = await axios.post('http://47.115.39.175:8080/studentslist', { classID });
    if (response.status === 200) {
      this.students = response.data.data; // 确保从 data.data 中获取学生列表
      console.log('学生列表:', this.students);
    } else {
      console.error('获取学生列表失败:', response.data.message || '未知错误');
    }
  } catch (error) {
    console.error('获取学生列表出错:', error.response?.data?.message || '请稍后再试');
  }
}
  }
};
</script>

<style scoped>
#admin-page {
  position: fixed; 
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: white; 
  z-index: 999; 
  box-sizing: border-box;
}

.layout {
  display: flex;
  height: 100vh;
}

.left-menu {
  width: 200px;
  background-color: #f4f4f4;
  padding: 10px;
  box-shadow: 2px 0 5px rgba(0, 0, 0, 0.1);
}

.content {
  flex: 1;
  padding: 20px;
  overflow-y: auto;
}

table {
  width: 100%;
  border-collapse: collapse;
}

th, td {
  border: 1px solid #ddd;
  padding: 8px;
  text-align: left;
}

th {
  background-color: #f2f2f2;
}

.right-actions {
  position: absolute;
  top: 20px;
  right: 30px; 
  display: flex;
  flex-direction: column;
  align-items: flex-end; 
}
.user-info {
  margin-bottom: 10px;
  font-size: 14px;
  color: #666;
}
.username {
  font-weight: bold;
  color: #333;
}
/* 增加一级菜单之间的间距 */
.menu-item-spacing {
  margin-top: 15px; 
}
.menu-title {
  font-size: 18px;
  font-weight: bold;
  cursor: pointer;
  padding: 5px 0;
  color: #000;
  user-select: none;
  display: flex;       
  align-items: center;
}
.arrow {
  font-size: 12px;
  margin-left: 8px;
  color: #999;
}

.sub-menu {
  margin-top: 5px;
  padding-left: 20px;
  list-style: none;
}
.sub-menu li {
  font-size: 16px;
  padding: 8px 0;
  cursor: pointer;
  color: #333; 
  transition: color 0.2s;
}
/* 选中状态变蓝 */
.sub-menu li.active {
  color: #409EFF; 
  font-weight: bold;
}
.sub-menu li:hover {
  color: #66b1ff; 
}
.logout-btn {
  width: auto !important;
  padding: 8px 25px;
  background-color: #ff4d4f;
  color: white;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  cursor: pointer;
}
.logout-btn:hover {
  background-color: #ff7875;
}
/* 学生信息展示区域样式 */
.student-info {
  position: absolute;
  top: 60px; /* 根据需要调整位置 */
  right: 30px;
  width: 300px; /* 根据需要调整宽度 */
  background-color: #f9f9f9;
  border: 1px solid #ddd;
  border-radius: 4px;
  padding: 15px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
.student-info h2 {
  font-size: 18px;
  margin-bottom: 10px;
  color: #333;
}
.student-info ul {
  list-style: none;
  padding: 0;
  margin: 0;
}
.student-info li {
  margin-bottom: 10px;
  font-size: 14px;
  color: #666;
}
.student-info strong {
  color: #333;
}
</style>