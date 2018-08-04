<template>
	<router-view/>
</template>

<script>
export default {
  name: "App",
  created() {
    console.log(`
========================
Made with 🎪 by @follgad
========================
		`);
    console.log("https://github.com/follgad");
  }
};
</script>

<style>
:root {
  --background: #d3d0cb;
  --accent: #587b7f;

  --dark: #393e41;
  --darker: #1e2019;

  --blue-light: #8ea4d2;
  --blue: #6279b8;
  --blue-dark: #49516f;

  --green-dark: #496f5d;
  --green: #4c9f70;
}
body {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  text-align: center;
  color: var(--dark);
  background-color: var(--background);
  min-height: 100vh;
  box-sizing: border-box;
  overflow-y: auto;
  margin: 0;
}
::selection {
	background-color: var(--blue-light);
}
</style>