<template>
  <div id="app">
    <h1>记事本 · 第 2 章实验</h1>
    <NoteCard />
    <hr />

    <!-- 记事本雏形区块，套上容器加class -->
    <div class="note-task-card">
      <h2>📝 记事本雏形（综合任务）</h2>
      <input 
        v-model="newNote" 
        placeholder="输入笔记内容" 
        @keyup.enter="addNote" 
        class="input-text"
      />
      <button @click="addNote" class="btn-primary">添加</button>
      <br />
      <div class="radio-group">
        <label><input type="radio" value="高" v-model="priority" /> 高</label>
        <label><input type="radio" value="中" v-model="priority" /> 中</label>
        <label><input type="radio" value="低" v-model="priority" /> 低</label>
      </div>
      <br />
      <div class="checkbox-group">
        <label><input type="checkbox" value="待办" v-model="tags" /> 待办</label>
        <label><input type="checkbox" value="灵感" v-model="tags" /> 灵感</label>
      </div>
      <p class="info-line">实时展示 → 内容：{{ newNote }}｜优先级：{{ priority }}｜标签：{{ tags.join('、') || '（未选）' }}</p>
      <ul class="note-list">
        <li v-for="(note, index) in notes" :key="note.id" class="note-item">
          {{ index + 1 }}. {{ note.text }} 【{{ note.priority }}】{{ note.tags.join('/') }}
          <button @click="notes.splice(index, 1)" class="btn-del">删除</button>
        </li>
      </ul>
      <p v-if="notes.length === 0" class="empty-tip">暂无笔记，快添加一条吧</p>
      <p v-show="notes.length > 0" class="count-tip">共 {{ notes.length }} 条</p>
    </div>

    <hr />
    <h2>🎯 事件实验室（2.4—2.5）</h2>
    <EventLab />
  </div>
</template>
<script>
import NoteCard from './components/NoteCard.vue';
import EventLab from './components/EventLab.vue';
export default {
  components: { NoteCard, EventLab  },
  data() {
    return {
      newNote: '',      // 输入框内容
      priority: '中',   // 优先级（单选，单值）
      tags: [],         // 标签（复选，数组）
      notes: [],        // 笔记列表：{ id, text, priority, tags }
      nextId: 1         // 唯一 id 发号器
    }
  },
  methods: {
    addNote() {
      if (!this.newNote.trim()) return              // 空内容不添加
      this.notes.push({
        id: this.nextId++,                          // 唯一 id：:key 的规范取值
        text: this.newNote.trim(),
        priority: this.priority,
        tags: [...this.tags]                        // 复制一份，避免和表单联动
      })
      this.newNote = ''
    }
  }
}
</script>
<style scoped>
#app {
  max-width: 900px;
  margin: 0 auto;
  padding: 24px;
  font-family: "Microsoft YaHei", sans-serif;
}

#app h1 {
  text-align: center;
  font-size: 32px;
  background: linear-gradient(135deg, #42b883, #35495e);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  font-weight: 800;
}

#app h2 {
  text-align: center;
  font-size: 22px;
  background: linear-gradient(135deg, #42b883, #35495e);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
  font-weight: 700;
  margin-top: 0;
}

hr {
  border: none;
  height: 3px;
  background: linear-gradient(90deg, transparent, #42b883, transparent);
  border-radius: 3px;
  margin: 30px 0;
}

/* ===== 记事本任务卡片，和NoteCard风格完全一致 ===== */
.note-task-card {
  border: 3px solid transparent;
  border-radius: 16px;
  padding: 24px;
  text-align: center;
  margin-bottom: 16px;
  position: relative;

  background:
    linear-gradient(#ffffff, #ffffff) padding-box,
    linear-gradient(135deg, #42b883, #35495e, #42b883) border-box;

  box-shadow:
    0 10px 30px rgba(66, 184, 131, 0.25),
    0 0 0 1px rgba(53, 73, 94, 0.08);
}

.note-task-card::before {
  content: "";
  position: absolute;
  top: 0;
  left: 50%;
  transform: translateX(-50%);
  width: 60px;
  height: 4px;
  border-radius: 0 0 4px 4px;
  background: linear-gradient(90deg, #42b883, #ffb454);
}

/* 输入框 */
.input-text {
  padding: 6px 10px;
  border: 1px solid #dcdfe6;
  border-radius: 8px;
  outline: none;
  transition: all 0.2s;
  font-size: 14px;
  margin: 6px 4px;
}
.input-text:focus {
  border-color: #42b883;
  box-shadow: 0 0 0 3px rgba(66, 184, 131, 0.2);
}

/* 单选、复选框 */
.radio-group, .checkbox-group {
  margin: 8px 0;
}
.radio-group label, .checkbox-group label {
  margin: 0 8px;
  font-size:15px;
}
input[type="radio"],
input[type="checkbox"] {
  transform: scale(1.1);
  margin-right: 4px;
  accent-color: #42b883;
}

/* 实时信息行 */
.info-line {
  background: linear-gradient(90deg, #eafaf1, #eaf3ff);
  padding: 10px 14px;
  border-radius: 10px;
  color: #2c3e50;
  font-weight: 600;
  display: inline-block;
}

/* 笔记列表 */
.note-list {
  list-style: none;
  padding: 0;
  margin-top:12px;
}
.note-item {
  margin: 8px auto;
  padding: 10px 14px;
  background: linear-gradient(90deg, #eafaf1, #ffffff);
  border-left: 4px solid #42b883;
  border-radius: 8px;
  color: #2c3e50;
  font-weight: 600;
  max-width: 700px;
  display:flex;
  align-items:center;
  justify-content:center;
  gap:10px;
}

/* 提示文字 */
.empty-tip {
  font-size:16px;
  color:#666;
  background:#fdf6ec;
  padding:8px 16px;
  border-radius:20px;
  display:inline-block;
}
.count-tip {
  font-weight:bold;
  color:#42b883;
  font-size:15px;
}

/* 按钮样式，和NoteCard统一 */
.btn-primary {
  padding: 6px 14px;
  margin: 6px 4px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 14px;
  font-weight: 600;
  color: #fff;

  background: linear-gradient(135deg, #42b883, #35495e);
  box-shadow: 0 4px 14px rgba(66, 184, 131, 0.3);
  transition: all 0.2s ease;
}
.btn-primary:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(66, 184, 131, 0.45);
  background: linear-gradient(135deg, #35495e, #42b883);
}
.btn-primary:active {
  transform: translateY(0);
}

/* 删除按钮 红色风格，区分添加按钮 */
.btn-del {
  padding: 4px 10px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 13px;
  font-weight: 600;
  color: #fff;
  background: linear-gradient(135deg, #f56c6c, #c43b3b);
  box-shadow: 0 4px 14px rgba(245, 108, 108, 0.3);
  transition: all 0.2s ease;
}
.btn-del:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(245, 108, 108, 0.45);
}
.btn-del:active {
  transform: translateY(0);
}
</style>
