<template>
	<div id="scrolling-ul" @mouseenter="mouseenter" @mousemove="barMove" @mouseleave="mouseleave" @mouseup="barRelease">
		<div
			v-if="barShow"
			:class="['scrolling-nice-bar', { capture: !barFree, barShow }]"
			:style="{
				display: barH ? 'block' : 'none',
				top: barTop + 'px',
				height: barH + 'px',
				...(bar.style || {})
			}"
			@mousedown="barCapture"
		></div>
		<ul :class="['scrolling-nice-ul', { bar: barShow || infinity, infinity, capture: !barFree, bst }]" ref="scrolling-nice-ul" @scroll="scroll">
			<slot />
		</ul>
	</div>
</template>

<script>
	export default {
		name: "vue-scrolling-ul",
		props: {
			bst: {
				type: Boolean,
				default: true
			},
			smooth: {
				type: Boolean,
				default: true
			},
			delay: {
				type: Number,
				default: 3000
			},
			start: {
				type: Boolean,
				default: true
			},
			speed: {
				type: Number,
				default: 30
			},
			bar: {
				type: Object,
				default: () => ({
					show: false
				})
			},
			infinity: {
				type: Boolean,
				default: true
			}
		},
		data() {
			return {
				n: 0,
				kids: [],
				liH: 0,
				isReverse: false,
				ulH: undefined,
				barH: undefined,
				barTop: undefined,
				scrollH: undefined,
				upList: [],
				downList: [],
				timer: undefined,
				scrollTop: undefined,
				barFree: true,
				preOY: undefined,
				operation: false
			};
		},
		computed: {
			barShow() {
				switch (this.bar.show) {
					case true:
					case false:
						return this.bar.show;
					case "auto":
					default:
						return this.infinity ? this.operation : this.bst;
				}
			}
		},
		methods: {
			tagOrder() {
				[...this.kids].reverse().forEach((k, i) => {
					k.style.order = this.kids.length - i - 1;
				});
			},
			getHeight(el) {
				let style = el.currentStyle || getComputedStyle(el);
				let reg = /\d*\.?\d+/;
				let tops = style.marginTop.match(reg);
				let bottoms = style.marginBottom.match(reg);
				let top = (tops && tops.length && tops[0] * 1) || 0;
				let bottom = (bottoms && bottoms.length && bottoms[0] * 1) || 0;
				return el.offsetHeight + top + bottom;
			},
			updateList() {
				let scrollH = this.scrollH,
					ulH = this.ulH,
					kids = this.kids,
					downIndex = 0,
					upIndex = 0;
				kids.reduce((o, i, j) => ((o += i.offsetHeight) >= scrollH - ulH && !downIndex ? (downIndex = j) : o), 0);
				[...kids].reverse().reduce((o, i, j) => ((o += i.offsetHeight) > scrollH - ulH && !upIndex ? (upIndex = j) : o), 0);
				this.downList = kids.slice(0, downIndex + 2);
				this.upList = [...kids].reverse().slice(0, upIndex + 2);
			},
			init() {
				this.$nextTick(() => {
					clearTimeout(this.timer);
					clearInterval(this.timer);
					let el = (this.el = this.$refs["scrolling-nice-ul"]);
					if (el.scrollHeight > el.clientHeight) {
						let scrollH = (this.scrollH = el.scrollHeight);
						let ulH = (this.ulH = el.clientHeight);
						let kids = (this.kids = [].filter.call(el.children, item => item.tagName == "LI"));
						this.bar && (this.barH = ulH ** 2 / scrollH);
						if (this.infinity) this.tagOrder();
						if (this.smooth) {
							if (this.scrollTop == undefined) this.scrollTop = el.scrollTop;
						} else {
							this.updateList();
						}
						this.timer = setTimeout(() => this.autoScroll(), Math.random() * 200);
					}
					this.$emit("mounted");
				});
			},
			autoScroll(i = undefined) {
				if (!this.start) return;
				if (this.smooth) {
					let kids = this.kids;
					if (i != undefined) this.scrollTop = i;
					this.timer = setInterval(() => {
						if ((this.isReverse && this.scrollTop + this.ulH + 1 > this.scrollH) || (!this.isReverse && this.scrollTop - 1 < 0)) {
							this.isReverse = !this.isReverse;
						}
						this.el.scrollTop = this.isReverse ? ++this.scrollTop : --this.scrollTop;
						this.infinityDisplace();
					}, this.speed);
				} else {
					(async () => {
						let arr = this[this.isReverse ? "upList" : "downList"];
						for (let k = i || 0; k < arr.length; k++) {
							await new Promise(res => {
								arr[k].scrollIntoView({
									behavior: "smooth",
									block: this.isReverse ? "end" : "start"
								});
								this.timer = setTimeout(() => res(), this.delay);
								if (k >= arr.length - 1) {
									if (!this.infinity) this.isReverse = !this.isReverse;
									this.autoScroll();
								}
							});
						}
					})();
				}
			},
			mouseenter() {
				clearTimeout(this.timer);
				clearInterval(this.timer);
				if (this.infinity) this.operation = true;
			},
			mouseleave(e) {
				this.barRelease(e);
				if (this.infinity) this.operation = false;
				let n = undefined;
				if (this.smooth) {
					n = this.el.scrollTop;
				} else {
					let top = this.isReverse ? this.scrollH - this.ulH - this.el.scrollTop : this.el.scrollTop,
						list = this[`${this.isReverse ? "up" : "down"}List`];
					list.reduce((o, i, j) => {
						if ((o += i.offsetHeight) >= top && n == undefined) {
							if (list[j + 1]) n = j + 1;
							else n = 0;
						}
						return o;
					}, 0);
					if (!n) this.isReverse = !this.isReverse;
				}
				this.timer = setTimeout(() => this.autoScroll(n), this.smooth ? 0 : this.delay);
			},
			scroll() {
				let top = this.el.scrollTop;
				this.barTop = ((this.ulH - this.barH) * top) / (this.scrollH - this.ulH);
				if (!this.smooth) this.infinityDisplace();
			},
			infinityDisplace() {
				if (!this.infinity || this.operation) return;
				let kids = this.kids;
				let countH = 0,
					i = 0;
				for (; i < kids.length; i++) {
					let height = this.getHeight(kids[i]);
					if (height + countH > this.el.scrollTop) break;
					countH += height;
				}
				let arr = kids.splice(0, i);
				if (arr && arr.length) {
					kids.push(...arr);
					this.tagOrder();
					this.el.scrollTop = this.scrollTop = this.scrollTop - countH;
					if (!this.smooth) this.updateList();
				}
			},
			barCapture() {
				this.barFree = false;
			},
			barMove(e) {
				if (this.barFree) return;
				let oY = e.clientY;
				if (this.preOY == undefined) this.preOY = oY;
				let deltaY = oY - this.preOY;
				this.preOY = oY;
				let tmpSum = this.barTop + deltaY;
				let maxDeltaY = this.ulH - this.barH;
				if (tmpSum < 0) this.barTop = 0;
				if (tmpSum > maxDeltaY) this.barTop = maxDeltaY;
				if (tmpSum >= 0 && tmpSum <= maxDeltaY) this.barTop += deltaY;
				this.el.scrollTop = ((this.scrollH - this.ulH) * this.barTop) / (this.ulH - this.barH);
			},
			barRelease() {
				this.barFree = true;
				this.preOY = undefined;
			}
		},
		mounted() {
			this.init();
		}
	};
</script>

<style lang="scss" scoped>
	#scrolling-ul {
		position: relative;
		.scrolling-nice-bar {
			display: none;
			position: absolute;
			z-index: 10;
			top: 0;
			right: 4px;
			width: 10px;
			height: 100px;
			border-radius: 10px;
			cursor: pointer;
			background: rgba(168, 168, 168, 0.5);
			transform: scale(1);
			// transition: background, transform 0.2s, opacity 0.2s;
			opacity: 0;
			&.capture,
			&:hover {
				transform: scale(1.3, 1);
				background: rgba(158, 158, 158, 0.8);
			}
			&.barShow {
				display: block;
				opacity: 1;
				transition: background 0.2s, transform 0.2s, opacity 0.5s;
				// transition-delay: 0.5s;
			}
		}
		.scrolling-nice-ul {
			height: 100%;
			overflow-y: auto;
			& > li {
				flex-shrink: 0;
			}
			&.infinity {
				display: flex;
				flex-direction: column;
				justify-content: flex-start;
				align-items: inherit;
			}
			&.bar::-webkit-scrollbar {
				display: none;
			}
			&.capture {
				user-select: none;
			}
			&:not(.bar).bst {
				&::-webkit-scrollbar {
					width: 16px;
				}
				&::-webkit-scrollbar-thumb {
					box-shadow: inset 0 0 5px rgba(0, 0, 0, 0.2);
					background: rgba(190, 190, 190, 0.8);
				}
				&::-webkit-scrollbar-track {
					background: rgba(168, 168, 168, 0.3);
				}
			}
		}
	}
</style>
