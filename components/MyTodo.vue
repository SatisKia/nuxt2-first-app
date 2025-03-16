<template>
  <div>
    <todo-input
      v-on:add="addTodo"
    />
    <todo-label
      v-for="todo in sortedTodo"
      v-bind:key="todo.id"
      v-bind:todo="todo"
      v-on:done="doneTodo"
      v-on:remove="removeTodo"
    />
  </div>
</template>

<script lang="ts">
import Vue from 'vue'
import { Todo } from '@/types/todo'

export default Vue.extend({
  data () {
    return {
      todoList: [] as Todo[]
    }
  },
  methods: {
    loadData () {
      // データの読み込み処理
      this.todoList = [] as Todo[]
      console.log('loadData')
    },
    saveData () {
      // データの書き込み処理
      console.log('saveData')
    },
    addTodo (text: string, color: string) {
      this.todoList = [...this.todoList, {
        id: (new Date()).getTime().toString(),
        done: false,
        date: new Date(),
        text: text,
        color: color
      }]
      this.saveData()
    },
    removeTodo (id: string) {
      this.todoList = this.todoList.filter(todo => todo.id !== id)
      this.saveData()
    },
    doneTodo (id: string) {
      const todo = this.todoList.find(todo => todo.id === id)
      if (todo) {
        todo.done = !todo.done
        this.saveData()
      }
    }
  },
  computed: {
    sortedTodo (): Todo[] {
      return this.todoList.slice().sort((a, b) => {
        return b.date.getTime() - a.date.getTime()
      })
    }
  },
  mounted () {
    // データの読み込み
    this.loadData()
  }
})
</script>
