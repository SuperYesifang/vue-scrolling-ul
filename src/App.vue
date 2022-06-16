<template>
	<div id="app">
		<div class="area" v-for="(area, index) in countrysList" :key="index">
			<h2>{{area.name}} 国家/地区列表</h2>
			<Main class="list">
				<li v-for="(country, index) in area.list" :key="index">{{country}}</li>
			</Main>
		</div>
	</div>
</template>

<script>
	import Main from "./components/Main.vue";
	export default {
		name: "app",
		components: {
			Main
		},
		data() {
			return {
				countrysList: `亚洲：  中国（CHN）  中国香港（HKG）  中国澳门（OMA）  中国台北(TPE、）  阿富汗（AFG）  巴林（BRN）  孟加拉国（BAN）  不丹 （BHU）  文莱（BRU）  柬埔寨（CAM）  朝鲜（PRK）  印度（IND）  印度尼西亚（INA）  伊拉克（IRQ）  伊朗（IRI）  日本（JPN）  约旦（JOR）  哈萨克斯坦（KAZ）  韩国（KOR）  科威特（KUW）  吉尔吉斯斯坦（KGZ）  老挝（LAO）  黎巴嫩（LIB）  马来西亚（MAS） 

马尔代夫（MDV）  蒙古（MGL）  缅甸（MYA)   尼泊尔（NEP）  巴基斯坦（PAK）  巴勒斯坦（PLE）  菲律宾 （PHI）  卡塔尔（QAT）  沙特阿拉伯（KSA）  新加坡（SIN）  斯里兰卡（ SRI）  叙利亚（SYR）  塔吉克斯坦（TJK）  泰国（ THA）  东帝汶（TLS）  土库曼斯坦（TKM）  阿拉伯联合酋长国（UAE）  乌兹别克斯坦（UZB）  越南（VIE ）  也门（ YEM）         

欧洲：  阿尔巴尼亚 （ALB）  安道尔（AND）  亚美尼亚（ ARM）  奥地利 （AUT）  阿塞拜疆（AZE）  白俄罗斯（BLR）  比利时（BEL ）  波黑（BIH）  保加利亚（BUL）  克罗地亚（CRO）  塞浦路斯（CYP）  捷克（CZE）  丹麦（DEN）  爱沙尼亚（EST）  芬兰（FIN）  马其顿（MKD ）  法国（FRA）  格鲁吉亚（GEO）  德国（GER）  英国（GBR ）  希腊（GRE）  匈牙利（HUN） 

冰岛（ISL ）  爱尔兰（IRL ）  以色列（ISR）  意大利（ITA）  拉脱维亚（LAT）  列支敦士登（ LIE）  立陶宛（LTU）  卢森堡（LUX）  马耳他（ MLT）  摩纳哥（MON）  荷兰（NED）  挪威（NOR）  波兰（POL）  葡萄牙（POR）  摩尔多瓦（MDA）  罗马尼亚（ROM）  俄罗斯（ RUS）  圣马力诺（SMR）  塞黑（SCG）  斯洛伐克（SVK）  斯洛文尼亚（SLO）  西班牙（ ESP）  瑞典（SWE ）  瑞士（SUI）  土耳其（TUR）   乌克兰（UKR）     

大洋洲：  美属萨摩亚（ASA）  澳大利亚（AUS）  库克群岛（COK）  密克罗尼西亚（FSM）  斐济（FIJi）  关岛（GUM ）  基里巴提（KIR）  瑙鲁（NRU）  新西兰（NZL)  帕劳( PLW ）  巴布亚新几内亚（PNG）  萨摩亚（SAM)  所罗门群岛（SOL）  汤加（TGA）  瓦努阿图（ VAN）     

美洲：  安提瓜和巴布达（ANT）  阿根廷（ARG）  阿鲁巴（ARU）  巴哈马（BAH）  巴巴多斯（BAR）  贝利塞（BIZ）  百慕大（BER）  玻利维亚（BOL）  巴西（ BRA）  英属维尔京群岛（IVB）  加拿大（CAN）  开曼群岛（CAY）  智利（ CHI）  哥伦比亚（COL）  哥斯达黎加（CRC）  古巴（CUB）  多米尼加（DMA）  民主多米尼加（DOM）  厄瓜多尔（ECU）  萨尔瓦多（ESA）  格林纳达（GRN）  危地马拉（GUA）  圭亚那（GUY）  海地（HAI）  洪都拉斯（HON）  牙买加（JAM）  墨西哥（MEX）  荷属安的列斯（AHO）  

尼加拉瓜（NCA）  巴拿马（PAN）  巴拉圭（PAR）  秘鲁（PER）  波多黎各（PUR ）  圣基特和内维斯（SKN）  圣卢西亚（LCA）  圣文森特和格林哪达（VIN）  苏里南（SURe）  特立尼达和多巴哥（TRI）  美国（USA ）  乌拉圭（URU）  委内瑞拉（ VEN）  维尔京群岛（ISV）     

非洲：  阿尔及利亚（ALG）  安哥拉（ANG）  贝宁（BEN ）  博茨瓦纳（BOT）  布尔基纳法索（BUR）  布隆迪（BDI）  喀麦隆（CMR）  佛得角（CPV）  中非（CAF）  乍得（CHA）  科摩罗（COM）  刚果（CGO）  科特迪瓦 （CIV ）  刚果民主共和国（COD ）  吉布提（DJ）  

埃及（EGY）  赤道几内亚 （GEQ）  厄立特里亚 （ERI）  埃塞俄比亚（ETH ）  加蓬（GAB）  冈比亚（GAM）  加纳（GHA）  几内亚（GUI）  几内亚比绍（GBS ）  肯尼亚（KEN）  莱索托（LES ）  利比里亚 （LBR）  利比亚阿拉伯加马西里亚（LBA）  马达加斯加（MAD ）  马拉维（MAW）  马里（MLI）  毛里塔尼亚（MTN ）  

毛里求斯（MRI）  摩洛哥（ MAR）  莫桑比克（MOZ）  纳米比亚（NAM ）  尼日尔 （NIG）  尼日利亚 （NGR）  卢旺达（RWA）  圣多美和普林西比（STP）  塞内加尔（SEN）  塞舌尔（SEY ）  塞拉利昂 （ SLE）  索马里（ SOM）  南非（RSA ）  苏丹（SUD）  斯威士兰（SWZ、 ）  多哥（TOG ）  突尼斯（TUN）  乌干达（UGA）  坦桑尼亚 （TAN）  赞比亚（ ZAM）  津巴布韦（ZIM ）`.match(/[^\s()（）]+[：:].+\n*.+\n*/g).reduce((o, i) => (o.push({name: i.split(/[:：]/)[0], list: i.split(/[:：]/)[1].match(/[^\s（）()]+[(（][^()（）]+[）)]/g)}),o), [])
			};
		}
	};
</script>

<style lang="scss">
	html,
	body,
	#app {
		margin: 0;
		padding: 0;
	}
	#app {
		display: flex;
		flex-wrap: wrap;
		.area {
			margin: 10px;
			padding: 10px;
			background: #ccc;
			h2 {
				text-align: center;
			}
		}
	}
	.list {
		width: calc(100% - 20px);
		height: 200px;
		padding: 10px;
		background-color: #fff;
		ul, li {
			list-style: none;
			margin: 0;
			padding: 0;
		}
		li {
			padding-left: 10px;
			border-top: 1px solid #888;
			font-size: 14px;
			line-height: 30px;
		}
	}
</style>
