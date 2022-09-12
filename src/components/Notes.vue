<template>
	<div :class="notesClass">
		<div :class="setClass(note)" v-for="(note, index) in notes" :key="index">
			<editable-text
				v-for="(type, index) in ['title', 'descr']"
				:key="index"
				:value="note[type]"
				:className="`note__${type}`"
				:required="type === 'title'"
				@applyEdit="note[type] = $event"
			/>
			<span class="note__date">{{ note.date }}</span>
			<span class="note__close" @click="remove(note)">X</span>
		</div>
	</div>
</template>

<script>
	import editableText from '@/components/EditableText.vue';

	export default {
		components: { editableText },
		props: {
			notes: {
				type: Array,
				required: true
			},
			grid: {
				type: Boolean
			}
		},
		computed: {
			notesClass() {
				return {
					notes: true,
					notes_grid: this.grid
				};
			}
		},
		methods: {
			remove(note) {
				this.$emit('removeNote', note);
			},
			setClass(note) {
				const shouldSetClass = note.priority !== 'normal';

				return {
					note: true,
					[`note_${note.priority}-priority`]: shouldSetClass
				};
			}
		}
	};
</script>

<style lang="less">
	@import '@/assets/less/notes';
</style>
