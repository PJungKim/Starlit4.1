# GUE Programming Language

## 1. 소개

- GUE는 Starlit 언어의 초창기 시절을 재현하면서도 완성도 있는 코드를 구현할 수 있게 만든 프로그래밍 언어입니다.

### 1.1. GUE 상징

<img src = "../res/GUE/GUELogo.png">

- 분홍색과 보라색은 Starlit 언어의 고유한 색상인 보라색 계통을 의미하며, Starlit 언어 계통의 언어임을 의미합니다.
- 숫자 1 모양은 숫자 1을 의미하기도 하면서 프로그래밍 언어 GUE를 한글 `그`로 표현한 것입니다.
- 숫자 1은 초창기의 Starlit 언어의 형태를 의미합니다.
- GUE는 GUE is a Universal Edition을 의미합니다. 초창기의 언어를 완성도 있게 재구성했음을 의미합니다.
- 이름을 그라고 붙인 이유는 사실 `그 언어`라고 부르면 더욱 친숙해지리라 생각했던 점도 없진 않습니다.

#### 1.1.1. GUE 로고 3색상

- 원본

  <img src = "../res/GUE/GUELogo.png" width = "30%">

- 분홍 계열

  <img src = "../res/GUE/GUELogo2.png" width = "30%">

- 보라 계열

  <img src = "../res/GUE/GUELogo3.png" width = "30%">

### 1.2. Why I made GUE...?

- 그 프로그래밍 언어는 프로그래밍 언어가 아닌 단순 LED 매트릭스 제어를 위한 언어로 만든 초창기 Starlit을 재현해보기 위해 만들었습니다.
- 초창기 LED 매트릭스를 제어하기 위해 명령어를 늘어놓는 방식으로 LED 작동 방법을 설명하자는 취지로 만들었습니다.
- 이 언어를 베이스로 해서 벌써 Starlit 3.2까지 출시하였고 어느덧 과거의 추억이 잊혀져 갔습니다.
- 추억을 여기에나마 남겨 보기 위해 과거의 스타라잇 코드를 재현해 본 언어입니다.
- 메모리 구조의 잘못된 활용을 했었기 때문에 과거의 프로그래밍 언어를 100% 똑같이 구현하기는 어려웠으며, 현재의 안정화된 Starlit 코드와 연동시켜 작동할 수 있게 재구성해 보았습니다.

## 2. 예제 코드

### 2.1. Hello, World?

- 초창기 Starlit 언어의 Hello World는 시리얼 통신을 사용해 아두이노의 시리얼 포트에 출력했습니다.
  ```
  start>>
  Print "Hello,_World!\n"
  end 
  ```
  
- Starlit 3.2와의 연동을 위해 OLED 객체에 출력하는 코드입니다.
  ```
  start>>
  Print OLED "Hello, World!\n"
  end 
  ```

- 현재의 GUE 언어는 띄어쓰기 이슈가 해결되었기 때문에 띄어쓰기를 `_`로 사용하지는 않습니다.

### 2.2. 버튼 사용하기

- 버튼을 눌러 OLED에 숫자 출력하는 예제
  ```
  start>>
  while 1
      BUTTON_Read _s
      if s==BUTTON_1
          Print OLED "1"
      elif s==BUTTON_2
          Print OLED "2"
  end 
  ```

#### 2.2.1. 스타라잇 문법의 차용

- Starlit 1 시절때와는 다르게 GUE 언어는 Starlit 3.2의 문법도 활용 가능합니다.
  ```
  while 1
      s = BUTTON_Read()
      if s==BUTTON_1
          ....
  ```
- 당시에는 대입문 자체가 전혀 다른 모양이었습니다. 함수 역시 GUE형, Starlit형 모두 사용 가능합니다.
  - GUE 방식: 함수명 매개1 매개2 ...
  - Starlit 방식: 함수명(매개1, 매개2, ...)
 
## 3. Starlit 1과의 비교

### 3.1. 전체적인 언어의 구조

  ```
  start>>
  Print OLED "Hello, World!\n"
  end 
  ```

  - GUE 언어는 `start>>`에서 시작해서 `end `로 끝납니다.
    - Starlit 1 시절에는 `start>>`에서 시작하면 그 지점부터 코드 영역이었고, 코드가 끝나는 지점에서는 `end `를 붙였습니다. 당시에는 `end` 끝에 반드시 띄어쓰기를 한 번 더 해야 했습니다. 하지만, 현재의 GUE 언어는 띄어쓰기의 유무에 상관없이 모두 정상 작동합니다.
    - Starlit 1 시절에는 LED Matrix 작동 스크립트가 주 목적이었기 때문에 `start>>`는 시작을 의미하는 명령어였고, `end `는 스크립트를 끝내라는 명령어였습니다. 하지만, 현재는 이 코드가 프로그래밍 언어화하여 `start>>` 자체가 함수를 의미합니다.
    - Starlit 1은 함수 개념이 전혀 없었다가 Starlit 2에서 제대로 된 함수가 도입되었고, Starlit 3.2에 이르러서 입/출력이 가능한 함수를 사용할 수 있습니다. GUE 언어는 제대로 된 함수가 정의되어 있는 Starlit 3.2 기반이기 때문에 Starlit 1 시절과는 다르게 함수가 제대로 갖추어진 형태로 쓸 수 있습니다.

  - GUE 언어는 `Print` 한방에 OLED에 출력할 수 있습니다.
    - Starlit 1은 이미지 저장공간, 변수 저장공간이 정해져 있어서 LED Matrix에 출력하기 위해서 상당히 많은 절차를 밟아 문자열을 출력했습니다.
    - GUE 언어는 모양만 Starlit 1의 형태를 따온 언어이기 때문에 OLED에 단 한줄로 문자열을 출력할 수 있습니다.
   

    
