<template>
  <div id="admin-page">
    <!-- 左侧菜单：绝对定位到左上角 -->
    <nav class="left-menu">
      <ul>
        
        <!-- === 菜单 1：推广管理 === -->
        <li>
          <div class="menu-title" @click="toggleSubMenu">
            推广管理
            <span class="arrow">{{ showSubMenu ? '▼' : '▶' }}</span>
          </div>
          
          <ul v-if="showSubMenu" class="sub-menu">
            <li 
              :class="{ 'active': activeItem === 'account' }" 
              @click="selectMenu('account')"
            >
              账户列表
            </li>
            <li 
              :class="{ 'active': activeItem === 'order' }" 
              @click="selectMenu('order')"
            >
              订单列表
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

      </ul>
    </nav>

    <!-- 右侧按钮：保持不变 -->
    <div class="right-actions">
      <button class="logout-btn" @click="handleLogout">退出</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'AdminPage',
  data() {
    return {
      // 各个一级菜单的折叠状态
      showSubMenu: true,      // 推广管理
      showConsoleMenu: false, // 控制台
      showClientMenu: false,  // 客户端管理 (新增)
      
      // 当前选中的菜单项ID
      activeItem: 'account' 
    };
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
    
    // 统一的选择菜单逻辑
    selectMenu(itemName) {
      this.activeItem = itemName;
      console.log('当前选中:', itemName);
    },
    
    handleLogout() {
      this.$emit('logout');
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

.left-menu {
  position: absolute;
  top: 20px;
  left: 30px; 
  width: 200px;
  text-align: left;
}

.right-actions {
  position: absolute;
  top: 20px;
  right: 30px; 
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
</style>