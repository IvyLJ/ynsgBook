### 7.1 计算属性和侦听器

##### 1. 计算属性： computed
```
<div id="example">
  <p>Original message: "{{ message }}"</p>
  <p>Computed reversed message: "{{ reversedMessage }}"</p>
</div>
```
```
var vm = new Vue({
  el: '#example',
  data: {
    message: 'Hello'
  },
  computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
})
```

##### 2. 方法： methods
```
  <p>Computed reversed message: "{{ reversedMessage() }}"</p>
```
```
var vm = new Vue({
  ...
  methods: {
      reversedMessage: function () {
          return this.message.split('').reverse().join('')
      }
  }
})
```

##### 3. 侦听： watch
```
  <div id="demo">{{ fullName }}</div>
```
```
var vm = new Vue({
  el: '#demo',
  data: {
    firstName: 'Foo',
    lastName: 'Bar',
    fullName: 'Foo Bar'
  },
  watch: {
    firstName: function (val) {
      this.fullName = val + ' ' + this.lastName
    },
    lastName: function (val) {
      this.fullName = this.firstName + ' ' + val
    }
  }
})
```
