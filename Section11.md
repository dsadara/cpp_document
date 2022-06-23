# Section11: 인라인 함수(Inline Functions)

> 중간고사 대비 주제
>
> 1. 인라인 vs 매크로
* 코드의 가독성과 성능 두가지 토끼를 잡은 것
## 1. 함수를 호출할 때의 많은 과정들
* 함수는 메모리 안에 \"할당\"되어 있음
* 함수를 호출하기 위해 필요한 단계들
  1. 변수들을 스택에 푸쉬(push)
  2. 함수 주소로 점프
  3. 함수를 실행
  4. 호출자 함수로 다시 점프
  5. 1번 단계에서 넣어뒀던 변수들을 팝
* 이렇게 많은 단계가 있다
  * 그래서 좀 느리다 
  * CPU캐시 최적이 아닐 수도 있음
    * 최근에 사용한 함수나 변수가 아닌 멀리 있는 것들을 사용하려면 메모리에서 읽어와야 함 그래서 시간이 좀 걸린다 
  * 모든 CPU 아키텍처에서는 더 느림
    * 그럼에도 아직 \"모든 걸 함수로 만들라\"라는 잘못된 조언이 떠돌아 다님
* 그럼에도 불구하고 함수를 쓰면 좋은 경우들이 있음
  * 재활용성이 있거나 가독성을 높일 수 있으면 
  ```c++
  Vector result = v1.Add(v2);
  ```
  ```c++
  Vector result;
  result.mX = v1.mX + v2.mX;
  result.mY = v1.mY + v2.mY;
  ```
  * 하지만 연산 그 자체는 매우 단순하기에
  * 함수호출에 필요한 오버헤드를 떠맡기는 좀 부담 됨
  * 해법이 없을까?
    * **인라인 함수**
## 2. 인라인(inline) 함수
* 함수 앞에 inline을 붙여주면 됨  
* 인라인 멤버함수
  ```c++
  // Animal.h
  class Animal
  {
  public:
    Animal(int age);
    
    inline int GetAge() const;
  }
  
  int Animal::GetAge() const
  {
    return mAge;
  }
  ```
* 멤버 아닌 인라인 함수
```c++
// Math.h
inline int Square(int number)
{
  return number * number;
}
```
### 2.1. 인라인 함수의 동작원리
* 사실상 복붙과 비슷
  * 함수호출 대신에
```c++
// Main.cpp
Cat* myCat = new Cat(2, "Coco");
int age = myCat->GetAge();  // 이거를 myCat.mAge로 바꿔준다 
```
  * 컴파일시 일어남
  * 컴파일러가 멤버에 접근권한을 체크함
* 매크로(MACRO)와 매우 비슷한 개념
  * \#define
    ```c++
    #define SQUARE(x) (x)*(x)
    
    int main()
    {
      int result = (3)*(3);
    }
    ```
    * 전처리시 일어남
* 그럼 대신 매크로를 써도 될까?
  * 쓰지말자..
  * 매크로는 디버깅하기 힘듦
    * 콜스택에 함수이름이 안 보임
    * 중단점(breakpoint)도 설정 불가능
  * 매크로는 범위(scope)를 준수하지 않음
    * 인라인 처럼 access check를 하지 않는다   
  * 정말정말 매크로를 쓸 이유가 있지 않는 한 인라인 함수를 쓰자
    * 매크로는 쓰는 부분은 어느 부분을 컴파일 하고 안할지 정할 때! 
* 인라인 함수 변환과정
  * 멤버 아닌 함수
    ![image](https://user-images.githubusercontent.com/22488593/174964593-91c0d8cc-3eb4-47b9-a67d-23def4e3fefd.png)
  * 멤버 함수
    ![image](https://user-images.githubusercontent.com/22488593/174964791-c5a303f9-92aa-47e7-9a60-330cc29a1fed.png)
### 2.2 인라인 함수를 쓸 때 주의점
* inline 키워드는 힌트일 뿐
  * 실제로는 인라인 안될 수도 있음
  * 컴파일러가 자기 맘대로 아무 함수나 인라인 할 수도 있음
  * 이것 때문에 \_\_forceinline 이라고 강제로 inline을 하게 해주는 키워드가 존재한다
    * \_\_가 붙여진 건 C++표준이 아닌 플랫폼 종속적인 것임
* 인라인 함수 구현이 헤더 파일에 위치해야 함
  * 복붙을 하려면 컴파일러가 그 구현체를 불 수 있어야 함
  * 각 cpp 파일은 따로 컴파일 됨
  * 따라서 b.h를 인클루드 하는 a.cpp파일을 컴파일 할 때, 컴파일러는 b.cpp에 뭐가 들어 있는지 모름  
* 간단한 함수에 적합
  * 특히 getter나 setter에
* 실행파일의 크기가 증가하기가 쉬움
  * 동일한 코드를 여러 번 복붙하니까
    * 많이 호출하는 함수는 인라인을 피하는 것이 좋다 
  * 남용하지 말 것
  * 실행파일이 작을수록 CPU 캐시하고 잘 작동 -> 속도가 빨라질 수 있다
### 2.3. 인라인 함수 사용법
* 멤버 아닌 인라인 함수
  ![image](https://user-images.githubusercontent.com/22488593/174965863-7db25dbd-6894-4181-9443-b2c62f11681b.png)
* 인라인 멤버함수
  ![image](https://user-images.githubusercontent.com/22488593/174965890-89235638-b2cf-48eb-9066-cb7673250b8f.png)
