<template>
<div
    ref="observe_elem"
    :class='{[autoAnimateClass]:active,[manualAnimateClass]:(manual && manual === "on"),menualItem:true}'>
    <slot></slot>
</div>
</template>

<script>
export default {
	name: 'EnterAnim',
	props: ['animate-class', 'manual'],
	computed: {
		// 非手动控制时的入场动画class名称
		autoAnimateClass() {
			// 如果手动开启，禁用入场动画，改手动控制
			if (this.manual) {
				return ''
			} else {
				return this.animateClass || this.defaultAnimateClass
			}
		},
		// 手动激活时的入场class
		manualAnimateClass() {
			return 'manual-active'
		},
	},
	data() {
		return {
			// 默认入场动画class名称
			defaultAnimateClass: 'active',
			// 是否已入场
			active: false,
			// ref元素
			el: null,
		}
	},
	methods: {
		isActive(elemArr) {
			if (elemArr[0].intersectionRatio > 0) {
				this.active = true
			} else {
				this.active = false
			}
		},
		// isInSight() {
		// 	const bound = this.el.getBoundingClientRect()
		// 	const clientHeight = window.innerHeight
		// 	if (bound.top <= clientHeight + 100) {
		// 		this.active = true
		// 	} else {
		// 		this.active = false
		// 	}
		// },
		// throttle(fn, wait = 500) {
		// 	let previous = null
		// 	return function() {
		// 		const now = new Date()
		// 		const context = this
		// 		const args = arguments
		// 		if (!previous) {
		// 			previous = now
		// 		}
		// 		const remaining = now - previous
		// 		if (wait && remaining >= wait) {
		// 			fn.apply(context, args)
		// 			previous = now
		// 		}
		// 	}
		// },
	},

	mounted() {
		this.el = this.$refs.observe_elem
		// 如果存在IntersectionObserver并且manual不为on，使用自动入场动画
		if (window.IntersectionObserver && !this.manual) {
			this.io = new IntersectionObserver(this.isActive, {
				// threshold: [0],
			})
			// 开始观察
			this.io.observe(this.el)
		}
		// 性能不好 不用了
		// else {
		// 	this.isInSight()
		// 	this.listener = document.addEventListener(
		// 		'scroll',
		// 		this.throttle(this.isInSight, 100),
		// 	)
		// }
	},
	beforeDestroy() {
		this.io && this.io.disconnect()
		// this.listener &&
		// 	document.removeEventListener(
		// 		'scroll',
		// 		this.throttle(this.isInSight),
		// 	)
	},
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
@keyframes fade {
	from {
		opacity: 0;
	}
	to {
		opacity: 1;
	}
}
.active {
	animation: fade 1s;
}
.menualItem {
	transition: all 0.3s;
}

.manual-active {
	filter: blur(10px);
}
</style>
