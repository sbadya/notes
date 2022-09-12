<template>
	<input
		:placeholder="placeholder"
		class="notes-header-search"
		v-model.trim="search"
	/>
</template>

<script>
	export default {
		props: {
			placeholder: {
				type: String,
				default: 'Search'
			},
			value: {
				type: String,
				required: true
			}
		},
		data() {
			return {
				search: this.value
			};
		},
		watch: {
			search(val) {
				this.$emit('search', val);
			}
		}
	};
</script>
