# 3. TypeScript 接口

- Call,COnstruct and Index signatures
- "Open interfaces"
- Access modifier Keywords
- Heritage clauses("extends","implements")
  
## 1. Type Aliases - extends
```js
/**
 * (1) Types aliases allow us to give a type a name
 */
type StringOrNumber=string|number;

//this is the ONLY time you'll see a type on the RHS(right hand side) of assignment
type HasName={name:string}

//self-referencing types don't work!(we'll get there!)
const ax=[1,2,3,1,1,[3,1,1,2]];
type NumVal=1|2|3|NumArr;
type NumArr=NumVal[]

/**
 * (2)Interfaces can extend from other interfaces
 */
export interface HasInternationalPhoneNumber extends HasPhone{
  countryCode:string;
}
```

## 2. Call-Construct Signatures
```js
**
 * (3) they can also used to describe call signatures
 */
interface ContactMessenger1 {
  (contact: HasEmail | HasPhone, message: string): void;
}

type ContactMessenger2 = (
  contact: HasEmail | HasPhone,
  message: string
) => void;

//NOte:we don't need type annotations for contact or message
const emailer: ContactMessenger1 = (_contact, _message) => {
  /* ... */
};

/**
 * (4) construct signatures can be described as well
 */
interface ContactConstructor {
  new (...arg: any[]): HasEmail | HasPhone;
}
```

## 3. DIctionary Objects -Index Signatures

```js
interface PhoneNumDict1 {
  [numberName: string]:
    | undefined
    | {
        areaCode: number;
        num: number;
      };
}
const e: PhoneNumDict1 = {};
e.abc;
//可以推断出值类型e.abc:
// {
//   areaCode: number;
//   num: number;
// }
if (typeof e.abc==='string') {
  e.abc;  //never
}
//不需要特殊定义undefined
interface PhoneNumDict {
  [numberName: string]: {
    areaCode: number;
    num: number;
  };
}
const d: PhoneNumDict = {};
d.abc;
//可以推断出值类型d.abc:
// {
//   areaCode: number;
//   num: number;
// }

const phoneDict:PhoneNumDict={
  office:{areaCode:321,num:12345},
  home:{areaCode:321,num:67890}
}
```

## 4. Combining Interfaces
```js
/**
 * (6) they may be used in combination with other types
 */

//argument the existing PhoneNumberDict
//i.e.,imported it from a library,adding stuff to it

interface PhoneNumberDict{
  home:{
    /**
     * (7) interfaces are "open",meaning any declarations of the 
     * same name are merged
     */
    areaCode:number,
    num:number;
  };
  office:{
    areaCode:number,
    num:number;
  }
}

const phoneDict1:PhoneNumberDict={
  office:{areaCode:321,num:12345},
  home:{areaCode:321,num:67890} //注释home，ERROR:缺少属性 "home"，
}

phoneDict1.home;
phoneDict1.office;
//phoneDict1.mobile;//ERROR:类型“PhoneNumberDict”上不存在属性“mobile”


//self-referencing types don't work!(we'll get there!)
const f:NumVal1 = [1, 2, 3, 1, 1, [3, 1, 1, 2]];
type NumVal1 = 1 | 2 | 3 | NumArr;
interface NumArr1 extends Array<NumVal1>{}
```

## 5. Type Tests

