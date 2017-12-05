### 该项目是一个练习Vue2.x（外卖订单的实现）
# sell

> A Vue.js project

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build

# build for production and view the bundle analyzer report
npm run build --report
```

For a detailed explanation on how things work, check out the [guide](http://vuejs-templates.github.io/webpack/) and [docs for vue-loader](http://vuejs.github.io/vue-loader).

#### Eslint

> 一个测试框架

```
	env:脚本运行的环境；可以预设好其它环境的全局变量，es6,node,brower等环境变量
	'env':{
		'browser':true,
		'commonjs':true,
		'es6':true
	}


	globals:额外的全局变量
	globals:{
		vue:true,
		wx:true
	}

	rules:开启规则和发生错误时报告的等级
	'rules':{
		//no-var
		'no-var','error',

		//要求或禁止var声明初始化
		'init-declaration':2,

		//强制使用单引号
		'quotes':['error','single'],

		// 要求或禁止使用分号而不是 ASI
	    'semi': ['error', 'never'],

	    // 禁止不必要的分号
	    'no-extra-semi': 'error',

	    // 强制使用一致的换行风格
	    'linebreak-style': ['error', 'unix'],

	    // 空格2个
	    'indent': ['error', 2, {'SwitchCase': 1}],

	    // 指定数组的元素之间要以空格隔开(,后面)， never参数：[ 之前和 ] 之后不能带空格，always参数：[ 之前和 ] 之后必须带空格
	    'array-bracket-spacing': [2, 'never'],

	    // 在块级作用域外访问块内定义的变量是否报错提示
	    'block-scoped-var': 0,

	    // if while function 后面的{必须与if在同一行，java风格。
	    'brace-style': [2, '1tbs', {'allowSingleLine': true}],

	    // 双峰驼命名格式
	    'camelcase': 2,

	    // 数组和对象键值对最后一个逗号， never参数：不能带末尾的逗号, always参数：必须带末尾的逗号， 
	    'comma-dangle': [2, 'never'],

	    // 控制逗号前后的空格
	    'comma-spacing': [2, {'before': false, 'after': true}],

	    // 控制逗号在行尾出现还是在行首出现
	    'comma-style': [2, 'last'],

	    // 圈复杂度
	    'complexity': [2, 9],

	    // 以方括号取对象属性时，[ 后面和 ] 前面是否需要空格, 可选参数 never, always
	    'computed-property-spacing': [2, 'never'],
	    
	    // TODO 关闭 强制方法必须返回值，TypeScript强类型，不配置
	    // 'consistent-return': 0
	}

	
```
### 项目布局出现整体可滑动问题解决
取消对高度设置的像素

### vue设置动画


### 遇坑问题
1.(Emitted value instead of an instance of Error）
 Component template should contain exactly one root element. If you are using v-if on multiple elements, use v-else-if to chain them instead.
 解决：
 需要使用一个div包裹template上的内容

2.点击食物和+号都会跳转到食物详情页
	1.是由于事件冒泡所产生的，在点击中阻止冒泡@click.stop.prevent="addFood"
	2.在事件中添加了事件的显示（_drop(target){this.$refs.food.show()}）

3.better-scroll使用中不能滑动
	是由于需要滑动的区块没有包裹在一个父级的元素上

4.同一个类里多个类名的优先级问题
	CSS样式中将覆盖的样式放在被覆盖的样式后面(在css里，同权重的样式，写在后面的会覆盖写在前面的样式！)
	





