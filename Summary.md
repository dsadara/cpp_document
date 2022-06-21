# Section2
## namespace
* 함수 클래스들을 하나로 뭉침, 이름 충돌 방지 가능
## 조정자
1. showpos noshowpos : \+ 출력
2. dec hex oct
3. uppercase nouppercase
4. showbase noshowbase : 진법표기
5. left internal right
6. showpoint noshowpoint : 소수점 이하 표기
7. fixed scientific
8. boolalpha noboolalpha
### iomanip 조정자
1. setw() : 컬럼 수 설정
2. setfill() : 빈 컬럼 문자 설정
3. setprecision() : 표현 자리수
#### cout 멤버 메서드
1. setf() : 조정자 플래그 세트
2. width() : 컬럼 수 설정

# Section4
* 값에 의한 호출 : 복사본이 들어가 원본 변경불가능
* 참조에 의한 호출 : 참조를 전달해 원본 변경 가능 

## 참조
* 포인터의 안전한 형태
* 변수에 별칭을 붙여주는 역할
* NULL이 될 수 없음
* 초기화 중 반드시 대입 나중에 참조 대상 못 바꿈 

### 출력결과 매개변수
* 출력결과용 매개변수는 포인터로
    * \& 로 구분이 됨
    ```c++
    TryDivide(&a, b, c);
    ```
* 읽기전용 매개변수는 const 참조로
