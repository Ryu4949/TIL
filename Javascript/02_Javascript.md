# Javascript_02

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
</head>
<body>
  <h1>Hello!</h1>
  <script src="01_js.js"></script>
</body>
</html>
```

```javascript
console.log('hola!');
```

- html 파일에서 `open with live server`
- 브라우저에서 콘솔창 열고 vscode에 입력하면 콘솔창에 실시간 반영되는 것을 볼 수 있다.







- reduce

  - ```javascript
    cons = myArr = [ 1,2 ,3 ,4, 5]
    myArr.reduce( () => {}, init)
    myArr.reduce (acc, elem, idox, x =32, 15)
    
    const rlt = myrr.reduce((acc, elem, idx, arr)) => {
        console.log(acc)
        return elem = 2 + acc
    }, 0)
    ```

  - console.log(rlt)



- find

  - filter와 유사
  - find는 filter와 달리 조건이 참이되게 하는 값을 찾면 그것만 찾고 끝남

  ```javascript
  const myArr = [1, 2, 3, 4, 5]
  
  const rlt = myArr.find(elem, idx, arr) => {
      return elem > 3
  }
  ```

  위의 경우 4를 만나면 딱 끝남



- some
  - 하나라도 true면 true를 반환





## 함수

- 정의 방법은 함수 선언식(function declartion)과 함수 표현식(function expression) 2가지

- 함수는 일급 객체(First-class citizen)에 해당하며 

  - 변수에 할당 가능
  - 함수의 매개변수로 전달 가능
  - 함수의 반환 값으로 사용 가능

- 이상 세 가지의 특징을 가진다.

- 함수는 '값'이다

  - '값': 1, 문자열, 배열 등등등.. 여기에 함수도 포함된다는 뜻

- 함수가 '값'이라는 건..

  ```javascript
  const myNum = 1
  const myStr = 'aiden'
  const myArr = [1, 2, 3]
  const myObj = ['a': 1]
  const myFunc = function test() {}
  ```

  - 리스트는 index로 접근이 가능하다는 특징이 있는 값
  - 함수는 소괄호를 통해서 실행할 수 있다는 특징이 있는 값
  - 이 특징을 제외하고는 다른게 없다는 것이다.

### 함수 선언식(function statement, declaration)

- 함수의 이름과 함께 정의하는 방식

- 함수의 이름 + 매개변수(args) + 몸통(중괄호 내부)으로 구성

  ```javascript
  function add(num1, num2) {
      return num1 + num2
  }
  
  add(1, 2)
  ```



### 함수 표현식(function expression)

- 함수를 표현식 내에서 정의하는 방식

- 함수의 이름 생략하여 익명함수로 정의 가능

- 정의한 함수를 별도의 변수에 할당하는 것

- 함수이름(생략가능) + 매개변수 + 몸통

  ```javascript
  const add = function (num1, num2) {
      return num1 + num2
  }
  add (1, 2)
  ```



### 기본 인자(default arguments)

- 인자 작성시 '=' 뒤에 기본인자 선언 가능

  ```javascript 
  const greeting = function (name = 'Anonymous') {
      return `Hi ${name}`
  }
  greeting() //Hi Anonymous
  ```



### 매개변수와 인자의 개수 불일치 허용

#### 매개변수보다 인자의 개수가 많은 경우

- 에러 발생하지 않고, 초과한 개수의 인자는 자연스레 무시됨

```javascript
const noArgs = function () {
    return 0
}
noArgs(1, 2, 3)	//0

const twoArgs = function (arg1, arg2) {
    return [arg1, arg2]
}
twoArgs(1, 2, 3)	//[1, 2]
```

#### 매개변수보다 인자의 개수가 적을 경우

- 마찬가지로 에러가 발생하지 않고, 부족한 부분은 `undefined`가 됨

  ```javascript
  const threeArgs = function (arg1, arg2, arg3) {
      return [arg1, arg2, arg3]
  }
  threeArgs()	//[undefined, undefined, undefined]
  threeArgs(1)	//[1, undefined, undefined]
  threeArgs(1, 2)	//[1, 2, undefined]
  ```

  

### Rest Parameter

- rest parameter(...)을 사용하면 정해지지 않은 수의 매개변수를 배열로 받을 수 있음

- 만약 rest parameter로 처리한 매개변수에 인자가 넘어오지 않으면 빈배열로 처리

  ```javascript
  const restOpr = function (arg1, arg2, ...restArgs) {
      return [arg1, arg2, restArgs]
  }
  restArgs(1, 2, 3, 4, 5)	// [1, 2, [3, 4, 5]]
  restArgs(1, 2)	// [1, 2, []]
  ```



### Spread Operator

- 배열 인자를 전개하여 전달할 때 사용

```javascript
const spreadOpr = function (arg1, arg2, arg3) {
    return arg1 + arg2 + arg3
}
const numbers = [1, 2, 3]
spreadOpr(...numbers)	//6
```



## 선언식 vs 표현식

### 함수의 타입

- 선언식과 표현식 모두 `function`



### 호이스팅(hoisting)

#### 선언식

- hoisting 발생

```javascript
add(2, 7)	//9
function add (num1, num2) {
    return num1 + num2
}
```

#### 표현식

- hosting 발생하지 않음. 위와 같은 경우 에러 발생
- 함수 표현식으로 정의된 함수는 변수로 평가되어 변수의 scope 규칙을 따름

```javascript
sub(7, 2)
var sub = function (num1, num2) {
    return num1 - num2
}
//TypeError: sub is not a function

sub(7, 2)
const sub = function(num1, num2) {
    return num1 - num2
}
//ReferenceError: sub is not defined
```

에러메시지에서 약간 차이는 있으나, 일반 변수에서 hoisting이 발생했던 `var`의 경우에도 함수 표현식에서는 hoisting이 발생하지 않는다.





## Arrow Function

### 화살표 함수

- 함수를 간결하게 정의할 수 있는 문법
- `function` 키워드 생략 가능
- 함수의 매개변수가 하나라면 '( )'도 생략 가능
- 함수 몸통이 표현식 하나라면 '{ }'과 `return`도 생략 가능
- `function`키워드를 사용할 때와 거의 차이가 없으나 `this` 키워드를 사용할 때 차이가 발생하는데 이에 대해서는 후술

```javascript
const arrow1 = function (name) {
    return `${name}님 안녕하세요?`
}

const arrow2 = (name) => {
    return `${name}님 안녕하세요?`
}

const arrow3 = name => {
    return `${name}님 안녕하세요`
}

const arrow4 = name => `${name}님 안녕하세요?`
```



## 문자열(String)

### includes

- `string.includes(value)`
- 문자열에 `value`가 존재하는지 판별 후 참/거짓 반환

```javascript
const str = 'a santa at nasa'

str.includes('santa')	//true
str.includes('asan')	//false
```



### split

- `string.split(value)`
- `value`에 값이 없는 경우 기존 문자열을 배열에 담아 반환
- 빈 문자열일 경우('') 각 문자로 나눈 배열을 반환(공백도)
- 기타 문자열일 경우, 해당 문자열로 나눈 배열을 반환

```javascript
const str = 'a cup'
str.split()	//['a cup']
str.split('')	//['a', ' ', 'c', 'u', 'p']
str.split(' ')	//['a', 'cup']
```



### replace

- `string.replace(from, to)`
  - 문자열에 `from`값이 있는 경우 1개만 `to`값으로 교체하여 반환
- `string.replaceAll(from, to)`
  - 문자열에 `from`값이 있는 경우 모두 `to` 값으로 교체하여 반환

```javascript
const str = 'a b c d'
str.replace(' ', '-')	//'a-b c d'
str.replaceAll(' ', '-')	//'a-b-c-d'
//----------------------------------------------
const str = 'abcd'
str.replace('', 'x')	//'xabcd'
str.replaceAll('', 'x')	//'xaxbxcxdx'
```



### trim

- `string.trim()`
  - 문자열 시작과 끝의 모든 공백문자를 제거하여 반환
- `string.trimStart()`
  - 문자열 시작의 공백문자를 제거한 문자열 반환(문자열 기준 왼쪽의 공백제거)
- `string.trimEnd()`
  - 문자열 끝의 공백문자 제거하여 반환

```javascript
const str = '           hello         '
str.trim()	//'hello'
str.trimStart()	//'hello      '
str.trimEnd()	//'      hello'
```





## 배열(Arrays)











- 함수를 어디에서 '호출'했는지가 중요한게 아니고, 어디서 '선언' 했는지가 중요하다.

  ```javascript
  const x = 1
  
  function foo() {
      const x = 10
      bar()
  }
  
  function bar() {
      console.log(x)
  }
  
  foo()
  bar()
  
  //1
  //1
  ```

  - 참고) Lexical Scope



---

## 객체



### 객체의 정의와 특징





### 객체와 메서드

- 객체명에 따옴표로 묶어도 되고 벗겨도 되는데, 만약 객체명 사이사이에 띄어쓰기가 있다면 무조건 따옴표가 있어야 함

- 객체에서 바로쓰는 `this`는 객체를 의미하지 않음. `window`를 의미함

  ```javascript
  const obj = {
      name: 'aiden',
      test: this
  }
  
  obj.test
  //찍어보자!
  ```





## lodash

- 라이브러리

- array나 object 등 자료구조를 다룰 때 사용하는 유용하고 간편한 함수 제공

- `sortBy`, `random`, `range`, `cloneDeep` 등을 특히 많이 사용함

  - `cloneDeep`이 파이썬의 `deepcopy`에 해당

- 코드를 가져와야 사용 가능

  - https://lodash.com/docs/4.17.15

    ```html
    <script src="https://cdn.jsdelivr.net/npm/lodash@4.17.21/lodash.min.js"></script>
    ```

    