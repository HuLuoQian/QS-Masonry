<template>
	<view class="QS-Masonry-Template">
		<view :id="pre_id + index" :style="{ 'margin-bottom': itemSpace }" v-for="(item, index) in list" :key="index">
			<block v-if="type==='xxx'">
				
			</block>
			<block v-else>
				<def :listItem="item" :type="type"></def>
			</block>
		</view>
	</view>
</template>

<script>
	import def from './components/QS-Masonry-Template-Def.vue';
	export default {
		components: {def},
		props: {
			list: {
				type: Array,
				default:() => []
			},
			type: {
				type: String,
				default: ''
			},
			itemSpace: {
				type: [Number, String],
				default: '10px'
			}
		},
		data() {
			return {
				pre_id: 'QS_Masonry_ID_'
			}
		},
		methods: {
			getQuery() {
				return new Promise((rs,rj)=>{
					this.$nextTick(()=>{
						setTimeout(()=>{
							let view;
							// #ifdef MP-ALIPAY
							view = uni.createSelectorQuery();
							// #endif
							// #ifndef MP-ALIPAY
							view = uni.createSelectorQuery().in(this);
							// #endif
							
							for(let i = 0; i < this.list.length; i++) view.select(`#${this.pre_id + i}`).fields({size: true})
							
							view.exec(data =>{
								rs(data);
							})
						}, 0)
					})
				})
			},
			getBoxQuery() {
				return new Promise((rs,rj)=>{
					this.$nextTick(()=>{
						setTimeout(()=>{
							let view;
							// #ifdef MP-ALIPAY
							view = uni.createSelectorQuery();
							// #endif
							// #ifndef MP-ALIPAY
							view = uni.createSelectorQuery().in(this);
							// #endif
							
							view.select(`.QS-Masonry-Template`).fields({size: true})
							
							view.exec(data =>{
								rs(data[0]);
							})
						}, 0)
					})
				})
			}
		}
	}
</script>

<style scoped>
	@import url("css/box-sizing-border-box.css");
	.QS-Masonry-Template{
		width: 100%;
		display: flex;
		flex-direction: column;
	}
</style>
