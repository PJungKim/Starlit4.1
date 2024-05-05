# GUE Programming Language

GUE, GUE is a Universal Edition
You can also call GUEUE, GUEUEUE, ....., But It will be read `gue`, like the last sound of vague, colleague...
The name GUE is also mean `그`, which is the meaning of `it` in korean language.
The letter `그` is simillar to number 1 in European style.
So It also means of the number 1. This is based on Starlit 1.

## Why I made GUE...?

- 그 프로그래밍 언어는 프로그래밍 언어가 아닌 단순 LED 매트릭스 제어를 위한 언어로 만든 초창기 Starlit을 재현해보기 위해 만들었습니다.
- 초창기 LED 매트릭스를 제어하기 위해 명령어를 늘어놓는 방식으로 LED 작동 방법을 설명하자는 취지로 만들었습니다.
- 이 언어를 베이스로 해서 벌써 Starlit 3.2까지 출시하였고 어느덧 과거의 추억이 잊혀져 갔습니다.
- 추억을 여기에나마 남겨 보기 위해 과거의 스타라잇 코드를 재현해 본 언어입니다.

- 메모리 구조의 잘못된 활용을 했었기 때문에 과거의 프로그래밍 언어를 100% 똑같이 구현하기는 어려웠으며, 현재의 안정화된 Starlit 코드와 연동시켜 작동할 수 있게 재구성해 보았습니다.

## Hello, World?

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

## 버튼 사용하기

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

## 스타라잇 문법의 차용

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
