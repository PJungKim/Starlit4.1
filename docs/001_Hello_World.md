# Hello, World 출력하기

이전 [다음](https://github.com/PJungKim/Starlit3/blob/main/docs/002_Color_Size.md)

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
    #OLED():OLED_t;
    OLED.Init();
    OLED << "Hello, World!\n";
    while(!BUTTON_Read()){}
}
```

### 2.2. 코드 실행

- 다시 Starlit 창에 들어가서 1번을 누른 뒤 Enter를 눌러 실행합니다. 아래 그림처럼 출력되면 정상입니다.

  <img src = "..\res\GITHUB_SETUP_SLC7.png" width = "40%" height = "40%">

### 2.3. 코드 해설

## 3. AResult.sbc

- sbc는 Starlit Binary Code의 약자로, 컴파일을 하면 이 파일이 생성되며, 가상 머신인 SVM에서 돌아갑니다.
- 이 파일도 앞에서의 방법처럼 연결 프로그램을 StarSVM.exe로 설정하시면 바로 작성한 코드를 실행할 수 있습니다.
  
  
  


