<template>
	<p class="message">{{ message }}</p>
</template>

<script>
	export default {
		props: {
			message: {
				type: String,
				required: true,
				default: 'Default Message'
			}
		}
	};
</script>

<style lang="less">
	.message {
		color: red;
		padding: 20px;
		text-align: center;
	}
</style>
