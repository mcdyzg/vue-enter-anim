<template>
    <div class="home">
        <button @click='auto = !auto'>切换模式</button>
        <EnterAnim
            v-if='auto'
            animate-class='fade'
            v-for='item in 10'>
            <HelloWorld msg='aaa' />
        </EnterAnim>
        <EnterAnim
            v-else
            :manual='manual'
            v-for='item in 10'>
            <HelloWorld msg='aaa' />
        </EnterAnim>
    </div>
</template>

<script>
// @ is an alias to /src
import EnterAnim from '@/components/EnterAnim.vue'
import HelloWorld from '@/components/HelloWorld.vue'

export default {
	name: 'home',
	components: {
		EnterAnim,
		HelloWorld,
	},
	data() {
		return {
			// true:进入视窗时触发动画, false:手动通过控制manual属性控制动画
			auto: false,
			// 是否显示模糊背景
			manual: 'on',
		}
	},
	mounted() {
		setTimeout(() => {
			this.manual = 'off'
		}, 2000)
	},
}
</script>

<style lang="scss">
.item {
	height: 200px;
	background: #999;
}
@keyframes fade {
	from {
		opacity: 0;
		// filter: blur(20px);
	}
	to {
		opacity: 1;
		// filter: blur(0);
	}
}
.fade {
	animation: fade 2s;
}
</style>
