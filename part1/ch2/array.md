### 2.2 Array类型

1. 定义：引用类型值array是Array类型的实例。创建：  
 （1） new+构造函数
   ```
   var colors = new Array();
   var colors = new Array(3);
   var colors = new Array("green","red","yellow");
   ```
 （2） 数组字面量
   ```
   var colors = [];
   var colors = ["green","red","yellow"];
   var colors = [,,];
   ```

2. 检测数组  
  (1) instanceof:假定单一的全局环境  
  ```
  if(colors instanceof Array){...}
  ```
  (2) Array.isArray()  
  ```
  if(Array.isArray(colors)){...}
  ```
