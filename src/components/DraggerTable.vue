<script setup lang="ts">
import { nextTick, ref } from "vue"
import { Table } from "ant-design-vue"
import Sortable from "sortablejs"
import { onMounted } from "vue"

const columns = [
	{
		title: "index",
		dataIndex: "index",
		key: "index"
	},
	{
		title: "姓名",
		dataIndex: "name",
		key: "name"
	},
	{
		title: "年龄",
		dataIndex: "age",
		key: "age"
	},
	{
		title: "住址",
		dataIndex: "address",
		key: "address"
	}
]

const list = ref<any>([
	{ id: 1, index: 1, name: "a" },
	{ id: 2, index: 2, name: "b" },
	{ id: 3, index: 3, name: "c" }
])

const rowDrop = () => {
	const tbody = document.querySelector(".ant-table-tbody") // 元素选择器名称根据实际内容替换
	Sortable.create(tbody, {
		// 官网上的配置项,加到这里面来,可以实现各种效果和功能
		animation: 150,
		ghostClass: "blue-background-class",
		onEnd({ newIndex, oldIndex }: { newIndex: number; oldIndex: number }) {
			console.log(newIndex, oldIndex)
			const newList = JSON.parse(JSON.stringify(list.value))
			const currRow = newList.splice(oldIndex, 1)[0]
			newList.splice(newIndex, 0, currRow)
			newList.forEach((item: any, i: number) => (item.index = i + 1))
			nextTick(() => {
				list.value = newList
			})
		}
	})
}

onMounted(() => {
	rowDrop()
})
</script>

<template>
	<Table :columns="columns" :dataSource="list" rowKey="id" :pagination="false" />
	<div class="text-center">
		{{ JSON.stringify(list) }}
	</div>
</template>
