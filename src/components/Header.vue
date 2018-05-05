<template>
	<h1 class="header" :class="{ large, small }">
		<slot />
	</h1>
</template>

<script>
export default {
	props: {
		large: Boolean,
		small: Boolean,
	}
}
</script>

<style scoped>
	.header {
		text-shadow: .2rem .2rem .1rem rgb(180, 177, 170);
		margin: 0;
		font-size: 2.5rem;
	}
	.header.large {
		font-size: 5rem;
		margin: 1rem 0 .5rem 0;
	}
	.header.small {
		font-size: 2rem;
	}
</style>
