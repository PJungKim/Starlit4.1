# Starlit 4.1

- Starlit의 새로운 버전, Starlit 4
- [설명서(기본 문법)](Starlit4.md)

## 1. 연혁

- 2019년 LED 매트릭스 디스플레이를 제어하기 위한 유사 코드로써 Dot Project 제작(現 Starlit 1).
- 2022년 차기 버전인 `Starlight 2` 설계
- 2023년 차기 버전인 `Starlit 3` 설계
- 2024년 4월 1일, 컴파일러 안정화 후 `Starlit 3.3` 출시
- 2025년 4월 1일, `Starlit 4` 출시 예정

## 2. 디자인 철학

### 전혀 달라 보이지만, 같은 언어로 작성한 코드이다.
Even if it looks different, This is written in same language.
- 같은 언어로 실행 결과가 같은 코드는 다양한 형태로 작성할 수 있다.
```Py
f"Hello, World!"
```
```Py
main
    f"Hello, World!"
```
```C++
main(){
    f"Hello, World!";
}
```

### 다양성을 인정할 때 비로소 깔끔한 코드가 탄생할 수 있다.
When approving diversity, The clean code can be developped.
- 함수의 목적에 맞게 스타일을 다르게 할 수도 있고, 코드에 따라 스타일을 다르게 정의해도 좋다.

```Py
SumInts(a:int, b:int):int{
    sum = 0;
    for(i:a:b)
        sum += i;
    return sum;
}

main:
    a = int
    b = int
    "a입력 - " >> OLED >> a << "\n"
    "b입력 - " >> OLED >> b << "\n"
    s = AddFrom(a)to(b)
    f"a~b의 모든 정수의 합은 {s}입니다."
```

### Pythonic은 가장 권장되는 코드 스타일이지만, 하나의 스타일일 뿐이다.
Even if the most recommended style is Pythonic Style, But it is just the one of lots of styles.
- Pythonic하게 작성하는 쪽이 가장 간결하고 읽기 편하지만, 때로는 다른 스타일을 섞어 쓰는 것도 틀리지는 않다.
- Pythonic을 고집할 때보다 더 줄 개수를 줄일 수 있는 방식도 얼마든지 존재할 수 있다.
- 지나치게 줄 수를 줄이는 것 또한 권장되지 않는다.
```Py
main:
    a = int; b = int
    "a입력 - " >> OLED >> a << "\n"
    "b입력 - " >> OLED >> b << "\n"
    if(a>b){f"a가 b보다 큽니다.";}
    elif(a<b){f"a가 b보다 작습니다.";}
    else{f"a가 b는 같습니다.";}
```



## 3. 주요 업데이트사항

- 극단적인 자유도의 문법
- 상황에 맞게 유연하게 코딩할 수 있는 환경 제공 및 개발 속도 대폭 향상
  - Starlit 3 예제

    ```C++
    $import(oled);

    main(){
        OLED = OLED_Black();
        OLED << "Hello, World!\n";
        while(!BUTTON_Read()){}
    }
    ```

  - Starlit 4 예제
    ```Py
    main
        OLED << "Hello, World!\n"
    ```

