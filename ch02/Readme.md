# 기본타입과 커스텀타입

typescript 의 타입과 커스텀 타입에 대해서 알아본다

<br/>

## 변수 타입 선언
---

javascript 변수에 타입은 왜 선언해야 하는가?  
Typescript 를 사용하지 않고,  
Javascript 의 변수를 선언하는게 더 편리하지 않은가?

이는 각각의 장점이 다르다
 
자바스크립트는 숫자를 담은 변수에 문자열을 재 할당해도  
괜찮다. 자동 형변환이 가능하기 때문이다.
앞서서  javascript 는 dynamic typing 이라고 했다.

> javascript
```javascript
let taxCode = 1;
taxCode = 'lowIncome';
```
<br/>

하지만 typescript 는 static typing 이라 불가능하다.   
즉 문자열이면 문자열, 숫자면 숫자를 대입해야 한다.

> typescript
```typescript
let taxCode: number = 1;
taxCode = 'lowIncome';
// an errorType 'lowIncome' is not assignable to type 'number'.
```
<br/>

강제적 타입 선언이 귀찮게 느껴질 것이다.  
하지만 장기적은 관점에서 볼때 생산성이 눈에 띌 정도로  
향상된다는 사실을 알게 된다.

> 만약 개발자가 실수로 변수 타입을 숫자에서 문자열로 바꾼다면, 런타임이 아닌 컴파일러 단계에서 이 오류를 바로 잡을 수 있기 때문입니다.

<br/>

타입을 명시적 또는 암시적으로 선언할 수 있다. (Type Inference: 타입추론) 

> ***명시적*** 이란 구체적으로 표현한다는 뜻이다.
```typescript
let taxCode: number = 1; // 명시적 선언
```

> ***암시적*** 이란 코드로 표현하지 않아도 컴파일러가 알아서 처리한다는 뜻이다.
```typescript
let taxCode = 1; // 암시적 선언
```
<br/>

이는 typescript 의 compiler 가 코드를 읽고 암시적으로  
처리하면 알아서 number type으로 인식해준다.  
이는 Type Inference(타입 추론) 라고 한다. 

<br/>

## 기본적인 타입 표기
---

> typescript
```typescript
let firstName: String;
let age: number;
```

타입 스크립트에는 다음과 같은 타입표기가 있디.  
대부분 타입은 Self-descriptive 인 이름을 가진다.

| Type | spec |
| :--- | :--- |
| string | 문자열 |
| boolean | boolean 값 |
| number | 숫자 |
| symbol | Symbol 생성자를 호출해 생성된 고윳값 |
| any | 모든 타입을 허용하는 타입, <br/> 코드를 쓰는 동안 정해지지 않은 변수를 지정할 수 있음 |
| unknown | any 와 비슷하나 먼저 타입을 지정하거나 좁히지 <br/> 않으면 조작이 허용되지 않음 |
| naver | 도달할 수 없는 코드를 나타냄 |
| void | 값이 없음 |

<br/>

Symbol 은 ES6 에서 추가된 변경 불가능한  
원시 타입으로 객체 프로퍼티를 만들 수 있다.  

다음의 sym1 과 sym2 의 값은 동일하지 않다.

> typescript
```typescript
const sym1 = Symbol("orderID");
const sym2 = Symbol("orderID");
```
<br/>

새 symbol 을 추가할 때(new keyword 는 생략됨)  
설명 description 추가는 옵션(예: orderID) 이다.  

symbol은 객체 프로퍼티의 고유값을 가진 키를  
생성할 때 사용된다.
<br/>

> typescript
```typescript
const ord = Symbol('orderID');
const myOrder = {
    [ord]: "123"; // ComputedPropertyName 을 사용하여 Symbol 값을 propertyName 으로 지정
}
console.log(myOrder[ord]);
```

<br/>

타입스크립트 역시 자바스크립트에서 '값 없음'을  
나타내는 null 과 undefined 타입을 가진다.  
값이 할당되지 않은 변수는 초기값으로 undefined 를  
가지며 그 자체로 undefined 이다.  
값을 반환하지 않는 함수 역시 undefined 이다.

반면 null 은 명시적으로 값이 비어있음을 나타낸다.
그리고 null 은 객체이다.

다음의 코드는 string or null을 반환하는 코드이다.

<br/>

> typescript
```typescript
function getName(): string | null { .. }
```

문자열을 반환하는 함수를 선언해도 null을  
반환할수 있다. 이렇게 null을 명식적으로  
작성해 주면 코드 가독성이 향상된다.

> any 타입은 숫자, 텍스트, 부을 또는 Customer 같은  커스텀 타입값을 할당할 수 있다.  

그러나 any 타입을 사용하면 타입 체크의 장점을  
잃으므로 코드 가독성이 떨어지기 때문에 되도록  
사용하지 않길르 추천한다.

> never type 은 절대 반환하지 않는 타입을 말한다.
<br/>
절대로 실행이 종료되지 않는 함수나 오류를 발생  
시키기 위해서만 존재하는 함수를 예로 든다

> typescript
```typescript
const logger = () => {
    while(true)
        console.log('서버가 실행중 입니다.');
        // 화살표 함수는 never type 을 반환
}
```
<br/>

> void 타입은 변수 선언이 아니라, 값을 반환하지  
> 않는 함수를 선언하는데 사용한다.

> typescript
```typescript
function logError(errorMessage: string): void {
    console.log(errorMessage)
}
```

never 타입과 다르게 void 함수는 실행을 완료하지만  
값을 반환하지 않는다.

> 타입스크립트 내 타입 표기는 선택 사항입니다.  
> 일부 변수에 타입 표기가 없다면, 타입 스크립트의  
> 타입 검사기는 해당 타입을 유추합니다.

다음을 보자.
> typescript
```typescript
let nomal = "Jhon Smith"; // 타입없이 초기화
let name2: string = "Jhon Smith"; // 타입추가후 초기화
```
<br/>

첫 번째 생은 자바스크립트 스타일로 변수 name1을  
선언하고 초기화하며, name1 의 타입은 문자열임을  
유추할 수 있다.
<br/>

> typescript
```typescript
const age = 25; // 타입없음
function getTax(income: number): number {
    return income * 0.15;
}
let yourTax = getTax(50000); // 타입없음
```

<br/>

타입스크립트는 문자열 리터럴을 타입으로 사용할 수 있다.  
아래 코드의 변수 name3 의 값 Jhon Smith 가 타입값으로 선언되었다

<br/>

> typescript
```typescript
let name3: 'Jhon Smith'; // Jhon Smith 라는 문자열 리터럴의 타입을 가진 name3
```
<br/>

즉 이 값은 Jhon Smith 라는 리터럴 값을 가져야한다.
만약 그렇지 않으면 Error 가 발생한다.

> typescript
```typescript
let name3: 'Jhon Smith';
/*
name3 = 'Mary Lou'; 
// Error: Type '"Mary Mou"' is not assignable to type '"Jhon Smith"'
*/
name3 = 'Jhon Smith';
```

위처럼 string literal 을 사용해서 타입 선언하는  
경우는 상당히 드물다. 대신 유니온과 열거 타입을  
사용한다
<br/>
<br/>

> ***참고사항***  
>
> 초기값없이 변수를 선언하면 타입스크립트 컴파일러는  
> any 타입으로 유추합니다.  
> 이처럼 컴파일러가 변수 타입을 유추하는 것을  
> 타입 확장(type widenine) 이라고 부릅니다.  
> 아래 변수 productId 의 값은 undefined 입니다.
> > typescript
> ```typescript
> let productId;
> productId = null;
> productId = undefined;
> ```
> 타입스크립트 컴파일러는 any 타입으로 유추하고  
> null 과 undefined 값에 할당합니다.
> 따라서 변수 productId 의 타입은 any 입니다
>
> 타입스크립트 컴파일러는 --stricNullCheck 옵션을 통해  
> 정해진 변수에 null 이 입력되는 것을 막습니다.
> > 
> ```json
> compilerOptions: {
>   ...
>   "strictNullCheck": true
> }
> ```
> 
> > typescript
> ```typescript
> let productId = 123;
> productId = null; // compile error
> productId = undefined; // compile error
> ```
>
> strictNullCheck 는 undefined 를 잡을 때도 도움이  
> 된다. 예를 들어 어떤 함수가 조건부 프로퍼티가  
> 들어있는 객체를 반환할 때 여러분이 짠 코드는 해당  
> 프로퍼티가 존재한다고 착각하고 함수를 사용하려 할 수 있다. 
>

<br/>

## 함수 본문 내 타입선언
---
<br/>
타입스크립트 함수와 함수 표현식은 자바스크립트와  
유사하지만 파라미터 타입과 반환 값을 명시적으로  
선언한다.

다음은 세금을 받는 함수이다.
이 함수는 세가지 파라미터를 받는다.  
* 함수명: calcTax
* 인자:
* state
* income
* dependents

거주지에 따라 부양가족 1인당 $500 혹은 $300 세금을 공제 받는다.

> javascript
```javascript
function calcTax(state, income, dependents) {
    if (state == "NY")
        return income * 0.06 - 500 * dependents;
    if (state == "NJ")
        return income * 0.05 - 300 * dependents;
}

calcTax("NY", 50000, "two"); // NaN
```

위는 충분히 실수할 우려가 있다.  
하지만 typescript 는 compiler 가 check 하므로  
오류를 미리 알고 대처할 수 있다.

> typescript
```typescript
function calcTax(state: string, income: number, dependents: numger): number {
    if (state == "NY")
        return income * 0.06 - 500 * dependents;
    if (state == "NJ")
        return income * 0.05 - 300 * dependents;
}

calcTax("NY", 50000, "two"); // Error
```

또한 해당 타입의 값을 쉽게 알수 있으므로,  
프로그래머가 해당 함수의 인자를 알맞게 추가 가능하다.  
위의 타입명시는 마치 명세서처럼 동작한다.

> 컴파일하는 동안 타입 검사는 실제 프로젝트 개발 중 많은 시간을 절약해 줍니다.

> ***calcTax() 함수 고치기***  
> 
> > typescript
> ```typescript
> function calcTax(state: string, income: number, dependency: number): number | undefined { ... }
> // Function lacks ending return statement and return type does not include 'undefined'.
> ```
> calcTax() 함수의 첫 번째 파라미터인 state 는 NY 와  
> NJ 의 두가지 경우만 처리합니다.  
> 없는 주를 추가하면 undefined 를 반환합니다.  
> 타입스크립트 컴파일러는 결과값이 undefined 가 될수  
> 있다는 경고 메세지를 표시하지 않습니다.  
> 아래와 같이 타입스크립트 구문을 추가해 결과 값이  
> number 또는 undefined 타입으로 변환될 수 있음을  
> 알릴 수 있습니다.
> > typescript
> ```typescript
> function calcTax(state: string, income: number, dependency: number): number | undefined { ... }
> ```

## 유니온 타입
---

union 은 or 연산자처럼 변수에 지정할 수 있는 타입이  
여러개일 경우 사용한다.

> typescript
```typescript
let padding: string | number; // assign to string or number 
```

다음 함수를 살펴보자.  
다음 함수는 vlaue 와 padding 인자를 받으며,  
padding 이 숫자값이면 value 앞에 숫자만큼의 공백을  
붙인다.  
만약 padding 이 string 이면 value 앞에 string 을  
붙혀 값을 반환한다.
<br/>

> typescript
```typescript
function padLeft(value: string, padding: any): string {
    if (typeof padding == 'string')
        return padding + value;
    if (typeof padding == 'number')
        return Array(padding + 1).join(' ') + value;
    throw new Error(`Expected string or number, got '${ padding }'.`);
}
```
<br/>

> ***typeguard***  
> 
> 조건문을 적용해 변수 타입을 세분화하는 것을   
> 타입 축소 type narrow 라고 하며, if 문은 typeof  
> 타입가드를 사용해 둘 이상의 타입을 허용하도록  
> 그 범위를 축소했습니다.  
> 타입 가드를 사용하는데 일반 타입이면 typeof 를 사용  
> 하면 참조 타입 같은경우 instanceof 를 사용하여  
> 값의 타입을 확인합니다.
> 

만약 다음처럼 any 가 아닌 union 을 사용하여 값을 축소하면  
가장 아래의 예외처리를 사용할 필요 없이 compiler 에  
의해 타입체크가 된다.

> typescript
```typescript
function padLeft(value: string, padding: string | number): string {
    if (typeof padding == 'string')
        return padding + value;
    if (typeof padding == 'number')
        return Array(padding + 1).join(' ') + value;
}

padLeft("i'm string", 3) // "   i'm string"
padLeft("i'm string", true) // compile error
```
<br/>

다음은 never 타입을 설명해 본다.
>typescript
```typescript
function padLeft(value: string, padding: string|number): string {
    if (typeof padding === "number") {
        return Array(padding + 1).join(" ") + value;
    }
    if (typeof padding === "string") {
        return padding + value;
    } else {
        return padding;
    } // else block 은 실행되지 않는다.
}
```
<br/>

never 는 어떤 타입과도 호환되지 않는 타입이다.  
논리적으로 끝까지 실행될 수 없는 함수의 반환 값은  
never 타입이 된다.

실제 IDE 에서 else 절의 padding 값을 마우스로  
올려보면 padding 값의 type 이 never 인것을  
알 수 있다.

<br/>

## 커스텀 타입 정의
---
타입스크립트는 type, interface, enum keyword 및  
class declaration 으로 custom type 을 만들 수 있다. 


### 타입 키워드 사용
---
type keyword 는 새로운 타입을 선언하거나  
type aliase 를 사용해 이미 존재하는 타입에 다른  
이름을 붙여 사용할 수 있다.  

환자의 이름, 키, 몸무게를 보여주는 앱을 만든다고  
가정해 보자.
> typescript
```typescript
type Foot = number;
type Pound = number;
```
<br/>

이 타입 별칭을 사용해 환자를 나타내는 Patient 타입을  
만들어보자.

> typescript
```typescript
type Patient = {
    name: string;
    height: Foot;
    weight: Pound;
}

let patient: Patient = {
    name: 'Joe Smith',
    height: 5,
    weight: 100
};
```

타입 별칭은 자바스크립트 코드로 컴파일되지 않는다.  
다음은 자바스크립트로 컴파일 했을때의 코드이다.
> javascript
```javascript
let patient = {
    name: 'Joe Smith',
    height: 5,
    weight: 100
};
```
만약 patient 에서 weight 을 빠트리면 Error 가 발생한다.
정해둔 type 대로 하지 않았기 때문이다.

> typescript
```typescript
let patient: Patient = {
    name: 'Joe Smith',
    height: 5,
}; 
// Property 'weight' is missing in type '{ name: string; height: number; }' but required in type 'Patient'.

```
weight 을 빼도 Error 가 발생하지 않도록 하고 싶다면,  
다음처럼 ? 조건부 프로퍼티를 사용한다.
<br/>
> typescript
```typescript
type Foot = number;
type Pound = number;

type Patient = {
    name: string;
    height: Foot;
    weight?: Pound; // 조건부 property 사용
}

let patient: Patient = {
    name: "Jhon Smith",
    height: 5,
}; 
```
함수 시크니처에도 type 키워드와 타입 별칭을 사용  
가능하다. 

> typescript
```typescript
type ValidatorFn = (c: FormControl) => { [key: string]: any } | null;
```
> typescript
```typescript
{ [key: string]: any } // 모든 타입의 프로퍼티를 가질 수 있는 객체를 의미 
```
 아래와 같이 FormControl class 생성자를 만들 때  
 앞서 정의한 커스텀 타임 ValidatorFn 을 파라미터로  
 사용가능하다.
 > typescript
 ```typescript
class FormControl {
    constructor (initialValue: string, validator: ValidatorFn | null) { ... }
} 
```

### 클래스 내 커스텀 타입 사용
---

자바스크립트는 클래스 프로퍼티를 선언하는 구문이  
없지만 타입스크립트는 그렇지 않다

다음을 보자

> typescript
```typescript
class Person {
    firstName: string;
    lastName: string;
}

const p = new Person();
p.firstName = 'jh';
p.lastName = 'g';
```
이를 javascript  로 컴파일하면 다음과 같다
> javascript
```javascript
"use strict"

class Person {}

const p = new Person();
p.firstname = 'jh'
p.lastName = 'g';
```

javascript file 에서는
Person 클래스의 프로퍼이가 없다.  
Person 생성자를 선언하지 않았기 때문에 인스턴스를  
만든 후에 프로퍼티를 초기화 한다.

생성자는 클래스 인스턴스가 생성된 후에 한번만 실행되는  
특별한 함수이다.

다음처럼 초기화 해보도록 해보자.
> typescript
```typescript
class Person {
    constructor(public firstName: string, public lastName: string) {
    }
}

const p: Person = new Person('jh', 'g');
```
이렇게 하면 다음처럼 javascript 가 생성된다.
> javascript
```javascript
class Person {
    constructor(firsName, lastName) {
        this.firsName = firsName;
        this.lastName = lastName;
    }
}

const p = new Person("jh", "g");
```

class property 를 선언할때 readonly access modifier 를  
사용할 수 있다.  

class constructor 내부 등에 프로퍼티를 초기화 하는 경우  
그 값이 바뀌지 않아야 하는 경우가 종종 있다.
readonly access midifier는  변경 불가능한 상수를   
나타내는 const keyword 와 비슷하다.  
다만, const 는 클래스 프러퍼티에 사용할 수 없다


> typescript
```typescript
class Block {
    readonly nance: number;
    readonly hash: string;
    constructor (
        readonly index: number,
        readonly previousHash: string,
        readonly timestamp: number,
        readonly data: string,
    ) {
        const { nance, hash } = this.mine();
        this.nance = nance;
        this.hash = hash;
    }
    // ...
}
```

Block class 는 총 6개의 readonly 프로퍼티를 가진다  
생성자 파라미터에 클래스 프로퍼티를 명시적으로  
선언하지 않아도 된다.
이는 편의상 생략한것 같다.

<br/>

### 인터페이스를 사용한 커스텀 타입
---
대다수 많은 객체 지향 언어는 interface 문법 구조가 있다.  
이는 객체 프로퍼티 또는 메서드 구현을 위해 사용된다.

interface 는 자바스크립트 문법에 존재하지 않는다.  
그러므로 compile 시 interface 는 없다.

> typescript
```typescript
interface Person {
    firstName: string;
    lastName: string;
}
```

interface keyword 로 커스텀 타입을 선언했다.  
interface 는 class 의 설계도라 생각해도 된다,
다음처럼 class Person 에 implements 로 구현 가능하다

혹은 Person 의 타입을 가진 객체를 선언해도 사용가능하다.
다음의 코드를 보자

> typescript
```typescript
interface IPerson {
    firstName: string;
    lastName: string;
}

function savePerson(p: Preson): void {
    console.log("save ", p);
}

class Person implements IPerson {
    constructor(public firstName: string, public lastName: string) {}
}

const p1: IPerson = new Person("jh", "g");
const p2: IPerson = {
    firstName: "jh",
    lastName: "g",
};
/*
const p3: IPerson = {
    firstName: "jh",
}; // Error
*/

savePerson(p1); // Person{... }
savePerson(p2); // {... }
```
p1 을 보면 Person 의 타입이 아니라 IPerson type 이다.  
이는 Person 이 IPerson 에 의해 implements 되었기에    
같은 type 의 property 를 갖기에 가능하다.  

p2 역시 IPerson 에서 정의한 property 를 가졌기에  
컴파일러가 error 를 발생시키지 않는다.

마치 type 을 사용한 타입지정을 객체단위로 지정한것과  
같다.  

이는 마치 객체의 설계도처럼 사용된다.


> typescript
```typescript

interface IPerson {
    firstName: string;
    lastName: string;
}

function savePerson(p: Preson): void {
    console.log("save ", p);
}

const p = {
    firstName: "jh",
    lastName: "g",
}; // p 객체에  type 을 제거해본다.

savePerson(p); // Error 가 발생하지 않는다.
```
위처럼 동작하는 이유는 Structure type system 때문이다.  
타입 스크립트는 두 타입의 구조만을 가지고 호환성을  
결정한다.

서로 다른 타입임에도 맴버가 서로 일치한다면 두 타입은  
서로 호환되며 명시적인 표시는 필요하지 않는다.

<br/>

### 구조적 타입 시스템과 명목적 타입 시스템
---

number, string 등 원시 타입은 간단한 이름만을 갖고  
있지만, 객체와 클래스 등 복잡한 타입은 이름과 더불어  
많은 프로퍼티를 가지고 있다.

두개의 타입이 있을 때 이 둘이 같은 타입인지  
다른 타입인지는 어떻게 판단할 수 있울까?  

nominal type system(명목적 타입 시스템) 을 사용하는  
자바 같은 일부 객체지향 언어는 같은 네임스페이스안에  
같은 이름으로 선언된 클래스를 동일하다고 판단한다.

nominal type system 에서는 Person 타입의 객체이면  
반드시 Person 타입의 변수에 할당해야 한다.  
물론 Polymorphism 을 제공하므로 Person 타입의 자손  
역시 할당은 가능하다.

다음은 자바 코드로 structure type system 을  
적용해 본것이다
> java
```java
class Person {
    String name;
}

class Customer {
    String name;
}

Customer cust = new Person(); // Compiile Error
```

반면 타입스크립트는 Structure type system 을 따르므로  
구조만 같다면 적용가능하다.
> typescript
```typescript
class Person {
    name: string;
}
// Property 'name' has no initializer and is not definitely assigned in the constructor.

class Customer {
    name: string;
}
// Property 'name' has no initializer and is not definitely assigned in the constructor.


const cust: Customer = new Person(); // 잘 작동한다.
```
위의 Error 는 name property 를 초기화 하지 않아서  
생기는 에러이다.  
이는 Structure type system 과는 전형 상관없으므로  
무시하자.

cust 를 보면 Customer type 인데 Person 으로  
할당해도 아무런 Error 가 없다.  
이는 구조적으로 같기에 컴파일러가 허용한 것이다.

이러한 constructure type system 을 Duck typing 이라고  
한다. 

> 새가 강에 떠있고, 뒤뚱거리며, 꽥괙거린다면 오리일 것이다. <br/>: Duck typing 

이는 객체리터럴을 사용하여 만들어둔 클래스 타입의  
변수나 상수에도 대입가능하다
> typescript
```typescript
class Person {
    name: string;
}

class Customer {
    name: string;
}

const cust: Customer = { name: "Mayr" };
const pers: Person = { name: "Mayr" };
```

전혀 문제 없이 작동한다.
만약 다음처럼 구조가 갖지 않을 경우  
과연 잘 작동할까?
이는 경우데 따라 다르다

> typescript
```typescript
class Person {
    name: string;
    age: number;
}

class Customer {
    name: string;
}

const cust: Customer = new Person(); // 잘 작동한다.
```
타입스크립트는 Person 과 Customer 가 같은 구조를  
갖추었음을 확인한다.  
앞선 코드에서 name 이란 프로퍼티를 가진 Customer 타입의  
상수를 사용해 똑같이 name 프로퍼티를 갖는 Person  
객체를 가리키려 했다.

Person 클래스는 age 프로퍼티도 갖지만,  
Customer class 가 사용하는 건 name property 뿐이기  
때문에 구조적으로 사용가능하다

만약 반대의 경우라면 어떨까?
즉 const pers:Person 변수에 Customer 객체를 생성하는  
것이다.

> typescript
```typescript
class Person {
    name: string;
    age: number;
}

class Customer {
    name: string;
}

const pers:Person = new Customer();
// Error: Property 'age' is missing in type 'Customer' but required in type 'Person'.
```

이는 Person 객체에 age property 가 있는데,  
Customer 에는 없으므로 Error 가 난다. 즉 구조적으로  
맞지 않는다.
이말은 Person 객체내 age 프로퍼티에 메모리를 할당할수  
없으므로 생기는 문제이다.


<br/>

### 커스텀 타입의 유니온
---

사용자 활동에 응답하는 다양한 액션을 가진  
어플리케이션을 개발한다고 가정해보자

각 액션을 다른 이름의 클래스로 만든다.  
액션의 타입은 필수이며 옵션 사항으로 검색쿼리등을  
가진 페이로드를 가진다. 
> typescript
```typescript
export class SearchAction { // actionType 과 payload 가 있다
    actionType = "SEARCH";
    constructor(readonly payload: { searchQuery: string }) {}
}

export class SearchSuccessAction {
    actionType = "SEARCH_SUCCESS";
    constructor(readonly payload: { searchQuery: string[]}) {}
}

export class SearchFailedAction { // actionType 은 있지만 payload 가 없다
    actionType = "SEARCH_FAILED"; 
}

export type SearchActions = SearchAction | SearchSuccessAction | SearchFailedAction; // Union 타입 선언
```

식별 가능한 유니온(Discriminated Union) 은  
공통 프로퍼티 즉, 공통의 식별자를 가진 맴버로  
이루어진 타입을 말한다.

각 맴버마다 actionType 이라는 식별자가 있기 때문에  
유니온 식별에 해당한다.

다음코드를 더 살펴보자
> typescript
```typescript
interface Rectangle {
    kind: 'rectangle';
    width: number;
    height: number;
}

interface Circle {
   kind: 'circle';
   radius: number;
}

type Shape = Rectangle | Circle;
```

Shape 타입은 유니온 식별로 Rectangle 과 Circle 타입을  
가지며, 이들 타입 모두 공통으로 kind 프로퍼티를 가진다.

kind 프로퍼티의 값에 따라 Shape 크기를 계산할 수 있다.

> typescript
```typescript
function area(shape: Shape): number {
    switch(shape.kind) {
        case "rectangle":
           return shape.width * shape.height; 
        case "circle":
            return Math.PI * shape.radius ** 2;
    }
}

const myRectangle: Rectangle = {
    kind: 'rectangle',
    height: 20,
    wight: 10,
};

const myCircle: Circle = {
    kind: 'circle',
    radius: 10,
};

console.log(`Rectangle area: ${area(myRectangle)}`)
console.log(`Circle area: ${area(myCircle)}`)
```

이는 kind property 를 사용하여 Shape 크기를  
계산할 수 있다는 것을 볼 수 있다.

> ***Type Guard in***  
> 타입가드 in kdyword 는 타입 범위를 축소하는 표현이다.  
> Union type 인자를 가진 함수는 호출하는 동안  
> 실제 값을 체크할 수 있다.  
> 다음 코드를 살펴보자
> 
> >typescript
> ```typescript
>  interface A {a: number};
>  interface B {b: number};
> 
>  function foo(x: A|B): number {
>       if ("a" in x) 
>           return x.a;
>       return x.b;
>  }
> ```
> 이는 x 안에 a 가 있을 경우 x.a 를 반환하고,  
> 아니면 x.b 를 반환한다.   
> 이는 프로퍼티가 포함되어 있는지 확인하는 문구이다.  
> 사실 객체의 프로퍼티의 key 는 문자열이므로 비교시  
> 문자열로 비교한다.

<br/>

## any, unknown
---

any 와 unkown 의 차이점에 대해서 알아보자

any 타입의 변수는 모든 타입의 값을 가질 수 있다.
any 타입은 자바스크립트와 다를바 없다고 보면 된다.  
any 타입 역시 존재하지 않는 프로퍼티에 접근하려면
런타임 중 예기치 않은 요류가 발생할 수 있다.

unknown 타입은 3.0 버전에 도입되었다.  
컴파일러는 프로퍼티에 접근하기 전 unkown 타입의  
변수에 타입 범위를 줄이라고 경고한다. 따라서  
런타임 오류를 줄이라고 경고한다


> typescript
```typescript
type Person {
    address: string;
}

let person1: any;

person1 = JSON.parse('{"adress": "25 Broadway}');
// 잘못 표기된 adress
console.log(person1.address); // undefined
```

JSON 문자열 내 address 에 철자 오류가 있어서  
undefined 가 출력된다. 

이번에는 unkown 타입으로 바꾸어 보자
> typescript
```typescript
let person2: unknown;

person2 = JSON.parse('{"adress": "25 Broadway"}');

console.log(person2.address); // Compole Error
```
타입스크립트는 사용자가 정의한 타입 가드로 객체의  
특정 타입을 확인할 수 있다.

> ***is keyword***   
> 갑자기 등장한 keyword 이다.  
> 책에서는 따로 설명이 없다. 처음보는 문장이라  
> 어색하다. 
> 찾아보니 
> ```
> 타겟 is 타입
> ```
> 형식으로 동작하는 듯 하다.
> 이는 해당 ***타겟*** 의 booelan 값을 반환하고,
> 반환된 이후의 블록내부(if 문)에서   
> ***타켓*** 의 ***타입범위*** 를 좁혀준다. 
> 내가 이해한 바로는 이렇게 동작하는것 같다.


> typescript
```typescript
type Person = {
    discriminator: 'person'; // discriminator property
    address: string;
}

const person: unknown = JSON.parse(`{
    "address": "25 Broadway".
    "discriminator": "person",
}`);

// type guard
const isPerson = (o: any): o is Person => !!o && o.discriminator == 'person';

// 
if (isPerson(person)) {
    console.log(person.address);
} else {
    console.log("person is not a Person");
}
```

unkonwn 타입에 대한 지식이 아직 부족하다.  
그래서 좀더 정리된 내용을 찾아서 아래 적어 놓는다  
> **unkonwn**  
>
>ny처럼 어떤 타입의 value든 할당할 수 있으면서 실제로 사용할 때는 개발자로 하여금 타입을 체킹하도록 만들 수 있는 타입이 필요했다. 그래서 조금 덜 수용적인 unknown 타입을 만들게 된 것이다. 
>사용자로부터 입력을 받거나 잘 알려지지 않은 외부 API를 사용하는 등 실제로 어떤 값이 올 지 모를 때, 어떤 값이든 할당할 수 있지만 정작 이후에 그 값을 사용할 때는 타입 체킹을 해서 안전하게 사용하게 하기 위해 쓸 수 있다.