<template>
 <div>
	<h1>List</h1>
	<div>
		<select ref="select" v-model="filter">
			<option value="" disabled selected>Choose your option</option>
			<option value="active">active</option>
			<option value="not-active">not-active</option>
			<option value="completed">completed</option>
		</select>
		<label>Materialize Select</label>
	</div>

	<button class="btn btn-small" @click="filter = null" v-if="filter">Clear</button>
	<hr>

	<table v-if="tasks.length">
		<thead>
			<tr>
				<th>#</th>
				<th>Title</th>
				<th>Date</th>
				<th>Description</th>
				<th>Status</th>
				<th>Open</th>
			</tr>
		</thead>
		<tbody>
			<tr 
				v-for="(task, idx) of displayTasks"
				:key="task.id">
				<td>{{ idx + 1 }}</td>
				<td>{{ task.title }}</td>
				<td>{{ new Date(task.date).toLocaleDateString() }}</td>
				<td class="small-td"><div class="small-text">{{ task.description }}</div></td>
				<td>{{ task.status }}</td>
				<td>
					<router-link
						tag="button"
						class="btn btn-small"
						:to="'/task/' + task.id"
					>
					Open
					</router-link>
				</td>
				</tr>
		</tbody>
	</table>
	<p v-else>No tasks</p>
 </div>
</template>
 
<script>
 export default {
	data: () => ({
		filter: null
	}),
	computed: {
		tasks() {
			return this.$store.getters.tasks
		},
		displayTasks() {
			return this.tasks.filter(t => {
				if(!this.filter) {
					return true
				}
				return t.status === this.filter
			})
		}
	},
	mounted() {
		 M.FormSelect.init(this.$refs.select)
	}
 }
</script>
 
<style scoped>
	.small-td{
		max-width: 150px;
	}
	.small-text{
		white-space: nowrap;
		text-overflow: ellipsis;
		overflow: hidden;
	}
	
</style>