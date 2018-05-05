<template>
	<h1 class="header">
		<slot />
	</h1>
</template>

<script>
export default {

}
</script>

<style scoped>
	.header {
		text-shadow: .2rem .2rem .1rem rgb(180, 177, 170);
		font-size: 5rem;
		margin: 0;
	}
</style>
