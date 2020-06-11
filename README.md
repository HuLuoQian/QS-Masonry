* ## QQ交流群: 750104037 [点我加入](https://jq.qq.com/?_wv=1027&k=5OyZoXa)

* ## 快速指引
* ## [使用前必读](#tip)
* ## [示例项目目录结构](#mddir)
* ## [Attributes](#Attributes)
* ## [Events](#Events)
* ## [Methods](#Methods)
* ## [hasImage属性注意](#hasImage)
* ## [示例代码](#demo)

## <span id="tip">使用前必读</span>
* ### 该组件采用`模板分流方式`，所有列表样式放在 组件文件夹/components 文件夹中, 由外部传type筛选, 因此 所有的列表样式都要在 QS-Masonry-Template.vue 中注册并由type 属性控制
* ### ~~为了性能考虑, 没有对每一项的图片有计算操作, 所以可能会出现在一次更新中布局列之间高度相差较大(取决于图片的高度差大小与排列的随机性), 但是一般问题不大, 因为这个差值不会无限增大, 在下一次更新中会弥补这个差距~~
* ### 加强对图片高度的计算，需要在模板中对image组件加上load与error事件，当所有图片加载完成后可以_emit 事件来告诉父级当前项的图片已加载完成， 详见[hasImage属性注意](#hasImage)

## <span id="mddir">示例项目目录结构<span>

```
|-- QS-Masonry
    |-- App.vue
    |-- main.js
    |-- manifest.json
    |-- pages.json
    |-- README.md
    |-- components
    |   |-- QS-Masonry	//组件文件夹
    |       |-- QS-Masonry-Template.vue	//模板分流组件
    |       |-- QS-Masonry.vue	//核心vue
    |       |-- components	//样式池
    |       |   |-- QS-Masonry-Template-Def.vue
    |       |-- css
    |       |   |-- box-sizing-border-box.css
    |       |-- js
    |           |-- baseUtil.js
    |           |-- QS-Utils.js
    |-- pages
    |   |-- index
    |       |-- index.vue
    |-- static
        |-- logo.png

```

## <span id="Attributes">Attributes</span>

```
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
	},
	hasImage: {	//列表是否包含图片
		type: [Boolean, String],
		default: false
	}
}

```

## <span id= "Events">Events</span>
| 事件名| 说明|  形参|
| --------- | -------- | -----: |
| updated| 列表dom更新完成事件| |

## <span id="Methods">Methods</span>
| 方法名| 说明|  传入参数|
| --------- | -------- | -----: |
| setData| 设置列表数组数据| |
| resetLists| 清空列表数组数据| |

## <span id="hasImage">hasImage属性注意</span>
* ### 当列表中有图片时， 可以传hasImage为true，可以保证计算精准
* ### hasImage为true后自己写的vue文件(列表样式vue)中必须this.$emit('imageLoaded')来告知父级图片加载准备完成(如果一项数据中有多张图片可以计算image组件的load或error次数，当次数等于图片数量时再emit), 万一该项没有图片， 也需要在mounted生命周期中emit
* 

## <span id="demo">示例代码</span>
```html
<template>
	<view class="container">
		<QSMasonry ref="QSMasonry" :col="col"></QSMasonry>
		<button type="warn" style="position: fixed;bottom: 20px; right: 20px;" @tap="add">添加</button>
		<button type="warn" style="position: fixed;bottom: 20px; left: 20px;" @tap="changeCol">增加列数</button>
	</view>
</template>
```

```javascript
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
		_this.$refs.QSMasonry.setData(list)
	},
	methods: {
		add() {
			let _this = this;
			const list = Array(10).fill('').map((item, index)=>{ return { index: _this.list.length + index, img: _this.images[Math.floor(Math.random() * _this.images.length)].path } });
			_this.list = _this.list.concat(list);
			_this.$refs.QSMasonry.setData(_this.list)
		},
		changeCol() {
			if(this.col < 6)
				this.col += 1;
			else
				this.col = 2;
		}
	}
}
```