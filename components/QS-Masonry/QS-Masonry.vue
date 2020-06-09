<template>
	<view class="QS-Masonry" :style="{ padding: padding }">
		<view class="QS-Masonry-container">
			<view class="col-container" v-for="(item, index) in getCol" :key="lists[index].id">
				<view class="col-container-item" :style="{ width: getColWidth }">
					<QSMasonryTemplate ref="QSMasonryTemplate" :type="type" :list="lists[index].list || []" :itemSpace="itemSpace"></QSMasonryTemplate>
				</view>
				<view class="colSpace" :style="{ width: colSpace + 'px' }"></view>
			</view>
			<view class="QS-hide-template" :style="{ width: getColWidth }">
				<QSMasonryTemplate ref="QSMasonryHideTemplate" :type="type" :list="list_hide"></QSMasonryTemplate>
			</view>
		</view>
	</view>
</template>

<script>
	import QSMasonryTemplate from './QS-Masonry-Template.vue';
	import { isArray } from './js/QS-Utils.js';
	function getDefLists (col) {
		const arr = [];
		for(let i = 0; i < Number(col); i++) {
			arr.push({ list: [], id: String(i), height: 0 })
		}
		return arr
	}
	export default {
		components: {QSMasonryTemplate},
		props: {
			type: {	//列表type 用于模板分流组件区分不同的模板
				type: String,
				default: ''
			},
			list: {	//list数组
				type: Array,
				default: ()=>[]
			},
			col: {	//列数
				type: [Number, String],
				default: 2
			},
			padding: {	//最外部的间距
				type: String,
				default: '20rpx'
			},
			colSpace: {	//列之间的间距
				type: [Number, String],
				default: uni.upx2px(15)
			},
			itemSpace: {	//每项之间的上下间距
				type: [Number, String],
				default: '10px'
			}
		},
		data() {
			return {
				lists: getDefLists(Number(this.col)),
				data: [],
				list_hide: [],
				maxWidth: 0
			}
		},
		watch: {
			list(newValue) {
				this.setData(newValue);
			},
			col(newValue) {
				this.resetLists();
				this.lists = getDefLists(Number(newValue));
				this.$nextTick(()=>{
					this.setData(this.data)
				})
			}
		},
		computed: {
			getCol() {
				const arr = [];
				for(let i = 0; i < Number(this.col); i++) {
					arr.push('')
				}
				return arr
			},
			getColWidth() {
				if(this.maxWidth) {
					const col = Number(this.col);
					const colWidth = (this.maxWidth -( Number(this.colSpace)*(col-1)))/col;
					return `${colWidth}px`
				}
				return 0;
			}
		},
		mounted() {
			this.setData(this.list);
		},
		methods: {
			getMaxWidth() {
				return new Promise((rs, rj) => {
					let view;
					// #ifdef MP-ALIPAY
					view = uni.createSelectorQuery();
					// #endif
					// #ifndef MP-ALIPAY
					view = uni.createSelectorQuery().in(this);
					// #endif
					view.select('.QS-Masonry-container').fields({size:true})
					view.exec(data=>{
						this.maxWidth = data[0]?data[0].width:342;
						rs();
					})
				})
			},
			getListInfo(list) {
				let _this = this;
				return new Promise((rs, rj) => {
					_this.list_hide = [...list];
					_this.$nextTick(()=>{
						setTimeout(()=>{
							const ref = _this.$refs.QSMasonryHideTemplate;
							if(ref && typeof ref.getQuery === 'function') {
								try{
									ref.getQuery().then(data=>{
										_this.list_hide = [];
										rs(data);
									})
								}catch(e){
									//TODO handle the exception
									rj(e)
								}
							}else{
								rj()
							}
						}, 0)
					})
				})
			},
			async setData(list) {
				list = JSON.parse(JSON.stringify(list))
				try{
					if(!isArray(list)) {
						console.log('QSMasonry异常: 不是数组')
						return;
					}
					if(!this.maxWidth) await this.getMaxWidth();
					if(list.length > 0) {
						if(list.length > this.data.length ){
							this.updateLists(list.slice(this.data.length, list.length));
						}else{
							this.resetLists();
							this.updateLists(list);
						}
					}else{
						this.resetLists();
					}
					this.data = list;
				}catch(e){
					//TODO handle the exception
				}
			},
			updateListsHeight() {
				return new Promise(async (rs, rj)=>{
					const promises = [];
					const refs = this.$refs.QSMasonryTemplate;
					for(let i = 0; i < this.lists.length; i++) {
						promises.push(refs[i].getBoxQuery());
					}
					const infos = await Promise.all(promises);
					for(let i = 0; i < infos.length; i++) {
						const item = infos[i];
						if(item && item.height !== undefined)
							this.lists[i].height = item.height;
					}
					rs()
				})
			},
			async updateLists(list) {
				await this.updateListsHeight();
				const listInfo = await this.getListInfo(list);
				
				for(let i = 0; i < list.length; i++) {
					this.addListData(list[i], listInfo[i])
				}
				this.$nextTick(()=>{
					this.$emit('updated')
				})
			},
			addListData(data, info) {
				const lists = this.lists;
				let obj = lists[0];
				for(let i = 1; i < lists.length; i++) {
					if(obj.height > lists[i].height) obj = lists[i];
				}
				obj.list.push(data);
				obj.height += info===null?10:info.height;
			},
			resetLists() {
				const lists = this.lists;
				for(let i = 0; i < lists.length; i++) {
					while(lists[i].list.length > 0) {
						lists[i].list.pop();
					}
					lists[i].height = 0
				}
			}
		}
	}
</script>

<style scoped>
	@import url("css/box-sizing-border-box.css");
	.QS-Masonry{
		width: 100%;
		display: flex;
		flex-direction: row;
	}
	.QS-Masonry-container{
		width: 100%;
		display: flex;
		flex-direction: row;
	}
	.QS-Masonry .col-container {
		display: flex;
		flex-direction: row;
	}
	.col-container-item{
	}
	.QS-hide-template{
		opacity: 0;
		pointer-events: none;
		position: absolute;
		top: -99999px;
		left: -99999px;
	}
	.colSpace{
		height: 100%;
	}
</style>
