<!-- ============================================================
  WatchLab.vue —— 小实验 03：侦听器（2.7）
  目录：experiments/03-watch/
  要点：keyword 简写（新旧值）/ notes 标准+deep+immediate 自动保存
        / 'user.name' 路径 vs user deep 对比
============================================================ -->
<template>
  <div class="lab">
    <h2>实验 03｜侦听器（2.7）</h2>
    <p class="muted">勾选/增删触发"自动保存"；改 name 两种侦听都响，改 city 只有 deep 响。</p>
    <input v-model="keyword" placeholder="输入搜索词，看 Console 新旧值" />
    <p v-if="savedTip" class="save-tip">{{ savedTip }}</p>
    <ul class="notes">
      <li v-for="note in notes" :key="note.id">
        <label><input type="checkbox" v-model="note.done" /></label>
        {{ note.text }}
        <button @click="notes.splice(notes.indexOf(note), 1)">删除</button>
      </li>
    </ul>
    <button @click="notes.push({ id: Date.now(), text: '新增一条笔记（触发 deep 侦听）', done: false })">添加一条</button>
    <hr />
    <h3>👂 deep / 路径侦听对比</h3>
    <p>user.name = {{ user.name }}｜user.city = {{ user.city }}</p>
    <button @click="user.name = ['张三', '李四', '王五'][Math.floor(Math.random() * 3)]">改 user.name（两种侦听都响）</button>
    <button @click="user.city = ['成都', '绵阳', '乐山'][Math.floor(Math.random() * 3)]">改 user.city（只有 deep 响）</button>
  </div>
</template>

<script>
export default {
  name: 'WatchLab',
  data() {
    return {
      keyword: '',
      savedTip: '',
      saveTimer: null,
      notes: [
        { id: 1, text: '侦听器自动保存演示', done: false },
        { id: 2, text: 'deep 深层侦听', done: false }
      ],
      user: { name: '张三', city: '成都' }
    }
  },
  watch: {
    keyword(newVal, oldVal) {
      console.log(`搜索词从 "${oldVal}" 变为 "${newVal}"`)
    },
    notes: {
      handler() {
        console.log('notes 变了，当前', this.notes.length, '条')
        this.savedTip = '正在保存…'
        clearTimeout(this.saveTimer)
        this.saveTimer = setTimeout(() => {
          this.savedTip = '已自动保存 ✓'
        }, 600)
      },
      deep: true,
      immediate: true
    },
    'user.name'(v) {
      this.savedTip = `路径侦听：user.name 变成 "${v}"`
      console.log('路径侦听 user.name：', v)
    },
    user: {
      handler(v) {
        this.savedTip = 'deep 侦听：user 整体变了（含嵌套）'
        console.log('deep 侦听 user：', v.name, v.city)
      },
      deep: true
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
.save-tip {
  display: inline-block;
  background: #dbeafe;
  color: #2563eb;
  border: 2px solid #2563eb;
  border-radius: 999px;
  padding: 4px 16px;
  font-size: 14px;
  font-weight: 600;
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
