<template>
 <div>
	<div v-if="task">
		<h1> {{ task.id }}</h1>

		<form @submit.prevent = "submitHandler">
			<div class="chips" ref="chips"></div>
			<div class="row">
				<div class="input-field">
					<textarea  v-model="description" id="description" class="materialize-textarea"></textarea>
					<label for="textarea1">Description</label>
					<span class="character-counter" style="float: right; font-size: 12px;">{{ description.length }}/2048</span>
				</div>
				<div>
					<input type="text" ref="datepicker">
					<div v-if="task.status !== 'completed'">
						<button class="btn" type="submit">Update task</button>
						<button class="btn blue" type="button" @click="completeTask">Complete task</button>
					</div>
				</div>
			</div>
		</form>

	</div>
	<p v-else>No task</p>
 </div>
</template>
 
<script>
 export default {
	computed: {
		task() {
			return this.$store.getters.taskById(+this.$route.params.id)
		}
	},
	data: () => ({
		description: '',
		chips: null,
		date: null
	}),
  mounted() {
		this.description = this.task.description
		this.chips = M.Chips.init(this.$refs.chips, {
			placeholder: 'Task tags',
			data: this.task.tags
		})
		this.datepicker = M.Datepicker.init(this.$refs.datepicker, {
			format: 'dd.mm.yyyy',
			defaultDate: new Date(this.task.date),
			setDefaultDate: true
		})
		setTimeout(() => {
			M.updateTextFields()
		},0)
	},
	methods: {
		submitHandler() {
			this.$store.dispatch('updateTask', {
				id: this.task.id,
				description: this.description,
				date: this.datepicker.date
			})
			this.$router.push('/list')
		},
		completeTask() {
			this.$store.dispatch('completeTask', this.task.id)
			this.$router.push('/list')
		}
	},
	destroyed() {
		if(this.date && this.date.destroy) {
			this.date.destroy
		}

		if(this.chips && this.chips.destroy) {
			this.chips.destroy
		}
	}
 }
</script>
 
<style scoped>
	.blue{
		margin-left: 15px;
	}
</style>