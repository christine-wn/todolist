<template>
    <div id="app">
        <search-task @searchTask="searchTask"></search-task>
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
import DoingTask from './components/doingTask.vue'
import DoneTask from './components/doneTask.vue'
import SearchTask from './components/searchTask.vue'

import { defineComponent, reactive } from 'vue'

export default defineComponent({
    name: 'app',
    components: {
        AddTask,
        DoingTask,
        DoneTask,
        SearchTask
    },
    setup() {
        const doingTask = reactive<TodoType[]>([])
        const doneTask = reactive<TodoType[]>([])
        const totalTask = reactive<TodoType[]>([])
        const addTask = (todoObj:TodoType) => {
            doingTask.unshift(todoObj)
        }
        const isDone = (index:number,isdone: boolean) => {
            dealTask(index,isdone,doingTask,doneTask,'unshift')
        }
        const isDoing = (index:number,isdone: boolean) => {
            dealTask(index,isdone,doneTask,doingTask, 'push')
        }
        const dealTask = (index:number,isdone: boolean,fromtTask: TodoType[],toTask: TodoType[],oprator: string) => {
            fromtTask[index].done = isdone
            switch(oprator) {
                case 'push': 
                    toTask.push(fromtTask[index])
                    break;
                case 'unshift':
                    toTask.unshift(fromtTask[index])
            }
            fromtTask.splice(index, 1)
        }
        const isDelete = (index:number,info:string) => {
            switch(info) {
                case 'doingTask':
                    doingTask.splice(index,1)
                    break
                case 'doneTask':
                    doneTask.splice(index,1)
                    break
                default:
                    break
            }
        }
        const isModify = (todoIndex:number,index:number,info:string,value:string) => {
            switch(info) {
                case 'doingTask':
                    doingTask.splice(index,1,{
                        done: false,
                        title: value,
                        id: todoIndex
                    })  
                    break;
                case 'doneTask':
                    doneTask.splice(index,1,{
                        done: true,
                        title: value,
                        id: todoIndex
                    })  
                    break;
                default:
                    break;
            }
        }
        const searchTask = (targetValue:string) => {
            console.log(targetValue,'targetValue');
            doingTask.forEach(item => {
                if(item.title.indexOf(targetValue) !== -1) {
                    console.log(item,'itemxxxxxxx');
                }
            })
            console.log(totalTask,'totalTask');
        }
        return {
            doingTask,
            doneTask,
            addTask,
            isDone,
            isDoing,
            dealTask,
            isDelete,
            isModify,
            searchTask
        }
    },
})
</script>


<style lang="less">
</style>
