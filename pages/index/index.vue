<template>
	<view class="container">
		<QSMasonry ref="QSMasonry" hasImage :col="col"></QSMasonry>
		<button type="warn" style="position: fixed;bottom: 20px; right: 20px;" @tap="add">添加</button>
		<button type="warn" style="position: fixed;bottom: 20px; left: 20px;" @tap="changeCol">增加列数</button>
	</view>
</template>

<script>
	import QSMasonry from '@/components/QS-Masonry/QS-Masonry.vue';
	export default {
		components: { QSMasonry },
		data() {
			return {
				col: 2,
				list: [],
				
				images: [
				{
					path: 'http://imgsrc.baidu.com/forum/w%3D580/sign=791a660d9c2397ddd679980c6983b216/591f9758d109b3de80a0bb82c1bf6c81810a4c89.jpg',
				},
				{
					path: 'http://imgsrc.baidu.com/forum/w%3D580/sign=e8b1d9c798cad1c8d0bbfc2f4f3c67c4/ab5e4efbfbedab64dbb08428fa36afc37b311eed.jpg',
				},
				{
					path: 'http://imgsrc.baidu.com/forum/w%3D580/sign=a06847cac5fcc3ceb4c0c93ba244d6b7/85d3bc014a90f60394f557af3412b31bb151ed67.jpg',
				},
				{
					path: 'http://imgsrc.baidu.com/forum/w%3D580/sign=3792b3922da446237ecaa56aa8237246/c4be73094b36acaf746d5f1471d98d1000e99c1e.jpg',
				},
				{
					path: 'http://imgsrc.baidu.com/forum/w%3D580/sign=cbb0c62b3787e9504217f3642039531b/357828f5e0fe9925c181a0b839a85edf8cb1711e.jpg',
				},
				{
					path: 'http://imgsrc.baidu.com/forum/w%3D580/sign=d97763e5f5dcd100cd9cf829428a47be/bc48eec4b74543a96096332413178a82b801141e.jpg',
				},
				{
					path: 'http://imgsrc.baidu.com/forum/w%3D580/sign=0bce5a5ec4177f3e1034fc0540ce3bb9/a435064f78f0f73675c584e90755b319eac413f4.jpg',
				},
				{
					path: 'http://imgsrc.baidu.com/forum/w%3D580/sign=f480662e3cadcbef01347e0e9cae2e0e/8f5b1cd8bc3eb13517d8e851ab1ea8d3fc1f4489.jpg',
				},
				{
					path: 'http://imgsrc.baidu.com/forum/w%3D580/sign=ef3defefe8dde711e7d243fe97eecef4/906cfddcd100baa1f74e18514a10b912c9fc2e4e.jpg',
				},
				{
					path: 'http://imgsrc.baidu.com/forum/w%3D580/sign=b5dea914cefdfc03e578e3b0e43e87a9/45c1f303738da9774430d9c1bd51f8198718e38a.jpg',
				},
				{
					path: 'http://imgsrc.baidu.com/forum/w%3D580/sign=a3d88c1776899e51788e3a1c72a6d990/a65049086e061d959c5968d276f40ad163d9ca8a.jpg',
				},
				{
					path: 'http://imgsrc.baidu.com/forum/w%3D580/sign=f992cfd546fbfbeddc59367748f1f78e/6c7667d0f703918f03d163205c3d269758eec4ad.jpg',
				}
			]
			}
		},
		onLoad() {

		},
		onReady() {
			let _this = this;
			const list = Array(10).fill('').map((item, index)=>{ return { index: _this.list.length + index, img: _this.images[Math.floor(Math.random() * _this.images.length)].path } });
			_this.list = list;
			_this.$refs.QSMasonry.add(list)
		},
		methods: {
			add() {
				let _this = this;
				const list = Array(10).fill('').map((item, index)=>{ return { index: _this.list.length + index, img: _this.images[Math.floor(Math.random() * _this.images.length)].path } });
				_this.list = _this.list.concat(list);
				_this.$refs.QSMasonry.add(list)
			},
			changeCol() {
				if(this.col < 6)
					this.col += 1;
				else
					this.col = 2;
			}
		}
	}
</script>

<style scoped>
	.container{
		min-height: 100vh;
		background-color: #f2f2f2;
		width: 100%;
	}
</style>
