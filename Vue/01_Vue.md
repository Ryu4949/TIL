# Vue.js

[TOC]



## Intro

### Front-End Devlopment

- Vue.js? React?
  - Vue가 분업하기에 쉬움
    - Front-End Developer는 Data를 가져와서 화면에 뿌려주는 역할만할 수 있고, 디자이너는 Design 파일을 제공만 해도 괜찮도록 되어있음
    - 반면 React는 이러한 구조가 좀 섞여있음
    - 그래서 분업이 잘되어있는 국내 회사들은 Vue를 많이 사용하는 경향이 있음

### Vue.js

- 웹 페이지를 동적으로 그리기 위한 프레임워크
- 이전까지는 동적인 웹 페이지를 만들기 위해서는 1. 선택하고 2. 내용을 입력하고 3. 이벤트 등 기타작업의 과정을 거쳐야했는데, 이 과정을 더 편하게 하기 위한 것
- 현대적인 tool과 다양한 라이브러리를 통해 SPA(Single Page Application)를 완벽하게 지원

### SPA(Single Page Application)

- 우리가 보통 여러 페이지를 보는 것 같다고 생각하지만 실제로는 하나의 페이지임. 그리고 그 위에 내용의 일부가 바뀌거나 이벤트가 나타나거나 하는 것
- 이렇게 보고 있는 화면을 바꿔주는 일을 렌더링이라고 함
- 동작
  - 서버로부터 최초에만 페이지를 다운로드
  - 이후에는 동적으로 DOM을 구성 및 변경
- 동작 원리의 일부락 CSR(Client Side Rendering)의 구조를 따름

### CSR(Client Side Rendering)

- 브라우저가 해줌
- 장점
  - SSR의 단점
- 단점
  - SSR의 장점



### SSR(Server Side Rendering)

- 서버에서 클라이언트에게 보여줄 페이지를 모두 구성하여 전달하는 방식
- 장점
  - User가 한번에 페이지를 볼 수 있음
  - SEO(검색 엔진 최적화)에 적합
    - DOM에 이미 모든 데이터가 작성되어있기 때문
- 단점
  - 사용자가 요청할 때마다 새로운 페이지를 구성하여 전달해야 함
    - 반복되는 전체 새로고침으로 인해 UX가 떨어짐
    - 상대적으로 트래픽이 많아서 서버의 부담이 큼
- 장점은 취하면서 단점은 보완하기 위해 최근에는 SSR + CSR을 같이 사용함

### Vue.js의 역할

- MVVM
  - Model
  - View
  - View Model
- 여기서 View Model(VM) 부분을 Vue나 React가 담당하는 것







## Why Vue.js

```javascript
const app = new Vue({
	el: '#app',
    data: {
        username: 'Unknown'
    },
    methods: {
        onInputChange: function (event) {
            this.username = event.target.value
        }
    }
})
```

- `el`은 `element`의 약자로 저렇게만 써야함 외우자.





## Concepts of Vue.js

### MVVM Pattern in Vue.js

![image-20220504132346171](01_Vue.assets/image-20220504132346171.png)







## Vue version 2  vs 3

- Vue 2로 학습하고 **필요하면** Vue 3로 이전하는게 효과적

## Quick Start of Vue.js

### 코드 작성 순서

- "Data가 변화하면 DOM이 변경"

  :one: Data 로직 작성

  :two: DOM 작성



## Basic syntax of Vue.js

### Vue instance

- 모든 Vue 앱은 Vue 함수로 새 인스턴스를 만드는 것부터 시작

- Vue 인스턴스를 생성할 때는 Options 객체를 전달해야함

  ```javascript
  const app = new Vue({
      
  })
  ```

### Options/DOM - 'el'

- Vue 인스턴스에 연결(마운트) 할 기존 DOM 요소가 필요

- CSS 선택자 문자열 혹은 HTML Element로 작성

- `new`를 이용한 인스턴스 생성 때만 사용

  ```javascript
  const app = new Vue({
      el: '#app'
  })
  ```

### Options/Data - 'data'

- Vue 인스턴스의 데이터 객체

- Vue 인스턴스의 상태 데이터를 정의하는 곳

- Vue template에서 interpolation(보간법)을 통해 접근 가능

- v-bind, v-on 등과 같은 directive에서도 사용 가능

- Vue 객체 내 다른 함수에서 `this` 키워드를 통해 접근 가능

  ```javascript
  const app = new Vue({
      el: '#app',
      data: {
          message: 'Hello',
      }
  })
  ```

### Options/Data - 'methods'

- Vue 인스턴스에 추가할 메서드

- Vue template에서 interpolation을 통해 접근 가능

- v-on과 같은 directive에서도 사용 가능

- Vue 객체 내 다른 함수에서 this 키워드를 통해 접근 가능

- 주의점

  - 화살표 함수를 메서드를 정의하는데 사용하면 안됨
  - 화살표 함수가 부모 컨텍스트를 바인딩하기 때문에, `this`는 Vue 인스턴스가 아님

  ```javascript
  const app = new Vue({
      el: '#app',
      data: {
          message: 'Hello',
      },
      methods: {
          greeting: function () {
              console.log('hello')
          }
      }
  })
  ```

### 'this' keyword in vue.js

- Vue 함수 객체 내에서 vue 인스턴스를 가리킴
- 화살표 함수를 사용하면 안 되는 경우
  - data
  - method 정의





## Template Syntax

### Template Syntax

- 렌더링 된 DOM을 기본 Vue 인스턴스의 데이터에 선언적으로 바인딩할 수 있는 HTML 기반 템플릿 구문을 사용
  - Interpolation
  - Directive

### Interpolation

- Text
- Raw HTML
- Attributes
- JS 표현식

### Directive



### v-text

- `{{ message }}`랑 걍 똑같음
  - `<p v-text="message"></p>`
  - `<p>{{ message }}</p>` 위에거랑 똑같은거

### Directive

- `v-` 붙어있는거 다 디렉티브라고 함
  - `Vue`한테 뭔가를 시키는 애들

### v-html

- 거의 안씀

### v-bind

- `:`가 약어







---

- new를 이용하지 않는 인스턴스 생성 방법은 무엇일까(56p)
- '바인딩'에 대해서.
- methods는 v-on에는 되고 v-bind에는 안되나봄?
- data나 method나 directive에서 사용하는 경우 찾아보자.