# 정규 표현식



## 메타 문자

- 원래 그 문자가 가진 뜻이 아닌 특별한 용도로 사용되는 문자를 의미

### 문자 클래스 '[ ]'

- '[ ]' 사이의 하나 이상의 문자와 매치라는 의미
  - 예를 들어서, `[abc]`는 어떤 문자열이 a, b, c 중 하나 이상을 포함하고 있는 경우 '매치'된다고 함
- '[ ]' 사이에 '-(하이픈)'이 있으면 두 문자 사이의 범위를 의미
  - [a-c]는 [abc]와 같고, [0-5]는 [012345]와 같음
  - 모든 알파벳: [a-zA-Z]
  - 모든 숫자: [0-9]
- '[ ]'안에는 어떤 문자나 메타문자도 사용이 가능하나 '^'는 주의해야 함
- '^'는 not을 의미
- 자주 사용하는 별도의 표기법
  - `\d`: [0-9]와 동일
  - `\D`: [ ^0-9 ] 와 동일
  - `\s`: whitespace 문자와 매치. [ `\t\n\r\f\v `]와 동일
  - `\S`: whitespace문자가 아닌 것과 매치. [ ^`\t\n\r\f\v` ]와 동일
  - `\w`: 문자+숫자와 매치. [ `a-zA-Z0-9_`] 와 동일
  - `\W`: 문자+숫자가 아닌 문자와 매치. [ `a-zA-Z0-9_`]와 동일

### Dot(.)

- 줄바꿈 문자인 `\n`을 제외한 모든 문자와 매치됨

- 예시

  - `a.b`: a와 b 사이에 어떤 문자가 들어가도 모든 문자가 매치되나, a와 b 사이에 적어도 하나 이상의 문자는 있어야 함
  - 따라서 `aab`는 매치, `a0b`도 매치, `abc`는 매치X

- 비교

  - `a[.]b`: `a.b`와는 매치, `a0b`와는 매치 X
  - 여기서의 `.`은 문자 `.`그대로를 의미하기 때문

  

### 반복(*)

- `*`바로 앞에 있는 문자가 무한히 반복될 수 있다는 의미. 이때 반복횟수는 0번도 포함됨
- 예시
  - 정규식 `ca*t`는 `ct`, `cat`, `caaat` 등 모두와 매치됨



### 반복(+)

- `+` 바로 앞에 있는 문자가 최소 1번 이상 반복될 때 사용
- 즉 반복(*)의 예시에서 `ct`는 `ca+t`와 매치되지 않음



### 반복({ m, n }, ?)

- `{ }` 메타 문자를 사용하여 반복횟수를 고정할 수 있음
- `{ m, n }`과 같이 지정해주면 반복횟수가 `m`번 이상, `n`번 이하로 고정
- `m`과 `n`은 각각 생략할 수 있으며, 생략된 `m`은 0과, `n`은 무한대와 의미가 같음
  - 즉 `{ 1, }`은 `+`와 같고, `{ 0, }`은 `*`와 같으며 `{ , 3}`은 0번 이상 3회 이하의 반복을 의미함
- 관련 정규식
  - `{ m }`: 반드시 `m`번 반복
  - `?`: `{ 0, 1 }`과 같은 의미로, `?` 앞의 문자가 있어도 되고 없어도 된다는 의미



## 파이썬에서의 정규 표현식

### re 모듈

```python
import re
p = re.compile('ab*')
```

위와 같이 컴파일된 패턴 객체 `p`를 생성할 수 있음. 패턴이란 정규식을 컴파일 한 결과를 의미



### 정규식을 이용한 문자열 검색

- 컴파일된 패턴 객체가 제공하는 메서드

  - `match()`

    - 문자열의 처음부터 정규식과 매치되는지 조사함. 따라서 문자열의 뒷부분에 매치되는 부분이 있더라도 시작부분에서 매치되지 않으면 `None`을 반환
    - 매치된다면 `match` 객체를 돌려줌

    ```python
    import re
    p = re.compile('[a-z]+')
    
    m = p.match('python')
    print(m)	#<re.Match object; span=(0, 6), match='python'>
    
    a = p.match('3python')
    print(a)	#None
    ```

    

  - `search()`

    - 문자열 전체를 대상으로 검색하여 정규식과 매치되는 부분이 있는지를 확인함

    ```python
    import re
    p = re.compile('[a-z]+')
    
    m = p.search('python')
    print(m)	#<re.Match object; span=(0, 6), match='python'>
    
    a = p.search('3python')
    print(a)	#<re.Match object; span=(1, 7), match='python'>
    ```

    

  - `findall()`

    - 정규식과 매치되는 모든 문자열(substring)을 리스트로 돌려줌

    ```python
    import re
    p = re.compile('[a-z' ']+')
    result = p.findall("life is too short")
    print(result)	#['life', 'is', 'too', 'short']
    
    result = p.findall("life is too ashort")
    print(result)	#['life', 'is', 'too', 'ashort']
    
    result = p.findall("life is too 3short")
    print(result)	#['life', 'is', 'too', 'short']
    
    result = p.findall("life is too a3short")
    print(result)	#['life', 'is', 'too', 'a', 'short']
    ```

    

  - `finditer()`

    - `findall()`과 동일하나 결과를 리스트가 아닌 반복 가능한 객체(iterator object)로 돌려줌. 반복 가능한 객체가 포함하는 각각의 요소는 `match` 객체



### `match` 객체의 메서드

- `group()`

  - 매치된 문자열을 돌려줌

- `start()`

  - 매치된 문자열의 시작 위치를 돌려줌

- `end()`

  - 매치된 문자열의 끝 위치를 돌려줌

- `span()`

  - 매치된 문자열의 (시작, 끝)에 해당하는 튜플을 돌려줌

- 예시

  ```python
  m = p.match("python")
  m.group()
  'python'
  m.start()	#0
  m.end()		#6
  m.span()	#(0, 6)
  ```



### 컴파일 옵션

- 아래의 컴파일 옵션 목록에서 괄호안의 문자는 해당 옵션의 약어를 의미

  - `DOTALL(S)`: `.`이 줄바꿈 문자를 포함하여 모든 문자와 매치할 수 있도록 함. 원래 `.`는 줄바꿈 문자를 제외한 모든 문자와 매치했었음

  - `IGNORECASE(I)`: 대소문자에 관계없이 매치할 수 있도록 함

  - `MULTILINE(M)`: 여러 줄과 매치할 수 있도록 함(`^, $`등의 메타문자 사용과 관계가 있는 옵션임)

  - `VERBOSE(X)`: 정규식을 보기 편하게 만들 수 있고, 주석 등을 사용할 수 있는 verbose 모드를 사용할 수 있도록 해줌

- `DOTALL(S)`

  ```python
  import re
  p = re.compile('a.b')
  m = p.match('a\nb')
  print(m)	#None
  
  p = re.compile('a.b', re.DOTALL)	#re.S와 같이 약어로 작성해도 ok
  m = p.match('a\nb')
  print(m)	#<re.Match object; span=(0, 3), match='a\nb'>
  ```

- `IGNORECASE(I)`

  ```python
  p = re.compile('[a-z]+', re.I)
  p.match('python')	#<re.Match object; span=(0, 6), match='python'>
  
  p.match('Python')	#<re.Match object; span=(0, 6), match='Python'>
  
  p.match('PYTHON')	#<re.Match object; span=(0, 6), match='PYTHON'>
  ```

- `MULTILINE(N)`

  - `^`: 문자열의 **처음**이 항상 `^` **다음**에 있는 문자열로 이루어져있어야 함
  - `$`: 문자열의 **마지막**이 항상 `$` **앞**의 문자열로 이루어져 있어야 함

  ```python
  import re
  p = re.compile("^python\s\w+")
  
  data = """python one
  life is too short
  python two
  you need python
  python three"""
  
  print(p.findall(data))	#['python one']
  
  #----------------------------------
  p = re.compile("^python\s\w+", re.MULTILINE)
  
  data = """python one
  life is too short
  python two
  you need python
  python three"""
  
  print(p.findall(data))	#['python one', 'python two', 'python three']
  ```

- `VERBOSE(X)`

  - 이해가 어려운 정규식을 주석 또는 줄 단위로 구분하는 것이 가능해짐

  ```python
  charref = re.compile(r'&[#](0[0-7]+|[0-9]+|x[0-9a-fA-F]+);')
  
  #--------------
  
  charref = re.compile(r"""
   &[#]                # Start of a numeric entity reference
   (
       0[0-7]+         # Octal form
     | [0-9]+          # Decimal form
     | x[0-9a-fA-F]+   # Hexadecimal form
   )
   ;                   # Trailing semicolon
  """, re.VERBOSE)
  ```

  `re.VERBOSE` 옵션을 사용하면 문자열에 사용된 whitespace는 컴파일할 때 제거됨(단 `[ ]`안에 사용한 whitespace는 제외). 그리고 줄 단위로 # 기호를 사용하여 주석문을 작성할 수 있음



### 백슬래시 문제

- 백슬래시가 포함된 문자열을 찾고자할 때 혼란을 줄 수 있음
- 따라서 정규식에 사용한 백슬래시가 문자열 자체임을 나타내기 위해서는 그 앞에 백슬래시를 하나 더붙여야 함
  - 예를 들어 "\section"이라는 문자열을 찾기 위한 정규식을 만들 때
  - 이 식은 `[ \t\n\r\f\v]ection`으로 해석됨
  - 그래서 의도한대로 매치하기 위해서는 `\\section`으로 변경해야 함
    - `p = re.compile('\\section')`
- 그런데 만약 백슬래시가 두개 연속으로 포함된 문자열을 전달하기 위해서는 백슬래시를 4개 써야하는 번거로움이 있음
- 그래서 생겨난 것이 Raw String 규칙
  - 컴파일해야 하는 정규식이 Raw String임을 알려줄 수 있는 문법
    `p = re.compile(r'\\section')`



## 그 밖의 메타 문자







---

# Q.

- `\w`, `\W` 에서 _(언더바)는 뭐임??
- `findall()`은 공백을 기준으로 나눠서 보는건가??