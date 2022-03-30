# 타입스크립트 기초

트렌스 파일과 컴파일 두 용어는 언뜻 비슷해 보이지만  
차이점이 있다.

> ***Trancefile*** 은 타입스크립트 코드를  
> 자바스크립트 코드로 변환한다.  
> 한 언어의 소스 코드를 비슷한 추상화수준의  
> 다른 언어로 변환하는 것이다.

> ***Compile*** 은 서로 다른 추상화 수준의 언어로  
> 변환하는 것을 말한다. 

타입스크립트의 변환 과정은 트랜스파일에 해당하지만  
커뮤니티에서는 편의상 컴파일이라고 한다.

타입스크립트는 자바스크립트의 상위집합 Superset 이다.
이는 모든 ECMA script 버전을 사용가능하다.  

> 수학적으로 상위집합이란 전체 집합을 포함합니다.

타입스크립트가 정적 타이핑 Static typing 을 지원하는 반면,  
자바스크립트 집합은 동적 타이핑 Dynamic typing 만을 지원한다.

> Typing 이라는 용어는 변수에 타입을 정의하는 것을 말합니다.

자바스크립트는 프로그램이 실행 중인 런타임에도 변수 타입에 대해  
알지 못하기 때문에, 변수 타입을 변경할 수 있다. 반면 타입 스크립트는  
정적 타이핑을 지원하므로 한번 타입을 지정하면 그 타입의 리터럴을  
사용해야 한다.

<br/>

> 타입스크립트 예시
```typescript
let customerId: string;
customerId = 123; // 컴파일 오류
```
<br/>

> Javascript 예시
```javascript
let customerId = "A15BN"; // String 할당
customerId = 123; // 123으로 변경
```

또한 가장 큰 장점은 함수의 파라미터에 값을 대입할 때 이다.  
javascript 는 함수의 파라미터에 값을 할당할 때 type 에 대해서  
알기 어렵다
<br/>
```javascript
function getFinalPrice(price, discount) {
    return price - price / discount;
}
```
이는 까딱 잘못하면 다음처럼 호출할수도 있다.

```javascript
getFinalPrice(1000, "10%"); // NaN
```
우리가 의도한 대로 작동하지 않는다.  
의도한 대로 하려면 if 문을 통해 값이 숫자인지 확인해야만 한다.
```javascript
function getFinalPrice(price, discount) {
    if (typeof price ==  "number" && typeof discount == "number")
        return price - price / discount;
    console.log("잘못된 값입니다.");
    return;
}
```
타입검사를 매번 하는 것은 굉장히 귀찮은 일이다.  
하지만 Static typeing language 인 typescript 는 이러한 타입검사가  
필요없다. 왜냐하면 파라미터에 타입을 지정해줘야 하기 때문이다.
```typescript
function getFinalPrice(prince: number, discount: number): number {
    return price - price / discount;
}
```
이는  tsc 가 컴파일 하기 전에 타입스크립트 정적 코드 분석기가 오류를  
감지하고 경고해준다.  
특정 변수 타입을 정의할 때, 코드 에디터나 IDE 내 자동완성기능이  
있어 파라미터 이름과 getFinalPrice 함수 타입을 쉽게 확인할 수 있다.

<br/>

## 1.2 타입스크립트 애플리케이션 개발 과정
---

타입스크립트 코드를 작성하고 애플리케이션을 배포하기까지  
과정에 대해서 알아보자.

> 타입스크립트 앱의 배포과정
```
                변환               통합                        배포
Typescript File --> Javascript File --> Single Javascript File --> Javascript Engine
```
<br/>

타입스크립트는 기존 자바스크립트 라이브러리도 사용가능하다.  
실제로 사용시 오류가 발생하지 않는다.  
단, tsc 에 의해 도움을 받기는 힘들다.  
이러한 단점을 극복하고자 C 언어의 Header File 처럼  
ts.d File 을 제공하여 해당 자바스크립트 라이브러에 타입을  
지정해 줄 수 있다.  
실제로 lodash 는 ts.d File 을 이용하여 각 자바스크립트 변수마다 타입을  
지정해서 tsc 가 이를 읽고 활용한다.  
lodash 가 bundling 되기까지의 과정을 보고 싶다면 p27 을 보자
간단하다.

<br/>

## 1.3 타입스크립트 컴파일러
---

타입스크립트 프로그램을 자바스크립트로 변환시키는 방법은 간단하다.  
타입스크립트 컴파일러는 IDE 에 이미 내장되어 있다.  

```
npm init; // npm init 한다
npm i typescript; // typescript 를 devdependancy 로 설치한다

npm run tsc -v // typescript 의 tsc 가 잘 설치되었는지 version 
               // 확인한다 
```

타입스크립트가 web browser 에서 사용되려면 javascript 로   
transfile 해야 한다  
타입스크립트를 자바스크립트로 컴파일 해본다
<br/>
<br/>
> main.ts
```typescript
function getFinalPrice(price: number, discount: number): number {
    return price - price / discount;
}

console.log(getFinalPrice(100, 10));
console.log(getFinalPrice(100, "10%"));
```

> shell
```
npx tsc --init; // Typescript 초기화
npx tsc main // Argument of type 'string' is not assignable to parameter of type 'number';
```
<br/>

number type의 literal value 를 넣어야 하는데,  
String value  를 넣어서 Compile error 가 발생했다.
하지만 main.js 는 생성되었다.
<br/>
<br/>

> shell

```
> ls

Readme.md main.js main.ts node_modules package-lock.json tsconfig.json
```
> main.js
```javascript
function getFinalPrice(price, discount) {
    return price - price / discount;
}
console.log(getFinalPrice(100, 10));
console.log(getFinalPrice(100, "10%")); // 오류는 런타임에서만 출력
````
<br/>

이처럼 tsc 는 컴파일전에 Error 를 Emit 했지만,  
javascript file 을 생성시켰다.  
이유는 간단하다. javascript 상에서는 유효한 파일이기  
때문이다.

그러나 실제 프로젝트에서는 오류가 있는 파일을 생성하지 않도록 한다.
<br/>

타입스크립트 컴파일러는 20개가 넘는 옵션이 있다.  
그중 하나인 ***noEmitOnError*** 에 대해서 알아보자.  

<br/>

> ***noEmitOnError*** : 타입스크립트 컴파일러는 모든 오류가 고쳐질때까지 이전에 생성된 자바스크립트 파일을 덮어 쓰지 않는다.

<br/>

> shell
```
> npx tsc main --noEmitOnError
```
<br/>

main.ts 내 오류를 고치지 않는 이상 main.js 파일이 생성되지 않는다.

> shell
```
tsc --t ES5 main // --t 는 특정 자바스크립트 버전을 가리킨다.
```
<br/>

타입스크립트 컴파일러는 해당 소스, 최종 디렉토리, 타겟등  
옵션을 미리 설정할 수 있다.  
Project folder 에 있는 tsconfig.json 파일이 있는  
상태에서 tsc 명령어를 입력하면 컴파일러가 tsconfig.json  
내 모든 옵션을 읽는다.
<br/>
<br/>

> tsconfig.json
```json
{
    "compilerOptions": {
        "baseUrl": "src", // src 폴더내의 .ts 파일을 컴파일한다.
        "outDir": "./dest", // 컴파일한 .js 파일을 dest 폴더에 저장한다.
        "noEmitOnError": true, // Error 면 자바스크립트 파일을 생성하지 않는다. 
        "target": "es5" // ecma script target version
    }
}
```
<br/>

컴파일러의 옵션 대상은 구문 검사에도 사용된다.  
예를 들어 es3 를 타켓으로 지정하고,  
 객체 프로퍼티 접근자인 getter나 setter 메서드를  
 사용하면  
 ```
 "Accessors are only available when targeting  
 ECMA script 4 and higher."
 ```
라는 Error message 가 나온다  
이 메서드는 ES3 에서 없는 메서드이기 때문이다.
<br/>
<br/>
> tsconfig.json
```json
"compilerOptions": {
    "noEmitOnError": true,
    "target": "es5",
    "watch": true // watch mode 로 작동한다.
}
```
<br/>

> ***watch***: watch 모드로 타입스크립트 파일이  변경될때  마다 다시 컴파일 한다.

<br/>

> shell
```
npx tsc 
// 이 명령어를 사용하면 tsconfig.json 의 환경설정을 읽고 바로 실행한다. 이 경우는 위에 watch 모드로 작동되도록 한다.
```
<br/>

watch mode 를 중단하려면 Ctrl + C 를 누른다.

