# vue-scrolling-ul

![vue-scrolling-ul](https://img.shields.io/badge/vue--scrolling--ul-v1.1.4-%23C50008?logo=npm)
![vuejs](https://img.shields.io/badge/vuejs-v2.x-%234FC08D?logo=vuedotjs)
![nodejs](https://img.shields.io/badge/nodejs-lts--v14.x-%23036200?logo=nodedotjs)
![blog](https://img.shields.io/badge/blog-yesifang.com-orange?logo=data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAACAAAAAgCAMAAABEpIrGAAAABGdBTUEAALGPC/xhBQAAACBjSFJNAAB6JgAAgIQAAPoAAACA6AAAdTAAAOpgAAA6mAAAF3CculE8AAABjFBMVEUAAAAIAQUiBhQVBA05CyK0I2z4MJTgKoV8GEoKAgZyFkT8MZfTKX4dBRFWEDP9MZfMJ3kGAQQHAQTlK4htFUEAHRMATDAAbUQAf1EAh1QAgFAAbUUATDAAHhNMDy7KJngAeUsAKBp9GEr4MJMDAQIAmWEAWzkABAOGGlD9MZYAcUgABQNoFD7mLIoAZUCdHl4ANiKiH2EpCBgAh1UAAgERAwrVKH9nFD0ALBwSAwuqIWXmK4pTEDIAWTgrCBp2F0eVHVmKG1NWETMAdEgAgVAAAQIAJTcATXIAZJQAbqUAap0AVoEAfE4AAQEAN1EAgMAAaEIACQ4Aap4ARiwACQ0AebMAmV8AEwwAAAAAZ5oAZT8AMkkAkFoAEQsAebMAl14AGCQAkl0ALx4AOlYAeEsAGRAATHAAbkUAll0All4AbkYAMB4ATXMABwQAIxYANiIAPicANyIAJBYAQF4AIjIAis0AAgMAhsYAZJYARWYAk9oAHy4ABQcAfbkAO1gAis3/MZgAmmEAld3///8EabibAAAAgHRSTlMACCIVObX54XwKcv3UHVb+zQYH5m0xfrTU4NW1fzJMy8hDffkD/pcHh/69CGjnqJ5ZoynfBBHWZ0kSqudTlCt2lotWwNUCQIOrvrWVzwFe3a4QtnQPz/0gAbKnVe4c0Psp9E9jximBtvj4t0+FCzpaZlo7bTruA+Wtdfs1CNdm7ZpKyEIAAAABYktHRIP8tM/SAAAAB3RJTUUH5QoVBh0NInrzjgAAATtJREFUOMt902VbwzAUBeDLcAYMhru7uzPcXYcP1+EyPMkvZ03TNk0TztfzNnL7BECeCFck/JOo6BiEYuPiVX2CG9EkJsn7ZA9iSUmV9d40ZCYdICMzKzsnNy+/wASFVo+KALCR4hIGSjlQVm4BXFFZRUE1B2q8HMC4tk4D9RxoABvAjRpwuS3QJADcrIkW6witImhrD4OOTtZ7ukAEuFtboqeXjqqvH5xgQL/qoG9oeET/FQIYdQxWAGNmMT4xOTU9MyuCOVbPLywSGhEs6f3yCiFysEr7tXWiABubWu/fIiqwTRfYISqwu0fBvgoc0DlCgCjA4ZF+hWMFODllMzizgfML2l5eXfuNGd7YAARv7+4fHoPc9J/swJlnrn+Rgdc3C4SkT+vd7D8+peDr2+h/FK838Ev3D4W//wNiKCWwWalJAwAAACV0RVh0ZGF0ZTpjcmVhdGUAMjAyMS0xMC0yMVQwNjoyOToxMyswMDowMP1Zb/cAAAAldEVYdGRhdGU6bW9kaWZ5ADIwMjEtMTAtMjFUMDY6Mjk6MTMrMDA6MDCMBNdLAAAAAElFTkSuQmCC)

> This is a Vue component that provides a list of ul that scrolls automatically.

<div align="center">
	<a href="https://nodei.co/npm/vue-scrolling-ul/"><img src="https://nodei.co/npm/vue-scrolling-ul.png?downloads=true&downloadRank=true&stars=true"></a>
</div>

## Run Simple Demo

```shell
$ git clone https://github.com/SuperYesifang/vue-scrolling-ul.git
$ cd vue-scrolling-ul
$ npm install
$ npm run dev
```

## Usage

### 1. Global Use in Vue-Cli Project

-   main.js

```js
import Vue from "vue";
import ScrollingUl from "vue-scrolling-ul";

Vue.use(ScrollingUl);

new Vue({
	el: "#app",
	render: h => h(App)
});
```

-   App.vue

```vue
<template>
	<div id="app">
		<scrolling-ul>Some LI Tags ...</scrolling-ul>
	</div>
</template>
omit...
```

### 2. Direct Use in Vue-Cli Project

-   App.vue

```vue
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
		}
		omit...
	};
</script>
```

## Options

Some Vue prop options to config ScrollingUl.

| prop     | description                                                                                                | type         | default         |
| -------- | ---------------------------------------------------------------------------------------------------------- | ------------ | --------------- |
| start    | Turn on auto scrolling.                                                                                    | Boolean      | `true`          |
| smmoth   | Turn on smooth scrolling.                                                                                  | Boolean      | `true`          |
| infinity | Turn on infinite scrolling.                                                                                | Boolean      | `true`          |
| bar      | Configure virtual scroll bar.                                                                              | `barOptions` | `{show:'auto'}` |
| speed    | Scrolling speed. (unit: `pixel/ms`, remark: Must be a positive number)                                     | Number       | `30`            |
| delay    | Scrolling gap time. (unit: `ms`, remark: Only when `smooth` prop equals `false`,Must be a positive number) | Number       | `3000`          |
| nice     | Turn on customizes the CSS style and cancels the default style.                                            | Boolean      | `false`         |

### barOptions

Type: Object

| property | description                             | type                | default  |
| -------- | --------------------------------------- | ------------------- | -------- |
| show     | Turn on virtual scroll bar.             | Boolean \| `"auto"` | `"auto"` |
| style    | Custom CSS style of virtual scroll bar. | Object              | omit...  |

## More Custom Style

If you want to customize more styles using CSS.You can use the following className. (Only when `nice` prop equals `true`)

| className                    | description                                        |
| ---------------------------- | -------------------------------------------------- |
| `scrolling-nice-bar`         | the visual bar when normal.                        |
| `scrolling-nice-bar.barShow` | then visual bar when show.                         |
| `scrolling-nice-ul`          | the scrolling ul list when normal.                 |
| `scrolling-nice-ul.bar`      | then scrolling ul list when visual bar is enabled. |
