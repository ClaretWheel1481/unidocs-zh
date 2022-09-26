### 插屏广告

插屏广告组件是由客户端原生的图片、文本、视频控件组成的；插屏广告与信息流或横幅广告相比展现尺寸更大，同样能够满足您对大量曝光和用户转化的需求。

![](https://vkceyugu.cdn.bspapp.com/VKCEYUGU-a90b5f95-90ba-4d30-a6a7-cd4d057327db/5dc1ce6b-b786-4175-aec5-dd2ab4a5e34c.png)


**平台差异说明**

|App|H5|微信小程序|支付宝小程序|百度小程序|字节跳动小程序|QQ小程序|快应用|360小程序|快手小程序|京东小程序|
|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|:-:|
|√（3.4.8+）|x|√（3.4.8+）|x|x|x|x|x|x|x|x|


**开通配置广告**

[开通广告步骤详情](https://uniapp.dcloud.net.cn/uni-ad.html#start)


### 语法

`<ad-interstitial></ad-interstitial>`


**属性说明**

|属性名																			|类型													|默认值		|说明																																									|平台差异	|
|:-																					|:-														|:-				|:-																																										|:-				|
|adpid																			|String&#124;Number&#124;Array|					|广告位id，如果传入的是数组，会从索引0开始请求失败后继续下一个，适用于已配置底价的逻辑|					|
|preload																		|Boolean											|true			|页面就绪后加载广告数据																																|					|
|loadnext																		|Boolean											|false		|自动加载下一条广告数据																																|					|
|options																		|Object												|					|透传到作用域插槽																																			|					|
|v-slot:default="{loading, error, options}"	|															|					|作用域插槽可以获取组件内部广告加载状态和加载错误信息																	|					|
|@load																			|EventHandle									|加载事件	|微信小程序暂不支持																																		|					|
|@close																			|EventHandle									|关闭事件	|微信小程序暂不支持																																		|					|
|@error																			|EventHandle									|错误事件	|微信小程序暂不支持																																		|					|

**方法说明**

|方法名|说明|
|:-|:-|
|load|加载广告数据|
|show|显示广告|


HBuilder 基座的测试广告位 `adpid` 为 `1111111113`


简单示例

```html
<template>
  <view>
    <ad-interstitial adpid="1111111113" :loadnext="true" v-slot:default="{loading, error}">
      <button :disabled="loading" :loading="loading">显示广告</button>
      <view v-if="error">{{error}}</view>
    </ad-interstitial>
  </view>
</template>
```


完整示例

```html
<template>
  <view class="content">
    <ad-interstitial adpid="1111111113" :loadnext="true" v-slot:default="{loading, error}" @load="onadload" @close="onadclose" @error="onaderror">
      <button :disabled="loading" :loading="loading">显示广告</button>
      <view v-if="error">{{error}}</view>
    </ad-interstitial>
  </view>
</template>

<script>
export default {
  data() {
    return {
    }
  },
  methods: {
    onadload(e) {
      console.log('广告数据加载成功');
    },
    onadclose(e) {
		 console.log("onadclose",e);
    },
    onaderror(e) {
      // 广告加载失败
      console.log("onaderror: ", e.detail);
    }
  }
}
</script>
```

**错误码**

[错误码相关问题排查](https://uniapp.dcloud.net.cn/component/ad-error-code.html)
