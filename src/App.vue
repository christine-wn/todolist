<template>
    <div id="app">
        <add-task @addTask="addTask"></add-task>
        <p v-if="doingTask.length">正在进行</p>
        <doing-task 
          :doingTask="doingTask" 
          @isDone="isDone" 
          @isDelete="isDelete"
          @isModify="isModify">
        </doing-task>
        <p  v-if="doneTask.length">已完成</p>
        <done-task 
          :doneTask="doneTask" 
          @isDoing="isDoing"
          @isDelete="isDelete"
          @isModify="isModify">
        </done-task>
    </div>
</template>
 
<script lang="ts">
import AddTask, { TodoType } from './components/addTask.vue'
import doingTask from './components/doingTask.vue'
import doneTask from './components/doneTask.vue'

import { defineComponent, reactive } from 'vue'

export default defineComponent({
    name: 'app',
    components: {
        AddTask,
        doingTask,
        doneTask
    },
    setup() {
        const doingTask = reactive<TodoType[]>([])
        const doneTask = reactive<TodoType[]>([])
        const addTask = (todoObj:TodoType) => {
            doingTask.unshift(todoObj)
        }
        const isDone = (index:number) => {
            doingTask[index].done = true
            doneTask.unshift(doingTask[index])
            doingTask.splice(index, 1)
        }
        const isDelete = (index:number,info:string) => {
            if(info === 'doingTask') {
                doingTask.splice(index,1)
            } else {
                doneTask.splice(index,1)
            }
        }
        const isModify = (todoIndex:number,index:number,info:string,value:string) => {
            console.log(doingTask,'doingTaskyyyyyyyyyy');
            if(info === 'doingTask') {
                doingTask.splice(index,1,{
                    done: false,
                    title: value,
                    id: todoIndex
                })
            }
        }
        const isDoing = (index:number) => {
            doneTask[index].done = false
            doingTask.push(doneTask[index])
            doneTask.splice(index, 1)
        }
        return {
            doingTask,
            doneTask,
            addTask,
            isDone,
            isDoing,
            isDelete,
            isModify
        }
    },
})
</script>


<style lang="less">
</style>
