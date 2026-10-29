<template>
  <div class="app">
    <h1>待办事项</h1>
    <div class="input-row">
      <input type="text" v-model="newTodo" @keyup.enter="addTodo" placeholder="输入待办事项" />
      <button class="btn-add" :disabled="!newTodo.trim()" @click="addTodo">添加</button>
    </div>
    <ul class="list" v-if="todos.length">
      <li v-for="todo in todos" :key="todo.id">
        <input type="checkbox" v-model="todo.done" />
        <span class="text" :class="{ done: todo.done }">{{ todo.text }}</span>
        <button class="btn-del" @click="removeTodo(todo.id)">删除</button>
      </li>
    </ul>
    <p class="empty" v-else>暂无待办事项</p>
    <div class="footer">
      <span
        >共 {{ todos.length }} 项 · 已完成 {{ doneCount }} 项 · 未完成 {{ undoneCount }} 项</span
      >
      <button class="btn-clear" :disabled="doneCount === 0" @click="clearDone">清除已完成</button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const newTodo = ref('')
const todos = ref([])
let nextId = 1

function addTodo() {
  const text = newTodo.value.trim()
  if (!text) return
  todos.value.push({ id: nextId++, text, done: false })
  newTodo.value = ''
}

function removeTodo(id) {
  todos.value = todos.value.filter((t) => t.id !== id)
}

function clearDone() {
  todos.value = todos.value.filter((t) => !t.done)
}

const doneCount = computed(() => todos.value.filter((t) => t.done).length)
const undoneCount = computed(() => todos.value.filter((t) => !t.done).length)
</script>

<style scoped>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
body {
  font-family: 'Microsoft YaHei', 'PingFang SC', Arial, sans-serif;
  background: #f0f2f5;
  display: flex;
  justify-content: center;
  align-items: flex-start;
  min-height: 100vh;
  padding: 40px 16px;
}
.app {
  width: 100%;
  max-width: 520px;
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
  padding: 24px;
}
h1 {
  font-size: 24px;
  color: #333;
  margin-bottom: 20px;
  text-align: center;
}
.input-row {
  display: flex;
  gap: 10px;
  margin-bottom: 16px;
}
.input-row input {
  flex: 1;
  padding: 10px 14px;
  font-size: 15px;
  border: 1px solid #dcdfe6;
  border-radius: 8px;
  outline: none;
  transition: border-color 0.2s;
}
.input-row input:focus {
  border-color: #409eff;
}
button {
  padding: 10px 20px;
  font-size: 15px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  transition:
    background 0.2s,
    opacity 0.2s;
}
.btn-add {
  background: #409eff;
  color: #fff;
}
.btn-add:hover:not(:disabled) {
  background: #66b1ff;
}
.btn-del {
  background: #f56c6c;
  color: #fff;
  padding: 6px 12px;
  font-size: 13px;
}
.btn-del:hover {
  background: #f78989;
}
button:disabled {
  background: #c0c4cc;
  cursor: not-allowed;
  opacity: 0.7;
}
.list {
  list-style: none;
  margin-bottom: 16px;
}
.list li {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 6px;
  border-bottom: 1px solid #eee;
}
.list li input[type='checkbox'] {
  width: 16px;
  height: 16px;
  cursor: pointer;
}
.list li .text {
  flex: 1;
  font-size: 15px;
  color: #333;
  word-break: break-all;
}
.list li .text.done {
  color: #999;
  text-decoration: line-through;
}
.empty {
  text-align: center;
  color: #bbb;
  padding: 20px 0;
  font-size: 14px;
}
.footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid #eee;
  padding-top: 14px;
  font-size: 14px;
  color: #666;
}
.btn-clear {
  background: #e6a23c;
  color: #fff;
  padding: 6px 12px;
  font-size: 13px;
}
.btn-clear:hover:not(:disabled) {
  background: #ebb563;
}
</style>
