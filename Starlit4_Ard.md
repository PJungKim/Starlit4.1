# Starlit4 Ardunio Style

- Starlit 4의 임베디드 환경에서 사용하는 예제를 적어봅니다.
- 총 4개의 카테고리로 분류합니다.
- 경우에 따라 내부 회로 활성화 기능을 켠 뒤 실행할 수도 있습니다.
  - 내부 LED : `ELED_Begin()` 활성화 및 GPIO 3번(Red)/5번(Green)/6번(Blue) 사용, PWM 사용 가능
  - 내부 버튼 : (GPIO 핀번호 최댓값 + 1) + 버튼 번호를 `DigitalRead()` 사용.
    - 예 : GPIO 핀번호 최댓값이 21일 때 1번 버튼은 23번(21+1+1).
    - 내부 버튼은 pinMode 설정 불필요.

## 1. GPIO/LED/Switch 관련 예제

### 1.1. GPIO 출력 - LED 깜빡이기

- 회로 정보
  |Pin|Mode|Pullup|Connect|
  |--|--|--|--|
  |3|OUTPUT|None|LED - Res - GND|
  - LED를 외부에서 사용하는 경우 저항(220Ohm) 사용.
  - 내부 LED 사용 시 ELED_Begin() 사용.
- GPIO 출력 코드(Arduino Style) 예제 : LED 1초 주기로 깜빡이기
  ```Py
  setup
      PinMode(3, OUTPUT) # 3번 핀을 출력으로 설정
      # ELED_Begin() # 내부 회로 사용 시.

  loop
      DigitalWrite(3, HIGH)
      Delay(500) # 켜짐(0.5초) + 꺼짐(0.5초) = 1초 주기
      DigitalWrite(3, LOW)
      Delay(500)
  ```
  ```C++
  setup(){
      PinMode(3, OUTPUT); /// 3번 핀을 출력으로 설정
      /// ELED_Begin(); /// 내부 회로 사용 시.
  }

  loop(){
      DigitalWrite(3, HIGH);
      Delay(500); /// 켜짐(0.5초) + 꺼짐(0.5초) = 1초 주기
      DigitalWrite(3, LOW);
      Delay(500);
  }
  ```

### 1.2. GPIO 입력 - 스위치를 눌린 상태에서 LED 켜지는 코드

- 회로 정보 *(23번핀 내부스위치 1번)*
  |Pin|Mode|Pullup|Connect|
  |--|--|--|--|
  |3|OUTPUT|None|LED - Res - GND|
  |4|INPUT|Pulldown|스위치 - VDD(외부스위치 사용 시)|
  |*23*|INPUT|None|내부스위치 1번|
  - LED를 외부에서 사용하는 경우 저항(220Ohm) 사용.
  - 내부 LED 사용 시 ELED_Begin() 사용.
  - 외부 버튼 사용 시 20k Pullup 저항 사용

- GPIO 입력 코드(Arduino Style) 예제 : 버튼이 눌릴 때만 LED 켜기
  ```Py
  setup
      PinMode(3, OUTPUT) # 3번 핀을 출력으로 설정
      # ELED_Begin() # 내부 LED 사용 시.
      PinMode(4, INPUT) # 외부스위치 사용 시

  loop
      if DigitalRead(4): # 버튼이 눌린 경우
          DigitalWrite(3, HIGH) # LED 켜짐
      else: # 버튼이 눌리지 않는 경우
          DigitalWrite(3, LOW) # LED 꺼짐
  ```
  ```C++
  setup(){
      PinMode(3, OUTPUT); /// 3번 핀을 출력으로 설정
      /// ELED_Begin(); /// 내부 LED 사용 시.
      PinMode(4, INPUT); /// 외부스위치 사용 시
  }

  loop(){
      if DigitalRead(4){/// 버튼이 눌린 경우
          DigitalWrite(3, HIGH);/// LED 켜짐
      }
      else{///버튼이 눌리지 않은 경우
          DigitalWrite(3, LOW);/// LED 꺼짐
      }
  }
  ```

### 1.3. PWM을 이용한 LED 출력 예제 - 밝아졌다 어두워지는 LED
- 회로 정보
  |Pin|Mode|Pullup|Connect|
  |--|--|--|--|
  |3|OUTPUT|None|LED - Res - GND|
  - LED를 외부에서 사용하는 경우 저항(220Ohm) 사용.
  - 내부 LED 사용 시 ELED_Begin() 사용.
  - PWM 가능 여부 반드시 확인
- PWM 출력 예제 : 밝아졌다 어두워지는 패턴
  ```Py
  setup
      PinMode(3, PWMOUT) # 3번 핀을 출력으로 설정
      # ELED_Begin() # 내부 LED 사용 시.

  level = 0 # LED 밝기
  delta = 5 # LED 밝기 변화량
  loop
      level += delta # LED 밝기 증감
      if level == 255 || level == 0:
          delta = -delta # LED 밝기가 한계에 달하면 증감을 반대로 변경
      analogWrite(3, level) # LED 출력
      Delay(5) # 5ms(0.005초) 기다리기
  ```
  ```C++
  setup(){
      PinMode(3, PWMOUT); /// 3번 핀을 출력으로 설정
      /// ELED_Begin(); /// 내부 LED 사용 시.
  }

  level = 0; /// LED 밝기
  delta = 5; /// LED 밝기 변화량
  loop(){
      level += delta; /// LED 밝기 증감
      if(level == 255 || level == 0){
          delta = -delta; /// LED 밝기가 한계에 달하면 증감을 반대로 변경
      }
      analogWrite(3, level); ///
      Delay(5);
  }
  ```

### 1.4. PWM을 이용한 LED 출력 예제 - 무지개색으로 변하는 LED
- 회로 정보
  |Pin|Mode|Pullup|Connect|
  |--|--|--|--|
  |3|OUTPUT|None|LED(Red) - Res - GND|
  |5|OUTPUT|None|LED(Green) - Res - GND|
  |6|OUTPUT|None|LED(Blue) - Res - GND|
  - RGB LED 사용(Red, Green, Blue 핀을 각각 3, 5, 6에 연결, Common Anode)
  - LED를 외부에서 사용하는 경우 저항(220Ohm) 사용.
  - 내부 LED 사용 시 ELED_Begin() 사용.
  - PWM 가능 여부 반드시 확인
- PWM 출력 예제 : 밝아졌다 어두워지는 패턴
  ```Py
  setup
      PinMode(3, PWMOUT) # 3번 핀(빨강)을 출력으로 설정
      PinMode(5, PWMOUT) # 5번 핀(초록)을 출력으로 설정
      PinMode(6, PWMOUT) # 6번 핀(파랑)을 출력으로 설정
      # ELED_Begin() # 내부 LED(RGB) 사용 시.
      # 초기 LED 색 설정
      analogWrite(3, 255)
      analogWrite(5, 0)
      analogWrite(6, 0)

  level = 0
  loop
      if level <= 255: # 빨강->노랑, 초록 증가
          analogWrite(5, level)
      elif level <= 510: # 노랑->초록, 빨강 감소
          analogWrite(3, 510-level)
      elif level <= 765: # 초록->청록, 파랑 증가
          analogWrite(6, level - 510)
      elif level <= 1020: # 청록 -> 파랑, 초록 감소
          analogWrite(5, 1020 - level)
      elif level <= 1275: # 파랑 -> 자홍, 빨강 증가
          analogWrite(3, level - 1020)
      else: # 자홍 -> 빨강, 파랑 감소
          analogWrite(6, 1530 - level)
      level += 5
      level %= 1530
      Delay(5)
  ```
  ```C++
  setup(){
      PinMode(3, PWMOUT); /// 3번 핀(빨강)을 출력으로 설정
      PinMode(5, PWMOUT); /// 5번 핀(초록)을 출력으로 설정
      PinMode(6, PWMOUT); /// 6번 핀(파랑)을 출력으로 설정
      /// ELED_Begin(); /// 내부 LED(RGB) 사용 시.
      /// 초기 LED 색 설정
      analogWrite(3, 255);
      analogWrite(5, 0);
      analogWrite(6, 0);
  }

  level = 0;
  loop(){
      if(level <= 255){ /// 빨강->노랑, 초록 증가
          analogWrite(5, level);
      }
      elif(level <= 510){ /// 노랑->초록, 빨강 감소
          analogWrite(3, 510-level);
      }
      elif(level <= 765){ /// 초록->청록, 파랑 증가
          analogWrite(6, level - 510);
      }
      elif(level <= 1020){ /// 청록 -> 파랑, 초록 감소
          analogWrite(5, 1020 - level);
      }
      elif(level <= 1275){ /// 파랑 -> 자홍, 빨강 증가
          analogWrite(3, level - 1020);
      }
      else{ /// 자홍 -> 빨강, 파랑 감소
          analogWrite(6, 1530 - level);
      }
      level += 5;
      level %= 1530;
      Delay(5);
  }
  ```

### 1.5. 버튼을 눌러 LED 밝기 조절하는 코드
- 회로 정보
  |Pin|Mode|Pullup|Connect|
  |--|--|--|--|
  |3|OUTPUT|None|LED(Red) - Res - GND|
  |5|OUTPUT|None|LED(Green) - Res - GND|
  |6|OUTPUT|None|LED(Blue) - Res - GND|
  |*23*|INPUT|None|내부스위치 1번|
  |*24*|INPUT|None|내부스위치 2번|
  |*25*|INPUT|None|내부스위치 3번|
  |*26*|INPUT|None|내부스위치 4번|
  |*27*|INPUT|None|내부스위치 5번|
  |*28*|INPUT|None|내부스위치 6번|
  - RGB LED 사용(Red, Green, Blue 핀을 각각 3, 5, 6에 연결, Common Anode)
  - LED를 외부에서 사용하는 경우 저항(220Ohm) 사용.
  - 내부 LED 사용 시 ELED_Begin() 사용.
  - PWM 가능 여부 반드시 확인
  - 내부스위치가 6개 미만인 경우 외부 스위치 병행 사용 가능(pinMode 설정 필요)

- LED 밝기 조절 패턴(1~3:증가 4~6:감소, 빨강:1/4 초록2/5 파랑3/6)
  ```Py
  red = 0
  gre = 0
  blu = 0
  setup
      PinMode(3, PWMOUT) # 3번 핀(빨강)을 출력으로 설정
      PinMode(5, PWMOUT) # 5번 핀(초록)을 출력으로 설정
      PinMode(6, PWMOUT) # 6번 핀(파랑)을 출력으로 설정
      # ELED_Begin() # 내부 LED(RGB) 사용 시.
      # 외부 스위치 사용 시 pinMode 추가

  loop
      if DigitalRead(23) && red < 255: # 빨강 증가
          red += 1
      if DigitalRead(24) && gre < 255: # 초록 증가
          gre += 1
      if DigitalRead(25) && blu < 255: # 파랑 증가
          blu += 1
      if DigitalRead(26) && red > 0: # 빨강 감소
          red -= 1
      if DigitalRead(27) && gre > 0: # 초록 감소
          gre -= 1
      if DigitalRead(28) && blu > 0: # 파랑 감소
          blu -= 1
      Delay(5)
      analogWrite(3, red)
      analogWrite(5, gre)
      analogWrite(6, blu)
  ```
  ```C++
  red = 0;
  gre = 0;
  blu = 0;
  setup(){
      PinMode(3, PWMOUT); /// 3번 핀(빨강)을 출력으로 설정
      PinMode(5, PWMOUT); /// 5번 핀(초록)을 출력으로 설정
      PinMode(6, PWMOUT); /// 6번 핀(파랑)을 출력으로 설정
      /// ELED_Begin(); /// 내부 LED(RGB) 사용 시.
      /// 외부 스위치 사용 시 pinMode 추가
  }

  loop(){
      if(DigitalRead(23) && red < 255) /// 빨강 증가
          red += 1;
      if(DigitalRead(24) && gre < 255) /// 초록 증가
          gre += 1;
      if(DigitalRead(25) && blu < 255) /// 파랑 증가
          blu += 1;
      if(DigitalRead(26) && red > 0) /// 빨강 감소
          red -= 1;
      if(DigitalRead(27) && gre > 0) /// 초록 감소
          gre -= 1;
      if(DigitalRead(28) && blu > 0) /// 파랑 감소
          blu -= 1;
      Delay(5);
      analogWrite(3, red);
      analogWrite(5, gre);
      analogWrite(6, blu);
  }
  ```
  - C-Style은 부분적으로 Pythonic 요소를 사용했음.(코드 부피 절약 목적)


### 1.6. BUTTON State Machine

- Button State Machine이란?
  - 버튼이 눌렸는지 안눌렸는지 확인하는 알고리즘
- Button State Machine의 필요성
  - Bouncing Effect 발생 : 버튼이 눌리면 순간적으로 떨림에 의해 High-Low-High-Low로 신호가 튐 -> 버튼이 여러번 눌린 것처럼 감지
  - Button의 변화 명확히 감지 : 버튼이 눌린 것을 감지하는 것이 아니라 버튼에서 발생한 Edge에서 한 번만 실행해야 함.
- Button State Machine 구조
  |State|설명|
  |--|--|
  |대기상태|버튼이 눌리지 않은 상태|
  |누름 감지|버튼이 눌리지 않다가 눌림을 처음 감지하고 인식 시간만큼 유지|
  |누른 상태|버튼이 눌려져 있는 상태|
  |똄 감지|버튼이 눌려져 있는 상태에서 처음 떼짐을 감지하고 인식 시간만큼 유지|
- Button 인식 시점
  - Positive Edge 방식(InSwitch2) : 버튼을 누르고 일정 시간이 지난 시점에서 버튼이 눌림을 감지
    - 누름 감지에서 누른 상태로 넘어갈 때 값 출력
    - 장점 : 빠르게 작동시킬 수 있다.
    - 단점 : 신중하게 작동하기 어려우며, 길게 누르는 동작을 취할 때 반드시 짧게 누르는 동작 1번 출력된 이후에 출력된다.
  - Negative Edge 방식(InSwitch) : 버튼을 누르고 뗀 시점에서 인식
    - 뗌 감지에서 대기상태로 넘어갈 때 값 출력
    - 장점 : 더블클릭, 길게 누름을 별개로 감지할 수 있다.
    - 단점 : 대체로 작동 속도가 느린 편이다.
- Inswitch 코드(Negative Edge 방식)
  - 아래 코드에서 BUTTON_State는 내부버튼의 값을 비트 OR 연산하여 출력하는 값으로 보면 된다.
    ```Py
    bstate = 0                      # 버튼 현재 상태
    bsum = 0                        # 버튼 누적을 위한 변수
    btime = 0                       # 버튼 입력 시간을 저장하는 변수
    pbtn = 0                        # Edge를 감지하기 위해 이전 값을 저장하는 변수
    MINBTIME = 50                   # Edge가 발생하고 대기하는 시간
    BUTTON_LONG_FLAG1 = 0x00010000  # Long Flag(길게 누름을 처음 감지)
    BUTTON_LONG_FLAG2 = 0x00020000  # Long Flag(길게 누름을 감지한 이후 연쇄 출력 시)

    InSwitch:int
        btn = BUTTON_State()
        when bstate:
            case 0: # Idle State
                if btn:
                    pbtn = btn
                    bstate = 1
                    bsum = btn
                    btime = Tick()
            case 1: # Rising State - 출력 따로 없음
                if pbtn != btn
                    pbtn = btn
                    bsum |= btn
                    btime = Tick()
                if Tick() - btime > MINBTIME:
                    bstate = 2
            case 2: # Pushed State - 길게 누르거나 버튼 전체를 떼면 출력
                if !btn:
                    pbtn = 0
                    bstate = 3
                    btime = Tick()
                    return bsum
                elif Tick() - btime > 500:
                    bstate = 4
                    btime = Tick()
                    bsum |= BUTTON_State()
                    return bsum | BUTTON_LONG_FLAG1
                else:
                    bsum |= BUTTON_State()
            case 3: # Falling State - 버튼을 뗐을 때, 이후에는 버튼 입력 받지 않고 대기
                if btn:
                    btime = Tick()
                if Tick() - btime > MINBTIME:
                    bstate = 0
                    bsum = 0
            case 4: # 길게 누른 경우 실행
                if !btn:
                    pbtn = 0
                    bstate = 3
                    bsum = 0
                    btime = Tick()
                elif Tick() - btime > 80: # 연쇄적으로 길게 누르는 것 출력.
                    btime = Tick()
                    return bsum | BUTTON_LONG_FLAG2
                else:
                    bsum |= BUTTON_State()
        return 0
    ```
    ```C++
    bstate = 0;                     /// 버튼 현재 상태
    bsum = 0;                       /// 버튼 누적을 위한 변수
    btime = 0;                      /// 버튼 입력 시간을 저장하는 변수
    pbtn = 0;                       /// Edge를 감지하기 위해 이전 값을 저장하는 변수
    MINBTIME = 50;                  /// Edge가 발생하고 대기하는 시간
    BUTTON_LONG_FLAG1 = 0x00010000; /// Long Flag(길게 누름을 처음 감지)
    BUTTON_LONG_FLAG2 = 0x00020000; /// Long Flag(길게 누름을 감지한 이후 연쇄 출력 시)

    InSwitch():int{
        btn = BUTTON_State();
        when(bstate){
            case 0: /// Idle State
                if(btn){
                    pbtn = btn;
                    bstate = 1;
                    bsum = btn;
                    btime = Tick();
                }
            case 1: /// Rising State - 출력 따로 없음
                if(pbtn != btn){
                    pbtn = btn;
                    bsum |= btn;
                    btime = Tick();
                }
                if(Tick() - btime > MINBTIME)
                    bstate = 2;
            case 2: /// Pushed State - 길게 누르거나 버튼 전체를 떼면 출력
                if(!btn){
                    pbtn = 0;
                    bstate = 3;
                    btime = Tick();
                    return bsum;
                }
                elif(Tick() - btime > 500){
                    bstate = 4;
                    btime = Tick();
                    bsum |= BUTTON_State();
                    return bsum | BUTTON_LONG_FLAG1;
                }
                else
                    bsum |= BUTTON_State();
            case 3: /// Falling State - 버튼을 뗐을 때, 이후에는 버튼 입력 받지 않고 대기
                if(btn){
                    btime = Tick();
                }
                if(Tick() - btime > MINBTIME){
                    bstate = 0;
                    bsum = 0;
                }
            case 4: /// 길게 누른 경우 실행
                if(!btn){
                    pbtn = 0;
                    bstate = 3;
                    bsum = 0;
                    btime = Tick();
                }
                elif(Tick() - btime > 80){ /// 연쇄적으로 길게 누르는 것 출력.
                    btime = Tick();
                    return bsum | BUTTON_LONG_FLAG2;
                }
                else
                    bsum |= BUTTON_State();
        }
        return 0;
    }
    ```
- Inswitch2 코드(Positive Edge 방식)
    ```Py
    bstate = 0                      # 버튼 현재 상태
    bsum = 0                        # 버튼 누적을 위한 변수
    btime = 0                       # 버튼 입력 시간을 저장하는 변수
    pbtn = 0                        # Edge를 감지하기 위해 이전 값을 저장하는 변수
    MINBTIME = 50                   # Edge가 발생하고 대기하는 시간
    BUTTON_LONG_FLAG1 = 0x00010000  # Long Flag(길게 누름을 처음 감지)
    BUTTON_LONG_FLAG2 = 0x00020000  # Long Flag(길게 누름을 감지한 이후 연쇄 출력 시)

    InSwitch2:int
        btn = BUTTON_State()
        when bstate:
            case 0: # Idle State
                if btn:
                    pbtn = btn
                    bstate = 1
                    bsum = btn
                    btime = Tick()
            case 1: # Rising State - Push로 바뀔 때 한 번 출력
                if pbtn != btn
                    pbtn = btn
                    bsum |= btn
                    btime = Tick()
                if Tick() - btime > MINBTIME:
                    bstate = 2
                    return bsum
            case 2: # Pushed State - 길게 누를 때 출력과 동시에 State를 4로 변경
                if !btn:
                    pbtn = 0
                    bstate = 3
                    bsum = 0
                    btime = Tick()
                elif Tick() - btime > 500:
                    bstate = 4
                    btime = Tick()
                    return bsum | BUTTON_LONG_FLAG1
                else:
                    bsum |= BUTTON_State()
            case 3: # Falling State - 버튼을 뗐을 때, 이후에는 버튼 입력 받지 않고 대기
                if btn:
                    btime = Tick()
                if Tick() - btime > MINBTIME:
                    bstate = 0
            case 4: # 길게 누른 경우 실행
                if !btn:
                    pbtn = 0
                    bstate = 3
                    bsum = 0
                    btime = Tick()
                elif Tick() - btime > 80:
                    btime = Tick()
                    return bsum | BUTTON_LONG_FLAG2
                else:
                    bsum |= BUTTON_State()
        return 0
    ```
    ```C++
    bstate = 0;                     /// 버튼 현재 상태
    bsum = 0;                       /// 버튼 누적을 위한 변수
    btime = 0;                      /// 버튼 입력 시간을 저장하는 변수
    pbtn = 0;                       /// Edge를 감지하기 위해 이전 값을 저장하는 변수
    MINBTIME = 50;                  /// Edge가 발생하고 대기하는 시간
    BUTTON_LONG_FLAG1 = 0x00010000; /// Long Flag(길게 누름을 처음 감지)
    BUTTON_LONG_FLAG2 = 0x00020000; /// Long Flag(길게 누름을 감지한 이후 연쇄 출력 시)

    InSwitch2():int{
        btn = BUTTON_State();
        when(bstate){
            case 0: /// Idle State
                if(btn){
                    pbtn = btn;
                    bstate = 1;
                    bsum = btn;
                    btime = Tick();
                }
            case 1: /// Rising State - Push로 바뀔 때 한 번 출력
                if(pbtn != btn){
                    pbtn = btn;
                    bsum |= btn;
                    btime = Tick();
                }
                if(Tick() - btime > MINBTIME){
                    bstate = 2;
                    return bsum;
                }
            case 2: /// Pushed State - 길게 누를 때 출력과 동시에 State를 4로 변경
                if(!btn){
                    pbtn = 0;
                    bstate = 3;
                    bsum = 0;
                    btime = Tick();
                }
                elif(Tick() - btime > 500){
                    bstate = 4;
                    btime = Tick();
                    return bsum | BUTTON_LONG_FLAG1;
                }
                else
                    bsum |= BUTTON_State();
            case 3: /// Falling State - 버튼을 뗐을 때, 이후에는 버튼 입력 받지 않고 대기
                if(btn)
                    btime = Tick();
                if(Tick() - btime > MINBTIME)
                    bstate = 0;
            case 4: /// 길게 누른 경우 실행
                if(!btn){
                    pbtn = 0;
                    bstate = 3;
                    bsum = 0;
                    btime = Tick();
                }
                elif(Tick() - btime > 80){
                    btime = Tick();
                    return bsum | BUTTON_LONG_FLAG2;
                }
                else
                    bsum |= BUTTON_State();
        }
        return 0;
    }
    ```

### 1.7. Button State Machine의 활용 : 버튼을 눌러 정해진 색의 LED 켜기

- 아래는 버튼을 눌러 10색상환에 해당되는 색을 출력하는 코드이다.
    |버튼|LED 색상|버튼|LED 색상|
    |--|--|--|--|
    |0|빨강|5|청록|
    |1|주황|6|바다색|
    |2|노랑|7|파랑|
    |3|연두|8|보라|
    |4|초록|9|자홍|

- 소스 코드
    ```Py
    $import(inswitch.sh4)

    setup
        InitSwitch
        PinMode(3, PWMOUT)
        PinMode(5, PWMOUT)
        PinMode(6, PWMOUT)
        ELED_Begin
        
    loop
        btn = InSwitch2()
        when btn:
            case BUTTON_0: # RED
                AnalogWrite(3, 255)
                AnalogWrite(5, 0)
                AnalogWrite(6, 0)
            case BUTTON_1: # ORANGE
                AnalogWrite(3, 255)
                AnalogWrite(5, 96)
                AnalogWrite(6, 0)
            case BUTTON_2: # YELLOW
                AnalogWrite(3, 255)
                AnalogWrite(5, 255)
                AnalogWrite(6, 0)
            case BUTTON_3: # LIME
                AnalogWrite(3, 128)
                AnalogWrite(5, 255)
                AnalogWrite(6, 0)
            case BUTTON_4: # GREEN
                AnalogWrite(3, 0)
                AnalogWrite(5, 255)
                AnalogWrite(6, 0)
            case BUTTON_5: # CYAN
                AnalogWrite(3, 0)
                AnalogWrite(5, 255)
                AnalogWrite(6, 255)
            case BUTTON_6: # SEA
                AnalogWrite(3, 0)
                AnalogWrite(5, 128)
                AnalogWrite(6, 255)
            case BUTTON_7: # BLUE
                AnalogWrite(3, 0)
                AnalogWrite(5, 0)
                AnalogWrite(6, 255)
            case BUTTON_8: # VIOLET
                AnalogWrite(3, 128)
                AnalogWrite(5, 0)
                AnalogWrite(6, 255)
            case BUTTON_9: # MAGENTA
                AnalogWrite(3, 255)
                AnalogWrite(5, 0)
                AnalogWrite(6, 255)
    ```
    ```C++
    $import(inswitch.sh4);

    setup(){
        InitSwitch;
        PinMode(3, PWMOUT);
        PinMode(5, PWMOUT);
        PinMode(6, PWMOUT);
        ELED_Begin;
    }

    loop(){
        btn = InSwitch2();
        when(btn){
            case BUTTON_0: /// RED
                AnalogWrite(3, 255);
                AnalogWrite(5, 0);
                AnalogWrite(6, 0);
            case BUTTON_1: /// ORANGE
                AnalogWrite(3, 255);
                AnalogWrite(5, 96);
                AnalogWrite(6, 0);
            case BUTTON_2: /// YELLOW
                AnalogWrite(3, 255);
                AnalogWrite(5, 255);
                AnalogWrite(6, 0);
            case BUTTON_3: /// LIME
                AnalogWrite(3, 128);
                AnalogWrite(5, 255);
                AnalogWrite(6, 0);
            case BUTTON_4: /// GREEN
                AnalogWrite(3, 0);
                AnalogWrite(5, 255);
                AnalogWrite(6, 0);
            case BUTTON_5: /// CYAN
                AnalogWrite(3, 0);
                AnalogWrite(5, 255);
                AnalogWrite(6, 255);
            case BUTTON_6: /// SEA
                AnalogWrite(3, 0);
                AnalogWrite(5, 128);
                AnalogWrite(6, 255);
            case BUTTON_7: /// BLUE
                AnalogWrite(3, 0);
                AnalogWrite(5, 0);
                AnalogWrite(6, 255);
            case BUTTON_8: /// VIOLET
                AnalogWrite(3, 128);
                AnalogWrite(5, 0);
                AnalogWrite(6, 255);
            case BUTTON_9: /// MAGENTA
                AnalogWrite(3, 255);
                AnalogWrite(5, 0);
                AnalogWrite(6, 255);
        }
    }
    ```


## 2. PuTTY/UART 관련 예제

- PuTTY 관련 예제는 Teraterm에서도 실행 가능하다.
- Teraterm은 매크로 지원이 가능하므로 시간만 가능하다면 테라텀으로 특정 시나리오를 돌려 보는 것도 상당히 흥미로운 일이다. 

### 2.1. UART를 통해 출력하기

- 1초에 한 번씩 Hello, World 출력하는 예제

```Py
setup
    FTDI_Begin(115200) # FTDI 활성화

loop
    PuTTY << "Hello, World!" # PuTTY(TeraTerm) 창에 출력
    Delay(1000)
```
```C++
setup(){
    FTDI_Begin(115200); /// FTDI 활성화
}

loop(){
    PuTTY << "Hello, World!"; /// PuTTY(TeraTerm) 창에 출력
    Delay(1000);
}
```


### 2.2. 글자 색 입혀 출력하기

- 1초에 한 번씩 Hello는 초록색, World는 빨간색으로 출력하기
```Py
setup
    FTDI_Begin(115200) # FTDI 활성화

loop
    PuTTY << "/gHello, /rWorld!" # Hello는 초록색, World는 빨간색으로 출력
    Delay(1000)
```
```C++
setup(){
    FTDI_Begin(115200); /// FTDI 활성화
}

loop(){
    PuTTY << "/gHello, /rWorld!"; /// Hello는 초록색, World는 빨간색으로 출력
    Delay(1000);
}
```

- 글자 색상표
  |기호|색상|기호|색상|기호|색상|
  |---|----|---|----|---|---|
  |`/r`|빨강|`/g`|초록|`/b`|파랑|
  |`/o`|주황|`/t`|에메랄드|`/v`|남보라|
  |`/y`|노랑|`/c`|청록|`/m`|자홍|
  |`/l`|연두|`/s`|바다|`/p`|장미색|
  |`/w`|흰색|`/k`|검정|`/K` `/W`|회색(일부 환경에서는 미지원)|
- 글자 색상 제어 관련
  - `/#FFFF00` : HEX Code를 사용하여 색상을 입힐 수 있다.
    - `"/#%06x" % color` 꼴의 연산으로 색을 입혀도 좋다.
    - `f"/#{col:06x}"` 꼴의 표현으로도 색을 입힐 수 있다.
  - 연한 색상 : 대문자 사용(예: 빨강이 `/r`이면 분홍은 `/R`)
  - 어두운 색상 : `d` 추가해서 사용(예: 주황이 `/o`이면 갈색은 `/do`)
  - 흐린 색상 : `/dR` 같은 경우 분홍에서 명도만 낮춘 흐린 색상이 출력된다.

- 추후 PuTTY 환경에서도 간편하게 배경색, 밑줄, 볼드체 등을 적용하는 코드도 추가할 예정이다.

### 2.3. 글자 위치 정하고 출력하기

- 항상 같은 위치에서 1씩 증가하여 글자 출력하는 코드(0.1초 간격)
```Py
setup
    FTDI_Begin(115200) # FTDI 활성화

v = 0
loop
    PuTTY << f"/0Value = {v}" # PuTTY(TeraTerm) 창에 0번째 줄에 출력
    Delay(100) # 0.1초 기다림.
    v += 1 # v값 1 증가
```
```c++
setup(){
    FTDI_Begin(115200); /// FTDI 활성화
}

v = 0;
loop(){
    PuTTY << f"/0Value = {v}"; /// PuTTY(TeraTerm) 창에 0번째 줄에 출력
    Delay(100); /// 0.1초 기다림.
    v += 1; /// v값 1 증가
}
```

### 2.4. PuTTY창에서 데이터 받아오기

- 두 수의 합 출력 예제

```Py
setup
    FTDI_Begin(115200)

a = 0
b = 0
loop
    "Input a : " >> PuTTY >> a
    "Input b : " >> PuTTY >> b
    PuTTY << f"The Sum is {a + b}\r\n"
```
```C++
setup(){
    FTDI_Begin(115200);
}

a = 0;
b = 0;
loop(){
    "Input a : " >> PuTTY >> a;
    "Input b : " >> PuTTY >> b;
    PuTTY << f"The Sum is {a + b}\r\n";
}
```







## 3. OLED/Display 관련 예제

## 4. LED Matrix Display 관련 예제