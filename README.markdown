## Contents

* [The any type](#any-type)
* [The unknown type](#unknown-type)
* [Intersection and union types](#intersection-and-union-types)
* [Literal types](#literal-types)
* [Type aliases](#type-aliases)
* [Function return types](#function-return-types)
* [Functions as types](#functions-as-types)
* [Never type](#never-type)
* [Classes](#classes)
* [Access modifiers](#access-modifiers)
* [Static properties and methods](#static-properties-and-methods)
* [Interfaces](#interfaces)
* [Inheritance](#inheritance)
* [Namespaces](#namespaces)
* [Abstract classes](#abstract-classes)

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

TypeScript allows the developer to "create types" by merging multiple distinct types together. What's called `intersection` types.

```js
let obj: { name: string } & { age: number } = {
name: 'tom',
age: 25
}
console.log(obj);
```
The code above will print: `{name: 'tom', age: 25}`

The other type is similar, but in case of unions, instead of merging types we define using them in either one or another fshion:

```js
let unionObj: null | { name: string } = null;
unionObj = { name: 'jon'};
console.log(unionObj);
```
The unionObj variable is declared to be of the `null` type or `{ name: string }`, by the use of the `|` character. The code above will accept both type values: `null` or a `string`

## Literal types

`Literal` types are similar to union types, but they use a set of hardcoded string or number values.

```js
let literal: "tom" | "linda" | "jeff" | "sue" = "linda";
literal = "sue";
console.log(literal);
```

Trying to set `literal = "john";` gives an error.

## Type aliases

Type alias represents a method to give a different name to a type:

Example 1:
```js
type Points = 20 | 30 | 40 | 50;
let score: Points = 20;
console.log(score);
```

Example 2:
```js
type ComplexPerson = {
    name: string,
    age: number,
    birthday: Date,
    married: boolean,
    address: string
}
```

## Function return types

Function return declaration examples:
```js
function runMore(distance: number): number {
    return distance + 10;
}

function sleepIn(hours: number): void {
console.log("I slept " + hours + " hours");
}
```

## Functions as types

In TypeScript, a type can also be an entire function signature.

```js
type Run = (miles: number) => boolean;

let runner: Run = function (miles: number): boolean {
    if(miles > 10){
        return true;
    }
    return false;
}

console.log(runner(9));
```

## Never type

```js
function oldEnough(age: number): never | boolean {
    if(age > 59) {
        throw Error("Too old!");
    }
    if(age <=18){
        return false;
    }
    return true;
}
```

## Classes

Class represents a container for related set of fields and methods, that can be instantiated and used:

```js
class Person {

    msg: string;
    constructor() {}
    
    speak() {
        console.log(this.msg);
    }
}

const tom = new Person();
tom.msg = "hello";
tom.speak();
```

## Access modifiers

`private` - to hide property from direct modification:
```js
class Person {

    constructor(private msg: string) {}

    speak() {
        console.log(this.msg);
    }
}

const tom = new Person("hello");
// tom.msg = "hello";
tom.speak();
```
By setting the field to `private`, we make it inaccessible from outside of the class.

`readonly` - modifier allos property to be set once in the constructor
```js
class Person {

    constructor(private readonly msg: string) {}

    speak () {
        this.msg = "speak " + this.msg;
        console.log(this.msg);
    }
}

const tom = new Person("hello");
// tom.msg = "hello";
tom.speak();
```

`Getter`: A property that allows modification or validation of a related field before returning it

`Setter`: A property that allows modification or computation of a value before setting to a related field
```js
class Speaker {

    private message: string;
    constructor(private name: string) {}

    get Message() {
        if(!this.message.includes(this.name)){
            throw Error("message is missing speaker's name");
        }
        return this.message;
    }

    set Message(val: string) {
        let tmpMessage = val;
        if(!val.includes(this.name)){
            tmpMessage = this.name + " " + val;
        }
        this.message = tmpMessage;
    }
}

const speaker = new Speaker("john");
speaker.Message = "hello";
console.log(speaker.Message);
```

Note: Typescript should be set to use `ES6` for getters and setters!
```js
tsc --target "ES6" <filename.ts>
```

## Static properties and methods

Marking something `static` inside a class makes this member is a member of a class type and not of a class instance.

```js
class Runner {
    static lastRunTypeName: string;
    constructor(private typeName: string) {}
    
    run() {
        Runner.lastRunTypeName = this.typeName;
    }
}

const a = new Runner("a");
const b = new Runner("b");
b.run();
a.run();

console.log(Runner.lastRunTypeName);
```
Example above shows the usage when need to check the las class instance that has called some function.


## Interfaces

`Interfaces` used when needed to show only a signature of a type hiding internal logic

```js
interface Employee {
    name: string;
    id: number;
    isManager: boolean;
    getUniqueId: () => string;
}

const linda: Employee = {
    name: "linda",
    id: 2,
    isManager: false,
    getUniqueId: (): string => {
        let uniqueId = linda.id + "-" + linda.name;
        if(!linda.isManager) {
            return "emp-" + uniqueId;
        }
        return uniqueId;
    }
}

console.log(linda.getUniqueId());

const pam: Employee = {
    name: "pam",
    id: 1,
    isManager: true,
    getUniqueId: (): string => {
        let uniqueId = pam.id + "-" + pam.name;
        if(pam.isManager) {
            return "mgr-" + uniqueId;
        }
        return uniqueId;
    }
}

console.log(pam.getUniqueId());
```

## Inheritance

`Inheritance` in object-oriented programming is a method for doing code reuse.

```js
class Vehicle {
    constructor(private wheelCount: number) {}
    
    showNumberOfWheels() {
        console.log(`moved ${this.wheelCount} miles`);
    }
}

class Motorcycle extends Vehicle {
    constructor() {
        super(2);
    }
}

class Automobile extends Vehicle {
    constructor() {
        super(4);
    }
}

const motorCycle = new Motorcycle();
motorCycle.showNumberOfWheels();

const autoMobile = new Automobile();
autoMobile.showNumberOfWheels();
```

## Namespaces

The concept of `namespaces` allows tocreate scoping contrianers and separate one set of code from another.

```js
namespace A {
    class FirstClass {}
}

namespace B {
    class SecondClass {}
    const test = new FirstClass();
}
```
The code above gives an error even before compiling. VSCode IntelliSence is already complaing about the fact that `FirstClass` cannot be found.

## Abstract classes

`Abstract classes` are useful in type of scenarious, wheere there is need to have both class and an interface instead of either use one of them.

```js
namespace AbstractNamespace {
    abstract class Vehicle {
        constructor(protected wheelCount: number) {}
        
        abstract updateWheelCount(newWheelCount: number): void;
        
        showNumberOfWheels() {
            console.log(`moved ${this.wheelCount} miles`);
        }
    }

    class Motorcycle extends Vehicle {
        constructor() {
            super(2);
        }
        
        updateWheelCount(newWheelCount: number){
            this.wheelCount = newWheelCount;
            console.log(`Motorcycle has ${this.wheelCount}`);
        }
    }

    class Automobile extends Vehicle {
        constructor() {
            super(4);
        }
        updateWheelCount(newWheelCount: number){
            this.wheelCount = newWheelCount;
            console.log(`Automobile has ${this.wheelCount}`);
            }
        showNumberOfWheels() {
            console.log(`moved ${this.wheelCount} miles`);
        }
    }

    const motorCycle = new Motorcycle();
    motorCycle.updateWheelCount(1);
    
    const autoMobile = new Automobile();
    autoMobile.updateWheelCount(3);
}
```
The example above also demostrates overriding implementation of method in child class.