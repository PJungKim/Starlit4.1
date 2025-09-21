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

## 2. PuTTY/UART 관련 예제

## 3. OLED/Display 관련 예제

## 4. LED Matrix Display 관련 예제