## Contents

* [The any type](#any-type)
* [The unknown type](#unknown-type)
* [Intersection and union types](#intersection-and-union-types)

## Any type

Any type is a dynamic type that can be set to any other type. If you declare a variable to be of the any type, this means that you can set it to anything and reset it to anything else later as well. It is in effect no type because the compiler will not check it on your behalf.

Since `val` is of the any type, we can set it to whatever we like and later call `push` into it since push is a method of `Array`.
```js
let val: any = 22;
val = "string value";
val = new Array();
val.push(33);
console.log(val);
```
## Unknown type

Unknown type is a type released in TypeScript version 3. It is similar to any in that once a variable of this type is declared, a value of any type can be set to it. That value can subsequently be changed to any other type. So, I could start by setting my variable to a string type and then later set it to number. However, you cannot call any of its members or set the variable as a value to another variable without first checking what its type really is.

The following code gives error:
```js
let val: unknown = 22;
val = "string value";
val = new Array();
val.push(33);
console.log(val);
```

If we test that the `value` is an intance of `Array`, it will give no error:
```js
let val: unknown = 22;
val = "string value";
val = new Array();
if (val instanceof Array) {
val.push(33);
}
console.log(val);
```

## Intersection and union types

TypeScript allows the developer to "create types" by merging multiple distinct types together. What's called intersection types.

```js
let obj: { name: string } & { age: number } = {
name: 'tom',
age: 25
}
console.log(obj);
```
The code above will print: `{name: 'tom', age: 25}`