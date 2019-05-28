## install组件
```
npm i vue-mobile-city
```
## 预览效果
![enter image description here](https://github.com/zuorong/vue-mobile-city/blob/master/src/assets/1558765182188.jpg?raw=true)
![enter image description here](https://github.com/zuorong/vue-mobile-city/blob/master/src/assets/1558765217564.jpg?raw=true)
## 属性
| 属性  | 类型  |  默认值 | 描述  |
| ------------ | ------------ | ------------ | ------------ |
| **visible.sync**  |  boolean  |  false  |  控制隐藏或者显示联动面板  |
| **title**  |  string  |  请选择地址  | 标题文本   | 
| **columns**  |  number  |  3  |  双列联动或三列联动(2/3)  |

## 方法
| 事件  | 返回  | 描述  |
| ------------ | ------------ | ------------ |
|  **confirm**  |  function  |  返回选中城市名称以及城市编码
|  **cancel**  |  function  |  取消事件 |
## 使用方法
```
# 全局注册组件
import cascader from 'vue-mobile-city'
Vue.component('Cascader', cascader)
```
```
# 局部注册组件
import Cascader from 'vue-mobile-city'
export default {
  components: { Cascader }
}
```
## example运行
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
