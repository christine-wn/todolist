<template>
    <div id="app">
        <div class="header">
            <search-task @searchTask="searchTask"></search-task>
        </div>
        <div class="normal" v-if="foundTask.length === 0">
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
        <div class="foundTask" v-if="foundTask.length">
            查找结果:
            <show-foundTask :foundTask="foundTask"></show-foundTask>
        </div>
    </div>
</template>
 
<script lang="ts">
import AddTask, { TodoType } from './components/addTask.vue'
import DoingTask from './components/doingTask.vue'
import DoneTask from './components/doneTask.vue'
import SearchTask from './components/searchTask.vue'
import showFoundTask from './components/showFoundTask.vue'

import { defineComponent, reactive } from 'vue'

export default defineComponent({
    name: 'app',
    components: {
        AddTask,
        DoingTask,
        DoneTask,
        SearchTask,
        showFoundTask
    },
    setup() {
        const doingTask = reactive<TodoType[]>([])
        const doneTask = reactive<TodoType[]>([])
        // totaltask存放全部的任务
        const totalTask = reactive<TodoType[]>([])
        // foundTask存放搜索到的任务
        const foundTask = reactive<TodoType[]>([])
        let timer:number
        const addTask = (todoObj:TodoType) => {
            doingTask.unshift(todoObj)
        }
        const isDone = (index:number,isdone: boolean) => {
            dealTask(index,isdone,doingTask,doneTask,'unshift')
        }
        const isDoing = (index:number,isdone: boolean) => {
            dealTask(index,isdone,doneTask,doingTask, 'push')
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
            // 法一
            // console.log(targetValue,'targetValue');
            // const flagTask = reactive<TodoType[]>([])
            // foundTask.forEach((item,index) => {
            //     foundTask.splice(index,1)
            // })  
            // clearTimeout(timer)
            // timer = setTimeout(() => {
            //     doingTask.concat(doneTask).forEach(item => totalTask.push(item)) 
            //     totalTask.forEach(item => {
            //         if(item.title.indexOf(targetValue) !== -1) {
            //             flagTask.push(item)
            //         }
            //     })
            //     flagTask.forEach(item => foundTask.push(item))
            // },1000)

            // 法二
            console.log(targetValue,'targetValue');
            clearTimeout(timer)
            timer = setTimeout(() => {
                // 初始化foundTask,reactive创建的数组不能采用[]初始化
                foundTask.forEach((item,index) => {
                    foundTask.splice(index,1)
                })  
                doingTask.concat(doneTask).forEach(item => totalTask.push(item))
                totalTask.forEach(item => {
                    if(item.title.indexOf(targetValue) !== -1) {  
                        // 第二次搜索时,foudTask删除已经存在本次相匹配的item和上次匹配本次不匹配的item
                        foundTask.forEach((subItem,subIndex) => {
                            if (subItem.title.indexOf(item.title) !== -1 || subItem.title.indexOf(targetValue) == -1) {
                                foundTask.splice(subIndex,1)
                            }
                        })
                        // 第一次搜索时,foundTask直接push匹配的item
                        foundTask.push(item)
                        // console.log(foundTask,'foundTask');
                    }
                })
            },1000)
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
        return {
            doingTask,
            doneTask,
            foundTask,
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
