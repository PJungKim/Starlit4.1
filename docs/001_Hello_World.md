# 1. Hello, World 출력하기

[이전](https://github.com/PJungKim/Starlit3/blob/main/docs/000_Tutorial.md) [다음](https://github.com/PJungKim/Starlit3/blob/main/docs/002_Color_Size.md)

## 1. 파일 만들고 열기

### 1.1. 파일 생성하기

- 파일 경로에 `_`와 영어를 제외한 어떠한 문자도 포함되지 않는 위치에 새 파일을 만듭니다.

- 파일 탐색기를 열어서 그 위치에 들어간 다음, 마우스를 우클릭하여 `새로 만들기` - `텍스트 문서`를 선택합니다.

  <img src = "..\res\GITHUB_NEWFILE.png" width = "40%" height = "40%">

- 원하는 형태로 이름을 바꿉니다. 이때, 확장자를 `.slc`로 변경해야 합니다. 혹시나 확장자를 바꾸지 못한다면, 파일 탐색기의 보기에 들어가신 다음 파일 확장명에 체크해 주시기 바랍니다.

  <img src = "..\res\GITHUB_NEWFILE2.png" width = "40%" height = "40%">

- 경고창이 뜨면 예를 눌러 주시기 바랍니다.

  <img src = "..\res\GITHUB_NEWFILE3.png" width = "40%" height = "40%">

### 1.2. 파일 실행하기

- 파일을 실행하면 Starlit 창이 뜨는데, `5번 키보드(Edit)`를 선택 후 `엔터`를 눌러 주세요.

  <img src = "..\res\GITHUB_NEWFILE4.png" width = "40%" height = "40%">

- 이제 Scriptum이 실행되는데, 여기에 코드를 작성하시면 됩니다.

  <img src = "..\res\GITHUB_NEWFILE5.png" width = "40%" height = "40%">

## 2. 코드 작성하기

### 2.1. 코드 작성

- 이제 코드를 작성할 차례입니다. 아래와 같이 완벽하게 똑같이 작성해 주시면 됩니다.

```
$import(oled);

main(){
    OLED = OLED_Black();
    OLED << "Hello, World!" << OLED_endl;
    while(!BUTTON_Read()){}
}
```

### 2.2. 코드 실행

- 다시 Starlit 창에 들어가서 1번을 누른 뒤 Enter를 눌러 실행합니다. 아래 그림처럼 출력되면 정상입니다.

  <img src = "..\res\GITHUB_SETUP_SLC7.png" width = "40%" height = "40%">

### 2.3. 코드 해설

#### 2.3.1. OLED = OLED_Black();

- OLED 객체를 정의합니다.
- OLED_Black은 Adafruit의 SSD1331 RGB OLED 기준으로 배경색이 검정인 상태로 초기화함을 의미합니다.

#### 2.3.2. OLED << "Hello, World!" << OLED_endl;

- OLED에 "Hello, World!"를 출력하라는 의미입니다.
- `OLED_endl`은 OLED 객체에 대해서 줄바꿈을 수행하라는 의미입니다.
- C++ 언어의 문법을 참고하여 만들었습니다.

#### 2.3.3. while(!BUTTON_Read()){}

- 버튼을 누를 때까지 기다리라는 의미입니다.
- waitkey와 매우 유사하지만 실제 임베디드 환경에서 버튼이 있을때 그 버튼을 누를 때까지 기다립니다.
- StarSVM에서는 BUTTON_Read와 연결된 키보드가 정의되어 있는데 그 키를 누르면 누른 값이 출력됩니다.

#### 2.3.4. 주의사항

- C++에서 std::cout이라고 했다고 Starlit에서 :: 연산자를 쓸 수 있는 것은 아닙니다.
- C/C++에서 while문 뒤에 세미콜론`;`을 사용할 수 있지만 Starlit에서는 이 부분이 매우 엄격하여 세미콜론 사용이 불가능합니다. 반드시 중괄호를 열고 닫으시기 바랍니다.

## 3. AResult.sbc

- sbc는 Starlit Binary Code의 약자로, 컴파일을 하면 이 파일이 생성되며, 가상 머신인 SVM에서 돌아갑니다.
- 이 파일도 앞에서의 방법처럼 연결 프로그램을 StarSVM.exe로 설정하시면 바로 작성한 코드를 실행할 수 있습니다.
  
  
  


