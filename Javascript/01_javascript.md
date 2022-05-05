# javascript

[TOC]

## Javascript Intro



### Javascript의 필요성

- 브라우저 화면을 동적으로 만들기 위함
- 브라우저를 조작할 수 있는 유일한 언어





## Browser



### Browser에서 할 수 있는 일

- DOM(Document Object Model) 조작
  - 문서(HTML)조작
- BOM(Browser Object Model) 조작
  - navigator, screen, location, frames, history, XHR
- JavaScript Core (ECMAScript)
  - Data Structure(Object, Array), Conditional Expression, Iteration



#### DOM

- 해석
- 파싱(Parsing): 구문 분석, 해석
- 브라우저가 문자열을 해석하여 DOM Tree로 만드는 과정
- 이 때 브라우저마다 엔진이다르고, 그래서 속도차이가 남

#### BOM

#### JavaScript Core(ECMAScript)





## ECMAScript





## 세미콜론





## 코딩 스타일 가이드





## 변수와 식별자



### 식별자의 개념 및 특징

- 변수를 구분할 수 있는 변수명
- 반드시 문자, $, _ 로 시작
- 대소문자는 구분되며 클래스명 외에는 모두 소문자로 시작
- 예약어는 사용 불가능



### 작성 스타일

- 카멜 케이스
  - 변수, 객체, 함수에 사용되며 두 번째 단어의 첫글자부터 대문자로 사용
- 파스칼 케이스
  - 클래스, 생성자에 사용
- 대문자 스네이크 케이스
  - 상수에 사용



### 선언, 할당, 초기화

- 선언(Declaration): 변수를 생성하는 행위 또는 시점

  ```javascript
  let foo
  console.log(foo)
  ```

  - 할당(Assignment): 선언된 변수에 값을 저장하는 행위 또는 시점

  ```javascript
  foo = 11
  console.log(foo)
  ```

- 초기화(Initialization): 선언된 변수에 ***처음으로*** 값을 저장하는 행위 또는 시점

  ```javascript
  let bar = 0 // 선언+할당
  console.log(bar)
  ```

  

### 변수 선언

- let, const, var



#### let

- 재할당 할 예정인 변수 선언 시 사용

- 변수 재선언 불가능

- 재할당 가능

  ```javascript
  let number = 10
  number = 10
  
  console.log(number) // 10
  ```

- 재선언 불가능

- 블록스코프



#### const

- 재할당 할 예정이 없는 변수 선언 시 사용

- 반드시 선언할 때 값을 할당해줘야 함

- 변수 재선언 불가능

- 재할당 불가능

  ```javascript
  const number = 10
  number = 10	// error
  ```

- 재선언 불가능

- 블록스코프



#### var

- var로 선언한 변수는 재선언/재할당 모두 가능

- ES6 이전에 사용되던 키워드고, 호이스팅 되는 특성으로 인해 예기치 못한 문제를 발생할 수 있으므로 ES6 이후부터는 var 대신 const와 let을 사용하는 것을 권장

- 함수 스코프

  ```javascript
  var number = 10
  var number = 50
  ```




#### 명령어의 생략

- 별도의 변수 선언 명령어가 없이도 변수 선언이 가능

- 재선언 및 재할당이 가능하다는 점에서 `var`와 유사(물론 선언 명령어가 없기 때문에 재선언과 재할당을 구분할 수는 없음)

- `var`과의 차이

  - 함수 스코프의 `var`과는 달리 전역 스코프를 가짐

    ```javascript
    var a = 5;
    function num() {
        var a = 6;
        console.log(a);
    }
    num()	//6
    console.log(a)	//5
    //------------------------------------
    b = 5;
    function num2() {
        b = 6;
        console.log(b);
    }
    num2()	// 6 
    console.log(b)	// 6
    ```

  - 호이스팅의 문제가 발생하지는 않는 것처럼 보임

    ```javascript
    console.log(a)
    a = 5
    // Uncaught ReferenceError: a is not defined
    ```

    

  



### 블록스코프(block scope)

- if, for, 함수 등의 중괄호 내부를 가리킴

- 블록스코프를 가지는 변수는 블록 바깥에서 접근이 **불가능**

  ```javascript
  let x = 1
  
  if (x === 1) {
      let x = 2
      console.log(x)	//2
  }
  
  console.log(x)	//1
  ```

  

### 함수 스코프(function scope)

- 함수의 중괄호 내부를 가리킴
- 함수 스코프를 가지는 변수는 함수 바깥에서 접근 불가능



### 호이스팅(hoisting)

- 변수를 선언 이전에 참조할 수 있는 현상

- 변수 선언 이전의 위치에서 접근 시 `undefined`를 반환

  ```javascript
  console.log(username) // undefined
  var username = '홍길동'	// Shift+Enter로 내려오면 error가 안남
  
  console.log(email)
  let email = ~ // error
      
  console.log(age)
  const age = 50 // error
  ```





## 데이터 타입



### 데이터 타입 종류

- 크게 원시타입(Primitive type)과 참조타입(Reference type)으로 분류
- 원시타입: Number, String, boolean, undefined, null, Symbol
- 참조타입: Array, Function, ...



### 원시타입(Primitive type)

- 객체(object)가 아닌 기본 타입
- 변수에 해당 타입의 값이 담김
- 다른 변수에 복사할 때 실제 값이 복사됨



#### 숫자(Number) 타입

- 정수/실수 구분이 없음
- 부동소수점 형식을 따름
- `NaN(Not-A-Number)`
  - 계산이 불가능할 경우 반환되는 값
  - 수가 아님을 나타내지만 `NaN`의 `type`은 `number`임에 주의

```javascript
const a = 13
const b = -5
const c = 3.14
const d = 2.998e8
const e = Infinity // 양의 무한대
const f = -Infinity // 음의 무한대
const g = NaN
```




#### 문자열(String) 타입

- 텍스트 데이터를 나타내는 타입
- 작은따옴표, 큰따옴표 모두 가능
- 템플릿 리터럴(Template Literal)
  - ES6부터 지원
  - 따옴표 대신 backtick으로 표현
  - `${ expression }`형태로 표현식 삽입 가능

```javascript
const firstName = 'Brandan'
const lastName = 'Eich'
const fullName = `${firstName} ${lastName}`

console.log(fullName)	// Brandan Eich
```



#### undefined

- 변수의 값이 없음을 나타내는 데이터 타입

- 변수 선언 이후 직접 값을 할당하지 않으면 자동으로 `undefined`가 할당됨

  ```javascript
  let firstName
  console.log(firstName)	// undefined
  ```

- 이 외에도 `undefined`가 할당되는 경우는 다음과 같다.

  - 사용자가 명시적으로 `undefined`를 지정하는 경우

  - 존재하지 않는 속성에 접근할 때

    ```javascript
    let obj = { name: 'ryu' }
    console.log(obj.age)	// undefined
    ```

  - 함수에 `return`문이 없는 경우

  - 호출되지 않는 함수를 실행하는 경우

- `null`, `empty`와의 구분이 필요



#### null

- 변수의 값이 없음을 **의도적**으로 표현할 때 사용하는 데이터 타입
- `null` 타입과 `typeof` 연산자
  - `typeof` 연산자: 자료형 평가를 위한 연산자
  - `null` 타입은 ECMA 명세의 원시 타입의 정의에 따라 원시 타입에 속하지만 `typeof` 연산자의 결과는 객체(object)로 표현됨
  - 명백한 오류임에도 아직 고쳐주지 않음
  - 맨처음에 `null`은 `null type`이 아니라 `object`였는데, 이 `null`을 `object`로 보고 작성한 옛날의 코드로 인해 고쳐주지 않는 것



#### Boolean 타입

- 논리적 참 또는 거짓을 나타내는 타입

- `true`, `false`로 표현

- 조건문, 반복문에서 유용하게 사용

  - 조건문/반복문에서 `boolean`이 아닌 데이터 타입은 자동 형변환 규칙에 따라 `true/false`로 변환됨

  ```javascript
  let isAdmin = true
  console.log(isAdmin) // true
  
  isAdmin = false
  console.log(isAdmin) // false
  ```

  

### undefined, null, empty의 비교

#### undefined

- 빈 값을 표현하기 위한 데이터타입
- 변수 선언 시 아무값도 할당하지 않은 경우 자바스크립트가 자동으로 할당
- `typeof` 연산자의 결과는 `undefined`
- 변수의 선언만 이루어진 상태
- 메모리에 변수의 위치는 정해졌지만 공간은 빈 상태



#### null

- 빈 값을 표현하기 위한 데이터 타입
- 개발자가 의도적으로 필요한 경우 할당
- `typeof` 연산자의 결과는 `object`
- 메모리에 변수의 위치가 정해지고 공간은 빈 상태
- 조건식 등에서 값이 비어있는 경우를 조건에 넣기 위해서 사용할 때도 필요



#### empty

- `typeof` 연산자의 결과는 `string`

  ```javascript
  const a = ''
  console.log(typeof(a))
  ```

- `undefined`나 `null`과는 다르게 변수의 선언과 데이터가 **있는** 상태

- 단지 데이터가 빈문자열일 뿐



### 자동 형변환(ToBoolean Conversinos)

| 데이터 타입 |    거짓    |        참        |
| :---------: | :--------: | :--------------: |
|  Undefined  | 항상 거짓  |        X         |
|    Null     | 항상 거짓  |        X         |
|   Number    | 0, -0, NaN | 나머지 모든 경우 |
|   String    | 빈 문자열  | 나머지 모든 경우 |
|   Object    |     X      |     항상 참      |



### 참조타입(Reference type)

- 객체(object) 타입의 자료형
- 변수에 해당 객체의 참조값이 담김
- 다른 변수에 복사할 때 참조 값이 복사됨

- 추후 서술



## 연산자



### 할당 연산자

- 오른쪽에 있는 피연산자의 평가 결과를 왼쪽 피연산자에 할당하는 연산자

  ```javascript
  let x = 0
  
  x += 10
  console.log(x)	//10
  
  x -= 3
  console.log(x)	//7
  
  x *= 10
  console.log(x)	//70
  
  x /= 10
  console.log(x)	//7
  
  x++
  console.log(x)	//8
  
  x--
  console.log(x)	//7
  ```

- Increment, Decrement 연산자

  - ++, --
  - Airbnb Style Guide에서는 ++, -- 보다는 +=, -=와 같이 분명한 표현을 더 권장함



### 비교 연산자

- 피연산자들을 비교하고 결과값을 `boolean`으로 반환하는 연산자
- 문자열은 유니코드 값을 사용하며 표준 사전 순서를 기반으로 비교

```javascript
const numOne = 1
const numTwo = 100
console.log(numOne < numTwo)	//true

const charOne = 'a'
const charTwo = 'z'
console.log(charOne > charTwo)	//false
```



### 동등 비교 연산자(==)

- 두 피연산자가 같은 값으로 평가되는지 비교 후 `boolean` 값을 반환
- 비교시 암묵적 타입 변환을 통해 타입을 일치시킨 후 같은 값인지 비교
- 두 피연산자가 모두 객체인 경우 메모리의 같은 객체를 바라보는지 판별
- ***예상치 못한 결과가 발생할 수 있으므로 특별한 경우를 제외하고 사용하지 않음***

```javascript
const a = 1004
const b = '1004'
console.log(a == b)	//true

const c = 1
const d = true
console.log(c == d)	// true

console.log(a + b)	// 10041004
console.log(c + d)	// 2
```



### 일치 비교 연산자(===)

- 두 피연산자가 같은 값으로 평가되는지 비교 후 `boolean`으로 반환
- 엄격한 비교가 이뤄지며 암묵적 타입 변환이 발생하지 않음
  - 엄격한 비교: 두 비교 대상의 타입과 값 모두 같은지 비교하는 방식
- 두 피연산자가 모두 객체일 경우 메모리의 같은 객체를 바라보는지 판별

```javascript
const a = 1004
const b = '1004'
console.log(a === b)	//false

const c = 1
const d = true
console.log(c === d)	//false
```



### 논리 연산자

- and 연산: &&
- or 연산: ||
- not 연산: !
- 단축 평가 지원
  - false && true -> false
  - true || false -> true

```javascript
console.log(true && false)	//false
console.log(true && true)	//true
console.log(1 && 0)	//0
console.log(4 && 7)	//7
console.log('' && 5)	//''

console.log(true || false)	//true
console.log(false || false)	//false
console.log(1 || 0)	//1
console.log(4 || 7)	//4
console.log('' || 5)	//5

console.log(!true)	//false
console.log(!'Bonjour')	//false
```



### 삼항연산자(Ternary Operator)

- 세 개의 피연산자를 사용하여 조건에 따라 값을 반환
- 가장 왼쪽의 조건식이 참이면 콜론(:) 앞의 값을 사용하고 그렇지 않으면 콜론 뒤의 값을 사용
- 변수에 할당 가능
- 한줄에 표기하는 것을 권장

```javascript
console.log(true ? 1 : 2)	//1
console.log(false ? 1 : 2)	//2

const result = Math.PI > 4 ? 'Yes' : 'No'
console.log(result)	//No
```







## 조건문



### 조건문의 종류와 특징

#### 'if' statement

- 조건 표현식의 결과값을 boolean 타입으로 변환 후 참/거짓 판단
- `if`/ `else if`/ `else`
  - 조건은 소괄호(condition)안에 작성
  - 실행할 코드는 중괄호 안에 작성
  - 블록스코프 생성

```javascript
const nation = 'Korea'

if (nation === 'Korea') {
    console.log('안녕하세요!')
} else if (nation === 'France') {
    console.log('Bonjour!')
} else {
    console.log('Hello!')
}
```





#### 'switch' statement

- 조건 표현식의 결과값이 어느 값(case)에 해당하는지 판별
- 주로 특정 변수의 값에 따라 조건을 분기할 때 활용
- 표현식(expression)의 결과값을 이용한 조건문
- 표현식의 결과값과 case문의 오른쪽 값을 비교
- break 및 default 문은 '선택적'으로 이용 가능
- break문이 없는 경우 break문을 만나거나 default 문을 실행할 때까지 다음 조건문 실행
- 블록 스코프 생성

```javascript
//1. break가 있는 경우

const nation = 'Korea'

switch(nation) {
    case 'Korea': {
        console.log('안녕하세요!')
        break
    }
    case 'France': {
        console.log('Bonjour!')
        break
    }
    default: {
        console.log('Hello!')
    }
}
//'안녕하세요!'를 출력하고 break가 있으니 중단


//2. break 가 없는 경우
const nation = 'Korea'

switch(nation) {
    case 'Korea': {
        console.log('안녕하세요!')
    }
    case 'France': {
        console.log('Bonjour!')
    }
    default: {
        console.log('Hello!')
    }
}
//Fall-through - 하나라도 case에 맞는 경우를 만나면 그 이후의 case문들의 경우에는 값 비교를 하지 않고 실행함. 그래서 이 경우에는 안녕하세요!/Bonjour!/Hello! 모두 출력됨

//3. break가 없는 경우(2)
const nation = 'Korea'

switch(nation) {
    case 'France': {
        console.log('Bonjour!')
    }
    case 'Korea': {
        console.log('안녕하세요!')
    }
    default: {
        console.log('Hello!')
    }
}
//이 경우에는 첫번째 case에서 France가 아니기 때문에 Bonjour는 출력하지 않음. 그 이후에 Korea이므로 안녕하세요!를 출력하고, 그 이후의 것들은 값 비교를 하지 않고 실행. 따라서 이 경우는 안녕하세요!/Hello!가 출력된다.
```





## 반복문

### 반복문의 종류와 특징

- while
- for
- for ... in
  - 주로 객체(object)의 속성들을 순회할 때 사용
  - 배열도 순회 가능하지만 인덱스 순으로 순회한다는 보장이 없기 때문에 권장하지 않음
- for ... of
  - 반복가능한(iterable) 객체를 순회하며 값을 꺼낼 때 사용
  - Array, Map, Set, String 등등..



### while

- 조건문이 참(true)인 동안 반복
- 조건은 소괄호 안에 작성(condition)
- 실행할 코드는 중괄호 안에 작성
- 블록스코프 생성

```javascript
while (condition) {
    //do something
}

//예시
let i = 0

while (i < 6) {
    console.log(i)	//0, 1, 2, 3, 4, 5
    i += 1
}
```



### for

- 세미콜론으로 구분되는 세 부분으로 구성
- `initialization`: 최초 반복문 진입 시 1회만 실행
- `condition`: 매 반복 시행 전 평가되는 부분
- `expression` 매 반복 시행 이후 평가되는 부분
- 블록스코프 생성

```javascript
for (initialization; condition; expression) {
    //do something
}

//예시
for (let i = 0; i < 6; i++) {
    console.log(i)
}
```



### for ... in

- 객체(object)의 속성(key)들을 순회할 때 사용
- 배열도 순회 가능하나 권장 X
- 실행할 코드는 중괄호 안에 작성
- 블록 스코프 생성

```javascript
for (variable in object) {
    //do something
}

//예시
const capitals = {
    korea: 'seoul',
    france: 'paris',
    USA: 'washington D.C.'
}

for (let capital in capitals) {
    console.log(capital)
}
```



### for ... of

- 반복가능한(iterable) 객체를 순회하며 값을 꺼낼 때 사용
- 실행할 코드는 중괄호 안에 작성
- 블록스코프 생성

```javascript
for (variable of iterables) {
    //do something
}

//예시
const fruits = ['딸기', '바나나', '메론']

for (let fruit of fruits) {
    fruit = fruit + '!'
    console.log(fruit)
}

for (const fruit of fruits) {
    //fruit 재할당 불가
    console.log(fruit)
}
```

- `for ... in`은 객체 속성 순회에 사용 가능 / 배열 순회에 사용 가능 but 배열 순회에 사용 권장 X
- `for ... of`는 반복 가능한 객체 순회에 사용 / 객체 속성 순회에는 사용 **불가**





















---

- 변수명 사이에는 언더바를 잘 안쓰나봄

- 재선언 불가능이면 한번 쓴 변수는 삭제하고 새로 선언은 가능하겠지?

- 변수에 값 할당해줄 때도 그렇고, Shift+Enter로 할당하고 다음줄에 `console`로 출력해도 자꾸 `undefined` 따라다니는데 왜 이럼?
- ~~(null) 왜 원시타입에 속하는데 객체로 표현됨?~~
- `null`은 빈값을 표현하기 위한 거라고 하는데, `undefined`도 직접 이걸 할당할 수가 있던데 구분이 필요한 이유가?? 
  - 나중에 `typeof`을 찍어봤을 때 `undefined`라고 나오면 의도적으로 값을 할당하지 않은 건지, 아니면 우연이나 실수로 할당되지 않은건지 구분할 수 없어서?

- `let firstName = undefined` 그러면 이거는 값을 할당한게 아닌거야?

- 그럼 `null`의 `boolean`값도 참이겠네

- == 의 특별한 경우가 뭐임?

- for ... in의 경우 객체 속성을 순회할 때는 순서대로?

- iterable한 객체 속성도 있을까?