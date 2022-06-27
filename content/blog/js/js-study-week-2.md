---
title: 2주차 JS 스터디 자료
date: 2022-06-27 13:06:45
category: js
thumbnail: '' 
draft: false
---

(주의) 정리 안 됨

### 다룰 개념
- Lexical Scoping
- Dynamic Scoping
- 변수 shadowing: 가장 먼저 찾아진 것만 찾아 갈 수 있는 것
    - https://developer.mozilla.org/ko/docs/Web/JavaScript/Closures
    - the inner identifier shadows the outer identifier
- with문
- eval
- IIFE
- block scope
- let의 문제
- hoisting
- mutual recursion

---
### 1주차 복습

```js
// 원시 값
let foo = 5;

// 원시 값을 변경해야 하는 함수 정의
function addTwo(num) {
   num += 2;
}
// 같은 작업을 시도하는 다른 함수
function addTwo_v2(foo) {
   foo += 2;
}

// 원시 값을 인수로 전달해 첫 번째 함수를 호출
addTwo(foo);
// 현재 원시 값 반환
console.log(foo);   // 5

// 두 번째 함수로 다시 시도
addTwo_v2(foo);
console.log(foo);   // 5
```

---
## 2주차 
> JS는 인터프리터인가? 컴파일러인가?

### 실행 컨텍스트(Excution Context)
> 함수를 실행할 때 필요한 환경정보를 담은 객체이다 

Execution Context 객체
- Variable Environment: 최초의 식별자 정보 
- Lexical Environment: 값이 바뀔 때 변경사항을 계속 추적
    - environmentRecord: 현재 Context의 식별자(= 호이스팅)
    - outerEnvironmentReference: 외부 식별자(= scope chain)
- eval
    - 런타임에 컴파일타임에 마치 존재했다는 듯이 취급함. 
    - lexical scope 수정 -> eval lexical 최적화 몇 개 불가능 -> 느려짐 
    - strict부터는 새로운 eval을 위한 스콥을 추가
        - 존재하는 것 수정 X. 빨라지긴해..
    - 템플릿 엔진
    - setTimeout을 문자열로 주지마세요... eval을 쓰는 것과 동일
- thisBinding

### Scope
####  Lexical Scope(Static Scope) 
컴파일러의 단계에서 온 것임 
- Compile단계의 결정
- Compile Time Scope = Author Time
- 어디서 invoke되는지는 고려X
- code를 쓴 뒤면 다 결정된거임!
- 물리적으로 어디서 선언됐는지가 중요
- 캐싱 가능하게 사전처럼 미리 긇어와서 최적화하기도 하고 ...

#### Dynamic Scope
- runtime의 결정임
- 어떻게 어디서 어떤 Scope에서 선언됐는지 1도 신경 안 씀
    - 누가 나를 호출했니?

#### Quiz
```js 
function foo() {
    console.log( a ); // 2? 3?
}

function bar() {
    var a = 3;
    foo();
}

var a = 2;

bar();
```

#### Lexical Scope을 망치는 놈
##### 1. `eval`
> 위에서 언급함. 

##### 2. `eval`보다 위험한 `with`
> lexical scope을 망치는 또 다른 놈

```js
var obj = {
  a: 2,
  b: 3,
  c: 4
};

obj.a = obj.b + obj.c;
obj.c = obj.b - obj.a;

with (obi) {
  a = b+ c;
  d = b - a;
  d = 3; // ??
}

obj.d; // undefined
d; // 3 !!
```
line 13: `d`는 뭐야
    - `with`도 Lexical Scope이랑 똑같이 작동해..
    - 새로운 Lexical Scope을 런타임에 생성해...
    - 컴파일러가 이걸 보는 순간 가장 최악을 가정하고 많은 최적화 옵션을 끄게 됨

#### IIFE 
##### 유래
- 2009년부터 예전부터 엄청 흔한 패턴, 이름은 없었음
- ben ellman?
    - 블로그에 올림 
        - benalman.com/news/2010/11/imm...
    - immediate invoked function expression
    - 표준 됨

##### 왜 씀?
    - Encapsulation, Protection, Abstraction, Information hiding
        - 코드가 길어지면
        - 글로벌 네임스페이스, 콜리션되는 것을 보호하기 위해
        - 1000개의 함수가 있는데 1개만 글로벌이길 원한다면?

##### Declaration vs Expression
{}이거 불가능
어떻게 함수나 변수를 숨길까?

`function -> decl`
`(function)-> exppr`
`expr() -> 바로 실행`

##### 사용법
```js 
(funciton(global){
 // private
 global.aa = 23
})(window);
```

 
#### 더 많은 Scope
(Es5)
- global scope
- function scope
- try/catch scope 
(Es6)
- block level

##### Block Scope
- es6+부터 등장
- block에 묶이는 것

#### let
##### Let 선언 예시 1
```js
function foo() {
  var bar = "bar"
  for (let i = 0; i < bar.length; i++) {
    console.log(bar.charAt(i));
  }
  console.log(i); // i는 for문을 위한 것!@!!
}
foo();
```

##### Let 선언 예시 2
```
for(var i=1; i<6; i++){
   setTimeout(function(){
      console.log(i);
   },1000)
}

for(let i=1; i<6; i++){
   setTimeout(function(){
      console.log(i);
   },1000)
}
```

`if` stmt에도 같은 이유로

##### 부가적인 장점
- 실수 예방
- 성능 좋아 
    - gc 빠르게
- ... 

#### var
- `var`은 함수 전체에 묶인다

#### 카일 심슨이 주장하는 `let`의 문제 
- ~~~hoist안 됨~~~ -> 공식문서상 됨
    - 제일 처음에 let을 넣어야 됨
    - https://stackoverflow.com/questions/31219420/are-variables-declared-with-let-or-const-hoisted
    - 말이 많음... init안된다고 하기도 함
        - initialized되지 않은 것을 접근하면 Reference Error 
        - feat by. `temporally dead zone`
        - `eval` 돼야지 initialize
- 리팩터링할 때 추가적으로 블럭을 신경써야됨
    - `var`는 함수면 상관 없음
- implicit 
    - `let`일 수도 아닐 수도
    - 중간에 가다가 `let`으로 선언될 수도..
```js
if(a) {
   console.log(b); // temporal dead zone
   let b = a; 
}
```

#### 새로운 제시
Let Block (Explicit함!)
```js
function foo(bar) {
  let (baz=bar) {
    console.log(baz) // "bar"
  }
  console.log(baz) // Error
}
foo("bar")
```
- 선언은 무조건 위에
- explicit block - let이 쓰인다는 것이 확실
- tc39가 거절함 (es6)

##### 해결법
```js
function foo(bar) {
   /*let*/ { let baz = bar;
    console.log(baz) // "bar"
  }
  console.log(baz) // Error
}
foo("bar")
```

##### transpiler
> 없는 기능을 JS로 transpile

tc39에서 `let`을 테스트할 때 쓰는 transpiler(tracer: google)
```
try(throw void 0)catch
   /*let*/ (foo) {
    foo = "foo";
    console.log(foo)l // "foo"
}
foo // ref error
```

#### hoisting
> 공식 spec에서는 없는 단어 
> 그냥 설명하기 위한 단어임

##### 코드 1
```js
a;
b;
var a = b;
var b = 2;
b;
a;
```

`hoist`는 이러한 Compile Phase를 설명하기 위한 개념적인 것
코드 1의 hoist
```
var a;
var b;
a;
b;
a = b;
b = 2;
b;
a;
```

##### 코드 2
```js
var a = b();
var c = d();
a; // ?
c; // ?

function b() {
  return c;
}

var d = function() {
  return b();
}
```
=> `// d is not a func`

코드 2의 hoist
```js
function b() {
  return c;
}
var a;
var c;
var d;
a = b();
c = d();
a;
c;
d = function() {
  return b();
}
```

##### 코드 3 
```
foo();
var foo = 2;
function foo() {
  console.log("bar");
}

function foo() {
  console.log("foo");
}
```
호이스트 과정
1. 함수가 제일 위로 올라간다
2. 변수가 제일 위로 올라간다

function은 hoist로 덮힐수도 있음
이미 foo라는 변수가 있으므로 var foo선언은 무시 됨.

교훈: 같은 이름을 쓰지말자..

#### 호이스팅 관련 질문
- 호이스팅(컴파일타임에 변수를 다 끌어오는 것)이 왜 필요할까?
    - Interpreter는 불가능 mutual recursion
    - 하나의 함수가 먼저 선언되므로 불가능할 것
￼
---
### 출처 
 - [카일심슨-You Dont Know JS](https://github.com/getify/You-Dont-Know-JS)
 - 주형

