<template>
	<el-popover
    placement="bottom-start"
	v-model="selectPopoverVisible"
	@show="showSelect" 
    width="350"
    trigger="click">
		<div class="city-select" ref="citySelect">
			<el-tabs type="border-card" v-model="cityPanelSelector">
				<el-tab-pane label="省份" name="province">
					<div class="panel-content">
						<ul>
							<li class="prov-list">
								<span class="list-title"></span>
								<div class="list-content">
									<el-button :class="{'is-active':selectArr[0] === 1}" @click="setSelect(1)" type="text" size="mini">全国</el-button>
								</div>
							</li>
							<li class="prov-list" v-for="(value,key) in proGroupList" :key="key">
								<span class="list-title">{{key}}</span>
								<div class="list-content">
									<el-button :class="{'is-active':selectArr[0] === item.value}"  type="text" @click="getCity(item.value)"  size="mini" v-for="item in value" :key="item.value">{{item.text}}</el-button>
								</div>
							</li>
						</ul>
					</div>
				</el-tab-pane>
				<el-tab-pane label="城市" name="city">
					<div class="panel-content city-list">
						<div class="hotCity" v-show="selectArr[0] === 1">
							<div style="padding:5px 0;">推荐热门城市</div>
							<el-button  size="mini" type="text"  @click="setHotCity(item)" v-for="item in hotCity" :key="item.value">{{item.text}}</el-button>
						</div>
						<el-button :class="{'is-active':selectArr[1] === item.value}" @click="setSelect(item.value)" size="mini"  type="text" :key="item.value" v-for="item in cityList">{{item.text}}</el-button>
					</div>
				</el-tab-pane>
			</el-tabs>
		</div>
	
		<el-button slot="reference" type="text" size="mini" class="select-trigger">
			{{areaText}}
			<i class="el-icon-arrow-down el-icon--right" :class="{'is-trigger-open':selectPopoverVisible}"></i>
		</el-button>
  </el-popover>
</template>

<script>
	import zone from '@/assets/js/zone.js'
	export default {
		name: 'citySelect',
		data() {
			return {
				proGroupList:{
					'A-G':[{text:'安徽',value:55},{text:'北京',value:35},{text:'重庆',value:38},{text:'福建',value:40},{text:'甘肃',value:62},{text:'广东',value:39},{text:'广西',value:57},{text:'贵州',value:60},{text:'澳门',value:67}],
					'H-K':[{text:'海南',value:58},{text:'河北',value:50},{text:'河南',value:49},{text:'黑龙江',value:54},{text:'湖北',value:48},{text:'湖南',value:56},{text:'江苏',value:42},{text:'江西',value:45},{text:'吉林',value:53}],
					'L-S':[{text:'辽宁',value:44},{text:'内蒙古',value:52},{text:'宁夏',value:63},{text:'青海',value:64},{text:'上海',value:36},{text:'山东',value:43},{text:'山西',value:51},{text:'陕西',value:47},{text:'四川',value:46}],
					'T-Z':[{text:'天津',value:37},{text:'新疆',value:65},{text:'西藏',value:61},{text:'云南',value:59},{text:'浙江',value:41},{text:'台湾',value:68},{text:'香港',value:66}]
				},
				cityList:[],
				hotCity:[
					{
						prov:41,
						text:'宁波',
						value:578
						
					},
					{
						prov:55,
						text:'合肥',
						value:765
					},
					{
						prov:41,
						text:'金华',
						value: 583
					},
					{
						prov:41,
						text:'杭州',
						value:577
						
					},
					{
						prov:42,
						text:'无锡',
						value:589
						
					},
					{
						prov:42,
						text:'南京',
						value:588
					},
					{
						prov:55,
						text:'滁州',
						value:774
					},
					{
						prov:43,
						text:'青岛',
						value:602
					},
					{
						prov:54,
						text:'绥化',
						value:763
					},
					{
						prov:42,
						text:'宿迁',
						value:597
					},
					{
						prov:55,
						text:'宿州',
						value:776
					},
					{
						prov:42,
						text:'徐州',
						value:596
					},
					{
						prov:43,
						text:'烟台',
						value:607
					},
					{
						prov:36,
						text:'上海',
						value:905
					},
					{
						prov:37,
						text:'天津',
						value:903
					},
					{
						prov:42,
						text:'苏州',
						value:590
					},
					{
						prov:48,
						text:'武汉',
						value:674
					},
					{
						prov:56,
						text:'长沙',
						value:782
					},
					{
						prov:54,
						text:'哈尔滨',
						value:752
					},
					{
						prov:54,
						text:'齐齐哈尔',
						value:753
					}
				],
				cityPanelSelector:'province',
				selectArr: [],
				selectPopoverVisible:false
			}
		},
		props: {
			value:{
				type:Array,
				default:function(){
					return [1];
				}
			}
		},
		computed:{
			areaText(){
				if(this.selectArr && this.selectArr.length){
					let prov = this.selectArr[0];
					if(prov === 1){
						return '全国';
					}else{
						let provObj = zone.find(item => item.value === prov);
						let provStr = provObj.text;
						let cityStr;
						if(provObj && provObj.items && this.selectArr[1]){
							cityStr = provObj.items.find(item => item.value === this.selectArr[1]).text;
						}
						if(!cityStr || provStr == cityStr){
							return provStr;
						}else{
							return provStr + ' / ' + cityStr;
						}
					}
				}else{
					return '全国';
				}
			}
		},
		methods:{
			showSelect(){
				this.cityPanelSelector = 'province';
				if(this.selectArr && this.selectArr.length && this.selectArr[0] != 1){
					let arrCache = zone.find(item => item.value == this.selectArr[0]);						
					if(arrCache && arrCache.items){
						this.cityList = arrCache.items;
					}else{
						this.cityList = [];
					}
				}
			},
			getCity(val){
				this.selectArr = [val];
				this.upDateProps();
				let arrCache = zone.find(item => item.value == val);						
				if(arrCache && arrCache.items){
					this.cityList = arrCache.items;
					this.cityPanelSelector = 'city';
				}else{
					this.cityList = [];
				}
			},
			setSelect(val){
				if(val === 1){
					this.selectArr = [val];
					this.cityList = [];
				}else{
					this.selectArr.splice(1, 1, val);
				}
				this.upDateProps();
				this.selectPopoverVisible = false;
			},
			setHotCity(item){
				this.selectArr = [item.prov,item.value];
				this.upDateProps();
				this.selectPopoverVisible = false;
			},
			upDateProps(){
				this.$emit('update:value',this.selectArr);
				this.$emit('change',this.selectArr.join(','));
			}
		},
		created(){
			console.log(zone.length)
			this.selectArr = this.value;
		}
	}
</script>
<style lang='scss' scoped>
	$my-city-font-color:#606266;
	$my-city-font-size:12px;
	
	.el-button{
		margin-left:0;
		margin-right:0px;
		font-size:$my-city-font-size;
	}
	.select-trigger{
		color:#fff;
		margin-top:7.5px;
		&:hover,&:focus{
			color:#409EFF;
		}
		.el-icon-arrow-down{
			font-size:14px;
			transition:transform 0.3s;
			&.is-trigger-open{
				transform: rotateZ(180deg);
			}
		}
	}
	
	.city-select{		
		.el-tabs--border-card{
			border:none;
			background:none;
			box-shadow: none;
		}
		.el-button--text{
			min-width:50px;
			color:$my-city-font-color;
			margin-left:0;
			&:hover,&:focus{
				color:#409EFF;
			}
			&.is-active{
				background-color:#409EFF;
				color:#fff;
			}
		}
	}
	.panel-content{
		padding: 0;
		margin:0;
		min-height: 232px;
	}
	.prov-list{
		overflow: hidden;
		text-align: center;
		
		box-sizing: content-box;
		font-size: 0;
		display: flex;
		.list-title{
			font-size:$my-city-font-size;
			color:$my-city-font-color - #333;
			display: inline-block;
			width:50px;
			line-height: 1;
			padding:8px 0;
		}
		.list-content{
			display: inline-block;
			width:calc(100% - 40px);
			text-align: left;
			vertical-align: top;
		}
	}
	.city-list .el-button{
		min-width:60px;
	}
	
</style>
