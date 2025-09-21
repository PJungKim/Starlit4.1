# Starlit 4 고급 설명서

- Starlit 4의 고급 기능을 정리했습니다.

*현재 제공되는 파일의 기능 중 #으로 시작하는 주석은 개발 단계에 있어 제공되지 않는점 양해 바랍니다.*

## 1. 자료형과 Reference

- Starlit 4에서는 Pointer라는 개념을 사용하지 않는다.
- Starlit에서는 Pointer 대신 Reference라는 개념을 사용하며, 이 Reference를 사용해 변수를 제어할 수 있다.

### 1.1. 변수의 레퍼런스값 구하는 코드

- 기본 문법 : `_(변수명)`

```Py
a = 1023
b = _a
```

- 여기서 b는  a의 레퍼런스를 저장하게 된다.
- 이때 b의 자료형은 `ref`형이 된다.
- 레퍼런스 값을 확인하기 위해서는 `val` 함수를 사용해보자.
- 화면에 표시할 때는 8자리의 16진수로 표시한다. 그렇지 않으면 자료 구조 특성상 4의 배수에 위치해 있는지 판단하기 어렵기 때문이다.

```Py
main:
    a = 1023
    b = _a
    c = val(b)
    OLED << f"ref = {c:08x}"
```

```C++
main(){
    a = 1023;
    b = _a;
    c = val(b);
    OLED << f"ref = {c:08x}";
}
```

### 1.2. 특정 위치의 변수 값 바꾸기

- Starlit에서는 특정 위치의 변수 값을 바꿀 수 있다.
- 아래 예제는 int 변수에 float값을 넣으면 어떤 값을 얻을 수 있을지 확인하는 예제이다.
  - `float(레퍼런스) = 값`의 형태로 특정 레퍼런스의 값을 변경할 수 있다.
```Py
main:
    a = 20
    b = _a
    float(b) = 1.5
    OLED << f"a = {a:08x}"
```
```C++
main(){
    a = 20;
    b = _a;
    float(b) = 1.5;
    OLED << f"a = {a:08x}";
}
```

### 1.3. 레퍼런스 변수

- 이번에는 값을 저장하는 것이 아닌 특정 레퍼런스를 참조하는 변수를 다뤄 본다.
- 레퍼런스 변수를 정의하기 전에 참조할 변수를 선정한 뒤 레퍼런스 변수를 통해 값을 바꿔보는 예제이다.
  - 레퍼런스 변수의 정의 : `b_ = _a`로, 좌변에는 새로 정의하는 변수+`_`, 우변은 참조되는 변수의 레퍼런스형인 `_a`꼴로 정의한다.
  - 레퍼런스 변수의 레퍼런스 값 : `_b_`
  - 레퍼런스 변수의 참조 위치 값 : `b`
  - `b_`를 정의하면 `_b`는 사용 불가능하다.
```Py
main:
    a = 20
    b_ = _a
    b = 1023
    OLED << f"a = {a}"
```
```C++
main(){
    a = 20;
    b_ = _a;
    b = 1023;
    OLED << f"a = {a}";
}
```

### 1.4. Call-by-Reference 함수

- 이번 예제는 Reference 값을 불러와서 두 정수의 값을 바꾸는 예제를 들어 본다.
  - 함수 정의할 때 `a = int`는 Call-by-value이고, `a = _int`는 Call-by-reference가 된다.
```Py
swap(a:_int, b:_int):
    temp = a
    a = b
    b = temp

main:
    a = 20
    b = 1023
    swap(a, b)
    OLED << f"a = {a}, b = {b}"
```
```C++
swap(a:_int, b:_int){
    temp = a;
    a = b;
    b = temp;
}

main(){
    a = 20;
    b = 1023;
    swap(a, b);
    OLED << f"a = {a}, b = {b}";
}
```

### 1.5. Return-by-Reference 함수

- 값을 반환할 때 레퍼런스를 반환하는 함수
- 함수를 정의할 때 `func():_int`꼴로 정의한다.
- **출력할 때는 값을 출력하는 것이 아닌 레퍼런스를 출력하는 것이므로 return의 자료형은 `ref`형이 되어야 한다.**

## 2. 배열과 반복 연산자

- 2장에서는 배열을 알아보고, 배열을 빠르게 연산할 수 있는 반복 연산자를 소개해 본다.

### 2.1. 배열의 정의

- 배열은 동일한 형식의 객체를 묶은 것을 의미한다.
- Python처럼 여러 자료형을 배열로 정의하기 어려우며, 같은 자료형의 데이터를 처리한다.
  - C언어도 비슷한 형태로 배열의 구성요소는 모두 동일한 형태로, 자료형에 엄격한 언어들은 대부분 이렇다.
  - Python처럼 a = [1, 2, 3]꼴로 배열을 정의할 수 있지만, 크기 변환이 불가능하고, 자료형이 첫번째 값 기준으로 모두 형변환된다.
- 배열을 선언만 하는 방법 : `a = int[20]`과 같은 형태로 변수 정의하듯이 정의하되, 뒤에 크기를 명시하면 된다.
  - 한 번 크기가 결정되면 크기를 바꾸지 못하기 때문에 크기를 바꿔야 한다면 넉넉한 크기로 정의한 다음 사용하자.
- 선언과 동시에 초기화 : `a = [0, 1, 2, 20]`과 같이 정의하면 크기가 4로 고정된다. C언어에서는 int a[] = {0, 1, 2, 20}으로 정의한 것과 같다.


### 2.2. 배열의 사용

- 배열의 출력
  - OLED에 배열을 출력할 때는 아래와 같이 출력하면 된다.
  ```Py
  for i:0:(length(a)-1):
      OLED << f"[{a[i]}]"
  ```
  ```C++
  for i:0:(length(a)-1){
      OLED << f"[{a[i]}]";
  }
  ```
  - `length(a)` : 배열 a의 크기를 의미하며, sizeof와 다르게 배열 성분의 개수를 구할 때 사용한다.
  - `a[i]` : 배열 a의 i번째 성분을 의미한다.
  - `for i:0:(length(a)-1):` : 0부터 배열의 크기보다 1 작은 값까지 반복
  - `OLED << f"[{a[i]}]"` : a의 값을 출력.
- 배열에 값 대입하기
  - 배열에 값을 대입하는 것은 변수에 값을 대입하는 것처럼 하면 된다.
  ```Py
  a[2] = 1023 # 배열 a의 2번째 성분에 1023을 대입한다.
  ```
- 배열에 값 한꺼번에 대입하기
  - 배열의 특정 영역에 값을 한번에 대입하는 것은 아래와 같이 할 수 있다.
    - 이때, 좌변에 적은 숫자는 시작 인덱스이고, 그 지점부터 순차적으로 대입이 진행된다.
  ```Py
  a[2] = [1023, 1] # 배열 a의 2번째 성분에 1023을 대입, 3번째 성분에 1 대입.
  ```
  - 특히 0번째 지점부터 대입한다면 `[2]`는 빼도 상관없다.
  ```Py
  a = [1023, 1] # 배열 a의 0번째 성분에 1023을 대입, 1번째 성분에 1 대입.
  ```
  - 단독으로 대입한다면 우변의 대괄호를 빼도 되기 때문에, a[0]과 a는 완전히 동일하다는 정리가 성립한다.
    - 이게 C언어와 Starlit 언어의 배열을 보는 관점의 결정적인 차이가 된다.
    - C언어에서 a가 배열이면 a의 실제 자료형은 포인터형이 되지만, Starlit은 a가 포인터가 아닌 값을 가리키는 자료형이 된다.
  ```Py
  a = 1023 # a가 배열이므로, 이 연산을 수행하면 0번째 성분만 값이 바뀐다.
  ```
- Starlit 언어의 취약점
  - Starlit은 변수와 배열을 굳이 구분하지 않는 특징이 있다. 따라서 a와 b를 연달아 정의했으면 a[1]과 b[0]은 동일한 레퍼런스 값을 가진다. 따라서 a[1]의 값을 바꾸면 b값이 바뀐다.
  ```Py
  main:
      a = 20
      b = 1023
      a[1] = 1
      OLED << f"b={b}"
  ```
  ```C++
  main(){
      a = 20;
      b = 1023;
      a[1] = 1;
      OLED << f"b={b}";
  }
  ```
  - 따라서 배열을 연산할 때 원하지 않는 변수의 값이 바뀌는 것을 막기 위해서는 배열에 들어가는 값이 올바른 범위에 들어가는지 확인해야 한다.

### 2.3. 반복 연산자

- 반복 연산자 `:`
  - `:` 연산자는 시작점부터 끝점까지 1씩 증가하며 반복 수행함을 의미한다.
  - 한 문장 내 `:` 연산자의 개수가 반복의 차원이다. 따라서 `:` 연산자를 지나치게 남발하면 코드 실행이 기하급수적으로 오래 걸린다.
  - 등호 `=` 기준으로 좌/우변에서 `:`를 사용한 경우에는 좌/우변이 따로 반복되지 않는다. 그 이유는 대입을 여러번 반복하는 것 자체가 별 의미가 없는 동작이기 때문이다. 따라서 a[0:2] = b[0:2]와 같이 연산하면 a[0]은 b[0]의 값이, a[1]은 b[1]의 값이 대입된다.
  - 반복 연산자는 if, while, for문 등에 중첩해서 사용할 수 없으며, f-문자열 안에서 반복 연산자 사용이 불가능하니 유의해야 한다.
  - 반복 연산자는 괄호를 제외한 그 어떤 연산자보다 우선순위가 높다.
- 반복변수 : 반복 연산자는 일반적으로 3항으로 사용하며, `i:0:3`과 같이 사용하면 i값이 0~3으로 변할 때 실행하라는 의미이다. 이때, i는 정의되지 않은 변수로, 이 문장에서만 사용하고 사라지는 **문장 단위 지역변수**임에 유의하자.
- 반복 연산자를 사용한 값의 대입
  - 4개짜리 배열에 0, 3, 6, 9 대입하기 
    ```Py
    main:
        a = int[4]
        a[i:0:3] = i * 3 # 반복변수 i값에 대해서 0부터 3까지 반복하라는 의미
        for i:0:(length(a)-1):
            OLED << f"[{a[i]}]"
    ```
    ```C++
    main(){
        a = int[4];
        a[i:0:3] = i * 3; /// 반복변수 i값에 대해서 0부터 3까지 반복하라는 의미
        for(i:0:(length(a)-1)){
            OLED << f"[{a[i]}]";
        }
    }
    ```

### 2.4. Reference 변수와 배열

- Call-by-reference를 사용하면 배열을 대입하는 함수를 만들 수 있다.
- 아래 예제는 배열에 저장된 모든 값의 합을 구하는 코드이다.
```Py
Sum(a:_int, siz:int):int
    sum = 0
    for i:0:siz-1:
        sum += a[i]
    return sum

main
    a = [20, 1023, 1, 67]
    sum = Sum(a, length(a))
    OLED << f"sum = {sum}"
```
```C++
Sum(a:_int, siz:int):int{
    sum = 0;
    for i:0:siz-1{
        sum += a[i];
    }
    return sum;
}

main(){
    a = [20, 1023, 1, 67];
    sum = Sum(a, length(a));
    OLED << f"sum = {sum}";
}
```

### 2.5. 고차원 배열

- Starlit도 고차원 배열이 존재한다.
- 2차원 배열 정의 및 출력 예제
```Py
main:
    a = int[3, 3]
    a = [1023, 1203, 1302,
         1032, 1230, 1320,
         2013, 2103, 20]
    for r:0:2:
        for c:0:2:
            OLED << f"{a[r, c]} "
```
```C++
main(){
    a = int[3, 3];
    a = [1023, 1203, 1302,
         1032, 1230, 1320,
         2013, 2103, 20];
    for(r:0:2){
        for(c:0:2){
            OLED << f"{a[r, c]} ";
        }
    }
}
```
- 배열을 정의할 때는 행 개수 -> 열 개수 순으로 정의하면 된다.
- 대입할 때는 순서대로 대입하면 된다.

### 2.6. 배열의 레퍼런스값 구하기

- 배열의 레퍼런스값은 _를 함수처럼 써서 구하면 된다.
```Py
main
    a = [20, 1023, 1, 67]
    b = _(a[2])
    c = val(b)
    OLED << f"{c:08x}"
```
```C++
main(){
    a = [20, 1023, 1, 67];
    b = _(a[2]);
    c = val(b);
    OLED << f"{c:08x}";
}
```
- 배열의 레퍼런스값을 알고 있으면 아래와 같이 특정 위치의 값을 바꿀 수 있다.
```Py
main
    a = [20, 1023, 1, 67]
    b = _(a[1])
    int(b)[1] = 0 # a[1] 기준으로 한 칸 뒤의 값인 a[2]가 0이 됨.
    OLED << f"{a[2]}"
```
```C++
main(){
    a = [20, 1023, 1, 67];
    b = _(a[1]);
    int(b)[1] = 0; /// a[1] 기준으로 한 칸 뒤의 값인 a[2]가 0이 됨.
    OLED << f"{a[2]}";
}
```



## 3. 구조체와 Class

- 구조체는 여러 자료형을 일정 규칙에 맞게 묶어 둔 새로운 자료형을 의미한다.
- 구조체의 예
  - 2차원 벡터 : `int`-`int` 또는 `float`-`float`
  - 커피와 가격 : `str`-`int`

### 3.1. Class의 정의
- 아래와 같이 Class를 정의할 수 있다. main 함수의 내용은 출력 예제이다.
```Py
class vectInt:
    x:int
    y:int

main
    v = vectInt
    v.x = 20
    v.y = 1023
    OLED << f"[{v.x}, {v.y}]"
```
```C++
$class(vectInt){
    x:int;
    y:int;
}

main(){
    v = vectInt;
    v.x = 20;
    v.y = 1023;
    OLED << f"[{v.x}, {v.y}]";
}
```
- `class 구조체명`의 형태로 정의한다.
- class의 성분을 적는다. 이때, 자료형을 뒤에 명시해 놓는다.


### 3.2. Class의 사용

- 구조체 대입하기
  - 앞에서는 각각 성분에 일일이 대입했지만, 그런거 없이 간단하게 대입하는 방법도 있다.
```Py
class vectInt:
    x:int
    y:int

main
    v = vectInt(20, 1023)
    OLED << f"[{v.x}, {v.y}]"
```
```C++
$class(vectInt){
    x:int;
    y:int;
}

main(){
    v = vectInt(20, 1023);
    OLED << f"[{v.x}, {v.y}]";
}
```

### 3.3. 구조체를 변수로 받는 함수

- Call-by-value : Vector 각 성분의 합
```Py
class vectInt:
    x:int
    y:int

Sum(vect:vectInt):int:
    return vect.x + vect.y

main:
    v = vectInt(20, 1023)
    s = Sum(v)
    OLED << f"sum = {s}"
```
```C++
$class(vectInt){
    x:int;
    y:int;
}

Sum(vect:vectInt):int{
    return vect.x + vect.y;
}

main(){
    v = vectInt(20, 1023);
    s = Sum(v);
    OLED << f"sum = {s}";
}
```
- Call-by-reference : Vector 성분 뒤집기
```Py
class vectInt:
    x:int
    y:int

Swap(vect:_vectInt):
    temp = vect.x
    vect.x = vect.y
    vect.y = temp

main:
    v = vectInt(20, 1023)
    Swap(v)
    OLED << f"[{v.x}, {v.y}]"
```
```C++
$class(vectInt){
    x:int;
    y:int;
}

Swap(vect:_vectInt){
    temp = vect.x;
    vect.x = vect.y;
    vect.y = temp;
}

main(){
    v = vectInt(20, 1023);
    Swap(v);
    OLED << f"[{v.x}, {v.y}]";
}
```




## 4. 객체의 Method와 연산자 오버로딩

- 앞에서 다룬 구조체를 확장해서 객체로 보고, Method라고 하는 기능을 부여해 보기로 한다.

### 4.1. Method 정의하기

- 객체 프린트 예제
  - `(vectInt).Print()`를 사용하면 벡터를 출력하는 예제이다.
  - `self`는 class 안에서 객체 자기 자신을 의미한다(reference 변수로 취급).
```Py
class vectInt:
    x:int
    y:int

    Print():
        OLED << f"[{self.x}, {self.y}]"

main:
    v = vectInt(20, 1023)
    v.Print()
```
```C++
$class(vectInt){
    x:int;
    y:int;

    Print(){
        OLED << f"[{self.x}, {self.y}]";
    }
}

main(){
    v = vectInt(20, 1023);
    v.Print();
}
```



### 4.2. 연산자 오버로딩

- 주요 연산자 오버로딩
  |연산자|인수 개수|오버로딩 함수|기본 기능|
  |--|--|--|--|
  |+|2|$add|두 수의 합<br>두 문자열 이어붙이기|
  |-|2|$sub|두 수의 차|
  |*|2|$mul|두 수의 곱|
  |/|2|$div|두 수의 몫|
  |%|2|$mod|두 수를 나눈 나머지|
  |+|1|$plus|양의 부호|
  |-|1|$minus|음의 부호|

- 벡터의 합 예제
```Py
class vectInt:
    x:int
    y:int

    Print():
        OLED << f"[{self.x}, {self.y}]"

$add(v1:vectInt, v2:vectInt):vectInt:
    vsum = vectInt
    vsum.x = v1.x + v2.x
    vsum.y = v1.y + v2.y
    return vsum

main:
    v1 = vectInt(1, 0)
    v2 = vectInt(2, 3)
    vs = v1 + v2
    vs.Print()
```
```C++
$class(vectInt){
    x:int;
    y:int;

    Print(){
        OLED << f"[{self.x}, {self.y}]";
    }
}

$add(v1:vectInt, v2:vectInt):vectInt{
    vsum = vectInt;
    vsum.x = v1.x + v2.x;
    vsum.y = v1.y + v2.y;
    return vsum;
}

main(){
    v1 = vectInt(1, 0);
    v2 = vectInt(2, 3);
    vs = v1 + v2;
    vs.Print();
}
```




