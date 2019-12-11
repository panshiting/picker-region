<template>
  <div class="picker">
    <div class="picker-title">
      <div ref="oneTab" v-show="oneshow" class="regin-title" @click="handleSelectProvince()" :class="[currentTab === 0 ? 'accolor':'']">
        {{ areaSelect && areaSelect.provName ? areaSelect.provName : '请选择' }}
      </div>
      <div ref="twoTab" v-show="twoshow" class="regin-title" @click="handleSelectCity()" :class="[currentTab === 1 ? 'accolor':'']">
        {{ areaSelect && areaSelect.cityName ? areaSelect.cityName : '请选择' }}
      </div>
      <div ref="threeTab" v-show="threeshow" class="regin-title"  @click="handleSelectCounty()" :class="currentTab === 2 ? 'accolor':''">
        {{ areaSelect && areaSelect.countyName ? areaSelect.countyName : '请选择'}}
      </div>
    </div>
		<div class="tabs-slider" :style="{transform:'translateX('+scrollLeft+'px)'}"></div>

		<div v-show="oneObjshow" class="list-warp">
			<div
				class="list-item"
				v-for="v in listProvince"
				:key="`province${v.provId}`"
				@click="handleClickProvince(v)"
				:class="{'select-active' : areaSelect.provId === v.provId}">
				{{v.provName}}
			</div>
		</div>

		<div v-show="twoObjshow" class="list-warp">
			<div
				class="list-item"
				v-for="v in listCity"
				:key="`city${v.cityId}`"
				@click="handleClickCity(v)"
				:class="{'select-active' : areaSelect.cityId === v.cityId}">
				{{v.cityName}}
			</div>
		</div>

		<div v-show="threeObjshow" class="list-warp">
			<div
				class="list-item"
				v-for="v in listCounty"
				:key="`county${v.countyId}`"
				@click="handleClickCounty(v)"
				:class="{'select-active' : areaSelect.countyId === v.countyId}">
				{{v.countyName}}
			</div>
		</div>
  </div>
</template>

<script>
export default {
  name: '',
  data () {
    return {
			// 省市区原数据
			listProvince: [
				{provName: '北京市', provId: 1},
				{provName: '广东省', provId: 2},
				{provName: '浙江省', provId: 3},
				{provName: '江苏省', provId: 4},
				{provName: '上海市', provId: 5},
				{provName: '河北省', provId: 6},
				{provName: '湖南省', provId: 7},
				{provName: '广西壮族自治区', provId: 8},
				{provName: '海南省', provId: 9},
				{provName: '吉林省', provId: 10},
				{provName: '湖南省', provId: 11}
			],
			listCity: [
				{cityName: '广州市', cityId: 1},
				{cityName: '惠州市', cityId: 2},
				{cityName: '深圳市', cityId: 3},
				{cityName: '清远市', cityId: 4},
				{cityName: '茂名市', cityId: 5},
				{cityName: '阳江市', cityId: 6},
				{cityName: '湛江市', cityId: 7},
				{cityName: '河源市', cityId: 8},
				{cityName: '汕头市', cityId: 9},
				{cityName: '潮汕市', cityId: 10}
			],
			listCounty: [
				{countyName: '天河区', countyId: 1},
				{countyName: '越秀区', countyId: 2},
				{countyName: '白云区', countyId: 3},
				{countyName: '黄浦区', countyId: 4},
				{countyName: '萝岗区', countyId: 5},
				{countyName: '番禺区', countyId: 6},
				{countyName: '增城区', countyId: 7},
				{countyName: '海珠区', countyId: 8}
			],
      oneshow: true,
      twoshow: false,
      threeshow: false,
      currentTab: 0,
      areaSelect: {
				provId: '',
				provName: '',
				cityId: '',
				cityName: '',
				countyId: '',
				countyName: ''
			},
			scrollLeft: 0,
			timer: null,
			oneObjshow: true,
			twoObjshow: false,
			threeObjshow: false
    }
	},
	mounted () {
		this.checkCor()
	},
	watch:{
		currentTab () {
			this.checkCor()
		}
	},
  methods: {
		checkCor () {
			this.timer && clearTimeout(this.timer)
			this.timer = setTimeout(() => {
				let tab = this.currentTab === 0 ? 'oneTab' : this.currentTab === 1 ? 'twoTab' : 'threeTab'
				let tabWidth = this.$refs[tab].offsetWidth
				let scrollLeft = this.$refs[tab].offsetLeft
				this.scrollLeft = scrollLeft + (tabWidth - 55) / 2
			}, 100)
		},
		handleSelectProvince () {
			this.currentTab = 0
			this.oneObjshow = true
			this.twoObjshow = false
			this.threeObjshow = false
			this.oneshow = true
			this.twoshow = false
			this.threeshow = false
			this.areaSelect.cityName = ''
			this.areaSelect.cityId = ''
			this.areaSelect.countyName = ''
			this.areaSelect.countyId = ''
		},
		handleSelectCity () {
			this.currentTab = 1
			this.oneObjshow = false
			this.twoObjshow = true
			this.threeObjshow = false
			this.oneshow = true
			this.twoshow = true
			this.threeshow = false
			this.areaSelect.countyName = ''
			this.areaSelect.countyId = ''
		},
		handleSelectCounty () {
			this.currentTab = 2
			this.oneObjshow = false
			this.twoObjshow = false
			this.threeObjshow = true
			this.oneshow = true
			this.twoshow = true
			this.threeshow = true
		},
		handleClickProvince (item) {
			this.currentTab = 1
			this.areaSelect.provId = item.provId
			this.areaSelect.provName = item.provName
			this.oneObjshow = false
			this.twoObjshow = true
			this.threeObjshow = false
			this.oneshow = true
			this.twoshow = true
			this.threeshow = false
		},
		handleClickCity (item) {
			this.currentTab = 2
			this.areaSelect.cityId = item.cityId
			this.areaSelect.cityName = item.cityName
			this.oneObjshow = false
			this.twoObjshow = false
			this.threeObjshow = true
			this.oneshow = true
			this.twoshow = true
			this.threeshow = true
		},
		handleClickCounty (item) {
			this.areaSelect.countyId = item.countyId
			this.areaSelect.countyName = item.countyName
		}
  }
}
</script>

<style lang="scss" scoped>
	.picker {
    .picker-title {
			border-bottom: 1px solid #e6e6e6;
			display: flex;
			.regin-title {
				color: #3d3d3d;
				margin-right: 15px;
				font-size: 16px;
				padding: 15px 20px;
				// padding: 15px 35px 15px 20px;
				overflow: hidden;
				text-overflow: ellipsis;
				white-space: nowrap;
				cursor: pointer;
			}
			.accolor {
				color: #0074ff;
			}
		}
		.tabs-slider {
			transition: 0.2s all linear;
			width: 55px;
			height: 2px;
			margin-top: -2px;
			background: #0074ff;
		}
		.list-warp {
			padding: 10px 15px;
			.list-item {
				color: #3d3d3d;
				font-size: 16px;
				height: 40px;
				line-height: 40px;
				cursor: pointer;
			}
		}
		.select-active {
			color: #0074ff;
			padding-right: 25px;
			display: inline-block;
			background: url(../assets/icon_right.png) no-repeat center right/10px 10px;
		}
	}
</style>