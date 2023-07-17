# echarts+dataV实现中国在线选择省市区地图
利用 [dataV](https://datav.aliyun.com/portal/school/atlas/area_selector) 的地图 GEO JSON数据配合 echarts 和 [element-china-area-data](https://www.npmjs.com/package/element-china-area-data)实现在线选择 省市区 地图
## 效果预览

![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/1e2dddaf417a495a9cb9c49ae767ff9d~tplv-k3u1fbpfcp-watermark.image?)

可以通过自行选择省市区在线获取地图数据，配合 echarts 渲染出来。

## 实现思路
先通过 `regionData` 中的数据配合组件库的 级联选择器 进行 省市区 的展示和选择，同时拿到选中 省市区 的 value 值去请求 `dataV` 中的 GEO JSON 数据

`regionData` 中的 省市区 数据结构为
```js
elementChinaAreaData.regionData = [{
    label: "北京市",
    value: "11",
    children: [{…}]
}, {
    label: 'xxx',
    value: 'xxx',
    children: [{...}]
}]
```
## 坑
1. `dataV` 地图数据请求地址为 `https://geo.datav.aliyun.com/areas_v3/bound/100000_full.json` 其中`https://geo.datav.aliyun.com/areas_v3/bound/`请求地址前缀不变， `100000_full`是全国地图数据的后缀，每个 省市区 后缀不同
2. `regionData` 中的 value 值逻辑是
```   
省级为 2位             如    广东省 value 为 44
市级为 4位             如    广州市 value 为 4401
区/县级为 6位          如    天河区 value 为 440106
直辖市为 2位           如    北京市 value 为 11
直辖市-市辖区级为 4位   如    北京市-市辖区 value 为 1101
```
但是 `dataV` 后缀长度都是 6 位，好在不足 6 位的只需要用 0 补齐就可以和 `dataV` 请求后缀联动起来

3. 直辖市 和 直辖市-市辖区是指同个地址，只是 `regionData` 多套了一层，所以应该请求同一个 value 后缀的地址，这里以直辖市的为准，下列是需要转换的直辖市，重庆市同时含有 区和县，但是 `dataV` 中没有做区分，`regionData` 又有做区分，统一成重庆市总的地图数据
```js
const specialMapCode = {
    '1101': '11', // 北京市-市辖区
    '1201': '12', // 天津市-市辖区
    '3101': '31', // 上海市-市辖区
    '5001': '50', // 重庆市-市辖区
    '5002': '50'  // 重庆市-县
}
```
4. `dataV` 请求地址到 区/县 级后不需要加 `_full` 后缀如 广东省-广州市-天河区 的请求地址为 `https://geo.datav.aliyun.com/areas_v3/bound/440106.json`


接合这几点我们可以缕清 `regionData` 和 `dataV` 数据接口地址的关系了，如果 value 长度不足 6 位，如 省/市 则需要用 0 补齐到 6位，且需要加 `_full`，而 县/区 则不用补 0 和 `_full`，直辖市-市辖区 则需要进行特殊处理，也只有四个直辖市，处理难度不大，下列代码是级联选择器选择后的处理逻辑，很好理解
```js
const padMapCode = value => {
    let currValue = specialMapCode[value] || value
    return currValue.length < 6 ? currValue += `${'0'.repeat(6 - currValue.length)}_full` : currValue
  }
```
其它页面展示代码请参考源码
