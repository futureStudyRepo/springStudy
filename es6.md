
# ES6 (ECMAScript 2015) 기초 문법 정리

ES6(ECMAScript 2015)는 JavaScript에 많은 새로운 기능과 문법을 도입하여 코드 작성과 유지보수를 간편하게 만들어 주었습니다. 아래는 ES6에서 추가된 주요 문법의 정리입니다.

---

## 1. `let`과 `const`
- **`let`**: 블록 범위 스코프를 가지며 값 재할당 가능.
- **`const`**: 블록 범위 스코프를 가지며 값 재할당 불가.

```javascript
let x = 10;
x = 20; // OK

const y = 30;
y = 40; // Error: Assignment to constant variable.
```

---

## 2. 화살표 함수 (Arrow Functions)
간결한 함수 표현식으로, `this` 바인딩이 기존 함수와 다르게 동작.

```javascript
const add = (a, b) => a + b;
console.log(add(5, 3)); // 8
```

---

## 3. 템플릿 리터럴
- 백틱(``` ` ```)을 사용해 문자열 내 변수와 표현식 삽입 가능.
- 여러 줄 문자열 지원.

```javascript
const name = 'John';
const age = 25;
console.log(`My name is ${name} and I am ${age} years old.`);

const multiLine = `This is
a multi-line string.`;
console.log(multiLine);
```

---

## 4. 디스트럭처링 (Destructuring)
배열이나 객체를 간편하게 분해하여 변수에 할당.

- **배열 디스트럭처링**
```javascript
const [a, b] = [1, 2];
console.log(a, b); // 1, 2
```

- **객체 디스트럭처링**
```javascript
const person = { name: 'Alice', age: 30 };
const { name, age } = person;
console.log(name, age); // Alice, 30
```

---

## 5. 기본 매개변수 (Default Parameters)
함수 매개변수에 기본값 설정 가능.

```javascript
function greet(name = 'Guest') {
    console.log(`Hello, ${name}!`);
}
greet(); // Hello, Guest!
```

---

## 6. 나머지 매개변수와 전개 연산자
- **나머지 매개변수**: 함수의 나머지 인자들을 배열로 받음.
```javascript
function sum(...numbers) {
    return numbers.reduce((a, b) => a + b, 0);
}
console.log(sum(1, 2, 3)); // 6
```

- **전개 연산자**: 배열이나 객체를 펼침.
```javascript
const arr = [1, 2, 3];
const newArr = [...arr, 4, 5];
console.log(newArr); // [1, 2, 3, 4, 5]
```

---

## 7. 모듈 (Modules)
`import`와 `export`를 사용해 모듈화 지원.

- **파일 1: math.js**
```javascript
export const add = (a, b) => a + b;
```

- **파일 2: app.js**
```javascript
import { add } from './math.js';
console.log(add(5, 3)); // 8
```

---

## 8. `Promise`
비동기 작업 처리용 객체.

```javascript
const fetchData = new Promise((resolve, reject) => {
    setTimeout(() => resolve('Data fetched'), 1000);
});

fetchData
    .then(data => console.log(data))
    .catch(err => console.error(err));
```

---

## 9. 맵과 셋 (Map and Set)
- **`Map`**: 키-값 쌍 저장.
- **`Set`**: 중복되지 않는 값 저장.

```javascript
const map = new Map();
map.set('name', 'Alice');
console.log(map.get('name')); // Alice

const set = new Set([1, 2, 2, 3]);
console.log(set); // Set(3) { 1, 2, 3 }
```

---

## 10. `for...of` 루프
이터러블 객체(배열, 문자열 등)를 순회.

```javascript
const arr = [10, 20, 30];
for (const value of arr) {
    console.log(value);
}
```

---

## 11. 클래스 (Classes)
객체지향 프로그래밍을 간결하게 표현.

```javascript
class Person {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }

    greet() {
        console.log(`Hello, my name is ${this.name} and I am ${this.age} years old.`);
    }
}

const john = new Person('John', 30);
john.greet(); // Hello, my name is John and I am 30 years old.
```

---

## 12. 심볼 (Symbol)
유일한 식별자를 생성.

```javascript
const sym1 = Symbol('desc');
const sym2 = Symbol('desc');
console.log(sym1 === sym2); // false
```

---

## 13. 제너레이터 (Generators)
특수한 함수로, 실행을 중단하고 재개할 수 있음.

```javascript
function* generatorFunc() {
    console.log('First yield');
    yield 1; // 첫 번째 중단점
    console.log('Second yield');
    yield 2; // 두 번째 중단점
    console.log('Third yield');
    yield 3; // 세 번째 중단점
    console.log('End of generator');
}

const gen = generatorFunc();
console.log(gen.next().value); // 'First yield' 출력 후 1 반환
console.log(gen.next().value); // 'Second yield' 출력 후 2 반환
console.log(gen.next().value); // 'Third yield' 출력 후 3 반환
console.log(gen.next().value); // 'End of generator' 출력 후 undefined 반환

```

---

ES6는 JavaScript의 사용성과 생산성을 크게 향상시킨 중요한 업데이트로, 위의 기능들은 대부분 현대 JavaScript 개발에서 필수적으로 사용됩니다.
