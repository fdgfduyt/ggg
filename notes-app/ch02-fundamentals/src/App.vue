<!-- ============================================================
  App.vue —— ch02-fundamentals 第 2 章实验台
============================================================ -->
<template>
  <div id="app">
    <!-- 头部标题区 -->
    <header class="page-header">
      <h1>ch02-fundamentals｜第 2 章实验台</h1>
      <p class="muted">第 03 课上半场（2.5.3—2.8）：修饰符串联 · 计算属性 · 侦听器 · 样式绑定</p>
    </header>

    <!-- 吸顶导航菜单 -->
    <nav class="lab-nav">
      <a href="#exp-01">01 修饰符串联</a>
      <a href="#exp-02">02 计算属性</a>
      <a href="#exp-03">03 侦听器</a>
      <a href="#exp-04">04 样式绑定</a>
    </nav>

    <!-- 实验内容卡片 -->
    <main class="lab-content">
      <section id="exp-01" class="exp-card">
        <ModifierChainLab />
      </section>
      
      <section id="exp-02" class="exp-card">
        <ComputedLab />
      </section>
      
      <section id="exp-03" class="exp-card">
        <WatchLab />
      </section>
      
      <section id="exp-04" class="exp-card">
        <StyleBindingLab />
      </section>
    </main>

    <!-- 右下角悬浮回到顶部按钮 -->
    <transition name="fade">
      <div 
        v-if="showBackTop" 
        class="back-to-top" 
        @click="scrollToTop"
        title="回到顶部"
      >
        ↑
      </div>
    </transition>
  </div>
</template>

<script>
import ModifierChainLab from './experiments/01-modifier-chain/ModifierChainLab.vue'
import ComputedLab from './experiments/02-computed/ComputedLab.vue'
import WatchLab from './experiments/03-watch/WatchLab.vue'
import StyleBindingLab from './experiments/04-style-binding/StyleBindingLab.vue'

export default {
  name: 'App',
  components: { ModifierChainLab, ComputedLab, WatchLab, StyleBindingLab },
  data() {
    return {
      showBackTop: false // 控制回到顶部按钮的显示与隐藏
    }
  },
  mounted() {
    // 监听页面滚动事件
    window.addEventListener('scroll', this.handleScroll)
  },
  beforeDestroy() {
    // 组件销毁前清理监听事件，防止内存泄漏
    window.removeEventListener('scroll', this.handleScroll)
  },
  methods: {
    handleScroll() {
      // 当页面滚动超过 300px 时，显示按钮
      this.showBackTop = window.scrollY > 300
    },
    scrollToTop() {
      window.scrollTo({
        top: 0,
        behavior: 'smooth'
      })
    }
  }
}
</script>

<style scoped>
/* ================= 1. 全局容器与排版 ================= */
#app {
  max-width: 1000px;
  margin: 0 auto;
  padding: 40px 20px 80px;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif;
}

/* 平滑滚动：点击锚点时丝滑过渡 */
html {
  scroll-behavior: smooth;
}

/* ================= 2. 头部标题区 ================= */
.page-header {
  text-align: center;
  margin-bottom: 30px;
}

.page-header h1 {
  font-size: 32px;
  font-weight: 800;
  color: #1e293b;
  margin-top: 0;
  margin-bottom: 12px;
  background: linear-gradient(to right, #1e293b, #3b82f6);
  -webkit-background-clip: text;
  color: transparent;
}

.muted {
  color: #64748b;
  font-size: 15px;
  margin: 0;
}

/* ================= 3. 吸顶导航栏 ================= */
.lab-nav {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 12px;
  margin: 0 0 32px;
  
  /* 吸顶效果 */
  position: sticky;
  top: 20px;
  z-index: 100;
  
  /* 导航容器背景和阴影 */
  background: rgba(255, 255, 255, 0.85);
  -webkit-backdrop-filter: blur(10px);
  backdrop-filter: blur(10px);
  padding: 12px 20px;
  border-radius: 16px;
  box-shadow: 0 4px 20px -2px rgba(0, 0, 0, 0.05);
  border: 1px solid #e2e8f0;
}

.lab-nav a {
  color: #475569;
  text-decoration: none;
  padding: 8px 20px;
  font-size: 15px;
  font-weight: 600;
  border-radius: 8px;
  transition: all 0.25s ease;
}

.lab-nav a:hover {
  background: #f1f5f9;
  color: #2563eb;
  transform: translateY(-1px);
}

/* ================= 4. 实验内容卡片区 ================= */
.lab-content {
  display: flex;
  flex-direction: column;
  gap: 32px; 
}

.exp-card {
  background: #ffffff;
  border-radius: 16px;
  padding: 32px;
  box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.02), 0 2px 4px -2px rgba(0, 0, 0, 0.02);
  border: 1px solid #e2e8f0;
  
  /* 关键动效：设置浮动过渡动画 */
  transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1), box-shadow 0.3s ease;
  
  /* 设置滚动偏移，防止吸顶导航挡住标题 */
  scroll-margin-top: 100px; 
}

/* 鼠标悬浮时卡片向上浮动，阴影加深 */
.exp-card:hover {
  transform: translateY(-6px);
  box-shadow: 0 20px 25px -5px rgba(0, 0, 0, 0.08), 0 8px 10px -6px rgba(0, 0, 0, 0.04);
}

/* ================= 5. 右下角悬浮回到顶部按钮 ================= */
.back-to-top {
  position: fixed;
  bottom: 40px;
  right: 40px;
  width: 48px;
  height: 48px;
  background: #3b82f6;
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 24px;
  font-weight: bold;
  cursor: pointer;
  box-shadow: 0 4px 12px rgba(59, 130, 246, 0.4);
  transition: all 0.3s ease;
  z-index: 999;
}

.back-to-top:hover {
  background: #2563eb;
  transform: translateY(-4px) scale(1.05);
  box-shadow: 0 6px 16px rgba(59, 130, 246, 0.6);
}

/* 按钮出现/消失的淡入淡出动画 */
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.3s ease, transform 0.3s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(20px);
}
</style>