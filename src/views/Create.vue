<template>
  <div class="row">
		
		<div class="col s6 offset-s3">
			<h1>Create</h1>
			<form @submit.prevent = "submitHandler">
				<div class="input-field">
					<input v-model="title" type="text" required>
					<label for="title">Title</label>
					<span class="helper-text" data-error="Title is valid"></span>
				</div>
				<div class="chips" ref="chips"></div>
				<div class="row">
					<div class="input-field col s12">
						<textarea  v-model="description" id="description" class="materialize-textarea"></textarea>
						<label for="textarea1">Description</label>
						<span class="character-counter" style="float: right; font-size: 12px;">{{ description.length }}/2048</span>
					</div>
					<div class="col s12">
						<input type="text" ref="datepicker">
						<button class="btn" type="submit">Create task</button>
					</div>
				</div>
			</form>
		</div>
  </div>
</template>

<script>
export default {
  name: 'Create',
	data: () => ({
		title: '',
		description: '',
		chips: null,
		date: null
	}),
  mounted() {
		this.chips = M.Chips.init(this.$refs.chips, {
			placeholder: 'Task tags'
		})
		this.datepicker = M.Datepicker.init(this.$refs.datepicker, {
			format: 'dd.mm.yyyy',
			defaultDate: new Date(),
			setDefaultDate: true
		});
	},
	methods: {
		submitHandler() {
			const task = {
				id: Date.now(),
				title: this.title,
				description: this.description,
				tags: this.chips.chipsData,
				status:'active',
				date: this.datepicker.date
			}
			this.$store.dispatch('createTask', task)
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

