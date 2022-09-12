<template>
	<p :class="className">
		<span @dblclick="setEdit()" v-if="!editable">{{ value }}</span>
		<input
			@blur="cancelEdit()"
			@keydown.esc="cancelEdit()"
			@keydown.enter="applyEdit()"
			v-if="editable"
			v-model="editableText"
			type="text"
			ref="editableNode"
		/>
	</p>
</template>

<script>
	export default {
		props: {
			value: {
				type: String,
				required: true
			},
			className: {
				type: String
			},
			required: {
				type: Boolean,
				default: false
			}
		},
		data() {
			return {
				editable: false,
				editableText: ''
			};
		},
		methods: {
			async setEdit() {
				this.editable = true;
				this.editableText = this.value;
				await this.$nextTick();
				const { editableNode } = this.$refs;
				editableNode.focus();
			},
			cancelEdit() {
				this.editable = false;
			},
			applyEdit() {
				const notApply = this.required && this.editableText === '';
				if (notApply) {
					return;
				}

				this.$emit('applyEdit', this.editableText);
				const { editableNode } = this.$refs;
				editableNode.blur();
			}
		}
	};
</script>

<style lang="less" scoped>
	input {
		background: transparent;
		border: none;
		color: inherit;
		font-family: 'Montserrat';
		font-size: inherit;
		margin: 0;
		padding: 0;
	}
</style>
