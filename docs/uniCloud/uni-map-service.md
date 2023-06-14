## uni-map-service 公共模块

聚合了多家地图供应商的服务端API

::: warning 注意
uni-map-service公共模块仅能在云函数/云对象内使用。如果您不了解公共模块，请[参阅](cf-common.md)
:::

> 插件市场地址：[https://ext.dcloud.net.cn/plugin?name=uni-map-service](https://ext.dcloud.net.cn/plugin?name=uni-map-service)

## 公共返回参数@publicresult

以下所有API均会返回的参数

|参数						|说明																																													|
|---						|---																																													|
|errCode				|为0代表成功，其他均为失败																																		|
|errMsg					|失败后的提示																																									|
|originalResult	| 原始返回结果（供应商接口原始返回结果，需new UniMapService时，设置needOriginalResult: true）	|
|result					| 插件返回结果（抹平各平台差异后的返回结果）																											|

## 初始化实例

在调用API前，需要先初始化实例

```js
// 初始化实例
let uniMapService = new UniMapService({
	provider: "qqmap", // 指定使用哪家地图供应商
	needOriginalResult: false, // 是否需要返回原始信息
});
```

**参数**

|参数								|类型		|必填	|说明														|兼容性	|
|:--								|:-:		|:-:	|:--														|:-:		|
|provider						|String	|是		|指定使用哪家地图供应商					|all		|
|needOriginalResult	|Boolean|否		|是否需要返回原始信息，默认false|all		|

**provider可选项**

- qqmap 腾讯地图
- amap 高德地图

## API@api

### 逆地址解析（坐标转地址）@location2address

**示例**

```js
// 引入uni-map-service公共模块
const UniMapService  = require('uni-map-service');
// 初始化实例
let uniMapService = new UniMapService({
	provider: "qqmap", // 指定使用哪家地图供应商
});
// 调用API
let result = await uniMapService.location2address({
  location: "39.908815,116.397507"
});
console.log("result", result);
```

**请求参数**

|参数				|类型		|必填	|说明																																		|兼容性		|
|:--				|:-:		|:-:	|:--																																		|:-:			|
|location		|String	|是		|经纬度（GCJ02坐标系），格式：location=lat<纬度>,lng<经度>							|all			|
|get_poi		|Number	|否		|是否返回周边地点（POI）列表<br/>0：不返回（默认）<br/>1：返回					|all			|
|poi_options|String	|否		|周边POI（AOI）列表控制参数																							|腾讯地图	|
|poitype		|String	|否		|返回附近POI类型																												| 高德地图|
|radius			|String	|否		|搜索半径（radius取值范围在0~3000，默认是1000。单位：米）								| 高德地图|
|roadlevel	|Number	|否		|道路等级<br/>0：显示所有道路<br/>1：过滤非主干道路，仅输出主干道路数据	|高德地图	|
|homeorcorp	|String	|否		|是否优化POI返回顺序																										| 高德地图|

**返回参数**

仅列出result内的参数，其他参数见 [公共返回参数](#publicresult)

|参数								|类型		|说明										|兼容性	|
|:--								|:-:		|:--										|:-:		|
|formatted_addresses| String|详细地址								|all		|
|country						| String|国家										|all		|
|province						| String|省											|all		|
|city								| String|市											|all		|
|district						| String|区											|all		|
|street							| String|街道/道路，可能为空字串|all		|
|street_number			| String|门牌，可能为空字串			|all		|
|adcode							| String|行政区划代码						|all		|
|pois								| Array	|周边POI								|all		|
| &emsp;&#124;-- id				|String	|id											|all		|
| &emsp;&#124;-- title		|String	|地点名称								|all		|
| &emsp;&#124;-- address	|String	|地址										|all		|
| &emsp;&#124;-- location	|Object	|经纬度									|all		|
| &emsp;&#124;-- distance	|Number	|距离（米）							|all		|
| &emsp;&#124;-- direction|String	|方位										|all		|
| &emsp;&#124;-- category	|String	|类别										|all		|

### 地址解析（地址转坐标）@address2location

**示例**

```js
// 引入uni-map-service公共模块
const UniMapService  = require('uni-map-service');
// 初始化实例
let uniMapService = new UniMapService({
	provider: "qqmap", // 指定使用哪家地图供应商
});
// 调用API
let result = await uniMapService.address2location({
  address: "北京市海淀区彩和坊路海淀西大街74号"
});
console.log("result", result);
```

**请求参数**

|参数		|类型		|必填	|说明																																						|兼容性	|
|:--		|:-:		|:-:	|:--																																						|:-:		|
|address|String	|是		|地址（注：地址中请包含城市名称，以及需要对地址进行URL编码，否则会影响解析效果）|all		|
|city		|String	|否		|指定查询的城市（不传则在全国范围内查询）																				|all		|

**返回参数**

仅列出result内的参数，其他参数见 [公共返回参数](#publicresult)

|参数					|类型		|说明										|兼容性	|
|:--					|:-:		|:--										|:-:		|
|location			|Object	| 经纬度								|all		|
| &emsp;&#124;-- lat|Number	|纬度										|all		|
| &emsp;&#124;-- lng|Number	|经度										|all		|
|adcode				|String	|行政区划代码						|all		|
|province			| String|省											|all		|
|city					| String|市											|all		|
|district			| String|区，可能为空字串				|all		|
|street				| String|街道/道路，可能为空字串|all		|
|street_number| String|门牌，可能为空字串			|all		|

### 坐标转换@translate

**示例**

```js
// 引入uni-map-service公共模块
const UniMapService  = require('uni-map-service');
// 初始化实例
let uniMapService = new UniMapService({
	provider: "qqmap", // 指定使用哪家地图供应商
});
// 调用API
let result = await uniMapService.translate({
	locations: [
		{ lat: 39.908815, lng: 116.397507 },
		{ lat: 39.908815, lng: 116.397107 }
	],
	type: 3
});
console.log("result", result);
```

**请求参数**

|参数					|类型		|必填	|说明																																																					|兼容性																|
|:--					|:-:		|:-:	|:--																																																			|:-:																	|
|locations		|Array	|是		|预转换的坐标，支持批量转换																																										|all																	|
| &emsp;&#124;-- lat|Number|纬度																																																					|all																	|
| &emsp;&#124;-- lng|Number|经度																																																					|all																	|
|type					|Number	|否		|输入的locations的坐标类型，<br/>可选值：<br/>1：GPS <br/>2：sogou <br/>3：baidu <br/>4：mapbar <br/>6：sogou	|腾讯地图：全部支持; 高德地图：1、3、4|

**返回参数**

仅列出result内的参数，其他参数见 [公共返回参数](#publicresult)

|参数					|类型		|说明																					|兼容性	|
|:--					|:-:		|:--																					|:-:		|
|locations		|Array	| 坐标转换结果，转换后的坐标顺序与输入顺序一致|all		|
| &emsp;&#124;-- lat|Number	|纬度																					|all		|
| &emsp;&#124;-- lng|Number	|经度																					|all		|

### IP定位@ip2location

**示例**

```js
// 引入uni-map-service公共模块
const UniMapService  = require('uni-map-service');
// 初始化实例
let uniMapService = new UniMapService({
	provider: "qqmap", // 指定使用哪家地图供应商
});
// 调用API
let result = await uniMapService.ip2location({
  ip: "111.206.145.41"
});
console.log("result", result);
```

**请求参数**

|参数	|类型		|必填	|说明		|兼容性	|
|:--	|:-:		|:-:	|:--		|:-:		|
|ip		|String	|是		| IP地址|all		|

**返回参数**

仅列出result内的参数，其他参数见 [公共返回参数](#publicresult)

|参数					|类型		|说明															|兼容性		|
|:--					|:-:		|:--															|:-:			|
|location			|Object	| 坐标														|腾讯地图	|
| &emsp;&#124;-- lat|Number	|纬度															|腾讯地图	|
| &emsp;&#124;-- lng|Number	|经度															|腾讯地图	|
|nation				|String	| 国家														|腾讯地图	|
|nation_code	|String	| 国家代码（ISO3166标准3位数字码）|腾讯地图	|
|adcode				|Number	| 行政区划代码										|all			|
|province			|String	| 省															|all			|
|city					|String	| 市,可能为空											|all			|
|district			|String	| 区,可能为空											|腾讯地图	|
|rectangle		|String	| 所在城市矩形区域范围						|高德地图	|

### 关键词输入提示@inputtips

**示例**

```js
// 引入uni-map-service公共模块
const UniMapService  = require('uni-map-service');
// 初始化实例
let uniMapService = new UniMapService({
	provider: "qqmap", // 指定使用哪家地图供应商
});
// 调用API
let result = await uniMapService.inputtips({
	keyword: "人民医院",
	city: "北京市"
});
console.log("result", result);
```

**请求参数**

|参数						|类型		|必填	|说明																																																																			|兼容性																																																																			|
|:--						|:-:		|:-:	|:--																																																																			|:-:																																																																				|
|keyword				|String	|是		| 用户输入的关键词（希望获取后续提示的关键词）																																														|all																																																																				|
|city						|String	|是		| 限制城市范围																																																														|all																																																																				|
|citylimit			|Boolean|否		| false：当前城市无结果时，自动扩大范围到全国匹配（默认）<br/>true：固定在当前城市																												| all																																																																				|
|location				|Object	|否		| 定位坐标，传入后，若用户搜索关键词为类别词（如酒店、餐馆时），<br/>与此坐标距离近的地点将靠前显示|all			
| &emsp;&#124;-- lat|Number	|纬度															|all	|
| &emsp;&#124;-- lng|Number	|经度															|all	|																																																															|
|get_subpois		|Number	|否		| 是否返回子地点，如大厦停车场、出入口等取值<br/>0：不返回（默认） <br/>1：返回）																													|腾讯地图																																																																		|
|policy					|Number	|否		| 检索策略																																																																|腾讯地图																																																																		|
|filter					|String	|否		| 筛选条件																																																																|腾讯地图																																																																		|
|address_format	|String	|否		|可选值：short 返回“不带行政区划的”短地址																																																|腾讯地图																																																																		|
|page\_index		|Number	|否		| 页码，从1开始，最大页码需通过count进行计算，必须与page_size同时使用																																			|腾讯地图																																																																		|
|page\_size			|Number	|否		| 每页条数，取值范围1-20，必须与page_index 同时使用																																												|腾讯地图																																																																		|
|datatype				|Number	|否		| 返回的数据类型，多种数据类型用“\|”分隔 <br/>可选值：<br/>all：返回所有数据类型 <br/>poi：返回POI数据类型 <br/>bus：返回公交站点数据类型 <br/>busline：返回公交线路数据类型|高德地图																																																																		|

**返回参数**

仅列出result内的参数，其他参数见 [公共返回参数](#publicresult)

|参数								|类型		|说明																																																|兼容性		|
|:--								|:-:		|:--																																																|:-:			|
|data								|Array	| 提示词数组，每项为一个POI对象																																			|all			|
| &emsp;&#124;-- id				|String	|若数据为POI类型，则返回POI ID;若数据为bus类型，则返回bus id;若数据为busline类型，则返回busline id。|all			|
| &emsp;&#124;-- title			|String	|地点名称																																																|all			|
| &emsp;&#124;-- address	|String	|地址																																																|all			|
| &emsp;&#124;-- category	|String	|分类																																																|腾讯地图	|
| &emsp;&#124;-- type			|String	|POI类型，值说明：0:普通POI / 1:公交车站 / 2:地铁站 / 3:公交线路 / 4:行政区划												|腾讯地图	|
| &emsp;&#124;-- location	|String	|经纬度																																															|all			|
| &emsp;&emsp;&#124;-- lat|Number	|纬度																																																|all			|
| &emsp;&emsp;&#124;-- lng|Number	|经度																																																|all			|
| &emsp;&#124;-- adcode		|Number	|行政区划代码																																												|all			|
| &emsp;&#124;-- province	|String	|省																																																	|腾讯地图	|
| &emsp;&#124;-- city			|String	|市																																																	|腾讯地图	|
| &emsp;&#124;-- district	|String	|腾讯地图：区/县，当type（POI类型）为3（公交线路）时，district由city补全<br/>高德地图为:省+市+区（直辖市为“市+区”）				|all	|
|sub_pois						|String	|子地点列表，仅在输入参数get\_subpois=1时返回																												|腾讯地图	|
| &emsp;&#124;-- parent_id|String	|主地点ID，对应data中的地点ID																																				|腾讯地图	|
| &emsp;&#124;-- id				|String	|地点唯一标识																																												|腾讯地图	|
| &emsp;&#124;-- title		|String	|地点名称																																														|腾讯地图	|
| &emsp;&#124;-- address	|String	|地址																																																|腾讯地图	|
| &emsp;&#124;-- category	|String	|POI（地点）分类																																										|腾讯地图	|
| &emsp;&#124;-- location	|String	|地址																																																|腾讯地图	|
| &emsp;&emsp;&#124;-- lat|Number	|纬度																																																|all			|
| &emsp;&emsp;&#124;-- lng|Number	|经度																																																|all			|
| &emsp;&#124;-- adcode		|String	|行政区划代码																																												|腾讯地图	|
| &emsp;&#124;-- city			|String	|地址																																																|腾讯地图	|
| &emsp;&#124;-- address	|String	|地点所在城市名称																																										|腾讯地图	|

### 路线规划（导航）@route

#### 驾车（driving）@drivingroute

**示例**

```js
// 引入uni-map-service公共模块
const UniMapService  = require('uni-map-service');
// 初始化实例
let uniMapService = new UniMapService({
	provider: "qqmap", // 指定使用哪家地图供应商
});
// 调用API
let result = await uniMapService.route({
	mode: "driving",
	from: "40.034852,116.319820",
	to: "39.771075,116.351395"
});
console.log("result", result);
```

**请求参数**

|参数											|类型		|必填	|说明																																																																																																																																							|兼容性															|
|:--											|:-:		|:-:	|:--																																																																																																																																							|:-:																|
|mode											|String	|是		| 交通方式 固定为：driving																																																																																																																												|all																|
|from											|String	|是		| 起点位置坐标，格式：lat,lng																																																																																																																											|all																|
|to												|String	|是		| 终点位置坐标，格式：lat,lng																																																																																																																											|all																|
|from_poi									|String	|否		| 起点POI ID，传入后，优先级高于from（坐标）																																																																																																																			|all																|
|to_poi										|String	|否		| 终点POI ID，传入后，优先级高于to（坐标）																																																																																																																			|all																|
|policy										|String	|否		| 算路策略 [详情](#drivingroutepolicy)																																																																																																																						|all																|
|waypoints								|String	|否		| 格式：lat1,lng1;lat2,lng2;																																																																																																								|all																|
|avoid_polygons						|String	|否		| 避让区域																																																																																																																																				|all																|
|road_type								|Number	|否		| [from辅助参数] 起点道路类型[详情](#routeroadtype)																																																																																																																|all																|
|plate_number							|String	|否		| 车牌号，填入后，路线引擎会根据车牌对限行区域进行避让，不填则不不考虑限行问题																																																																																																		|all																|
|cartype									|Number	|否		| 车辆类型（影响限行规则），<br/>取值：<br/>0：[默认]普通汽车 <br/>1：新能源 <br/>2：插电式混动汽车																																																																																								|腾讯地图：0、1; 高德地图：全部支持	|
|heading									|Number	|否		| [from辅助参数]在起点位置时的车头方向，数值型，取值范围0至360（0度代表正北，顺时针一周360度）																																																																																										|腾讯地图														|
|speed										|Number	|否		| [from辅助参数]速度，单位：米/秒，默认3。 当速度低于1.39米/秒时，heading将被忽略																																																																																																	|腾讯地图														|
|accuracy									|Number	|否		| [from辅助参数]定位精度，单位：米，取>0数值，默认5。 当定位精度>30米时heading参数将被忽略																																																																																												|腾讯地图														|
|from_track								|Number	|否		| [from辅助参数]起点轨迹																																																																																																																													|腾讯地图														|
|get_mp										|Number	|否		| 是否返回多方案 <br/>0：[默认]仅返回一条路线方案 <br/>1：返回多方案（最多可返回三条方案供用户备选）																																																																																							|腾讯地图														|
|get_speed								|Number	|否		| 是否返回路况（道路速度）<br/>0：[默认]不返回路况 1：返回路况																																																																																																										|腾讯地图														|
|added_fields							|Number	|否		| 返回指定标准附加字段，取值支持 cities 路线途经行政区划信息(按路线A途经顺序排序)																																																																																																	|腾讯地图														|
|no_step									|Number	|否		| 不返回路线引导信息，可使回包数据量更小，<br/>取值：<br/>0：[默认]返回路线引导信息 <br/>1：不返回																																																																																								|腾讯地图														|
|avoidroad								|String	|否		| 避让道路名，只支持一条避让道路																																																																																																																									|高德地图														|
|ferry										|Number	|否		| 是否使用轮渡 <br/>0：使用渡轮 <br/>1：不使用渡轮																																																																																																																|高德地图														|
|show_fields							|Number	|否		| 返回结果控制，show\_fields用来筛选response结果中可选字段。<br/>show\_fields的使用需要遵循如下规则：<br/>1、具体可指定返回的字段类请见下方返回结果说明中的“show\_fields”内字段类型<br/>2、多个字段间采用“,”进行分割<br/>3、show\_fields未设置时，只返回基础信息类内字段<br/>	|高德地图														|

##### 【驾车】方式policy值范围@drivingroutepolicy

0：速度优先（只返回一条路线），此路线不一定距离最短

1：费用优先（只返回一条路线），不走收费路段，且耗时最少的路线

2：距离优先（只返回一条路线），仅走距离最短的路线，但是可能存在穿越小路/小区的情况

3：速度优先（只返回一条路线），不走快速路，例如京通快速路

32：高德地图APP默认策略

33：躲避拥堵

34：高速优先

35：不走高速

36：少收费

37：大路优先

38：速度最快

39：躲避拥堵＋高速优先

40：躲避拥堵＋不走高速

41：躲避拥堵＋少收费

42：少收费＋不走高速

43：躲避拥堵＋少收费＋不走高速

44：躲避拥堵＋大路优先

45：躲避拥堵＋速度最快

101：该策略会通过终点坐标查找所在地点（如小区/大厦等），并使用地点出入口做为目的地，使路径更为合理

注意：高德地图支持除101外的所有策略，腾讯地图只支持 0、34、35、36、37、101

##### 【驾车】road_type值范围@routeroadtype

0：[默认]不考虑起点道路类型

1：在桥上

2：在桥下

3：在主路

4：在辅路

5：腾讯地图：在对面	 高德地图：隧道

6：桥下主路

7：腾讯地图：桥下辅路	 高德地图：环岛

9：腾讯地图：不支持	 高德地图：停车场内部

**返回参数**

仅列出result内的参数，其他参数见 [公共返回参数](#publicresult)

**驾车（driving）返回参数**

|参数																		|类型		|说明																																																																																					|兼容性															|
|:--																		|:-:		|:--																																																																																					|:-:																|
|routes																	|Array	| 路线方案																																																																																		|all																|
| &emsp;&#124;-- mode										|String	|方案交通方式																																																																																	|all																|
| &emsp;&#124;-- tags										|Array	|方案标签																																																																																			|腾讯地图														|
| &emsp;&#124;-- distance								|Number	|方案总距离，单位：米																																																																													|all																|
| &emsp;&#124;-- traffic\_light\_count	|Number	|方案途经红绿灯个数																																																																														|腾讯地图														|
| &emsp;&#124;-- restriction_status			|Object	|限行状态码：<br/>0 途经没有限行城市，或路线方案未涉及限行区域<br/>1 途经包含有限行的城市<br/>3 [设置车牌] 已避让限行<br/>4 [设置车牌] 无法避开限行区域（本方案包含限行路段）	|腾讯地图：0、1、3、4 高德地图：0、1|
| &emsp;&#124;-- polyline								|Array	|方案路线坐标点串																																																																															|腾讯地图														|
| &emsp;&#124;-- waypoints							|Array	|途经点，顺序与输入waypoints一致 （输入waypoints时才会有此结点返回）																																																					|腾讯地图														|
| &emsp;&emsp;&#124;-- title						|String	|途经点路名																																																																																		|all																|
| &emsp;&emsp;&#124;-- location					|Object	|途经点坐标：																																																																																	|all																|
| &emsp;&emsp;&emsp;&#124;-- lat				|Number	|纬度																																																																																					|all																|
| &emsp;&emsp;&emsp;&#124;-- lng				|Number	|经度																																																																																					|all																|
| &emsp;&emsp;&#124;-- polyline_idx			|Number	|该途经点在polyline中的索引位置（数组下标位置）																																																																|all																|
| &emsp;&emsp;&#124;-- duration					|Number	|预估到达耗时，单位：分钟																																																																											|all																|
| &emsp;&emsp;&#124;-- distance					|Number	|起点到该途经点的距离，单位：米																																																																								|all																|
| &emsp;&#124;-- taxi_cost							|Number	|预估打车费用，单位：元																																																																												|all																|
| &emsp;&#124;-- cities									|Array	|路线途经行政区划（按途经顺序排序），数组中每个对象为一个区划																																																									|腾讯地图														|
| &emsp;&emsp;&#124;-- adcode						|Number	|途经行政区划的代码，代码对应的省市																																																																						|腾讯地图														|
| &emsp;&#124;-- steps									|Array	|路线步骤																																																																																			|all																|
| &emsp;&emsp;&#124;-- instruction			|String	|阶段路线描述																																																																																	|all																|
| &emsp;&emsp;&#124;-- polyline_idx			|Array	|阶段路线坐标点串在方案路线坐标点串的位置																																																																			|腾讯地图														|
| &emsp;&emsp;&#124;-- road_name				|String	|阶段路线路名																																																																																	|all																|
| &emsp;&emsp;&#124;-- dir_desc					|String	|阶段路线方向																																																																																	|all																|
| &emsp;&emsp;&#124;-- distance					|Number	|阶段路线距离，单位：米																																																																												|all																|
| &emsp;&emsp;&#124;-- act_desc					|String	|阶段路线末尾动作：如：左转调头																																																																								|腾讯地图														|
| &emsp;&emsp;&#124;-- accessorial_desc	|String	|末尾辅助动作：如：到达终点																																																																										|腾讯地图														|
| &emsp;&#124;-- speed									|Array	|路况信息，只有设置get_speed=1才会返回																																																																				|腾讯地图														|
| &emsp;&emsp;&#124;-- polyline_idx			|String	|阶段路线坐标点串，在路线坐标点串的位置																																																																				|腾讯地图														|
| &emsp;&emsp;&#124;-- distance					|String	|距离，单位：米																																																																																|腾讯地图														|
| &emsp;&emsp;&#124;-- level						|String	|路况类型：0:畅通 1:缓行 2:拥堵 3:无路况 4:严重拥堵																																																														|腾讯地图														|
| -----------														|Object	|以下字段是当show\_fields="cost,tmcs,navi,cities,polyline"时返回																																																							|高德地图														|
| &emsp;&#124;-- cost										|Object	|设置后可返回方案所需时间及费用成本																																																																						|高德地图														|
| &emsp;&emsp;&#124;-- duration					|String	|线路耗时，分段step中的耗时																																																																										|高德地图														|
| &emsp;&emsp;&#124;-- tolls						|String	|此路线道路收费，单位：元，包括分段信息																																																																				|高德地图														|
| &emsp;&emsp;&#124;-- toll_distance		|String	|收费路段里程，单位：米，包括分段信息																																																																					|高德地图														|
| &emsp;&emsp;&#124;-- toll_road				|String	|主要收费道路																																																																																	|高德地图														|
| &emsp;&emsp;&#124;-- traffic_lights		|String	|方案中红绿灯个数，单位：个																																																																										|高德地图														|
| &emsp;&#124;-- tmcs										|Object	|设置后可返回分段路况详情																																																																											|高德地图														|
| &emsp;&emsp;&#124;-- tmc_status				|String	|路况信息，包括：未知、畅通、缓行、拥堵、严重拥堵																																																															|高德地图														|
| &emsp;&emsp;&#124;-- tmc_distance			|String	|从当前坐标点开始step中路况相同的距离																																																																					|高德地图														|
| &emsp;&emsp;&#124;-- tmc_polyline			|String	|此段路况涉及的道路坐标点串，点间用","分隔																																																																		|高德地图														|
| &emsp;&#124;-- navi										|Object	|设置后可返回详细导航动作指令																																																																									|高德地图														|
| &emsp;&emsp;&#124;-- action						|String	|导航主要动作指令																																																																															|高德地图														|
| &emsp;&emsp;&#124;-- assistant_action	|String	|导航辅助动作指令																																																																															|高德地图														|
| &emsp;&#124;-- cities									|Object	|设置后可返回分段途径城市信息																																																																									|高德地图														|
| &emsp;&emsp;&#124;-- adcode						|String	|途径区域编码																																																																																	|高德地图														|
| &emsp;&emsp;&#124;-- citycode					|String	|途径城市编码																																																																																	|高德地图														|
| &emsp;&emsp;&#124;-- city							|String	|途径城市名称																																																																																	|高德地图														|
| &emsp;&emsp;&#124;-- district					|String	|途径区县信息																																																																																	|高德地图														|
| &emsp;&emsp;&emsp;&#124;-- name				|String	|途径区县名称																																																																																	|高德地图														|
| &emsp;&emsp;&emsp;&#124;-- adcode			|String	|途径区县adcode																																																																																|高德地图														|
| &emsp;&#124;-- polyline								|String	|设置后可返回分路段坐标点串，两点间用“,”分隔																																																																|高德地图														|

#### 步行（walking）@walkingroute

**示例**

```js
// 引入uni-map-service公共模块
const UniMapService  = require('uni-map-service');
// 初始化实例
let uniMapService = new UniMapService({
	provider: "qqmap", // 指定使用哪家地图供应商
});
// 调用API
let result = await uniMapService.route({
	mode: "walking",
	from: "40.034852,116.319820",
	to: "39.771075,116.351395"
});
console.log("result", result);
```

**请求参数**

|参数							|类型		|必填	|说明																																																								|兼容性		|
|:--							|:-:		|:-:	|:--																																																								|:-:			|
|mode							|String	|是		| 交通方式 固定为：walking																																													|all			|
|from							|String	|是		| 起点位置坐标，格式：lat,lng																																												|all			|
|to								|String	|是		| 终点位置坐标，格式：lat,lng																																												|all			|
|to_poi						|String	|否		| 终点POI ID，传入后，优先级高于to（坐标）																																				|腾讯地图			|
|alternative_route|Number	|否		| 1：多备选路线中第一条路线<br/>2：多备选路线中前两条路线<br/>3：多备选路线中三条路线<br/>不传则默认返回一条路线方案|高德地图	|
|show_fields			|String	|否		| 返回结果控制，show\_fields用来筛选response结果中可选字段。<br/>show\_fields的使用需要遵循如下规则：<br/>1、具体可指定返回的字段类请见下方返回结果说明中的“show\_fields”内字段类型<br/>2、多个字段间采用“,”进行分割<br/>3、show\_fields未设置时，只返回基础信息类内字段<br/>	|高德地图														|


**返回参数**

仅列出result内的参数，其他参数见 [公共返回参数](#publicresult)

**步行（walking）返回参数**

|参数																					|类型		|说明																																																																																																																																																							|兼容性		|
|:--																					|:-:		|:--																																																																																																																																																							|:-:			|
|routes																				|Array	| 路线方案																																																																																																																																																				|all			|
| &emsp;&#124;-- mode													|String	|方案交通方式																																																																																																																																																			|all			|
| &emsp;&#124;-- distance											|Number	|方案总距离，单位：米																																																																																																																																															|all			|
| &emsp;&#124;-- duration											|Number	|方案估算时间，单位：分钟																																																																																																																																													|all			|
| &emsp;&#124;-- direction										|String	|方案整体方向																																																																																																																																																			|腾讯地图	|
| &emsp;&#124;-- polyline											|Array	|方案路线坐标点串																																																																																																																																																	|腾讯地图	|
| &emsp;&#124;-- steps												|Array	|路线步骤																																																																																																																																																					|all			|
| &emsp;&emsp;&#124;-- instruction						|String	|阶段路线描述																																																																																																																																																			|all			|
| &emsp;&emsp;&#124;-- polyline_idx						|Array	|阶段路线坐标点串在方案路线坐标点串的位置																																																																																																																																					|腾讯地图	|
| &emsp;&emsp;&#124;-- road_name							|String	|阶段路线路名																																																																																																																																																			|all			|
| &emsp;&emsp;&#124;-- dir_desc								|String	|阶段路线方向																																																																																																																																																			|all			|
| &emsp;&emsp;&#124;-- distance								|Number	|阶段路线距离，单位：米																																																																																																																																														|all			|
| &emsp;&emsp;&#124;-- act_desc								|String	|阶段路线末尾动作：如：左转调头																																																																																																																																										|腾讯地图	|
| &emsp;&emsp;&#124;-- type										|Number	|阶段路线的步行设施类型（type），包含：<br/>0普通道路，1过街天桥，2地下通道，3人行横道																																																																																																														|腾讯地图	|
| &emsp;&#124;-- show\_fields									|Object	|可选差异化结果返回																																																																																																																																																|高德地图	|
| &emsp;&emsp;&#124;-- cost										|Object	|设置后可返回方案所需时间及费用成本																																																																																																																																								|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- duration					|String	|线路耗时，分段step中的耗时																																																																																																																																												|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- taxi							|String	|预估打车费用																																																																																																																																																			|高德地图	|
| &emsp;&emsp;&#124;-- navi										|Object	|设置后可返回详细导航动作指令																																																																																																																																											|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- action						|String	|导航主要动作指令																																																																																																																																																	|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- assistant_action	|String	|导航辅助动作指令																																																																																																																																																	|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- walk_type				|String	|算路结果中存在的道路类型：<br/>0，普通道路 1，人行横道 3，地下通道 4，过街天桥<br/>5，地铁通道 6，公园 7，广场 8，扶梯 9，直梯<br/>10，索道 11，空中通道 12，建筑物穿越通道<br/>13，行人通道 14，游船路线 15，观光车路线 16，滑道<br/>18，扩路 19，道路附属连接线 20，阶梯 21，斜坡<br/>22，桥 23，隧道 30，轮渡	|高德地图	|
| &emsp;&emsp;&#124;-- polyline								|String	|设置后可返回分路段坐标点串，两点间用“,”分隔																																																																																																																																		|高德地图	|

#### 骑行（bicycling）@bicyclingroute

**示例**

```js
// 引入uni-map-service公共模块
const UniMapService  = require('uni-map-service');
// 初始化实例
let uniMapService = new UniMapService({
	provider: "qqmap", // 指定使用哪家地图供应商
});
// 调用API
let result = await uniMapService.route({
	mode: "bicycling",
	from: "40.034852,116.319820",
	to: "39.771075,116.351395"
});
console.log("result", result);
```

**请求参数**

|参数							|类型		|必填	|说明																																																								|兼容性		|
|:--							|:-:		|:-:	|:--																																																								|:-:			|
|mode							|String	|是		| 交通方式 固定为：walking																																													|all			|
|from							|String	|是		| 起点位置坐标，格式：lat,lng																																												|all			|
|to								|String	|是		| 终点位置坐标，格式：lat,lng																																												|all			|
|to_poi						|String	|否		| 终点POI ID，传入后，优先级高于to（坐标）																																				|腾讯地图			|
|alternative_route|Number	|否		| 1：多备选路线中第一条路线<br/>2：多备选路线中前两条路线<br/>3：多备选路线中三条路线<br/>不传则默认返回一条路线方案|高德地图	|
|show_fields			|String	|否		| 返回结果控制，show\_fields用来筛选response结果中可选字段。<br/>show\_fields的使用需要遵循如下规则：<br/>1、具体可指定返回的字段类请见下方返回结果说明中的“show\_fields”内字段类型<br/>2、多个字段间采用“,”进行分割<br/>3、show\_fields未设置时，只返回基础信息类内字段<br/>	|高德地图														|

**返回参数**

仅列出result内的参数，其他参数见 [公共返回参数](#publicresult)

**骑行（bicycling）返回参数**

|参数																					|类型		|说明																																																																																																																																																							|兼容性		|
|:--																					|:-:		|:--																																																																																																																																																							|:-:			|
|routes																				|Array	| 路线方案																																																																																																																																																				|all			|
| &emsp;&#124;-- mode													|String	|方案交通方式																																																																																																																																																			|all			|
| &emsp;&#124;-- distance											|Number	|方案总距离，单位：米																																																																																																																																															|all			|
| &emsp;&#124;-- duration											|Number	|方案估算时间，单位：分钟																																																																																																																																													|all			|
| &emsp;&#124;-- direction										|String	|方案整体方向																																																																																																																																																			|腾讯地图	|
| &emsp;&#124;-- polyline											|Array	|方案路线坐标点串																																																																																																																																																	|腾讯地图	|
| &emsp;&#124;-- steps												|Array	|路线步骤																																																																																																																																																					|all			|
| &emsp;&emsp;&#124;-- instruction						|String	|阶段路线描述																																																																																																																																																			|all			|
| &emsp;&emsp;&#124;-- polyline_idx						|Array	|阶段路线坐标点串在方案路线坐标点串的位置																																																																																																																																					|腾讯地图	|
| &emsp;&emsp;&#124;-- road_name							|String	|阶段路线路名																																																																																																																																																			|all			|
| &emsp;&emsp;&#124;-- dir_desc								|String	|阶段路线方向																																																																																																																																																			|all			|
| &emsp;&emsp;&#124;-- distance								|Number	|阶段路线距离，单位：米																																																																																																																																														|all			|
| &emsp;&emsp;&#124;-- act_desc								|String	|阶段路线末尾动作：如：左转调头																																																																																																																																										|腾讯地图	|
| &emsp;&#124;-- show\_fields									|Object	|可选差异化结果返回																																																																																																																																																|高德地图	|
| &emsp;&emsp;&#124;-- cost										|Object	|设置后可返回方案所需时间及费用成本																																																																																																																																								|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- duration					|String	|线路耗时，分段step中的耗时																																																																																																																																												|高德地图	|
| &emsp;&emsp;&#124;-- navi										|Object	|设置后可返回详细导航动作指令																																																																																																																																											|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- action						|String	|导航主要动作指令																																																																																																																																																	|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- assistant_action	|String	|导航辅助动作指令																																																																																																																																																	|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- walk_type				|String	|算路结果中存在的道路类型：<br/>0，普通道路 1，人行横道 3，地下通道 4，过街天桥<br/>5，地铁通道 6，公园 7，广场 8，扶梯 9，直梯<br/>10，索道 11，空中通道 12，建筑物穿越通道<br/>13，行人通道 14，游船路线 15，观光车路线 16，滑道<br/>18，扩路 19，道路附属连接线 20，阶梯 21，斜坡<br/>22，桥 23，隧道 30，轮渡	|高德地图	|
| &emsp;&emsp;&#124;-- polyline								|String	|设置后可返回分路段坐标点串，两点间用“,”分隔																																																																																																																																		|高德地图	|

#### 电动车（ebicycling）@ebicyclingroute

**示例**

```js
// 引入uni-map-service公共模块
const UniMapService  = require('uni-map-service');
// 初始化实例
let uniMapService = new UniMapService({
	provider: "qqmap", // 指定使用哪家地图供应商
});
// 调用API
let result = await uniMapService.route({
	mode: "ebicycling",
	from: "40.034852,116.319820",
	to: "39.771075,116.351395"
});
console.log("result", result);
```

**请求参数**

|参数							|类型		|必填	|说明																																																								|兼容性		|
|:--							|:-:		|:-:	|:--																																																								|:-:			|
|mode							|String	|是		| 交通方式 固定为：ebicycling																																													|all			|
|from							|String	|是		| 起点位置坐标，格式：lat,lng																																												|all			|
|to								|String	|是		| 终点位置坐标，格式：lat,lng																																												|all			|
|to_poi						|String	|否		| 终点POI ID，传入后，优先级高于to（坐标）																																				|腾讯地图			|
|alternative_route|Number	|否		| 1：多备选路线中第一条路线<br/>2：多备选路线中前两条路线<br/>3：多备选路线中三条路线<br/>不传则默认返回一条路线方案|高德地图	|
|show_fields			|String	|否		| 返回结果控制，show\_fields用来筛选response结果中可选字段。<br/>show\_fields的使用需要遵循如下规则：<br/>1、具体可指定返回的字段类请见下方返回结果说明中的“show\_fields”内字段类型<br/>2、多个字段间采用“,”进行分割<br/>3、show\_fields未设置时，只返回基础信息类内字段<br/>	|高德地图														|

**返回参数**

仅列出result内的参数，其他参数见 [公共返回参数](#publicresult)

**电动车（ebicycling）返回参数**

|参数																					|类型		|说明																																																																																																																																																							|兼容性		|
|:--																					|:-:		|:--																																																																																																																																																							|:-:			|
|routes																				|Array	| 路线方案																																																																																																																																																				|all			|
| &emsp;&#124;-- mode													|String	|方案交通方式																																																																																																																																																			|all			|
| &emsp;&#124;-- distance											|Number	|方案总距离，单位：米																																																																																																																																															|all			|
| &emsp;&#124;-- duration											|Number	|方案估算时间，单位：分钟																																																																																																																																													|all			|
| &emsp;&#124;-- direction										|String	|方案整体方向																																																																																																																																																			|腾讯地图	|
| &emsp;&#124;-- polyline											|Array	|方案路线坐标点串																																																																																																																																																	|腾讯地图	|
| &emsp;&#124;-- steps												|Array	|路线步骤																																																																																																																																																					|all			|
| &emsp;&emsp;&#124;-- instruction						|String	|阶段路线描述																																																																																																																																																			|all			|
| &emsp;&emsp;&#124;-- polyline_idx						|Array	|阶段路线坐标点串在方案路线坐标点串的位置																																																																																																																																					|腾讯地图	|
| &emsp;&emsp;&#124;-- road_name							|String	|阶段路线路名																																																																																																																																																			|all			|
| &emsp;&emsp;&#124;-- dir_desc								|String	|阶段路线方向																																																																																																																																																			|all			|
| &emsp;&emsp;&#124;-- distance								|Number	|阶段路线距离，单位：米																																																																																																																																														|all			|
| &emsp;&emsp;&#124;-- act_desc								|String	|阶段路线末尾动作：如：左转调头																																																																																																																																										|腾讯地图	|
| &emsp;&#124;-- show\_fields									|Object	|可选差异化结果返回																																																																																																																																																|高德地图	|
| &emsp;&emsp;&#124;-- cost										|Object	|设置后可返回方案所需时间及费用成本																																																																																																																																								|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- duration					|String	|线路耗时，分段step中的耗时																																																																																																																																												|高德地图	|
| &emsp;&emsp;&#124;-- navi										|Object	|设置后可返回详细导航动作指令																																																																																																																																											|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- action						|String	|导航主要动作指令																																																																																																																																																	|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- assistant_action	|String	|导航辅助动作指令																																																																																																																																																	|高德地图	|
| &emsp;&emsp;&emsp;&#124;-- walk_type				|String	|算路结果中存在的道路类型：<br/>0，普通道路 1，人行横道 3，地下通道 4，过街天桥<br/>5，地铁通道 6，公园 7，广场 8，扶梯 9，直梯<br/>10，索道 11，空中通道 12，建筑物穿越通道<br/>13，行人通道 14，游船路线 15，观光车路线 16，滑道<br/>18，扩路 19，道路附属连接线 20，阶梯 21，斜坡<br/>22，桥 23，隧道 30，轮渡	|高德地图	|
| &emsp;&emsp;&#124;-- polyline								|String	|设置后可返回分路段坐标点串，两点间用“,”分隔																																																																																																																																		|高德地图	|
		
#### 公交（transit）@transitroute

基于公共汽车、地铁、火车等公共交通工具，计算起到终点的路线换乘方案，同时提供少换乘、少步行等偏好设置，支持一次返回多条方案以供用户选择。

**示例**

```js
// 引入uni-map-service公共模块
const UniMapService  = require('uni-map-service');
// 初始化实例
let uniMapService = new UniMapService({
	provider: "qqmap", // 指定使用哪家地图供应商
});
// 调用API
let result = await uniMapService.route({
	mode: "transit",
	from: "40.034852,116.319820",
	to: "39.771075,116.351395"
});
console.log("result", result);
```

**请求参数**

|参数							|类型		|必填	|说明																																																																																																																																							|兼容性		|
|:--							|:-:		|:-:	|:--																																																																																																																																							|:-:			|
|mode							|String	|是		| 交通方式 固定为：transit																																																																																																																												|all			|
|from							|String	|是		| 起点位置坐标，格式：lat,lng																																																																																																																											|all			|
|to								|String	|是		| 终点位置坐标，格式：lat,lng																																																																																																																											|all			|
|from_poi					|String	|否		| 起点POI ID，传入后，优先级高于from（坐标）																																																																																																																			|all			|
|to_poi						|String	|否		| 终点POI ID，传入后，优先级高于to（坐标）																																																																																																																				|all			|
|policy						|String	|否		| 算路策略 [详情](#transitroutepolicy)																																																																																																																						|all			|
|departure_time		|Number	|否		| 出发时间，用于过滤掉非运营时段的线路，格式为Unix时间戳，默认使用当前时间																																																																																																				|腾讯地图	|
|ad1							|String	|是		| 仅支持adcode																																																																																																																																		|高德地图	|
|ad2							|String	|是		| 仅支持adcode																																																																																																																																		|高德地图	|
|city1						|String	|是		| 起点所在城市（仅支持citycode，相同时代表同城，不同时代表跨城）																																																																																																									|高德地图	|
|city2						|String	|是		| 目的地所在城市（仅支持citycode，相同时代表同城，不同时代表跨城）																																																																																																								|高德地图	|
|alternative_route|Number	|否		|  返回方案条数 最大3条，mode为公交时，最大10条																																																																																																																		|高德地图	|
|multiexport			|Number	|否		| 地铁出入口数量 <br/>0：只返回一个地铁出入口 <br/>1：返回全部地铁出入口																																																																																																					|高德地图	|
|max_trans				|Number	|否		| 最大换乘次数																																																																																																																																		|高德地图	|
|nightflag				|Number	|否		| 考虑夜班车 <br/>0：不考虑夜班车 <br/>1：考虑夜班车																																																																																																															|高德地图	|
|date							|String	|否		| 请求日期 例如:2013-10-28																																																																																																																												|高德地图	|
|time							|String	|否		| 请求时间 例如:9-54																																																																																																																															|高德地图	|

##### 【公交】方式policy值范围@transitroutepolicy

0：默认模式

1：最经济模式，票价最低

2：最少换乘模式，换乘次数少

3：最少步行模式，尽可能减少步行距离

4：最舒适模式，尽可能乘坐空调车

5：不乘地铁模式，不乘坐地铁路线

6：地铁图模式，起终点都是地铁站（地铁图模式下originpoi及destinationpoi为必填项）

7：地铁优先模式，步行距离不超过4KM

8：时间短模式，方案花费总时间最少

注意：高德地图支持所有策略，腾讯地图只支持 0、2、3、5、6、7

**返回参数**

仅列出result内的参数，其他参数见 [公共返回参数](#publicresult)

**公交（transit）返回参数**

|参数															|类型		|说明																																																																															|兼容性		|
|:--															|:-:		|:--																																																																															|:-:			|
|routes														|Array	| 路线方案																																																																												|all			|
| &emsp;&#124;-- mode							|String	|方案交通方式																																																																											|all			|
| &emsp;&#124;-- distance					|Number	|方案总距离，单位：米																																																																							|all			|
| &emsp;&#124;-- duration					|Number	|方案估算时间，单位：分钟																																																																					|腾讯地图	|
| &emsp;&#124;-- bounds						|Number	|整体路线的外接矩形范围，可在地图显示时使用，通过矩形西南+东北两个端点坐标定义而面，示例：“39.901405,116.334023,39.940289,116.451720”														|腾讯地图	|
| &emsp;&#124;-- cost							|Object	|方案所需时间及费用成本																																																																						|高德地图	|
| &emsp;&emsp;&#124;-- duration		|String	|线路耗时，方案总耗时，包含等车时间																																																																|高德地图	|
| &emsp;&emsp;&#124;-- transit_fee|String	|各换乘方案总花费																																																																									|高德地图	|
| &emsp;&#124;-- steps						|Array	|一条完整公交路线可能会包含多种公共交通工具，各交通工具的换乘由步行路线串联起来，形成这样的结构（即 steps数组的结构）：[步行 , 公交 , 步行 , 公交 , 步行(到终点)]	|all			|
| &emsp;&emsp;&#124;-- mode				|String	|本段交通方式，取值：<br/>walking：步行<br/>transit：公共交通工具<br/>不同的方式，返回不同的数据结构，须根据该参数值来判断以哪种结构进行解析，各类具体定义见下文	|all			|
| &emsp;&emsp;&#124;-- 其它字段		|--			|随mode不同有不同字段返回，见下文																																																																	|all			|
		
公交方式包含步行+公共交通混合

以下是交通方式 - 步行 - 响应结果

|参数													|类型		|说明																			|兼容性		|
|:--													|:-:		|:--																			|:-:			|
| mode													|String	| 交通方式，固定值：walking								|all			|
| distance										|Number	|本段step距离，单位：米										|all			|
| duration										|Number	|估算时间，单位：分钟											|all	|
| direction										|String	|方案整体方向描述，如：“南”							|腾讯地图	|
| polyline										|Array	|路线坐标点串，可用于在地图中绘制路线			|腾讯地图	|
|steps												|Array	| 分路段导航信息													|腾讯地图			|
| &emsp;&#124;-- instruction	|String	|阶段路线描述															|all			|
| &emsp;&#124;-- polyline_idx	|Array	|阶段路线坐标点串在方案路线坐标点串的位置	|腾讯地图	|
| &emsp;&#124;-- road_name		|String	|阶段路线路名															|all			|
| &emsp;&#124;-- dir_desc			|String	|阶段路线方向															|腾讯地图			|
| &emsp;&#124;-- distance			|Number	|阶段路线距离，单位：米										|all			|
| &emsp;&#124;-- act_desc			|String	|阶段路线末尾动作：如：左转调头						|all	|

响应结果 - 交通方式 - 公共交通（transit）
本段为公共交通方式的对象结构，对应steps[]数组下的子对象（“mode”:“transit”）

1. 交通工具 - 公共汽车（“vehicle”：“BUS”）：
本段为公共交通的子对象，vehicle属性值为"BUS"


|参数														|类型		|说明																																																																														|兼容性		|
|:--														|:-:		|:--																																																																														|:-:			|
|mode														|String	| 交通方式，固定值：transit																																																																			|all			|
|lines													|Array	| lines线路信息，因为起点到终点，可能存在多条线路可选，所以lines为数组，而多条线路经过站点相同，所以数组第一会给出较完整信息，其它项侧只返回路线名称等简要信息。|all			|
| &emsp;&#124;-- vehicle				|String	|交通工具：公交车（BUS）																																																																				|all			|
| &emsp;&#124;-- id							|String	|线路唯一标识																																																																										|all			|
| &emsp;&#124;-- title					|String	|线路名称																																																																												|all			|
| &emsp;&#124;-- station_count	|Number	|经停站数																																																																												|all			|
| &emsp;&#124;-- distance				|Number	|路线距离，单位：米																																																																							|all			|
| &emsp;&#124;-- duration				|Number	|路线估算时间，单位：分钟																																																																				|all			|
| &emsp;&#124;-- polyline				|Array	|线路坐标点串，可用于在地图中绘制路线																																																														|all			|
| &emsp;&#124;-- start_time			|String	|首班车时间																																																																											|all			|
| &emsp;&#124;-- end_time				|String	|末班车时间																																																																											|all			|
| &emsp;&#124;-- geton					|Object	|上车站																																																																													|all			|
| &emsp;&emsp;&#124;-- id				|String	|站点唯一标识																																																																										|all			|
| &emsp;&emsp;&#124;-- title		|String	|终点站名																																																																												|all			|
| &emsp;&emsp;&#124;-- location	|Object	|站点经纬度坐标																																																																									|all			|
| &emsp;&emsp;&emsp;&#124;-- lat|Number	|纬度																																																																														|all			|
| &emsp;&emsp;&emsp;&#124;-- lng|Number	|经度																																																																														|all			|
| &emsp;&#124;-- getoff					|Object	|下车站																																																																													|all			|
| &emsp;&emsp;&#124;-- id				|String	|站点唯一标识																																																																										|all			|
| &emsp;&emsp;&#124;-- title		|String	|终点站名																																																																												|all			|
| &emsp;&emsp;&#124;-- location	|Object	|站点经纬度坐标																																																																									|all			|
| &emsp;&emsp;&emsp;&#124;-- lat|Number	|纬度																																																																														|all			|
| &emsp;&emsp;&emsp;&#124;-- lng|Number	|经度																																																																														|all			|
| &emsp;&#124;-- stations				|Array	|经停站列表																																																																											|all	|
| &emsp;&emsp;&#124;-- id				|String	|站点唯一标识																																																																										|all	|
| &emsp;&emsp;&#124;-- title		|String	|终点站名																																																																												|all	|
| &emsp;&emsp;&#124;-- location	|Object	|站点经纬度坐标																																																																									|all
| &emsp;&emsp;&emsp;&#124;-- lat|Number	|纬度																																																																														|all	|
| &emsp;&emsp;&emsp;&#124;-- lng|Number	|经度																																																																														|all	|
| &emsp;&#124;-- running_status	|Number	|线路运营状态，取值范围：<br/>300：正常<br/>301：可能错过末班车<br/>302：首班车还未发出<br/>303：停运																														|腾讯地图	|
| &emsp;&#124;-- price					|Number	|预估费用，单位：分，返回-1时为缺少票价信息																																																											|腾讯地图	|
| &emsp;&#124;-- destination		|Object	|公交终点站（用于指示方向）																																																																			|腾讯地图	|
| &emsp;&emsp;&#124;-- id				|String	|站点唯一标识																																																																										|腾讯地图	|
| &emsp;&emsp;&#124;-- title		|String	|终点站名																																																																												|腾讯地图	|
| &emsp;&#124;-- type						|String	|公交类型																																																																												|高德地图	|

2. 交通工具 - 地铁（“vehicle”：“SUBWAY”）：
本段为公共交通的子对象，vehicle属性值为"SUBWAY"

|参数															|类型		|说明																																																																														|兼容性		|
|:--															|:-:		|:--																																																																														|:-:			|
|mode															|String	| 交通方式，固定值：transit																																																																			|腾讯地图			|
|lines														|Array	| lines线路信息，因为起点到终点，可能存在多条线路可选，所以lines为数组，而多条线路经过站点相同，所以数组第一会给出较完整信息，其它项侧只返回路线名称等简要信息。|腾讯地图			|
| &emsp;&#124;-- vehicle					|String	|交通工具：地铁（SUBWAY）																																																																				|腾讯地图			|
| &emsp;&#124;-- id								|String	|线路唯一标识																																																																										|腾讯地图	|
| &emsp;&#124;-- title						|String	|线路名称																																																																												|腾讯地图			|
| &emsp;&#124;-- station_count		|Number	|经停站数																																																																												|腾讯地图			|
| &emsp;&#124;-- running_status		|Number	|线路运营状态，取值范围：<br/>300：正常<br/>301：可能错过末班车<br/>302：首班车还未发出<br/>303：停运																														|腾讯地图			|
| &emsp;&#124;-- price						|Number	|预估费用，单位：分，返回-1时为缺少票价信息																																																											|腾讯地图	|
| &emsp;&#124;-- distance					|Number	|路线距离，单位：米																																																																							|腾讯地图	|
| &emsp;&#124;-- duration					|Number	|路线估算时间，单位：分钟																																																																				|腾讯地图	|
| &emsp;&#124;-- polyline					|Array	|线路坐标点串，可用于在地图中绘制路线																																																														|腾讯地图	|
| &emsp;&#124;-- destination			|Object	|公交终点站（用于指示方向）																																																																			|腾讯地图	|
| &emsp;&emsp;&#124;-- id					|String	|站点唯一标识																																																																										|腾讯地图	|
| &emsp;&emsp;&#124;-- title			|String	|终点站名																																																																												|腾讯地图	|
| &emsp;&#124;-- start_time				|String	|首班车时间																																																																											|腾讯地图	|
| &emsp;&#124;-- end_time					|String	|末班车时间																																																																											|腾讯地图	|
| &emsp;&#124;-- geton						|Object	|上车站																																																																													|腾讯地图	|
| &emsp;&emsp;&#124;-- id					|String	|站点唯一标识																																																																										|腾讯地图	|
| &emsp;&emsp;&#124;-- title			|String	|终点站名																																																																												|腾讯地图	|
| &emsp;&emsp;&#124;-- location		|Object	|站点经纬度坐标																																																																									|腾讯地图	|
| &emsp;&emsp;&emsp;&#124;-- lat	|Number	|纬度																																																																														|腾讯地图	|
| &emsp;&emsp;&emsp;&#124;-- lng	|Number	|经度																																																																														|腾讯地图	|
| &emsp;&emsp;&#124;-- exit				|Object	|站点经纬度坐标																																																																									|腾讯地图	|
| &emsp;&emsp;&emsp;&#124;-- id		|String	|唯一标识																																																																												|腾讯地图	|
| &emsp;&emsp;&emsp;&#124;-- title|String	|出入口名称																																																																											|腾讯地图	|
| &emsp;&#124;-- getoff						|Object	|下车站																																																																													|腾讯地图	|
| &emsp;&emsp;&#124;-- id					|String	|站点唯一标识																																																																										|腾讯地图	|
| &emsp;&emsp;&#124;-- title			|String	|终点站名																																																																												|腾讯地图	|
| &emsp;&emsp;&#124;-- location		|Object	|站点经纬度坐标																																																																									|腾讯地图	|
| &emsp;&emsp;&emsp;&#124;-- lat	|Number	|纬度																																																																														|腾讯地图	|
| &emsp;&emsp;&emsp;&#124;-- lng	|Number	|经度																																																																														|腾讯地图	|
| &emsp;&emsp;&#124;-- exit				|Object	|站点经纬度坐标																																																																									|腾讯地图	|
| &emsp;&emsp;&emsp;&#124;-- id		|String	|唯一标识																																																																												|腾讯地图	|
| &emsp;&emsp;&emsp;&#124;-- title|String	|出入口名称																																																																											|腾讯地图	|
| &emsp;&#124;-- stations					|Array	|经停站列表																																																																											|腾讯地图	|
| &emsp;&emsp;&#124;-- id					|String	|站点唯一标识																																																																										|腾讯地图	|
| &emsp;&emsp;&#124;-- title			|String	|终点站名																																																																												|腾讯地图	|
| &emsp;&emsp;&#124;-- location		|Object	|站点经纬度坐标																																																																									|腾讯地图	|
| &emsp;&emsp;&emsp;&#124;-- lat	|Number	|纬度																																																																														|腾讯地图	|
| &emsp;&emsp;&emsp;&#124;-- lng	|Number	|经度																																																																														|腾讯地图	|

3. 交通工具 - 火车（“vehicle”：“RAIL”）：
本段为公共交通的子对象，vehicle属性值为"RAIL"

|参数														|类型		|说明																																																																														|兼容性		|
|:--														|:-:		|:--																																																																														|:-:			|
|mode														|String	| 交通方式，固定值：transit																																																																			|all	|
|lines													|Array	| lines线路信息，因为起点到终点，可能存在多条线路可选，所以lines为数组，而多条线路经过站点相同，所以数组第一会给出较完整信息，其它项侧只返回路线名称等简要信息。|all	|
| &emsp;&#124;-- vehicle				|String	|交通工具：地铁（RAIL）																																																																					|all	|
| &emsp;&#124;-- title					|String	|车次名称																																																																												|all	|
| &emsp;&#124;-- station_count	|Number	|经停站数																																																																												|腾讯地图	|
| &emsp;&#124;-- running_status	|Number	|线路运营状态，取值范围：<br/>300：正常<br/>301：可能错过末班车<br/>302：首班车还未发出<br/>303：停运																														|腾讯地图	|
| &emsp;&#124;-- price					|Number	|预估费用，单位：分，返回-1时为缺少票价信息																																																											|腾讯地图	|
| &emsp;&#124;-- distance				|Number	|路线距离，单位：米																																																																							|all	|
| &emsp;&#124;-- duration				|Number	|路线估算时间，单位：分钟																																																																				|all	|
| &emsp;&#124;-- departure_time	|String	|发车时间																																																																												|腾讯地图	|
| &emsp;&#124;-- arrival_time		|String	|到达时间																																																																												|腾讯地图	|
| &emsp;&#124;-- days_count			|Number	|耗时天数，1为当天到达，2为隔天到达，以此类推																																																										|腾讯地图	|
| &emsp;&#124;-- polyline				|Array	|线路坐标点串，可用于在地图中绘制路线																																																														|腾讯地图	|
| &emsp;&#124;-- destination		|Object	|公交终点站（用于指示方向）																																																																			|腾讯地图	|
| &emsp;&emsp;&#124;-- id				|String	|站点唯一标识																																																																										|腾讯地图	|
| &emsp;&emsp;&#124;-- title		|String	|终点站名																																																																												|腾讯地图	|
| &emsp;&#124;-- geton					|Object	|上车站																																																																													|all	|
| &emsp;&emsp;&#124;-- id				|String	|站点唯一标识																																																																										|all	|
| &emsp;&emsp;&#124;-- title		|String	|终点站名																																																																												|all	|
| &emsp;&emsp;&#124;-- location	|Object	|站点经纬度坐标																																																																									|all	|
| &emsp;&emsp;&emsp;&#124;-- lat|Number	|纬度																																																																														|all	|
| &emsp;&emsp;&emsp;&#124;-- lng|Number	|经度																																																																														|腾讯地图	|
| &emsp;&emsp;&#124;-- adcode		|String	|上车站点所在城市的adcode																																																																				|高德地图	|
| &emsp;&emsp;&#124;-- time			|String	|上车点发车时间																																																																									|高德地图	|
| &emsp;&emsp;&#124;-- start		|Number	|是否始发站，1表示为始发站，0表示非始发站																																																												|高德地图	|
| &emsp;&#124;-- getoff					|Object	|下车站																																																																													|all	|
| &emsp;&emsp;&#124;-- id				|String	|站点唯一标识																																																																										|all	|
| &emsp;&emsp;&#124;-- title		|String	|终点站名																																																																												|all	|
| &emsp;&emsp;&#124;-- location	|Object	|站点经纬度坐标																																																																									|all	|
| &emsp;&emsp;&emsp;&#124;-- lat|Number	|纬度																																																																														|all	|
| &emsp;&emsp;&emsp;&#124;-- lng|Number	|经度																																																																														|all	|
| &emsp;&emsp;&#124;-- adcode		|String	|上车站点所在城市的adcode																																																																				|高德地图	|
| &emsp;&emsp;&#124;-- time			|String	|上车点发车时间																																																																									|高德地图	|
| &emsp;&emsp;&#124;-- end			|Number	|是否为终点站，1表示为终点站，0表示非终点站																																																											|高德地图	|
| &emsp;&#124;-- stations				|Array	|经停站列表																																																																											|腾讯地图	|
| &emsp;&emsp;&#124;-- id				|String	|站点唯一标识																																																																										|腾讯地图	|
| &emsp;&emsp;&#124;-- title		|String	|终点站名																																																																												|腾讯地图	|
| &emsp;&emsp;&#124;-- location	|Object	|站点经纬度坐标																																																																									|腾讯地图	|
| &emsp;&emsp;&emsp;&#124;-- lat|Number	|纬度																																																																														|腾讯地图	|
| &emsp;&emsp;&emsp;&#124;-- lng|Number	|经度																																																																														|腾讯地图	|
| &emsp;&#124;-- spaces					|Object	|仓位及价格信息																																																																									|腾讯地图	|
| &emsp;&emsp;&#124;-- code			|String	|仓位编码																																																																												|高德地图	|
| &emsp;&emsp;&#124;-- cost			|String	|仓位费用																																																																												|高德地图	|

3. 交通工具 - 出租车（“vehicle”：“TAXI”）：
本段为公共交通的子对象，vehicle属性值为"TAXI"

|参数														|类型		|说明																																																																														|兼容性		|
|:--														|:-:		|:--																																																																														|:-:			|
|mode														|String	| 交通方式，固定值：transit																																																																			|高德地图	|
|lines													|Array	| lines线路信息，因为起点到终点，可能存在多条线路可选，所以lines为数组，而多条线路经过站点相同，所以数组第一会给出较完整信息，其它项侧只返回路线名称等简要信息。|高德地图	|
| &emsp;&#124;-- vehicle				|String	|交通工具：地铁（TAXI）																																																																					|高德地图	|
| &emsp;&#124;-- price					|String	|预估费用，单位：分，返回-1时为缺少票价信息																																																											|高德地图	|
| &emsp;&#124;-- drivetime				|Number	|打车预计花费时间																																																																				|高德地图	|
| &emsp;&#124;-- distance				|Number	|打车距离																																																																				|高德地图	|
| &emsp;&#124;-- polyline				|String	|线路点集合																																																													|高德地图	|
| &emsp;&#124;-- startpoint				|String	|打车起点经纬度																																																																				|高德地图	|
| &emsp;&#124;-- startname				|String	|打车起点名称																																																																				|高德地图	|
| &emsp;&#124;-- endpoint				|String	|打车终点经纬度																																																																				|高德地图	|
| &emsp;&#124;-- endname				|String	|打车终点名称																																																																				|高德地图	|

## 全局错误码@errorcode
	
| 错误模块				|    错误码	|             说明																																				|
|:--							|:--:				|:--																																											|
| uni-map-service	|    110		| 请求来源未被授权																																				|
| uni-map-service	|    111		| 签名验证失败																																						|
| uni-map-service	|    112		| IP未被授权																																							|
| uni-map-service	|    113		| 此功能未被授权																																					|
| uni-map-service	|    120		| 此key每秒请求量已达到上限																																|
| uni-map-service	|    121		| 此key每日调用量已达到上限																																|
| uni-map-service	|    160		| sig参数不支持此请求类型																																	|
| uni-map-service	|    161		| sig参数不支持和非object的POST JSON一起使用																							|
| uni-map-service	|    190		| 无效的KEY																																								|
| uni-map-service	|    199		| 此key未开启webservice功能																																|
| uni-map-service	|    301		| 缺少必要字段key																																					|
| uni-map-service	|    311		| key格式错误																																							|
| uni-map-service	|    300		| 缺少必要字段																																						|
| uni-map-service	|    306		| 缺少参数																																								|
| uni-map-service	|    310		| 参数格式错误																																						|
| uni-map-service	|320				|参数数据类型错误																																					|
| uni-map-service	|330				|参数长度错误																																							|
| uni-map-service	|351				|存在不共存的参数																																					|
| uni-map-service	|324				|get和post中的同一参数值不相同																														|
| uni-map-service	|326				|起终点距离过近																																						|
| uni-map-service	|327				|附近无公交站																																							|
| uni-map-service	|328				|无可达公交路线																																						|
| uni-map-service	|329				|无可达火车路线																																						|
| uni-map-service	|331				|查询条件过长																																							|
| uni-map-service	|332				|途径点个数超过限制																																				|
| uni-map-service	|333				|存在无法吸附的坐标点																																			|
| uni-map-service	|335				|不支持该城市的公交查询																																		|
| uni-map-service	|341				|缺少keyword（关键词）																																		|
| uni-map-service	|344				|附近无火车站（公交）																																			|
| uni-map-service	|347				|查询无结果																																								|
| uni-map-service	|348				|参数错误																																									|
| uni-map-service	|364				|是否扩大搜索参数只能为0或1																																|
| uni-map-service	|365				|纬度不能超过±90																																					|
| uni-map-service	|366				|经度不能超过±180																																					|
| uni-map-service	|373				|起终点距离超长																																						|
| uni-map-service	|374				|起终点坐标错误																																						|
| uni-map-service	|375				|局域网IP无法定位																																					|
| uni-map-service	|377				|提供的起终点无法规划出导航线路																														|
| uni-map-service	|378				|提供的起终点无法规划出步行线路																														|
| uni-map-service	|379				|提供的起终点无法规划出公交线路																														|
| uni-map-service	|380				|坐标类型必须在有坐标的情况下使用																													|
| uni-map-service	|382				|IP无法定位																																								|
| uni-map-service	|384				|提供的起终点无法规划出骑行线路																														|
| uni-map-service	|387				|没有对应的POI																																						|
| uni-map-service	|393				|没有符合条件的数据																																				|
| uni-map-service	|394				|错误的查询条件																																						|
| uni-map-service	|395				|传入参数不合法																																						|
| uni-map-service	|396				|最多支持200个坐标点，且起终点数目乘积最多为625（距离矩阵）																|
| uni-map-service	|397				|一对多最多支持200个坐标点，多对多最多支持25个坐标点且起终点数目乘积最多为625（距离矩阵）	|
| uni-map-service	|500				|服务响应失败																																							|

## 常见问题

### 使用uni-map-service后，我还需要购买5万元的地图商业授权费用吗?

答：待补充