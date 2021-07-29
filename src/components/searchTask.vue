<template>
    <div class="searchTask">
        <input type="text" class="form-control" placeholder="搜索任务" :value="searchValue" @keyup.enter="searchTask($event)">
    </div>
</template>
<script lang="ts">
import { defineComponent, ref } from 'vue'

export default defineComponent({
    name: 'searchTask',
    setup(props,context) {
        const searchValue = ref('')
        const searchTask = (e:Event) => {
            // console.log(e,'eeeeeeeee');
            const targetValue = (e.target as HTMLElement).value
            context.emit('searchTask',targetValue)
        }
        return {
            searchTask,
            searchValue
        }
    },
})
</script>
<style scoped>
.searchTask {
    height: 40px;
    line-height: 38px;
    border-bottom: 1px solid #eee;
}

.form-control {
    width: 100%;
    height: 38px;
    box-shadow: none !important;
    border: none !important;
}
</style>
