<template>
	<div id="app">
		<h1>Tarefas</h1>
		<TaskProgress :progress="progress" />
		<!-- captura o evento emitido pelo componente -->
		<NewTask @taskAdded="addTask"/>
		
		<TaskGrid @taskDeleted="deleteTask" @taskStateChanged="toggleTaskState" :tasks="tasks" />
	</div>
</template>

<script>
import TaskGrid from './components/TaskGrid'
import NewTask from './components/NewTask'
import TaskProgress from './components/TaskProgress'

export default {
	components: { TaskGrid, NewTask, TaskProgress },
	data() {
		return {
			tasks: []
		}
	},
	computed: {
		progress() {
			const total = this.tasks.length
			const done = this.tasks.filter(t => !t.pending).length
			return Math.round(done / total * 100) || 0 
		}
	},
	watch: {
		tasks: {
			deep: true,
			handler() {
				localStorage.setItem('tasks', JSON.stringify(this.tasks))
			}
		}
	},
	methods: {
		addTask(task) {
			if (this.tasks.filter(t => t.name === task.name).length == 0) {
				this.tasks.push({
					name: task.name,
					pending: task.pending || true
				})
			}
		},
		deleteTask(task) {
			const i = this.tasks.indexOf(task)
			if (i >= 0) {
				this.tasks.splice(i, 1)
			}
		},
		toggleTaskState(task) {
			const i = this.tasks.indexOf(task)
			if (i >= 0) {
				this.tasks[i].pending = !this.tasks[i].pending
			}
		}
	},
	created() {
		const json = localStorage.getItem('tasks')
		this.tasks = JSON.parse(json) || []
	}
}
</script>

<style>
	body {
		font-family: 'Lato', sans-serif;
		background: linear-gradient(to right, rgb(22, 34, 42), rgb(58, 96, 115));
		color: #FFF;
	}

	#app {
		display: flex;
		flex: 1;
		flex-direction: column;
		justify-content: center;
		align-items: center;
		height: 100vh;
	}

	#app h1 {
		margin-bottom: 5px;
		font-weight: 300;
		font-size: 3rem;
	}
</style>
