<template>
	<view class="calendar-content">
		<!-- :start-date="'2019-2-10'"
		:end-date="'2019-3-15'" -->
		<text class="calendar-title">日历组件</text>
		<view class="calendar-tags-group">
			<view
				class="calendar-tags"
				:class="{ checked: item.checked }"
				v-for="(item, index) in tags"
				:key="index"
				@click="toggle(index, item)"
			>
				<view class="calendar-tags-item">{{ item.name }}</view>
			</view>
		</view>
		<button class="calendar-button" type="button" @click="open">打开日历</button>
		<text v-if="timeData.lunar" class="calendar-title">已选日期</text>
		<view v-if="timeData.lunar" class="calendar-info">
			<view>当前选择时间 : {{timeData.fulldate}}</view>
			<view v-if="tags['lunar'].checked">农历日期 : {{timeData.year +
									'年' +
									timeData.month +
									'月' +
									timeData.date +
									'日 （' +
									timeData.lunar.astro +
									')'}}</view>
			<view v-if="tags['lunar'].checked"> {{
					timeData.lunar.gzYear +
						'年' +
						timeData.lunar.gzMonth +
						'月' +
						timeData.lunar.gzDay +
						'日 (' +
						timeData.lunar.Animal +
						'年)'
				}}
				{{ timeData.lunar.IMonthCn + timeData.lunar.IDayCn }}
				{{ timeData.lunar.isTerm ? timeData.lunar.Term : '' }}</view>
		</view>
		<view v-if="show" class="calendar-mask" @click="closeMask">
			<view class="calendar-box" @click.stop="">
				<uni-calendar
					:lunar="tags['lunar'].checked"
					:fixedHeihgt="tags['fixedHeihgt'].checked"
					:slide="slide"
					:disableBefore="tags['disableBefore'].checked"
					:start-date="startDate"
					:end-date="endDate"
					:date="date"
					@change="change"
					@to-click="toClick"
				/>
				<button class="calendar-button-confirm" @click="confirm">确认选择日期</button>
			</view>
		</view>
	</view>
</template>

<script>
export default {
	data() {
		let tags = {
			lunar: {
				name: '农历',
				checked: false,
				attr: 'lunar'
			},
			fixedHeihgt: {
				name: '固定高度',
				checked: false,
				attr: 'fixedHeihgt'
			},
			vertical: {
				name: '垂直滚动',
				checked: false,
				attr: 'vertical'
			},
			horizontal: {
				name: '水平滚动',
				checked: false,
				attr: 'horizontal'
			},
			startDate: {
				name: '开始日期(2019-03-05)',
				checked: false,
				attr: 'startDate'
			},
			endDate: {
				name: '结束日期(2019-04-10)',
				checked: false,
				attr: 'endDate'
			},
			disableBefore: {
				name: '禁用今天之前的日期',
				checked: false,
				attr: 'disableBefore'
			},
			date: {
				name: '自定义当前日期(2019-03-20)',
				checked: false,
				attr: 'date'
			}
		};

		return {
			show: false,
			tags,
			slide: 'none',
			date: '',
			startDate: '',
			endDate: '',
			timeData:{}
		};
	},
	onLoad() {},
	methods: {
		closeMask() {
			this.show = false;
		},
		toggle(index, item) {
			this.tags[index].checked = !item.checked;
			// item.checked = !item.checked;
			if (index === 'horizontal') {
				this.tags['vertical'].checked = false;
			}
			if (index === 'vertical') {
				this.tags['horizontal'].checked = false;
			}
			// this.attribute[item.attr] = !item.checked;
		},
		open() {
			if (this.tags['horizontal'].checked) {
				this.slide = 'horizontal';
			} else if (this.tags['vertical'].checked) {
				this.slide = 'vertical';
			} else {
				this.slide = 'none';
			}
			if (this.tags['startDate'].checked) {
				this.startDate = '2019-03-05';
			} else {
				this.startDate = '';
			}
			if (this.tags['endDate'].checked) {
				this.endDate = '2019-04-10';
			} else {
				this.endDate = '';
			}
			if (this.tags['date'].checked) {
				this.date = '2019-03-20';
			} else {
				this.date = '';
			}
			this.show = true;
		},
		change(e){
			console.log('change 返回:',e.fulldate);
			this.timeData = e ;
		},
		toClick(e){
			console.log('点击事件',e.fulldate);
			this.timeData = e ;
		},
		confirm(){
			this.show = false;
		}
	}
};
</script>

<style>
page {
	background: #fff;
}

.calendar-content {
	padding: 20upx 0;
	padding-bottom: 60upx;
	font-size: 26upx;
}

.calendar-content > .calendar-title {
	line-height: 80upx;
	font-weight: bold;
	color: #333;
	font-size: 30upx;
	border-left: 2px #0d9ebb solid;
	padding-left: 20upx;
	margin: 0 20upx;
}
.calendar-tags-group {
	display: flex;
	flex-wrap: wrap;
	justify-content: space-between;
	margin: 0 10upx;
}
.calendar-tags {
	width: 50%;
	box-sizing: border-box;
}
.calendar-tags-item {
	padding: 10upx 20upx;
	border: 1px #999 solid;
	color: #666;
	border-radius: 10upx;
	text-align: center;
	margin: 10upx;
}
.calendar-tags-item:active {
	background: #f8f8f8;
}
.checked .calendar-tags-item {
	background: #0d9ebb;
	color: #fff;
	border: 1px rgba(0, 0, 0, 0.1) solid;
}
.calendar-button {
	margin: 10upx 20upx;
}
.calendar-info {
	padding: 0 20upx;
}

.calendar-mask {
	position: fixed;
	top: 0;
	bottom: 0;
	display: flex;
	align-items: center;
	width: 100%;
	background: rgba(0, 0, 0, 0.4);
}
.calendar-box {
	margin: 20upx;
	border: 1px #f5f5f5 solid;
	border-radius: 10upx;
	width: 100%;
	overflow: hidden;
	background: #fff;
}
.calendar-button-confirm {
	margin: 10upx;
}
</style>
