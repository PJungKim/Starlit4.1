# 이거 왜 되지?


## Starlit 4.1 Releases!

- Starlit 4.1 수정사항

  - Starlit 3에서의 문법도 거의 그대로 사용 가능

    ```
    main(){
        a1 = float;
        r = float;
        "첫째항 : " >> OLED >> a1 << "\n";
        "공비 : " >> OLED >> r << "\n";
        thr = a1 * 0.000001;
        sum = float;
        if(r <= 0 || r >= 1){
            OLED << "/r발산!";
        }
        else{
            while(a1 > thr){
                sum += a1;
                a1 *= r;
            }
            OLED << f"/g급수의 합= {sum:.4f}";
        }
    }
    ```

  - 중괄호를 빼도 유효하다!

    ```
    main()
        a1 = float;
        r = float;
        "첫째항 : " >> OLED >> a1 << "\n";
        "공비 : " >> OLED >> r << "\n";
        thr = a1 * 0.000001;
        sum = float;
        if(r <= 0 || r >= 1)
            OLED << "/r발산!";
        else
            while(a1 > thr)
                sum += a1;
                a1 *= r;
            OLED << f"/g급수의 합= {sum:.4f}";
    ```

  - 중괄호를 뺐으면 세미콜론도 과감하게 지우자!
  
    ```
    main
        a1 = float
        r = float
        "첫째항 : " >> OLED >> a1 << "\n"
        "공비 : " >> OLED >> r << "\n"
        thr = a1 * 0.000001
        sum = float
        if(r <= 0 || r >= 1)
            OLED << "/r발산!"
        else
            while(a1 > thr)
                sum += a1
                a1 *= r
            OLED << f"/g급수의 합= {sum:.4f}"
    ```

- [디자인 가이드라인](https://github.com/PJungKim/Starlit3/tree/main/docs/Starlit4.1.pdf)