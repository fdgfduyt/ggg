<template>
  <div id="app">
    <!-- 顶部标题区 -->
    <header class="app-header">
      <h1>ch05-routing｜第 5 章实验台</h1>
      <p class="muted">第 05 课（5.1—5.6）：路由——地址栏变，组件换，页面不刷新</p>
    </header>

    <!-- 导航菜单 -->
    <nav class="top-nav">
      <router-link to="/home">首页</router-link>
      <router-link to="/about">关于</router-link>
      <router-link to="/users">用户列表</router-link>
      <router-link to="/center">用户中心</router-link>
      <router-link to="/named">命名路由</router-link>
      <router-link to="/program">编程式导航</router-link>
      <router-link to="/search">搜索</router-link>
    </nav>

    <!-- 状态信息栏 -->
    <div class="status-bar">
      <span>当前地址：<code>{{ $route.fullPath }}</code></span>
      <span class="divider">|</span>
      <span>登录态：<b :class="isLogin() ? 'text-success' : 'text-danger'">{{ $route.path === '/login' || isLogin() ? '已登录' : '未登录' }}</b> (守卫看 token)</span>
    </div>

    <!-- 路由渲染出口 -->
    <main class="page-content">
      <router-view v-slot="{ Component }">
        <transition name="fade" mode="out-in">
          <component :is="Component" />
        </transition>
      </router-view>
    </main>
  </div>
</template>

<script>
export default {
  name: 'App',
  methods: {
    isLogin() {
      return !!localStorage.getItem('token')
    }
  }
}
</script>

<style scoped>
/* ================= 整体容器 ================= */
#app {
  max-width: 900px;
  margin: 0 auto;
  padding: 40px 20px;
  font-family: "Helvetica Neue", Helvetica, "PingFang SC", "Hiragino Sans GB", "Microsoft YaHei", Arial, sans-serif;
}

/* ================= 头部标题 ================= */
.app-header {
  text-align: center;
  margin-bottom: 30px;
}

.app-header h1 {
  font-size: 32px;
  font-weight: 800;
  margin-bottom: 12px;
  /* 现代感渐变色文字 */
  background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
  -webkit-background-clip: text;
  color: transparent;
}

.muted {
  color: #64748b;
  font-size: 15px;
  margin: 0;
}

/* ================= 顶部导航栏 ================= */
.top-nav {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 12px;
  margin-bottom: 24px;
  padding: 10px;
  background: #ffffff;
  border-radius: 16px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.05), 0 2px 4px -2px rgba(0, 0, 0, 0.05);
}

.top-nav a {
  color: #475569;
  text-decoration: none;
  padding: 8px 20px;
  font-size: 15px;
  font-weight: 600;
  border-radius: 8px;
  background-color: transparent;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

/* 悬停动效 */
.top-nav a:hover {
  background-color: #f1f5f9;
  color: #0f172a;
  transform: translateY(-1px);
}

/* 激活路由，渐变高亮 */
.top-nav a.router-link-active {
  background: linear-gradient(135deg, #42b983 0%, #2e8b57 100%);
  color: #ffffff;
  box-shadow: 0 4px 12px rgba(66, 185, 131, 0.3);
}

/* ================= 状态信息栏 ================= */
.status-bar {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 16px;
  background-color: #fffbeb;
  border: 1px solid #fef08a;
  color: #854d0e;
  padding: 12px 20px;
  border-radius: 12px;
  font-size: 14px;
  margin-bottom: 32px;
}

.status-bar code {
  background-color: #fef3c7;
  color: #b45309;
  padding: 2px 8px;
  border-radius: 6px;
  font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, monospace;
  font-weight: bold;
}

.status-bar .divider {
  color: #fcd34d;
}

.text-success {
  color: #16a34a;
}

.text-danger {
  color: #dc2626;
}

/* ================= 页面内容区 (路由出口) ================= */
.page-content {
  background: #ffffff;
  border-radius: 16px;
  padding: 32px;
  min-height: 300px;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.05), 0 4px 6px -4px rgba(0, 0, 0, 0.05);
  border: 1px solid #f1f5f9;
}

/* ================= 路由切换动画 ================= */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.25s ease, transform 0.25s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>