### 7.1 VUE基础
##### 7.1.1 实例
##### 1. 创建实例
```
var data = { a: 1 }
var vm = new Vue({
    data:data
})
```
##### 2. 数据与方法
(1) 当一个 Vue 实例被创建时，它将 data 对象中的所有的属性加入到 Vue 的响应式系统中。当这些属性的值发生改变时，视图将会产生“响应”，即匹配更新为新的值。
```
vm.a=2  //视图重新渲染
```
(2） 值得注意的是只有当实例被创建时 data 中存在的属性才是响应式的
```
vm.b = 'hi'  //不会触发任何视图的更新
```
(3) 这里唯一的例外是使用 Object.freeze()，这会阻止修改现有的属性，也意味着响应系统无法再追踪变化。
##### 3. 实例生命周期钩子
每个 Vue 实例在被创建时都要经过一系列的初始化过程——例如，需要设置数据监听、编译模板、将实例挂载到 DOM 并在数据变化时更新 DOM 等。同时在这个过程中也会运行一些叫做生命周期钩子的函数，这给了用户在不同阶段添加自己的代码的机会。

##### 4. 实例生命周期
![PNG](/asset/img/vue-lifecycle.png)

##### 7.1.2 模板语法
##### 1. 文本
数据绑定最常见的形式就是使用“Mustache”语法 (双大括号) 的文本插值：
```
<span>Message: {{ msg }}</span>
```
##### 2. 特性
Mustache 语法不能作用在 HTML 特性上，遇到这种情况应该使用 v-bind 指令：
```
<div v-bind:id="dynamicId"></div>
```