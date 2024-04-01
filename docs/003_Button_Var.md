# 변수 및 버튼 사용하기

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

  

