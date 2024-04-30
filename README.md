# Design Your Style, Starlit

## The Programming Language For Embedded Devices
- Starlit은 임베디드의 Flash Memory(MicroSD카드 또는 자체 Flash Memory 등)에 바이너리 저장해서 임베디드 내의 Virtual Machine을 실행해 작동하는 프로그래밍 언어입니다. 임베디드를 더욱 효율적으로 디자인할 수 있습니다.

## News
- Starlit 3.2 Update
  - Reference 변수 추가(포인터와는 약간 다르게 작동)
    - 변수형 앞에 `_`를 붙이면 그 변수에 대한 레퍼런스를 저장하는 형태로 사용할 수 있습니다.
    - `#a(_b):_int;` : 변수 `b`가 저장되어 있을 때, `a`는 `b`를 참조하는 레퍼런스 변수가 됩니다. 이때, `a`는 다른 변수를 참조하게 바꿀 수 있습니다.
    - `a = c;` : 변수 a가 참조하는 위치에 c를 대입합니다.
    - `a_ = _c` : 변수 a의 참조를 c 위치로 변경합니다.
    - Call-By-Reference
      - Reference 변수를 사용하면 함수의 Call-By-Reference 기능을 구현할 수 있습니다.
      - Class의 멤버 변수 역시 Reference로 선언할 수 있습니다. 특히 하위 클래스를 통째로 사용할 때 이 방식을 사용하면 훨씬 가볍게 사용할 수 있습니다.
  - 자동 변수 추가 기능
    - 지역변수의 경우 자료형을 따로 선언하지 않고 정의할 수 있습니다. 이때, 선언한 자료형을 변수가 사라질 때까지 계속 유지합니다.
    - `a = 0;`과 같이 작성했을 때, a가 선언되지 않은 상태면 자동으로 a를 선언합니다. 이때, `a = 0`에서의 값으로 초기화되며, 초기화 이후 변수가 사라질 때까지 계속 자료형은 유지합니다.
  - SI 단위계 추가
    - 소수형에 한해서 SI 단위계를 추가해서 정의할 수 있습니다.
    - `1.0m` : `1.0 * 10^(-3)` = `0.001`
    - 정수형 등 다른 유형의 변수는 SI 단위계 사용이 불가능합니다.
  - 복소수 연산 안정화
    - 두 복소수가 같은지 같지 않은지 비교하는 연산 추가
  - 백슬래시 기능 안정화
    - `\\`와 같이 Backslash 사용을 C언어와 비슷하게 사용할 수 있도록 하였습니다.


## Documents

| No. | Docs |
|-----|------|
|0|[튜토리얼](https://github.com/PJungKim/Starlit3/blob/main/docs/000_Tutorial.md)|
|1|[Hello, World! 출력하기](https://github.com/PJungKim/Starlit3/blob/main/docs/001_Hello_World.md)|
|2|[글자색, 글자크기 조절하기](https://github.com/PJungKim/Starlit3/blob/main/docs%2F002_Color_Size.md)|
|3|[변수 사용하기](https://github.com/PJungKim/Starlit3/blob/main/docs/003_Button_Var.md)|
|4|[조건문](https://github.com/PJungKim/Starlit3/blob/main/docs%2F004_condition.md)|
|5|반복문|

## History of Issues

### 2024. 04

- 04 Fixed Windows11 API Issue(Virtual Machine Only)
- 01 Starlit Release
