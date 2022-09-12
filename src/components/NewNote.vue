<template>
	<div class="new-note">
		<div class="new-note__item">
			<label>Title</label>
			<input type="text" v-model="note.title" />
		</div>
		<div class="new-note__item">
			<label>Description</label>
			<textarea v-model="note.descr"></textarea>
		</div>
		<div class="new-note__item">
			<label>Priority</label>
			<select v-model="note.priority">
				<option
					v-for="(priority, index) in priorities"
					:key="index"
					:value="priority.value"
				>
					{{ priority.text }}
				</option>
			</select>
		</div>
		<button class="btn btnPrimary" @click="addNote">New note</button>
	</div>
</template>

<script>
	const priorities = [
		{ text: 'Normal', value: 'normal' },
		{ text: 'High', value: 'high' },
		{ text: 'Very High', value: 'very-high' }
	];
	const note = {
		title: '',
		descr: '',
		priority: priorities[0].value
	};

	export default {
		data() {
			return {
				priorities,
				note: { ...note }
			};
		},
		methods: {
			addNote() {
				this.$emit('addNote', this.note);
				this.note = { ...note };
			}
		}
	};
</script>

<style lang="less" scoped>
	@import '@/assets/less/newNote';
</style>
