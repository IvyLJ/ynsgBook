# 3. TypeScript 类

- Fields
- Access modifier Keywords（public，private，protected）

## 1.Classes

```js
/**
 * (1) classes work similarly to what you're used to seeing in js
 * -  They can "implements" interfaces
 */
export class Contact implements HasEmail{
  //declaring that these member data fields will exist
  email:string;
  name:string;
  constructor(name:string,email:string){  //accept teo parameters
    this.email=email;   //pass the things our constructor receives onto the instance
    this.name=name;
  }
}
```

## 2. Access Modifiers - Initialization
```js
/**
 * (2) This looks a little verbose -- we have to specify the words
 * - Typescript has a shortcut: PARAMETER PROPERTIES
 */

/**
 * (3) Access modifier keywords - "who can access this thing"
 *
 * - public: everyone
 * - protected : me and subclasses
 * - private : only me
 */

class ParamPropContact implements HasEmail {
  constructor(public name: string, public email: string = 'no email') {
    //nothing needed
    //会编译出 this.name=name...
  }
}
const k = new ParamPropContact('a', 'b');
k.name;
k.email;

/**
 * protected
 * 类“ParamPropContactP”错误实现接口“HasEmail”。
  属性“email”在类型“ParamPropContactP”中受保护，但在类型“HasEmail”中为公共属性。
 */
// class ParamPropContactP implements HasEmail {
//   constructor(
//     public name: string,
//     protected email: string = 'no email'){
//       //nothing needed
//       //会编译出 this.name=name...
//     }
// }

/**
 * (4) class fields can have initializers(default)
 */
class OtherContact implements HasEmail, HasPhone {
  protected age: number = 0;
  //readonly age =0;
  //private password:string
  constructor(
    public name: string, 
    public email:string, 
    public phone: number) {
      this.age=35;
      //()password must either be initialized like this,or have a
    }
}
```


## 3. Definite Assignment - Lazy Initialization

```js
class OtherContact1 implements HasEmail, HasPhone {
  protected age: number = 0;
  private password: string | undefined;
  constructor(public name: string, public email: string, public phone: number) {
    this.age = 35;
    //()password must either be initialized like this,or have a default value
    // if(phone>0){
    //   this.password=Math.round(Math.random()*1e14).toString(32);
    // }
  }
  //如果不能保证初始化时会赋值,solution1:
  async init() {
    this.password = Math.round(Math.random() * 1e14).toString(32);
  }

  //如果不能保证初始化时会赋值,solution2:
  get passwordVal(): string {
    if (!this.password) {
      this.password = Math.round(Math.random() * 1e14).toString(32);
    }
    return this.password;
  }
}
```

## 4. Abstract Classes
```js
/**
 * (5)TypeScript even allows for abstract classes,which have a part
 * (half class ,half interface)
 */
abstract class AbstractContact implements HasEmail,HasPhone{
  public abstract phone:number; //must be implemented by non-abstract

  constructor(
    public name: string, 
    public email: string //must be public to satisfy HasEmail
    ){}

  abstract sendEmail():void; //must be implemented by non-abstract
}

/**
 * (6) implements must "fill in" any abstract methods or properties
*/
class ConcreteContact extends AbstractContact{
  constructor(
    public phone:number,
    name: string, 
    email: string 
  ){
    super(name,email)
  }
  sendEmail(){
    //mandatory!
    console.log("sending an email")
  }
}
```
