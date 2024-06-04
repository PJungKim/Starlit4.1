# Design Your Style, Starlit

## The Programming Language For Embedded Devices
- Starlit은 임베디드의 Flash Memory(MicroSD카드 또는 자체 Flash Memory 등)에 바이너리 저장해서 임베디드 내의 Virtual Machine을 실행해 작동하는 프로그래밍 언어입니다. 임베디드를 더욱 효율적으로 디자인할 수 있습니다.

## News
<img src="res/GUE/GUELogo.png">

- `그(GUE)` 프로그래밍 언어가 추가되었습니다.
- Starlit의 초창기 시절을 재구성하여 만들었습니다.
- 로고는 한글 `그`와 동시에 Starlit 언어의 초창기를 의미하는 숫자 `1`을 상징합니다.

Starlit 3.3버전 업데이트 소식

- Starlit 언어
  - Return-By-Reference 기능 사용 시 대입이 안되던 버그 수정
  - 더욱 편해진 배열 대입 기능
  - 클래스 멤버변수로 함수 참조 및 실행 가능
  - 지역할당문(Assign) 추가
  - 그 외 민감한 오류 수정
    - 보고된 오류 1 : Call-By-Reference를 클래스 메소드에 적용하면 모든 변수가 레퍼런스형이 되는 버그가 있었음
    - 보고된 오류 2 : 단항 연산자를 연산자 뒤에 사용하면 컴파일 오류나는 버그가 있었음. 괄호를 치면 컴파일 오류는 해결할 수 있었음.

- GUE 언어
  - ~~Starlit 문법 업데이트된 내용에 준해서 GUE 언어 역시 작동.~~
    - 사실상 GUE 언어에서 기술적으로 완벽히 호환되는 기능을 Starlit 도메인에서 추가한 것은 아니다.
    - 단항 연산자 이슈는 해결.
  - GUE 언어에서 연산자 사용 시 앞의 내용과 이어졌는데, 연산자 앞은 띄어쓰기가 되어 있고, 뒤는 붙여 쓰면 단항 연산자로 인식하도록 수정.
    - Starlit에서도 단항 연산자 이슈가 해결되었으니 GUE 언어에서도 단항 연산자를 자유자재로 써도 무방하다.
   
- 06. 06일 업데이트 예정



## Documents(Starlit 3.2)

| No. | Docs |
|-----|------|
|0|[튜토리얼](https://github.com/PJungKim/Starlit3/blob/main/docs/000_Tutorial.md)|
|1|[Hello, World! 출력하기](https://github.com/PJungKim/Starlit3/blob/main/docs/001_Hello_World.md)|
|2|[글자색, 글자크기 조절하기](https://github.com/PJungKim/Starlit3/blob/main/docs%2F002_Color_Size.md)|
|3|[변수 사용하기](https://github.com/PJungKim/Starlit3/blob/main/docs/003_Button_Var.md)|
|4|[조건문](https://github.com/PJungKim/Starlit3/blob/main/docs%2F004_condition.md)|
|5|반복문|

## Documents(GUE)

|No.|Docs|
|---|----|
|0|튜토리얼+ `Hello, World!`|
|1|변수 사용하기|
|2|조건문|
|3|반복문|

## History of Issues

### 2024. 04

- 30 Starlit 3.2 Update
  - Added Call_by_ref and ref_based_variable
  - Simple Variable Definition
  - SI-Unit for Float Literal
  - Fixed some string bugs.
- 04 Fixed Windows11 API Issue(Virtual Machine Only)
- 01 Starlit 3.1 Release
