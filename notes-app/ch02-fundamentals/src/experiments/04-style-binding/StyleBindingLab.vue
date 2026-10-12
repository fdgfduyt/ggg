<!-- ============================================================
  StyleBindingLab.vue —— 小实验 04：样式绑定（2.8）
  目录：experiments/04-style-binding/
  要点：:class 三种写法 / :style 对象绑定（色板滑块）
        / .dark 夜间模式（换 CSS 变量，作用于本实验卡片）
============================================================ -->
<template>
  <div class="lab" :class="{ dark: isDark }">
    <h2>实验 04｜样式绑定（2.8）</h2>
    <button @click="isDark = !isDark">{{ isDark ? '☀️ 本卡片切日间' : '🌙 本卡片切夜间' }}</button>
    <h3>① 对象语法：条件挂类</h3>
    <p :class="{ active: count > 0 }">count &gt; 0 时这行变绿加粗（count = {{ count }}）</p>
    <button @click="count++">count +1</button>
    <h3>② 数组语法：三选一切换</h3>
    <p :class="[sizeClass]">字号随按钮切换（{{ sizeClass }}）</p>
    <button @click="sizeClass = sizeClass === 'size-s' ? 'size-m' : sizeClass === 'size-m' ? 'size-l' : 'size-s'">循环切换 S→M→L</button>
    <h3>③ 数组嵌对象：固定类 + 条件类（最常用）</h3>
    <p :class="['badge', { done: isDone }]">{{ isDone ? '已完成' : '进行中' }}</p>
    <button @click="isDone = !isDone">切换完成态</button>
    <h3>:style 对象绑定：色板滑块</h3>
    <p :style="{ color: themeColor, fontSize: fontSize + 'px' }">颜色 {{ themeColor }}｜字号 {{ fontSize }}px</p>
    <label>颜色 <input type="color" v-model="themeColor" /></label>
    <label>字号 <input type="range" min="12" max="32" v-model.number="fontSize" /></label>
  </div>
</template>

<script>
export default {
  name: 'StyleBindingLab',
  data() {
    return {
      isDark: false,
      count: 0,
      sizeClass: 'size-m',
      isDone: false,
      themeColor: '#42b883',
      fontSize: 18
    }
  }
}
</script>

<style scoped>
.lab {
  background: #fff;
  border: 2px solid #2563eb;
  border-left: 6px solid #f43f5e;
  border-radius: 12px;
  box-shadow: 0 4px 14px rgba(37, 99, 235, 0.18);
  padding: 16px 20px;
  transition: background 0.2s, border-color 0.2s;
}
/* 本实验自己的夜间模式，只作用当前卡片 */
.lab.dark {
  background: #111827;
  border-color: #3b82f6;
  border-left-color: #fb7185;
  color: #e5e7eb;
}
.active {
  color: #16a34a;
  font-weight: bold;
}
.size-s { font-size: 12px; }
.size-m { font-size: 16px; }
.size-l { font-size: 22px; }
.badge {
  display: inline-block;
  border: 2px solid #2563eb;
  border-radius: 999px;
  padding: 4px 16px;
  background: #dbeafe;
  font-weight: 500;
}
.badge.done {
  color: #64748b;
  text-decoration: line-through;
  background: #e5e7eb;
}
button {
  background: #2563eb;
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 6px;
  margin: 4px;
  cursor: pointer;
  font-weight: 500;
}
button:hover {
  background: #1d4ed8;
}
h2 {
  color: #1e293b;
}
.lab.dark h2 {
  color: #f3f4f6;
}
h3 {
  font-size: 16px;
  margin: 14px 0 8px;
  color: #1e293b;
}
.lab.dark h3 {
  color: #e5e7eb;
}
input[type="range"] {
  accent-color: #2563eb;
  margin-left: 6px;
}
input[type="color"] {
  border: 2px solid #2563eb;
  border-radius: 6px;
  vertical-align: middle;
  margin-left:6px;
}
</style>

