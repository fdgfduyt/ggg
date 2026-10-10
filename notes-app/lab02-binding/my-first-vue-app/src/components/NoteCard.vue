<template>
  <div class="note-card">
    <h3>{{ title }}</h3>
    <p>这是一个单文件组件</p>
    <p>{{ message }}</p>
    <p>{{ message.toUpperCase() }}</p>
    <p>{{ 1 + 2 * 3 }}</p>
    <p v-text="rawText"></p>   <!-- 按纯文本渲染，HTML 标签不解析 -->
    <p v-html="htmlText"></p>  <!-- 解析 HTML 标签后渲染 -->
    <img :src="imgSrc" :title="tipTitle" width="96" />
    <p :class="{ active: count > 0 }">count 大于 0 时这行变高亮</p>
    <button @click="count++">count +1(内联语)</button>
    <button @click="say('你好', $event)">say(方法 + 传参）</button>
    <p>实时展示 → 姓名：{{ form.name }}｜性别：{{ form.gender }}｜爱好：{{ form.hobbies.join('、') }}</p>
    <input v-model="form.name" placeholder="输入姓名" />
    <label><input type="radio" value="男" v-model="form.gender" /> 男</label>
    <label><input type="radio" value="女" v-model="form.gender" /> 女</label>
    <label><input type="checkbox" value="篮球" v-model="form.hobbies" /> 篮球</label>
    <label><input type="checkbox" value="编程" v-model="form.hobbies" /> 编程</label>
     <button @click="show = !show">切换显隐（观察 DevTools 元素面板）</button>
    <p v-if="show">v-if:条件为真我才被"渲染"出来</p>
    <p v-else>v-else:上面条件为假时才轮到我</p>
    <p v-show="show">v-show:我一直存在，只是被 display:none 藏起来</p> 
    <ul>
  <li v-for="(subject, index) in subjects" :key="index">
    {{ index }} - {{ subject }}
  </li>
</ul>
  </div>
  <!-- 这两个按钮放在卡片外面，我给它们单独加class -->
  <div class="btns-outside">
    <button @click="subjects[1] = '英语（索引赋值）'">数组索引赋值</button>
    <button @click="form.school = '成都理工大学工程技术学院'">对象新增属性</button>
  </div>
  <p v-if="form.school" class="school-text">{{ form.school }}</p>
</template>
<script>
export default {
  name: 'NoteCard',
  data() {
    return {
        title: '我的第一条笔记' ,
        message: 'hello vue',
        count: 0,
        rawText: '<b>我是 v-text 的纯文本</b>',
        htmlText: '<b>我是 v-html 解析出的加粗</b>' ,
        imgSrc: 'https://vuejs.org/images/logo.svg',
        tipTitle: '我是 :title 绑定的悬停提示'  ,
         form: { name: '', gender: '', hobbies: [] },
         subjects: ['语文', '数学', 'Vue'],
         show: true // 你原来代码缺少show！必须加上，否则切换显隐按钮报错
    }
    
  },
    methods: {
    say(word, event) {
      alert(word + '｜事件类型：' + event.type)
    }
  },
  mounted() {
    setTimeout(() => {
      this.count = 100
    }, 3000)
  }
  
}
</script>
<style scoped>
/* ===== 卡片容器：强视觉冲击主体 ===== */
.note-card {
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

/* 顶部装饰条 */
.note-card::before {
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

/* ===== 标题 ===== */
.note-card h3 {
  margin-top: 8px;
  margin-bottom: 16px;
  font-size: 24px;
  font-weight: 800;
  letter-spacing: 1px;

  background: linear-gradient(135deg, #42b883, #35495e);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

/* ===== 段落 ===== */
.note-card p {
  margin: 10px 0;
  font-size: 15px;
  color: #303133;
  line-height: 1.7;
}

/* 普通强调文字 */
.note-card p strong,
.note-card p b {
  color: #42b883;
  font-weight: 700;
}

/* ===== 表达式结果高亮 ===== */
.note-card p:nth-of-type(4) {
  color: #e67e22;
  font-weight: 700;
  background: #fff7e6;
  display: inline-block;
  padding: 4px 12px;
  border-radius: 20px;
}

/* ===== v-text / v-html 区分色 ===== */
.note-card p[v-text] {
  color: #7f8c8d;
  background: #f4f6f8;
  padding: 6px 12px;
  border-radius: 8px;
}

.note-card p[v-html] {
  color: #2c3e50;
  background: #eafaf1;
  padding: 6px 12px;
  border-radius: 8px;
  border-left: 4px solid #42b883;
}

/* ===== 图片 ===== */
.note-card img {
  margin: 12px 0;
  padding: 10px;
  background: #f4f9f4;
  border-radius: 12px;
  border: 1px dashed #42b883;
}

/* ===== count 高亮 ===== */
.active {
  color: #fff !important;
  background: linear-gradient(135deg, #42b883, #2c6c9b);
  font-weight: 800;
  padding: 6px 14px;
  border-radius: 20px;
  display: inline-block;
  box-shadow: 0 4px 12px rgba(66, 184, 131, 0.35);
}

/* ===== 按钮：主按钮风格 ===== */
button {
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

button:hover {
  transform: translateY(-2px);
  box-shadow: 0 8px 20px rgba(66, 184, 131, 0.45);
  background: linear-gradient(135deg, #35495e, #42b883);
}

button:active {
  transform: translateY(0);
}

/* 外部两个按钮用橙色强调 */
.btns-outside button {
  background: linear-gradient(135deg, #ffb454, #ff7849);
  box-shadow: 0 4px 14px rgba(255, 120, 73, 0.35);
}

.btns-outside button:hover {
  background: linear-gradient(135deg, #ff7849, #ffb454);
  box-shadow: 0 8px 20px rgba(255, 120, 73, 0.45);
}

/* ===== 输入框 ===== */
.note-card input[type="text"] {
  padding: 6px 10px;
  border: 1px solid #dcdfe6;
  border-radius: 8px;
  outline: none;
  transition: all 0.2s;
}

.note-card input[type="text"]:focus {
  border-color: #42b883;
  box-shadow: 0 0 0 3px rgba(66, 184, 131, 0.2);
}

/* 单选/复选框放大一点 */
.note-card input[type="radio"],
.note-card input[type="checkbox"] {
  transform: scale(1.1);
  margin-right: 4px;
  accent-color: #42b883;
}

/* ===== 实时展示行 ===== */
.note-card p:nth-last-of-type(4) {
  background: linear-gradient(90deg, #eafaf1, #eaf3ff);
  padding: 10px 14px;
  border-radius: 10px;
  color: #2c3e50;
  font-weight: 600;
}

/* ===== v-if / v-else / v-show ===== */
.note-card p[v-if],
.note-card p[v-else],
.note-card p[v-show] {
  padding: 8px 12px;
  border-radius: 8px;
  background: #f4f9f4;
  border-left: 4px solid #42b883;
  color: #2c3e50;
}

.note-card p[v-else] {
  background: #fef0f0;
  border-left-color: #f56c6c;
}

.note-card p[v-show] {
  background: #fdf6ec;
  border-left-color: #e6a23c;
}

/* ===== 列表 ===== */
.note-card ul {
  list-style: none;
  padding: 0;
  margin-top: 12px;
}

.note-card li {
  margin: 6px 0;
  padding: 8px 12px;
  background: linear-gradient(90deg, #eafaf1, #ffffff);
  border-left: 4px solid #42b883;
  border-radius: 8px;
  color: #2c3e50;
  font-weight: 600;
}

/* ===== 卡片外按钮容器 ===== */
.btns-outside {
  text-align: center;
  margin: 12px 0 8px;
}

/* ===== 学校信息文字 ===== */
.school-text {
  text-align: center;
  font-size: 16px;
  font-weight: 800;
  color: #fff;
  background: linear-gradient(135deg, #2c6c9b, #42b883);
  display: inline-block;
  padding: 8px 18px;
  border-radius: 24px;
  margin: 10px auto 0;
  box-shadow: 0 6px 16px rgba(44, 108, 155, 0.35);
}
</style>
