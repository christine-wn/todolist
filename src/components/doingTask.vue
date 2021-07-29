<template>
    <div class="showTask">
		<div class="showTask_container" v-for="(item,index) in doingTask" :key="item.id">
			<div class="row">
				<div class="input-group">
					<span class="input-group-addon">
						<input type="checkbox" class="show-checkbox" :checked="item.done" @change="isDone(index)">
					</span>
					<input type="text" class="form-control" :value="item.title" @keyup.enter="isModify(item.id,index,'doingTask',$event)">
					<button type="button" class="remove-btn btn btn-default" @click="isDelete(index,'doingTask')">删除</button>
				</div>
        	</div>
		</div>
    </div>
</template>
<script lang="ts">
import { defineComponent, PropType } from 'vue'
import { TodoType } from './addTask.vue'

export default defineComponent({
    name: 'doingTask',
	props: {
		doingTask: {
			type: Array as PropType<TodoType[]>,
			required: true
		}
	},
    setup(props,context) {
		console.log(props.doingTask,'doingTask');
		const isDone = (index: number) => {
			console.log(index,'index');
			context.emit('isDone',index)
		}
		const isDelete = (index:number,info:string) => {
			console.log(index,'index');
			context.emit('isDelete',index,info)
		}
		const isModify = (id:number,index:number,info:string,e:Event) => {
			const value = (e.target as HTMLInputElement).value
			context.emit('isModify',id,index,info,value)
		}
		return {
			isDone,
			isDelete,
			isModify
		}
    },
})
</script>

<style lang="less">
.showTask_container {
    height: 40px;
    line-height: 38px;
    border-bottom: 1px solid #eee;
}
.show-checkbox:checked {
	background:#1673ff
}

.show-checkbox {
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
.show-checkbox:checked::after {
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
    margin-top: 1px;
}
.remove-btn {
    box-shadow: none !important;
    background: #dddddd !important;
    height: 40px;
}
</style>
