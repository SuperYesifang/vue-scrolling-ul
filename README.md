# vue-scrolling-ul

This is a Vue component that provides a list of ul that scrolls automatically

## Run Demo

```shell
$ npm i
$ npm run serve
```

## Usage

### 1. Global Use in Vue-Cli Project

-   main.js（vue v2.0）

```js
import Vue from "vue";
import ScrollingUl from "vue-scrolling-ul";

Vue.use(ScrollingUl);

new Vue({
	el: "#app",
	render: h => h(App)
});
```

### 2. Direct Use in Vue-Cli Project

-   App.vue

````vue
<template>
	<div id="app">
		<scrolling-ul>Some LI Tags ...</scrolling-ul>
	</div>
</template>

<script>
	import ScrollingUl from "vue-scorlling-ul";

	export default {
		name: "App",
		components: {
			ScrollingUl
		},
		data() {
			return {};
		}
	};
</script>
````
