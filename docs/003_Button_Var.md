# 3. 변수 사용하기

[이전](https://github.com/PJungKim/Starlit3/blob/main/docs/002_Color_Size.md) 다음

## 1. Starlit에서 변수 정의하기(선언)

### 1.1. 변수 정의, 대입, 출력 예제

```
$import(oled);
main(){
    #OLED():OLED_t;
    OLED.Init();
    while(!BUTTON_Read()){}
}
```

이 코드에서 `#OLED():OLED_t;` 아래쪽에 변수를 하나 추가해 보겠습니다. `#a():int;`라고 적으시면 됩니다. 그 다음 바로 아랫줄에 `a = 1;`이라고 적어 보겠습니다.

```
$import(oled);
main(){
    #OLED():OLED_t;
    #a():int;
    a = 1;
    OLED.Init();
    while(!BUTTON_Read()){}
}
```

이제 `OLED.Init();` 아랫쪽에 변수 a의 값을 출력해 보겠습니다. `OLED << a`와 같이 적어 주시면 됩니다.

```
$import(oled);
main(){
    #OLED():OLED_t;
    #a():int;
    a = 1;
    OLED.Init();
    OLED << a;
    while(!BUTTON_Read()){}
}
```

자, 이제 실행해 보겠습니다. 아래와 같은 결과가 나오면 됩니다.

  <img src = "..\res\EXAMPLE\Variable\Var1.png" width="20%" height="20%">

### 1.2. 변수 값에 색을 입혀서 출력하고 싶다고요?

이번에는 변수 값에 색을 입혀 보겠습니다. OLED에 색을 입히는 것은 2장에서 설명해 드렸는데요, 이를테면 빨간색으로 변수 a의 값을 출력해 보겠습니다. 빨간색을 나타내는 `"/r"`과 변수 a값을 합치면 됩니다. 하지만, 변수 a는 문자열이 아니기 때문에 문자열 형식으로 만들 수 있습니다. C언어에서 `%d` 생각 나시죠? 그러면 `"/r%d"`라고 쓰고 %d 위치에 a를 넣으시면 됩니다. 다음과 같이 작성해 주시면 됩니다.

`"/r%d" % a`

```
$import(oled);
main(){
    #OLED():OLED_t;
    #a():int;
    a = 1;
    OLED.Init();
    OLED << "/r%d" % a;
    while(!BUTTON_Read()){}
}
```

  <img src = "..\res\EXAMPLE\Variable\Var1R.png" width="20%" height="20%">
  
### 1.3. 변수 값에 입력받기

이번에는 사용자가 변수에 값을 입력하는 예제입니다. OLED 쪽으로 화살표를 치면 OLED에 출력된다는 뜻이었습니다. 반대로, OLED에서 변수로 화살표를 그리면 어떻게 될까요? 지금 한번 알아보겠습니다. 아래와 같이 코드를 작성해 보겠습니다. `OLED >> a`가 바로 OLED에 a를 입력받겠다는 뜻입니다.

```
$import(oled);
main(){
    #OLED():OLED_t;
    OLED.Init();
    #a():int;
    OLED >> a;
    OLED << "/1/ra는 /y%d/r입니다." % a;
    while(!BUTTON_Read()){}
}
```

  <img src = "..\res\EXAMPLE\Variable\VAR1I.png" width="20%" height="20%">

### 1.4. 예제 - 변수 2개를 받아 합 출력하기

이번에는 변수 2개(a, b)를 받아 두 수의 합을 출력하는 코드를 만들어 보겠습니다.
```
$import(oled);
main(){
    #OLED():OLED_t;
    OLED.Init();
    #a():int;
    #b():int;
    OLED << "/0a = " >> a;
    OLED << "/1b = " >> b;
    OLED << "/2/r%d/w + /r%d/w = /r%d/w입니다." % a % b % (a + b);
    while(!BUTTON_Read()){}
}
```

  <img src = "..\res\EXAMPLE\Variable\Var2P.png" width="20%" height="20%">

## 2. 버튼 사용하기(키보드) - 조건문 뒤쪽에 놓겠습니다.

### 2.1. 버튼 배치표

- 이번에는 버튼을 사용하는 방법을 알아보겠습니다. 우선, 아래와 같이 버튼이 배정되어 있습니다. Starlit은 임베디드용 프로그래밍 언어기 때문에 SVM 상에서 모든 키보드에 대해서 할당되는 것이 아닌 임베디드에 대해서 버튼이 할당되오니 유의해 주시기 바랍니다. 일반적으로 우측에 숫자 키가 따로 있다면, 그 숫자키를 적극적으로 이용하면 편합니다.

  |키보드|버튼 값|
  |------|-------|
  |숫자 1|BUTTON_1|
  |숫자 2|BUTTON_2|
  |숫자 3|BUTTON_3|
  |숫자 4|BUTTON_4|
  |숫자 5|BUTTON_5|
  |숫자 6|BUTTON_6|
  |숫자 7|BUTTON_7|
  |숫자 8|BUTTON_8|
  |숫자 9|BUTTON_9|
  |숫자 0|BUTTON_0|

### 2.2. 버튼 입력값에 따라 원하는 내용 출력하기

- 이번에는 버튼을 입력받으면 입력된 값에 따라 원하는 값을 출력해 보겠습니다.

```
$import(oled);
main(){
    #OLED():OLED_t;
    OLED.Init();
    OLED << "/01번 또는 2번 입력" % a;
    while(!BUTTON_Read()){}
}
```


  
