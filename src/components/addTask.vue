<template>
    <div class="addTask">
        <div class="row">
            <div class="input-group">
                <span class="icon-add-task">+</span>
                <input type="text" class="form-control" placeholder="添加任务" v-model="inputValue">
                <button type="button" class="add-btn btn btn-default" @click="addTask">添加</button>
            </div>
        </div>
    </div>
</template>
<script lang="ts">
import { defineComponent, reactive, ref } from 'vue'

export interface TodoType {
    done: boolean,
    title: string
}

export default defineComponent({
    name: 'addTask',
    emits: ['addTask'],
    setup(props,context) {
        const inputValue = ref('')
        // const todoList = reactive<TodoType[]>([])
        const addTask = () => {
            const todoObj = reactive<TodoType>({
                done: false,
                title: ''
            })
            todoObj.done = false
            todoObj.title = inputValue.value
            context.emit('addTask',todoObj)
            inputValue.value = ''
        }
        return {
            inputValue,
            // todoList,
            addTask
        }
    },
})
</script>

<style lang="less">
.addTask {
    height: 40px;
    line-height: 38px;
    border-bottom: 1px solid #eee;
}
.add-checkbox:checked {
	background:#1673ff
}

.icon-add-task {
    font-size: 30px;
    margin-top: -2px;
    color: #aaa;
}

.add-checkbox {
	width:20px;
	height:20px;
	background-color:#ffffff;
	border:solid 1px #dddddd;
	-webkit-border-radius:50%;
	border-radius:50%;
	font-size:0.8rem;
	margin:10px;
	padding:0;
	position:relative;
	display:inline-block;
	vertical-align:top;
	cursor:default;
	-webkit-appearance:none;
	-webkit-user-select:none;
	user-select:none;
	-webkit-transition:background-color ease 0.1s;
	transition:background-color ease 0.1s;
}
.add-checkbox:checked::after {
	content:'';
	top:5px;
	left:5px;
	position:absolute;
	background:transparent;
	border:#fff solid 2px;
	border-top:none;
	border-right:none;
	height:6px;
	width:10px;
	-moz-transform:rotate(-45deg);
	-ms-transform:rotate(-45deg);
	-webkit-transform:rotate(-45deg);
	transform:rotate(-45deg);
}
.form-control {
    height: 38px;
    box-shadow: none !important;
    border: none !important;
}
.add-btn {
    box-shadow: none !important;
    background: #dddddd !important;
    height: 40px;
}
</style>
