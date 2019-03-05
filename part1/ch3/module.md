#### 1. 定义

#### 2. 导出的基本语法

2.1 导出数据
```
export let name= "export data"
```
2.2 导出函数
```
export function sum(num1,num2){
    return num1+num2
}
```
2.3 导出类
```
export class Rectangle{
    constructor(width,length){
        this.width=width;
        this.length=length;
    }
}
```
2.4 先定义函数再导出
```
function multiply(num1,num2){
    return num1*num2
}
export multiply
```

#### 3. 导入的基本语法
3.1 基本语法：
```
import {identifier1,identifier2} from './example.js'
```

#### 4. 导入和导出时重命名
#### 5. 模块的默认值
#### 6. 重新导出一个绑定
#### 7. 无绑定导入
#### 8. 加载模块
