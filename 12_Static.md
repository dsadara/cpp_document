# Section12: static 키워드

> 중간고사 대비 주제
>
> 1. static 변수

## 1. 정적 멤버 변수

### 1.1 Java vs C++
* Java
  * 멤버 메서드 안 정적 변수 
  * 정적 멤버 함수
  * 정적 블록(block)
  * 정적 내포(nested) 클래스
* C++
  * 정적 변수
    * 멤버 함수 안
    * 멤버 아닌 함수 안
  * 정적 멤버 함수

### 1.2 static
* 범위(scope)의 제한을 받는 전역 변수
  * 전역 변수는 실행중에 딱 하나만 존재함
  * 전역 변수지만 영역이 전역이 아닌
* 어떤 범위?
  * 파일 속
    * 컴파일 되는 파일 하나 속에 
  * 네임스페이스 속
    * 네임스페이스 중괄호 안에 
  * 클래스 속
  * 함수 속 
* Java에서도 static 변수가 존재한다
  * OOP에 맞지 않는 개념이지만 static이 필요할 때가 있음
### 1.3 extern 키워드 
* 다른 파일의 전역변수에 접근을 가능케 해 줌
```c++
// ExternTest.h
extern int globalValue;
void IncreaseValue();

// ExternTest.c
#include "ExternTest.h"

int globalValue = 2;
void IncreaseValue()
{
  globalValue++;
}

// Main.c
#include "ExternTest.h"

int main()
{
  printf("%d", globalValue);
  // ...
}
```
* ExternTest.h의 내용은 그대로 #include "ExternTest.h"에 복사가 됨 
* Main.c의 globalValue는 컴파일 시에는 빈칸으로 존재하다가   
ExternTest.obj와 Main.obj가 합쳐지는 링크 과정에서 채워진다
* extern을 쓰면 어디서나 전역변수에 접근이 가능하다는건데 내가 아님 남이 extern을 써서 쉽게 가져가는게 좀 맘에 안 들 수도 있다
   * 이 때는 전역 변수에 static을 쓰면 외부에서 함부로 접근 못하고 링커 에러가 남
### 1.4 함수 속 정적 변수
* 함수로 범위(scope)가 제한된 전역 변수
  * 변수 result는 Accumulate() 안에서만 접근이 가능하다 
```c++
void Accumulate(int number);
{
  static int result = 0; // 변수 초기화는 한번만 가능 
  result += number;
  std::cout << "result = " << result << std::endl;
}

int main()
{
  Accumulate(10);   // 10
  Accumulate(20);   // 30
  return 0;
}
```
  * Accumulate(20) 실행 시 static 변수 초기화를 건너 뜀
* Java는 함수 속 정적변수가 없다
### 1.5 정적 멤버 변수 Java vs C++
* Java
    ```java
    public class Cat
    {
      private static int count = 0;
      Cat()
      {
        count++;
      }
    }
    ```
      * Java의 정적 멤버 변수는 객체 생성 전에도 접근할 수 있음
        * 클래스에는 있지만 오브젝트의 멤버 변수는 아님
* C++
    ```c++
    // Cat.h
    class cat
    {
    public:
      Cat();
    private:
      static int mCount;
    };

    // Cat.cpp
    int Cat::mCount = 0;
    Cat::Cat()
    {
      mCount++;
    }
    ```
* C++의 정적 멤버 변수는 \.cpp에서 초기화 해줘야 한다
### 1.6 정적 멤버 변수 정의
* 클래스당 하나의 COPY만 존재
* 개체의 메모리 레이아웃의 일부가 아님
* 클래스 메모리 레이아웃에 포함
* exe파일 안에 필요한 메모리가 잡혀 있음
  * 컴파일러가 이 변수의 인스턴스가 몇 개 존재해야 하는지 알기에
    * 1개
### 1.7 정적 멤버 변수 베스트 프랙티스
* 함수안에서 정적 변수를 넣지 말 것
  * 클래스 안에 넣을 것
    * 정적 변수를 보러 함수 구현체를 볼 필요 없고, 헤더파일만 찾아보면 된다 
* 전역변수 대신 정적 멤버 변수를 쓸 것
  * 전역변수는 누군가 extern으로 가져다 쓸 수 있어서 
  * 범위(scope)를 제한하기 위해+ 
* C스타일의 정적변수를 쓸 이유가 이제 없음
## 2. 정적 멤버 함수
* 논리적인 범위(scope)에 제한 된 전역 함수
   * 클래스 안 
* 해당 클래스의 정적 멤버에만 접근 가능
* 개체가 없이도 정적 함수를 호출할 수 있음
  ```c++
  Math::Square(10);
  ```
  * 대신 개체 멤버에 접근 불가능
    * 개체가 여러 개 있다고 하면 그 중 누구 멤버를 접근할지 알 수 없다
    ```c++
    // Cat.h
    class Cat
    {
    public:
      Cat(const std::string& name);
      static const char* GetName();
    private:
      char* mName;
      static int mCount;
    }
    
    // Cat.cpp
    int Cat::mCount = 0;
    Cat::Cat(const std::string& name)
    {
      ++mCount;
    }
    
    const char* Cat::GetName()
    {
      return mName; // 컴파일 오류 
    }
    ```
