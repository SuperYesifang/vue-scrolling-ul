<template>
	<div id="app">
		<Main class="list">
			<li>1</li>
			<li>2</li>
			<li>3</li>
			<li>4</li>
			<li>5</li>
			<li>6</li>
			<li>7</li>
		</Main>
	</div>
</template>

<script>
	import Main from "./components/Main.vue";
	export default {
		name: "app",
		components: {
			Main
		},
		data() {
			return {};
		}
	};
</script>

<style lang="scss">
	html,
	body,
	#app {
		margin: 0;
		padding: 0;
	}
	.list {
		width: 200px;
		height: 200px;
		background-color: #eee;
		li {
			height: 80px;
			background-color: pink;
			& + li {
				border-top: 1px solid #888;
			}
		}
	}
</style>
