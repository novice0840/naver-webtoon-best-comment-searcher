### Typescript를 왜 사용할까?

기존 Javascript는 타입을 표현하는게 불가능 함.  
Javascript에서는 문자열 10과 숫자 10을 더하면 다음과 같이 결과가 나온다.  
"10" + 10 = "1010"  
반면에 타입이 지정되어 있는 Java, C와 같은 언어에서는 위와 같은 입력을 할 경우 에러가 발생한다. 즉 에러가 필요한 상황에 에러가 발생하지 않는 경우가 있다. 이러한 문제를 고치기 위해서 나온 것이 "형"을 명확하게 명시하는 Typescript이다. Typescript를 사용하면 위와 같은 상황에서 에러를 발생시킨다. 즉, 잘못된 입력이 들어왔을 때 사전에 어플리케이션 측에서 알 수 있다.
<br/>
<br/>
<br/>

### 기본 타입

number 숫자형 타입

```typescript
let age: number = 30;
```

<br/>
boolean 참/거짓

```typescript
let isAdult: boolean = true;
```

<br/>
string 문자열 타입

```typescript
let name: string = "James";
```

<br/>
Array 배열 타입

```typescript
let a:number[] = [1,2,3] or let a:Array<number> = [1,2,3]
let people: string = ["James", "Jack", "john"] or let people: Array<string> = ["James", "Jack", "john"]
```

<br/>
Tuple 서로 다른 타입의 값들을 하나에 배열에 넣을 수 있다

```typescript
let b: [string: number];
b = ["z", 1];
```

<br/>
void 함수에서 반환 값이 없는 경우

```typescript
function sayHello() {
  console.log("hello");
}
```

<br/>
never 절대 발생하지 않는 값을 가르키는 경우

```typescript
function throwError(errorMsg: string): never {
  throw new Error(errorMsg);
}

function keepProcessing(): never {
  while (true) {
    console.log("I always does something and never ends.");
  }
}
```

<br/>
enum 상수값의 집합을 나타낼 수 있다

```typescript
enum PrintMedia {
  Newspaper,
  Newsletter,
  Magazine,
  Book,
}
// PrintMeida == {
//   "0": "Newspaper",
//   "1": "Newsletter",
//   "2": "Magazine",
//   "3": "Book",
//   "Newspaper": 0,
//   "Newsletter": 1,
//   "Magazine": 2,
//   "Book": 3
// }
```

<br/>

### Interface

주로 오브젝트를 나타내는데 사용된다

```typescript
type Score = "A" | "B" | "C" | "D"; // literal type

interface User {
  name: string;
  age: number;
  gender?: string; // optional 있어도 되고 없어도 됨
  readonly birthday: number;
  [grade: number]: Score; // Index Signatures / key가 숫자 value가 Score
}

let user: User = {
  name: "xx",
  age: 30,
  birthday: 2000,
  1: "A",
  2: "B",
};

// 함수도 나타낼 수 있다
interface Add {
  (num1: number, num2: number): number;
}

const add: Add = (x, y) => {
  return x + y;
};

// 클래스에서도 사용 가능하다
interface Car {
  color: string;
  wheels: number;
  start(): void;
}

interface SportsCar extends Car {
  door: number;
  stop(): void;
}

class McLaren implements SportsCar {
  color;
  wheels = 30;
  door = 20;

  constructor(c: string) {
    this.color = c;
  }

  start() {
    console.log("start");
  }

  stop() {
    console.log("stop");
  }
}

const myCar = new McLaren("red");
console.log(myCar.color);
myCar.start();
```

<br/>

### 함수

```typescript
// 일반적인 형태의 함수
function hello(name: string, age?: number): string {
  if (age !== undefined) {
    return `Hello, ${name}. You are ${age}`;
  } else {
    return `Hello, ${name}`;
  }
}

console.log(hello("Sam"));
console.log(hello("Sam", 30));

// 함수에서 this를 이용할 때
interface User {
  name: string;
}

const Sam: User = { name: "Sam" };

function showName(this: User, age: number, gender: "m" | "f") {
  console.log(this.name, age, gender);
}

const a = showName.bind(Sam);
a(30, "m");

// 함수 Overloading을 하는 경우
interface User {
  name: string;
  age: number;
}

function join(name: string, age: string): string;
function join(name: string, age: number): User;
function join(name: string, age: number | string): User | string {
  if (typeof age === "number") {
    return {
      name,
      age,
    };
  } else {
    return "나이는 숫자로 입력해주세요.";
  }
}

const sam: User = join("Sam", 30);
const jane: string = join("Jane", "30");
```

<br/>

### 클래스

```typescript
// Javascript에는 없는 Access modifier(public, private, protected)가 생김

class Car {
  /*
    public - 자식 클래스, 클래스 인스턴스 모두 접근 가능
    protected - 자식 클래스에서 접근 가능
    private - 해당 클래스 내부에서만 접근 가능 
    */

  name: string = "car";
  color: string;
  constructor(color: string) {
    this.color = color;
  }
  start() {
    console.log("start");
  }
}

class Bmw extends Car {
  constructor(color: string) {
    super(color);
  }
  showName() {
    console.log(super.name);
  }
}

const z4 = new Bmw("black");

// abstract class
abstract class Car {
  color: string;
  constructor(color: string) {
    this.color = color;
  }
  start() {
    console.log("start");
  }
  abstract doSomething(): void;
}

class Bmw extends Car {
  constructor(color: string) {
    super(color);
  }
  doSomething() {
    console.log("do something");
  }
}

const z4 = new Bmw("red");
z4.start();
```

<br/>

### Generics

변수의 형태가 유동적일 경우 사용 가능하다

```typescript
// T에는 변수의 형태가 온다
function getSize<T>(arr: T[]): number {
  console.log(arr.length);
  return arr.length;
}

const arr1 = [1, 2, 3];
getSize<number>(arr1); // 3

const arr2 = ["a", "b", "c"];
getSize<string>(arr2); // 3

const arr3 = [false, true, false];
getSize<boolean>(arr3); // 3

const arr4 = [{}, {}, { name: "Tim" }];
getSize<Object>(arr4); // 3

// T의 형태의 일부분을 미리 정할 수도 있다

interface User {
  name: string;
  age: number;
}

interface Car {
  name: string;
  color: string;
}

interface Book {
  price: number;
}

const user: User = { name: "a", age: 10 };
const car: Car = { name: "bmw", color: "red" };
const book: Book = { price: 3000 };

// showName의 인수로 들어오는 data는 String 형태의 name을 포함해야한다는 의미
function showName<T extends { name: string }>(data: T): string {
  return data.name;
}

showName(user);
showName(car);
showName(book);
// book에는 name이 없기 때문에 에러가 발생한다
```
