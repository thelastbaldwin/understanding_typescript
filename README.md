# Understanding TypeScript - Udemy

Notes from Udemy's Understanding TypeScript course

## Types

### Implicit Types
```typescript
let myName = "Steve";
let myAge = 34;
let hasHobbies = true;
```

### Explicit Types
```typescript
let myName: string = "Steve";
let myAge: number = 34;
let hasHobbies: boolean = true;
```

### Arrays
```typescript
let hobbies: string[] = ["Cooking", "Sports"];
```

### Tuples
```typescript
let address: [string, number] = ["Superstreet", 99];
```

### Enums
```typescript
enum Color {
    Gray,
    Green,
    Blue
};

let myColor: Color = Color.Green;
console.log(myColor) // 1
```

### Objects

```typescript
let userData: {
    name: string,
    age: number
} = {
    name: "Max",
    age: 27
}
```

### Any
Avoid using since this defeats the purpose of using TS

```typescript
let car: any = "BMW";
car = 50;
car = false;
```

## Functions
You can specify the argument types and function return types
```typescript
function sayHello(name: string): void {
    console.log(`Hello! ${name}`);
}

const add = (a: number, b: number): number => a + b;
```

You can specify the signature and return type for a variable

```typescript
let myAdd: (v1: number, v2: number) => number;
myAdd = add;
```

## Complex Example
Here is an example illustrating all the concepts above:
```typescript
let complex: {
    data: number[],
    output: (all:boolean) => number[]
} = {
    data: [100, 3.99, 10],
    output: function(all:boolean): number[] {
        return this.data;
    }
}
```

## Type Alias
```typescript
type Complex = {data: number[], output: (all:boolean) => number[]};

let complex2: Complex;
```

## Union Types
Useful when a variable can be one of several types.

```typescript
let myRealRealAge: number | string = 34;
myRealRealAge = "young-ish"

let canBeNull: number | null = 12;
canBeNull = null;
```

## Classes
Private properties are only accessible from the base class. Protected properties are accessible from the base class and any children
```typescript
class Person {
    private type: string;
    protected age: number = 34;
}
```

Properties can be set in the contstuctor with a shorthand
```typescript
constructor(public name: string){
    this.type = "basic";
}
```

### Getters and Setters
```typescript
class Plant {
    private _species: string = "Default";

    set species(v: string){
        if(v.length > 3){
            this._species = v;
        } else {
            this._species = "Default"
        }
    }

    get species():string {
        return this._species;
    }
}

const plant = new Plant();
plant.species = "abc";
console.log(plant.species); // "Default"
plant.species = "Legume";
console.log(plant.species); // "Legume"
```

### Static properties and methods
```typescript
class Helpers {
    static PI: number = 3.14;
    static calcCircumference(diameter: number): number {
        return this.PI * diameter;
    }
}

console.log(2 * Helpers.PI); // 6.28;
console.log(Helpers.calcCircumference(15)); // 47.1
```

### Abstract Classes
Cannot be instantiated directly. Must be inherited from. Used if providing a basic setup for other classes to conform to.

```typescript
abstract class Project {
    projectName: string = "Default";
    budget: number = 0;

    abstract changeName(name: string): void;
    calcBudget():number {
        return this.budget * 2;
    }
}

class ITProject extends Project {
    changeName(name: string): void {
        this.projectName = name;
    }
}

let newProject = new ITProject();
console.log(newProject); // {projectName: "Default", budget: 0}
newProject.changeName("Super IT Project");
console.log(newProject); // {projectName: "Super IT Project", budget: 0}
```

### Singleton
```typescript
class OnlyOne {
    private static instance: OnlyOne;

    private constructor(public name: string) {}

    static getInstance(){
        if(!OnlyOne.instance){
            OnlyOne.instance = new OnlyOne('The Only One');
        }
        return OnlyOne.instance;
    }
}

// let wrong = new OnlyOne('The Only One') // error
let right = OnlyOne.getInstance();
console.log(right.name); // 'The Only One'
```


## Namespaces
```typescript
namespace MyMath {
    const PI = 3.14;

    export function calculateCircumference(diameter: number){
        return diameter * PI;
    }

    export function calculateRectangle(width: number, length: number){
        return width * length;
    }
}

console.log(MyMath.calculateRectangle(10, 20)); // 200
console.log(MyMath.calculateCircumference(3)); // 9.42
// console.log(MyMath.PI); // error!
```

### Namespace imports
The `outFile` parameter must be set in the tsc CLI or in tsconfig.json.

```typescript
//namespace import
/// <reference path="./circleMath.ts" />
/// <reference path="./rectangleMath.ts" />

import CircleMath = MyMath.Circle;

console.log(MyMath.calculateRectangle(10, 20));
console.log(CircleMath.calculateCircumference(3));
```

Disadvantages: Basically inferior to modules. Namespaces can pollute the global namespace and the order of namespaces can get unnecessarily convoluted.

## Modules

These essentially work the same as ES6 imports. If using just the typescript compiler, you'll need some kind of module loading shim until native ES6 imports are supported. If using webpack, or another bundling solution, you'll need to follow those standards.

## Interfaces

Interfaces can enforce data contracts better than simply describing types. A way to guarantee that certain methods or properties are available.
```typescript
interface NamedPerson {
    firstName: string
}

function greet(person: NamedPerson){
    console.log("Hello, " + person.firstName);
}

function changeName(person: NamedPerson){
    person.firstName = "Anna";
}

const person = {
    firstName: "Max",
    age: 27
};

greet(person);
changeName(person);
greet(person);
```

Passing object literals can trigger errors when the object contains properties that don't exisit on the interface, even if the object contains all the required properties of that interface. Object literals are checked more strictly than named variables. Interfaces are not included in the compiled JS and can't be enforced at runtime.

### Optional properties
```typescript
interface NamedPerson {
    firstName: string;
    age?: number;
    [propName: string]: any;
}
```

### Methods
```typescript
interface NamedPerson {
    firstName: string;
    age?: number;
    [propName: string]: any;
    greet(lastName: string): void;
}
```

### Classes
```typescript
class Person implements NamedPerson {
    firstName: string;

    constructor(firstName: string){
        this.firstName = firstName;
    }

    greet(lastName: string){
        console.log("Hi, I am " + this.firstName + " " + lastName);
    }
}
```

### Functions
```typescript
interface DoubleValueFunc {
    (number1: number, number2: number): number;
}

let myDoubleFunction: DoubleValueFunc;
myDoubleFunction = function(value1: number, value2: number){
    return (value1 + value2) * 2
};

console.log(myDoubleFunction(10, 20)); //60
```

### Inheritance
```typescript
interface AgedPerson extends NamedPerson {
    age: number; // this property is now required
}

const oldPerson: AgedPerson = {
    age: 80,
    firstName: "Clint",
    greet(lastName: string){
        console.log("Hello");
    }
};
```

## Generics

### Functions
```typescript
function echo(data:any){
    return data;
}

function betterEcho<T>(data: T){
    return data;
}

console.log(betterEcho("Max").length);
// console.log(betterEcho<number>(27).length); //Property 'length' does not exist on type 'number'
console.log(betterEcho({name: "Max", age: 27}));
```

### Built-in

Array is a generic type by default.

```typescript
const testResults: Array<number> = [1.94, 2.33];
testResults.push(-2.99);
// testResults.push("foo"); // error

function printAll<T>(args: T[]){
    args.forEach(element => console.log(element));
}
printAll<string>(["Apples", "Bananas"])
```

### Types
```typescript
const echo2: <T>(data: T) => T = betterEcho;
console.log(echo2<string>("Something"));
```

### Class
```typescript
class SimpleMath<T extends number | string> {
    baseValue: T;
    multiplyValue: T;

    constructor(baseValue: T, multiplyValue: T){
        this.baseValue = baseValue,
        this.multiplyValue = multiplyValue;
    }
    calculate(): number {
        return +this.baseValue * +this.multiplyValue;
    }
}

const simpleMath = new SimpleMath<number>(10, 20);
const simpleMathString = new SimpleMath<string>("10", "20");
// const simpleMathBoolean = new SimpleMath<boolean>(true, false); // Type 'boolean' does not satisfy the constraint 'string | number'
console.log(simpleMath.calculate());
console.log(simpleMathString.calculate());
```

#### Multiple Types
```typescript
class SimpleMath<T extends number | string, U extends number | string> {
    baseValue: T;
    multiplyValue: U;

    constructor(baseValue: T, multiplyValue: U){
        this.baseValue = baseValue,
        this.multiplyValue = multiplyValue;
    }
    calculate(): number {
        return +this.baseValue * +this.multiplyValue;
    }
}

const simpleMath = new SimpleMath<number, number>(10, 20);
const simpleMathString = new SimpleMath<string, string>("10", "20");
const simpleMathMixed = new SimpleMath<number, string>(50, "50");

console.log(simpleMathMixed.calculate());
```
