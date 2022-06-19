# Section8: 개체지향 프로그래밍2
> 중간고사 대비 주제
>
> 1. 복사 생성자 및 대입 연산자
> 2. 연산자 오버로딩
### 오브젝트 복사시 고려할 점
* Java에서 오브젝트의 사본을 만들기 위해서는 Clone()함수를 사용함
* Clone() 함수를 구현해본다고 하면 생각해봐야 할 점 이 있다
  1. 오브젝트가 가진 값들이 모두 value type이면 새로운 오브젝트를 만들고 그대로 대입해주면 되는데
  2. 오브젝트가 가진 값에 reference type이 있으면 두가지 선택지가 있다
    * reference type값의 원본을 두 오브젝트끼리 공유를 하는 방법이 있고
      * 이 경우를 얕은 복사(shallow copy)가 된다 하고 (원본의 변경이 복사본에 영향을 미친다)    
    * reference type값의 원본을 복사해서 새 오브젝트값에 넣어주는 방법이 있다
      * 이 경우를 깊은 복사(deep copy)가 된다고 한다 (원본의 변경이 복사본에 영향을 미치지 않는다)
* C++에서는 오브젝트 복사를 생성자 단에서 지원해준다
## 1. 복사(Copy) 생성자
* 같은 클래스에 속한 다른 개체를 이용하여 새로운 개체를 초기화
  * 같은 크기, 같은 데이터의 개체
  * <class-name> (const <class-name>&);
  ```c++
  Vector(const Vector& other);
  
  Vector a;   // 매개변수 없는 생성자를 호출
  Vector b(a);  // 복사 생성자를 호출
  ```
### 1.1 암시적(implicit) 복사 생성자
  * 코드에 복사 생성자가 없는 경우, 컴파일러가 암시적 복사 생성자를 자동생성
  ```c++
  // Vector.h
  class Vector
  {
  private:
    int mX;
    int mY;
  }
  ```
  * 아래와 같이 암시적으로 기본 생성자와, 복사 생성자를 만들어준다
  ```c++
  // Vector.obj
  class Vector
  {
  public: 
    Vector() {}
    Vector(const Vector& other)
      : mX(other.mX)
      , mY(other.mY)
  {
  }
  private:
    // ...
  }
  ```
  * 암시적 복사 생성자는 **얕은 복사(shallow copy)**를 수행
      * 멤버 별 복사
  * 각 멤버의 값을 복사 함
  * 개체인 멤버변수는 그 개체의 복사 생성자가 호출됨
    ```c++
    Vector(const Vector& other)
      : mX(other.mX)
      , mY(other.mY)
    {
    }
    ```
      * 개체에다가 같은 클래스의 다른 개체를 삽입하므로 복사 생성자가 호출된다
### 1.1.1 포인터는 얕은 복사가 된다 
* 클래스에 포인터 형 변수가 있다면 어떻게 될까?
* 예: ClassRecord 클래스
```c++
// ClassRecord.h
class ClassRecord
{
public:
  ClassRecord(const int* scores, int count);
  ~ClassRecord();
private:
  int mCount;
  int* mScores;
}
  
// ClassRecord.cpp
ClassRecord::ClassRecord(const int* scores, int count)
    : mCount(count)
{
  mScores = new int[mCount];
  memcpy(mScores, scores )
}
  
ClassRecord::~ClassRecord()
{
  delete[] mScores;  
}
```
* 암시적 복사 연산자의 모습
```c++
// ClassRecord.cpp
ClassRecord::ClassRecord(const int* scores, int count)
    : mCount(count)
{
  mScores = new int[mCount];
  memcpy(mScores, scores )
}
  
// 암시적 복사 생성자
ClassRecord::ClassRecord(const ClassRecord& other)
  : mCount(other.mCount)
  , mScores(other.mScores)
{
}
  
// Main.cpp
ClassRecord classRecord(scores, 5);
ClassRecord* classRecordCopy = new ClassRecord(classRecord);
delete classRecordCopy;
```
  * 암시적 복사 연산자를 사용하면 mScores를 같이 공유한다
  ![image](https://user-images.githubusercontent.com/22488593/174463416-1da32ac2-68a8-401f-a173-0f6804e7521d.png)
  * 이 상태에서 delete classRecordCopy를 실행하면 소멸자가 실행되어 mScores를 지워버린다
  * classRecord는 지워버린 메모리를 가리키고 있는 상태
  ![image](https://user-images.githubusercontent.com/22488593/174463464-a781ecf9-580b-461f-b968-ff450f5c8df1.png)
  * 개체끼리 데이터를 공유하는 얕은 복사가 위험한 이유
### 1.1.2. 사용자가 만든 복사 생성자
  * 클래스 안에서 동적으로 메모리를 할당하고 있다면? 얕은 복사가 일어나 위험할 가능성이 매우 높음
  * 이때는 직접 복사 생성자를 만들어서 깊은 복사(deep copy)를 할 것
     * 포인터 변수가 가리키는 실제 데이터까지도 복사
```c++
ClassRecord::ClassRecord(const ClassRecord& other)
  : mCount(other.mCount)
{
  mScores = new int[mCount];
  memcpy(mScores, other.mScores, mCount * sizeof(int));
}
  
// Main.cpp
// scores에 5개의 점수가 들어있다고 가정하자
ClassRecord classRecord(scores, count);
ClassRecord* classRecordCopy = new ClassRecord(classRecord);
delete classRecordCopy;
```
* 이 복사생성자를 호출하면 어떤 일이 일어날까?
![image](https://user-images.githubusercontent.com/22488593/174464007-d0e5de99-3e79-4429-a68a-e70a6eb0743b.png)
* 더 이상 mScores의 원본을 공유하지 않음
* delete를 실행하면 문제가 더 이상 발생하지 않는다
![image](https://user-images.githubusercontent.com/22488593/174464032-a3405e68-04d2-407b-9892-acee4f96061e.png)
## 2. 함수 오버로딩(overloading)
* C에는 존재하지 않고 Java, C++에만 존재
* 같은 이름의 메소드를 중복하여 정의하는 것을 의미
* 매개변수 목록을 제외하고는 모든 게 동일함 
  * 반환형은 상관 없음
  * 함수 이름과 매개변수가 같고 반환형이 다르면 컴파일 에러가 남
      * 컴파일러는 어떤 함수를 호출해야할 지 모르니깐
    ```c++
    void Print(int score);         // OK
    void Print(const char* name);  // OK
    void Print(float gpa, const char* name);    // OK
    int Print(int Score);   // 컴파일 에러
    int Print(float gpa);   // OK
    ```
### 2.1. 함수 오버로딩 매칭하기
  * 오버로딩 된 함수 중에 어떤 함수를 호출해야 하는지 판단하는 과정
  * 함수 매칭 결과는 3개가 있음
    1. 매칭하는 함수를 찾을 수 없음
      * 컴파일 에러
    2. 매칭하는 함수를 여러 개 찾음
      * 누굴 호출할지 모호하므로 컴파일 에러 
    3. 가장 적합한 함수를 하나 찾음
      * OK
  * 함수 오버로딩 매칭하기 문제1
  ```c++
  void Print(int score);    // (1)
  void Print(const char* name);   // (2)
  void Print(float gpa, const char* name);    // (3)
  
  Print(100);   // (1)이 호출됨 
  Print("Pope");  // (2)
  Print(4.0f, "Pope");   // (3)
  Print(77.5f);   // (1), float형에서 int형으로 암시적 형 변환이 일어남(컴파일 경고)
  Print("Pope", 4.0f);    // 컴파일 에러, 컴파일러가 매개변수 순서를 뒤집어주지는 않는다
  ```
   * 함수 오버로딩 매칭하기 문제2
    ```c++
    int Max(int, int);    // (1)
    int Max(double, double);    // (2)
    int Max(const int a[], size_t);   // (3)
    int Min(int, int);  // (4)

    int main()
    {
      std::cout << Max(1, 3.14) << std::endl;  
    }
    ```
  * Max(1, 3.14)는 (1)이 호출될까 (2)가 호출될까
  * 둘다 정확한 매치 1개, 표준 변환(형 변환) 1개로 모호하다
    ![image](https://user-images.githubusercontent.com/22488593/174466000-51d17bb2-2a1a-4a48-9bcf-85bfcab2922e.png)
  * 이 경우 컴파일 에러
## C++ 연산자의 종류
  * 단항(unary) 연산자
  ![image](https://user-images.githubusercontent.com/22488593/174466378-a96124b0-4427-4f8b-b804-72fbec684c51.png)
  ![image](https://user-images.githubusercontent.com/22488593/174466338-afd70ac4-f5f8-4258-b40a-f388ee8787e8.png)
  * 이항(binary) 연산자
  ![image](https://user-images.githubusercontent.com/22488593/174466371-d476d9a6-04e4-4d33-8d3d-9343a4cf2f15.png)
  ![image](https://user-images.githubusercontent.com/22488593/174466355-e50dcbd4-31b4-45de-a83a-7ad190842851.png)
  * 그 외 연산자
  ![image](https://user-images.githubusercontent.com/22488593/174466420-ccdf27b7-5b23-4c8b-a7ec-a7b8da9a918b.png)
## 3. 연산자(operator) 오버로딩
### 3.1. 연산자
* 연산자는 사실 함수처럼 작동한다
  * 단항(unary) 연산자와 이항(binary) 연산자는 매개변수의 차이라고 볼 수 있음
* C++은 프로그래머가 연산자를 오버로딩 할 수 있음
### 3.2. 연산자 오버로딩
  * 하나의 연산자를 여러 의미로 사용할 수 있게 해줌
  * C와 Java는 연산자 오버로딩을 지원하지 않음
    * Java는 두 문자열을 비교할 떄 equals()함수를 사용하지만 C++은 ==연산자로 비교할 수 있다
  ```c++
  int1 = int1 + int2;   // 두 int형 변수를 더함
  float1 = float1 + float2;   // 두 float형 변수를 더함
  name = firstName + lastName;  // 두 string형 변수를 더함
  ```
  * 연산자 오버로딩은 두가지가 있음
      * 멤버 함수
      * 멤버 아닌 함수(전역함수로)
#### 3.2.1. 멤버 함수를 이용한 연산자 오버로딩
* 연산자 역시 메서드임
  ```c++
  Vector sum = v1 + v2;
  Vector sum = v1.operator+(v2);    // 위와 동일
  std::cout << number;
  std::cout.operator<<(number);     // 위와 동일
  ```
* 특정 연산자들은 멤버 함수를 이용해서만 오버로딩이 가능
  * \=, \(), \[], \->
* Vector의 operator+() 연산자를 오버로딩해보자
  ```c++
  Vector result = vector1 + vector2;
  Vector result = vector1.operator+(vector2);   // 이게 가능하게 연산자 오버라이딩
  ```
```c++
// Vector.cpp
Vector Vector::operator+(const Vector& rhs) const
{
  Vector sum;
  sum.mX = mX + rhs.mX;
  sum.mY = mY + rhs.mY;

  return sum;
}
```
* rhs는 복사할 필요가 없으니까 참조로 전달
* sum을 반환하고 멤버를 수정할 일이 없으므로 const멤버함수로 선언
* 멤버 연산자 작성하는 법
  ![image](https://user-images.githubusercontent.com/22488593/174467498-5483391b-b16c-4f0d-9b60-95438ebd4320.png)
  ```c++
  Vector Vector::operator-(const Vector& rhs) const;
  Vector Vector::operator*(const Vector& rhs) const;
  Vector Vector::operator/(const Vector& rhs) const;
  ```
#### 3.2.2. 멤버 아닌 함수를 이용한 연산자 오버로딩
* 이런 짓을 할 수 있을까?
```c++
std::cout << vector1.mX << ", " << vector1.mY << std::endl;
```
  * mX, mY는 각각 private 멤버변수로 접근할 수 없다
* 다른 시도
```c++
std::cout << vector1.GetX() << ", " << vector1.GetY() << std::endl;  
```
  * 모든 멤버변수에 getter가 있는 것이 아님
  * 그리고 getter를 쓰기도 귀찮음
* 우리가 원하는 것
```c++
Vector vector1(10, 20);
std::cout << vector1;
// 출력결과: 10 20
```
* 아래와 같은 연산자를 만들어야 함
  ```c++
  #include <iostream>
  std::cout.operator<<(vector1);
  void cout::operator<<(const Vector& rhs) const
  {
    // ..
  }
  ```
* cout의 멤버 함수를 만들려는데 cout에 접근할 수 없음
  * ostream 클래스에 넣으면 되지 않나?
* 우리는 헤더파일만 접근할 수 있고 cpp파일에는 접근할 수 없다
* 이때는 전역함수로 연산자 오버라이딩 함수를 만들어 줘야 함
* 여기서 알 수 있는 사실
  * **좌변이 되는 개체의 접근 권한이 없을 때는 전역함수의 오버로딩이 필요하다**
* 이제 다시 Vector의 operator<<() 연산자를 만들어보자 
  ```c++
  void operator<<(std::ostream& os, const Vector& rhs)
  {
    os << rhs.mX << ", " << rhs.mY;
  }
  ```
  * 위 코드는 문제가 있음
    1. 전역함수를 어디에 넣어야 할지 모름
    2. Vector클래스의 private멤버에 대한 접근 권한이 없음
  * 위 문제를 해결하기 위해서 **friend**키워드를 사용하면 된다 
## 4. friend 키워드
  * 클래스 정의 안에 friend 키워드 사용 가능
      * 다른 클래스나 함수가 나의 private또는 protected 멤버에 접근할 수 있게 허용
      * 우리집 친구(friend)는 우리집의 비밀 금고(private 멤버)에 접근 권한이 있다
### 4.1. friend 클래스
  * 다른 namespace의 클래스가 해당 클래스의 private 멤버를 접근할 수 있게 함 
```c++
// X.h
class X
{
  friend class Y;   // friend 클래스 추가!
private:
  int mPrivateInt;
};
  
// Y.h
#include "x.h"
class Y
{
public:
  void Foo(X& x);
};
  
// Y.cpp
void Y::Foo(X& x)
{
  x.mPrivateInt += 10;  // OK  
}
```
* Java 객체지향쪽에서의 안티패턴임
  * 다른 클래스에서 private멤버를 접근한다는 것은 캡슐화 개념에 위배되는 느낌임
  * 큰 객체를 작은 객체로 나누는 경우가 있을 때 원래 공유하던 변수를 공유를 해야하는 때가 있는데
  * 이 때 getter와 setter를 쓰면 같은 패키지안 모든 클래스가 이에 접근이 가능하다는 문제가 생김
  * 이 경우에는 c++의 friend를 사용하면 특정 클래스만 접근 가능하게 할 수 있다
### 4.2. friend 함수
  * 클래스 바깥의 함수에 private 접근 권한을 줄 수 있음
      * 전역함수, 다른 클래스의 멤버함수
  * 클래스 안 함수 선언부에 함수 시그니처를 적고 앞에 friend 키워드를 붙이면 됨 
```c++
// X.h
class X
{
  friend void Foo(X& x);    // friend함수는 클래스 X의 멤버 함수가 아니다 
private:
  int mPrivateInt;
};

// GlobalFunction.cpp
void Foo(X& x)
{
    x.mPrivateInt += 10; // OK  
}
```
### 4.3. 멤버 아닌 연산자 오버로딩을 작성하는 법
  * 정리하자면 이렇다
  ![image](https://user-images.githubusercontent.com/22488593/174468942-de6a57a0-7299-46c2-b3a7-cfbac4f3e4a9.png)
### 4.4. friend를 이용하여 Vector의 Operator<<()을 다시 구현하면
```c++
// Vector.h
class Vector
{
  friend void operator<<(std::ostream& os, const Vector& rhs);
  public:
    // ..
  private:
    // ..
}

// Vector.cpp
void operator<<(std::ostream& os, const Vector& rhs)
{
  os << rhs.mX << ", " << rhs.mY;  
}
  
// main.cpp
Vector vector1(10, 20);
std::cout << vector1;
```
* friend 함수 선언은 보통 public 위에 위치함
  * 외부 함수이므로 접근 제어자가 의미가 없다
* 함수 구현부는 아무데나 넣어도 되지만 Vector하고 관련이 있으므로 Vector.cpp에 위치시켰다
### 4.5. 연산자 중첩 시 문제
```c++
void operator<<(std::ostream& os, const Vector& rhs);
  
std::cout << vector1 << std::endl;  // 컴파일 에러
```
* std::cout \<\< vector1 연산 시 함수의 반환값이 없어서 뒤의 << 연산을 하지 못함 
### 4.6. Chaining이 가능한 operator<<()
```c++
  std::ostream& operator<<(std::ostream& os, const Vector& rhs)
  {
    os << rhs.mX << ", " << rhs.mY;
    return os;
  }
```
  * 반환값을 void가 아닌 ostream으로 변경
  * ostream에 출력을 하고 그 ostream을 반환하면 된다
  * ostream에 무엇을 쓸 것이기 때문에 const가 아님 
## 5. 연산자 오버로딩과 const
### 5.1. 어디에나 const를 붙이자
```c++
Vector operator+(const Vector& rhs) const;
```
* **const**를 쓰는 이유?
  * 멤버 변수의 값이 바뀌는 것을 방지
  * 최대한 많은 곳에 const를 붙일 것
  * 함수, 매개변수, 지역(local) 변수에 까지도
    * POCU의 코딩 표준
      * 모든 회사가 이 코딩표준을 가지고 있는 건 아님
* 값을 바꿀 일이 없으면 const를 붙이는 게 좋다
## 5.2 코드가 길면 일어나는 일
```c++
  int func(int x)
  {
    // 엄청 긴 코드
  
    x++;  // 모르고 증감시킴
    
    // 엄청 긴 코드
    // ~~
    
    switch(x);  // 수정된 값이 들어감
  }
```
* 코드가 길어지면 모르고 x를 수정할 수도 있다
* 그리고 멤버인 x를 증감시키는지 매개변수인 x를 증감시키는지 알 수 없다
  * Java에서는 this를 붙여서 구분
  * C++은 접두사 m을 붙여서 구분
## 5.3. const &
  * const &를 사용하는 이유?
     * 불필요한 개체의 사본이 생기는 것을 방지
     * 멤버 변수가 바뀌는 것도 방지
     * 오브젝트의 상태를 바꿀 일이 없으면 무조건 const를 붙이자 
  ```c++
  Vector operator+(const Vector& rhs) const;
  std::ostream& operator<<(const std::ostream& os, const Vector& rhs);    // 잘못된 예
  ```
  * 두번째는 ostream에 출력을 해야 하므로 const를 붙이는 건 잘못된 것이다
## 5.4. 연산자 오버로딩에 const를 사용하지 않는 경우
```c++
vector1 += vector2;
vector1 = vector1.operator+=(vector2);
```
* \+\= 의 연산자 오버로딩 함수에는 const가 들어갈까?
  * 위 연산은 호출의 기본이 되는 좌항을 바꾸는 예 이므로 const가 들어갈 수 없음
```c++
Vector& operator+=(const Vector& rhs);
```
* \&을 붙여서 반환하면 장점이
  * 개체 복사 없이 반환할 수 있음
  * 연산자 여럿을 연결해서 쓸 수 있음(Chaining)
    ```c++
    vector1 += vector2 += vector3;
    ```
* 나 자신을 참조로 반환하는 법
```c++
// Vector.cpp
Vector& Vector::operator+=(const Vector& rhs)
{
  mX += rhs.mX;
  mY += rhs.mY;
  
  return *this;
}
```
  * this는 나 자신을 가리키는 개체이고 그 포인터가 가리키는 개체를 반환하면 된다
## 5.5. \&를 사용하면 메모리에서 일어나는 일
```c++
// Vector.cpp
Vector Vector::operator+(Vector& rhs)
{
  Vector r;
  
  r.mX = this->mX + rhs.mX;
  r.mY = this->mY + rhs.mY;
  
  return r;
}
  
// main.cpp
Vector v1(10, 20);
Vector v2(30, 40);
  
Vector result = v1 + v2;
```
![image](https://user-images.githubusercontent.com/22488593/174478170-5903e921-ff27-475b-bd83-beaddfdece6c.png)
* rhs는 v2의 주소를 가리키고 있고
* operator+()함수를 수행하면 r에는 각각 v1 + v2한 값이 들어있다
![image](https://user-images.githubusercontent.com/22488593/174478244-8ca62e1d-a02e-4a2b-8b36-c666e3eac820.png)
* 함수가 반환되고 result에 60 40이 대입된 모습
### 5.5.1. 매개변수로 \&을 사용하지 않으면 일어나는 일
![image](https://user-images.githubusercontent.com/22488593/174478353-b92696da-970a-4b25-ad84-4fe25bc262aa.png)
* 스택의 operator+()영역에 v2의 값이 그대로 복사되어있다
* \&를 쓸 때와 안 쓸 때 차이는 스택 메모리를 좀 더 쓰고 안쓰고의 차이만 있다
### 5.6. 연산자 오버로딩의 제한사항
  1. 오버로딩된 연산자는 매개변수로 최소한 하나의 사용자정의 형을 가져야 함
    * 기본 타입이면 이미 존재하는 연산자임 
  2. 오버로딩된 연산자는 피연산자 수를 동일하게 유지해야 함
    * 이항 연산자면 피연산자수를 한 개로 만들 수는 없음
    * 단항연산자를 오버로딩할 수는 있음
  3. 새로운 연산자 부호를 만들 순 없음
  4. 오버로딩할 수 없는 연산자도 존재
## 6. 연산자 오버로딩을 남용하지 말 이유
```c++
Vector vector = vector1 << vector2;
```
* 위 코드만 보면 무슨 일을 하는지 알 수 없다
  * 밀어넣기 연산자?
  * 비트 쉬프트?
* 연산자만 보고서 어떤 함수인지 유추하기 힘들다면
  * 차라리 **함수를 만들 것**
### 6.1 대입(assignment) 연산자
  * operator=
  * 이 연산자가 하는 일은 복사 생성자와 거의 동일
    * 복사 생성자는 A를 생성할 때 B를 대입
    * 대입 연산자는 A에 B를 대입
    * A를 새로 만든다는 것 말고는 차이가 없다
    * 대입연산자는 메모리를 해제해 줄 필요가 있을수도 있다
        * A에 string 클래스가 멤버로 있으면 A의 것을 지우고 B의 것을 복사 해 A에 대입해줘야 한다
    * 복사 생성자를 구현했다면 대입 연산자도 구현해야 할 것임
### 6.2. 암시적 operator=
  * Operator= 구현이 안 되어 있으면 컴파일러가 operator=연산자를 자동을 만들어 줌
    * 암시적 복사 생성자 처럼 얕은 복사로 멤버별로 값을 대입 해줌 
  ![image](https://user-images.githubusercontent.com/22488593/174479843-d567846f-fb75-428a-969a-9338f9b95606.png)
## 7. 암시적 함수들을 제거하는 법 
* 클래스에 딸려오는 기본 함수들
   * 매개변수 없는 생성자
   * 복사 생성자
   * 소멸자
   * 대입(=) 연산자 
* 이것들을 원치 않는다면?
## 7.1. 전통적인 방법
* 방법 1
  ```c++
  class Vector
  {
  public:
    Vector(const Vector& other);
  private:
    int mX;
    int mY;
  };
  
  // Main.cpp
  Vector v1; // 컴파일 에러 
  ```
  * 생성자를 만들어서 기본 생성자가 생성이 안되게 함
  * 기본 생성자를 지우는 법이라고 하기에는 좀 애매함
* 방법 2
  ```c++
  // Vector.h
  class Vector
  {
  public:
  
  private:
    Vector() {};
  
    int mX;
    int mY;
  };
  // Main.cpp
  Vector v1;    // 컴파일 에러 
  ```
  * 기본 생성자를 private에 넣어 외부에서 호출을 못하게 함
  * factory 패턴에서 쓰인다
    * 기본 생성자를 숨겨 new 대신 create()로만 생성하게 함
## 7.2. 암시적 복사 생성자를 지우는 법
```c++
class Vector
{
public:
  Vector();
private:
  Vector(const Vector& other) {}
  
  int mX;
  int mY;
};
  
// Main.cpp
Vector v1;
Vector v2(v1);    // 컴파일 에러 
```
## 7.3. 암시적 소멸자를 지우는 법
```c++
class Vector
{
public:
  
private:
  ~Vector() {}
  
  int mX;
  int mY;
}
  
// Main.cpp
Vector v1;      // 컴파일 에러
Vector* v2 = new Vector();
delete v2;      // 컴파일 에러
```
  * 암시적 소멸자를 지우면 개체를 지울 때 컴파일 에러가 난다
## 7.4. 암시적 operator=를 지우는 법
```c++
// Vector.h
class Vector
{
public:

private:
  const Vector& operator=(const Vector& rhs);
  
  int mX;
  int mY;
};
  
// Main.cpp
Vector v1;
Vector v2 = v2; // 컴파일 에러
```
  * 함수 구현을 안해도 됨 
