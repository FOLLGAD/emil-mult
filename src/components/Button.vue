<template>
	<a href="javascript:void(0)" @click="onclick" class="button link" :class="{ uppercase }">
		<slot></slot>
	</a>
</template>

<script>
export default {
  props: ["onclick", "uppercase"]
};
</script>

<style scoped>
.link {
  text-decoration: none;
  display: inline-block;
  color: inherit;
}
.uppercase {
  text-transform: uppercase;
}
.button {
  padding: 1rem 2rem;
  background-color: var(--blue);
  border-radius: 0.8rem;
  color: #eee;
  letter-spacing: 0.2rem;
  border: none;
  font-size: 1.5rem;
  cursor: pointer;
	margin-bottom: 0.5rem;
  box-shadow: 0 0.5rem var(--blue-dark);
  transition: all 100ms ease;
  display: inline-block;
}
.button:hover,
.button:focus {
  transform: translateY(0.3rem);
  box-shadow: 0 0.2rem var(--blue-dark);
}
.button:active:hover {
  transform: translateY(0.5rem);
  box-shadow: 0 0rem var(--blue-dark);
  background-color: var(--blue-dark);
}
</style>
