<!-- ============================================================
  ComputedLab.vue —— 小实验 02：计算属性（2.6）
  目录：experiments/02-computed/
  要点：filteredNotes 搜索过滤 / totalChars 统计 / 缓存验证
  自足说明：本实验自带 3 条假数据，不依赖外部
============================================================ -->
<template>
  <div class="lab">
    <h2>实验 02｜计算属性（2.6）</h2>
    <p class="muted">搜索过滤 + 统计 + <b>缓存验证</b>（打开 F12 Console 看打印次数）。</p>
    <input v-model="keyword" placeholder="搜索笔记…" />
    <p class="stats">
      共 {{ notes.length }} 条｜筛选后 {{ filteredNotes.length }} 条｜总字数 {{ totalChars }}｜完成率 {{ donePercent }}%
    </p>
    <ul class="notes">
      <li v-for="(note, index) in filteredNotes" :key="note.id" :class="['', { done: note.done }]">
        <label><input type="checkbox" v-model="note.done" /></label>
        {{ index + 1 }}. {{ note.text }}【{{ note.priority }}】
      </li>
    </ul>
    <p v-if="filteredNotes.length === 0" class="empty">没有匹配「{{ keyword }}」的笔记</p>
    <hr />
    <h3>🧮 缓存验证：computed vs methods</h3>
    <p>totalChars 写 3 遍：{{ totalChars }}｜{{ totalChars }}｜{{ totalChars }}</p>
    <p>getTotalChars() 调 3 遍：{{ getTotalChars() }}｜{{ getTotalChars() }}｜{{ getTotalChars() }}</p>
    <button @click="refreshTick++">触发重渲染（已点 {{ refreshTick }} 次，改的是无关数据）</button>
  </div>
</template>

<script>
export default {
  name: 'ComputedLab',
  data() {
    return {
      keyword: '',
      refreshTick: 0,
      notes: [
        { id: 1, text: '学习单文件组件三段结构', priority: '高', done: false },
        { id: 2, text: 'v-model 表单三件套跑通', priority: '中', done: false },
        { id: 3, text: '给记事本加上删除功能', priority: '低', done: false }
      ]
    }
  },
  computed: {
    filteredNotes() {
      return this.notes.filter(n => n.text.includes(this.keyword))
    },
    totalChars() {
      console.log('[computed] totalChars 计算了 1 次')
      return this.notes.reduce((sum, n) => sum + n.text.length, 0)
    },
    donePercent() {
      if (this.notes.length === 0) return 0
      const done = this.notes.filter(n => n.done).length
      return Math.round((done / this.notes.length) * 100)
    }
  },
  methods: {
    getTotalChars() {
      console.log('[methods] getTotalChars 执行了 1 次')
      return this.notes.reduce((sum, n) => sum + n.text.length, 0)
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
}
.stats {
  color: #2563eb;
  font-weight: 600;
  font-size: 16px;
}
input {
  width: 100%;
  box-sizing: border-box;
  border: 2px solid #2563eb;
  border-radius: 8px;
  padding: 8px 12px;
  font-size:15px;
  outline: none;
}
input:focus {
  border-color: #f43f5e;
}
.notes {
  list-style: none;
  margin: 10px 0;
  padding: 0;
  display: grid;
  gap: 8px;
}
.notes li {
  border: 2px solid #2563eb;
  background: #dbeafe;
  border-radius: 10px;
  padding: 10px 14px;
}
.notes li.done {
  color: #64748b;
  text-decoration: line-through;
}
.empty {
  text-align: center;
  color: #64748b;
  border: 2px dashed #2563eb;
  border-radius: 10px;
  padding: 14px;
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
.muted {
  color: #64748b;
}
h2 {
  color: #1e293b;
}
h3 {
  color: #1e293b;
}
hr {
  border-color:#2563eb;
  margin:16px 0;
}
</style>
