<template>
    <div id="app">
        <add-task @addTask="addTask"></add-task>
        <p>正在进行</p>
        <doing-task :doingTask="doingTask" @isDone="isDone"></doing-task>
        <p>已完成</p>
        <done-task :doneTask="doneTask" @isDoing="isDoing"></done-task>
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
        const addTask = (e:TodoType) => {
            // console.log(e, 'eeeeeeee')
            doingTask.unshift(e)
        }
        const isDone = (e:number) => {
            console.log(e,'number');
            doingTask[e].done = true
            doneTask.unshift(doingTask[e])
        }
        return {
            doingTask,
            doneTask,
            addTask,
            isDone,
            // isDoing
        }
    },
})
</script>


<style lang="less">
</style>
