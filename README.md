# Understanding TypeScript - Udemy

Notes from Udemy's Understanding TypeScript course

## Types

### Implicit Types
    let myName = "Steve";
    let myAge = 34;
    let hasHobbies = true;

### Explicit Types
    let myName: string = "Steve";
    let myAge: number = 34;
    let hasHobbies: boolean = true;

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