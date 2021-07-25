# 2. TypeScript 基础

- 简单变量
- 数组 & 元组
- 对象（Objects）
- Union & Intersection Types 

## 1. 变量(Variables)
```js
/**
 * （1）x is  a string,b/c we've initialized it
 */
let x = "hello world";
/**
 * (2)reassignment is fine
 */
x = "hello mars";
/**
 * (3)but if we tru to change type
 */
//x=42   //ERROR:不能将类型“number”分配给类型“string”
/**
 * (4) let's look at const. The type is literally "hello world" 
 */
const y = "hello world"; 
const yObj={
    foo:"hello"
};
yObj.foo='bye';
function foo(arg:"helloW mars"){

}
//foo(y) //ERROR:类型“"hello world"”的参数不能赋给类型“"helloW mars"”的参数。
```

## 2. 变量声明(Variables)
```js
/**
 * (5) sometimes we need to declare a variable w/o initializing it
 */
let z;
z=41;
z="abc"; //(6)oh no! This isn't good

/**
 * (7) we could improve this situation by providing a type annotation
 * when we declare our variable
 */
let zz:number;
zz=41;
//zz="abc"  //ERROR:不能将类型“string”分配给类型“number”。
```

## 3. 数组 - 元组
```js
/**
 * (8) simple array types can be expressed using []
 */
let aa:number[]=[];
aa.push(33);
//aa.push("abc");//ERROR:类型“string”的参数不能赋给类型“number”的参数。
/**
 * (9) we can even define a tuple,which has a fixed length
 */
let bb:[number,string,string,number]=[
    123,"fake street","nowhere",10110
];
//bb=[1,2,3] //ERROR:不能将类型“number”分配给类型“string”。
//bad:使用的是数组方法
bb.push(1,2,2,2,2,2,2) //(method) Array<string | number>.push(...items: (string | number)[]): number
/**
 * (10) Tuple values often require type annotations(:[number,number])
 */
const xx =[32,31]  //number
const yy:[number,number]=[32,31];
```

## 4. 对象类型 - 接口
```js
/**
 * (11)object types can be expressed using {} and property names
 */
let cc:{houseNumber:number,streetName:string};
cc={
    houseNumber:123,
    streetName:"fake street"
}
/**
 * (12)you can use the optional operator(?)to
 * indicate that something may or may not be there
 */
 let dd:{houseNumber:number,streetName?:string};
 dd={
     houseNumber:123
 }
 /**
 *  (13) if we want to re-use this type,we can create an interface 
 */
interface Address{
    houseNumber:number,
    streetName?:string
}
//and refer to it by name
let ee:Address={houseNumber:33}
```
## 5. 交叉类型(Intersection) - 联合类型(Union Types)
```js
/**
 * (14) Intersection types
 * sometimes we have a type that can be one of several things
 */
export interface HasPhone {
  name: string;
  phone: number;
}
export interface HasEmail {
  name: string;
  email: string;
}
let contactInfo: HasEmail | HasPhone =
  Math.random() > 0.5
    ? {
        //we can assign it to a HasPhoneNumber
        name: 'Mike',
        phone: 13112345678,
      }
    : {
        //or a HasEmail
        name: 'Mike',
        email: '1234@example.com',
      };
contactInfo.name;  //note: we can only access the .name property
/**
 * (15) Union types
 */
let otherContactInfo:HasEmail & HasPhone={
    name:"Mike",
    email: '1234@example.com',
    phone: 13112345678,
};
otherContactInfo.name;  //we can access anything on _either_type
otherContactInfo.email;
otherContactInfo.phone;
```

## 6. 类型系统 - 对象Sharps
### 6.1 类型系统（Type Systems） & 类型等价（Type Equivalence）
```js
function validateInputField(input:HTMLInputElement){
    /* ... */
}
validateInputField(x) //can we regard x as an HTMLInputElement?
```
 - Nominal Type Systems(标明类型系统
) answer this question based on whether x is an instance of a class/type named HTMLInputElement
 - Structural Type Systems(结构类型系统) only care about the shape of an object. This is how Typescript works!

### 6.2 Object Shapes
 - 当我们谈论对象的形状(shape of an object)时，我们指的是属性的名称及其值的类型.

### 6.3 Wider vs. Narrower
   
 - ANYTHING: any
 - ARRAY: any[]
 - ARRAY OF STRING: string[]
 - ARRAY OF 3: [string,string,string]
 - ...
 - NOTHING: nerver


## 7. 函数

```js
/**
 * (1)function arguments and return values can have type annotations
 */
function sendEmail(to: HasEmail): { recipient: string; body: string } {
  return {
    recipient: `${to.name}<${to.email}>`, //Mike<1234@example.com>
    body: "You're pre-qualified for a loan!",
  };
}
/**
 * (2)or the arrow-function variant
 */
const sendTextMessage = (to: HasPhone): { recipient: string; body: string } => {
  return {
    recipient: `${to.name}<${to.phone}>`, //Mike<13112345678>
    body: "You're pre-qualified for a loan!",
  };
};
/**
 * (3)return types can almost always be inferred
 */
function getNameParts(contact: { name: string }) {
  const parts = contact.name.split(/\s/g); //split @ whitespace
  if (parts.length < 2) {
    throw new Error(`Can't calculate name parts from name "${contact.name}"`);
  }
  return {
    first: parts[0],
    middle:
      parts.length === 2
        ? undefined
        : //everything except first and last
          parts.slice(1, parts.length - 2).join(''),
    last: parts[parts.length - 1],
  };
}
// //typescript可推断出格式：
// function getNameParts(contact: {
//   name: string;
// }): {
//   first: string;
//   middle: string;
//   last: string;
// }
function getNameParts1(contact: { name: string }) {
  const parts = contact.name.split(/\s/g); //split @ whitespace
  if (parts.length === 1) {
    return { name: [parts[0]] };
  }
  return {
    first: parts[0],
    middle:
      parts.length === 2
        ? undefined
        : //everything except first and last
          parts.slice(1, parts.length - 2).join(''),
    last: parts[parts.length - 1],
  };
}
// //typescript可推断出格式：
// function getNameParts1(contact: {
//   name: string;
// }): {
//   name: string[];
//   first?: undefined;
//   middle?: undefined;
//   last?: undefined;
// } | {
//   first: string;
//   middle: string;
//   last: string;
//   name?: undefined;
// }
/**
 * (4) rest params work just as you'd think, Type must be array-ish
 */
const sum = (...vals: number[]) => vals.reduce((sum, x) => sum + x, 0);
console.log(sum(3, 4, 6)); //13
```

## 8. 函数签名重载(Function Signature Overloading)
```js
/**
 * (5)we can even provide multiple function signatures
 * "overload signatures"
 * function contactPeople(method:"email",...people:HasEmail[]):void;
 * function contactPeople(method:"phone",...people:HasPhone[]):void;
 */

//function implementation
function contactPeople1(
 method: 'email' | 'phone',
 ...people: (HasEmail | HasPhone)[]
): void {
 if (method === 'email') {
   (people as HasEmail[]).forEach(sendEmail);
 } else {
   (people as HasPhone[]).forEach(sendTextMessage);
 }
}
//email works
contactPeople1("email",{name:"foo",email:""})
//phone works
contactPeople1("phone",{name:"foo",phone:1234567})
//mixing does not work，但是不会报错
contactPeople1("email",{name:"foo",phone:1234567})  
/**
 * (6)we can even provide multiple function signatures
 * "overload signatures"
 */
//创建两个额外的函数类型定义
 function contactPeople(method:"email",...people:HasEmail[]):void;
 function contactPeople(method:"phone",...people:HasPhone[]):void;
//function implementation
function contactPeople(
  method: 'email' | 'phone',   //wide  method:string
  ...people: (HasEmail | HasPhone)[]
): void {
  if (method === 'email') {
    (people as HasEmail[]).forEach(sendEmail);
  } else {
    (people as HasPhone[]).forEach(sendTextMessage);
  }
}
//email works
contactPeople("email",{name:"foo",email:""})
//phone works
contactPeople("phone",{name:"foo",phone:1234567})
//mixing does not work
//contactPeople("email",{name:"foo",phone:1234567})  //ERROR:没有与此调用匹配的重载。

```

## 9. 词法作用域(Lexical Scope)

```JS
function sendMessage(
  this:HasEmail&HasPhone,
  preferredMethod:"phone"|"email"
) {
  if(preferredMethod==="email"){
    console.log("sendEmail");
    sendEmail(this);
  }else{
    console.log("sendTextMessage");
    sendTextMessage(this);
  }
}

const c={name:"Mike",phone:"13112345678",email:"1234@example.com"};

function invokeSoon(cb:()=>any,timeout:number) {
  setTimeout(()=>cb.call(null),timeout)
}

//this is not satisfied
//Error:类型为“void”的 "this" 上下文不能分配给类型为“HasEmail & HasPhone”的方法的 "this"。
//不能将类型“void”分配给类型“HasEmail”。
//invokeSoon(()=>sendMessage("email"),500)

//creating a bound function is one solution
const bound=sendMessage.bind(c,"email");
invokeSoon(()=>bound(),500)

//call/apply works as well
invokeSoon(()=>sendMessage.apply(c,['phone']),500)
//c:
// const c: {
//   name: string;
//   phone: string;
//   email: string;
// }
```