# Section7:개체지향 프로그래밍1

> 중간고사 대비 주제
>
> 1. 접근 제어자(Access Modifier)

## 1. OOP란?

### 1.1. C++에서의 OOP vs Java에서의 OOP

- C++, Java에 공통적으로 존재하는 OOP요소
  - 클래스, 개체, 생성자, 함수 오버로딩, 힙에 개체 생성하기
- C++에만 존재하는 OOP요소
  - 스택에 개체 생성하기, 복사 생성자, 소멸자, 연산자 오버로딩
- C++에서는 OOP와 OOP 아닌 것을 섞어 쓸 수 있음
  - C와의 후방호환성을 가짐
- C++은 언매니지드 언어
  - Java와 달리 메모리를 직접 관리해준다

### 1.2. OOP가 뭘까?

- 사람이 세상을 바라보는 방식

  - 사람이 바라보는 물체는 '상태'와 '행동'으로 나눌 수 있음
    ![image](https://user-images.githubusercontent.com/22488593/174020221-4b0f00a8-8800-4857-a9f9-c3fc7fc0c5d8.png)
  - 사람은 '이름', '성별', '나이'라는 상태를 가지고  
    '먹기', '걷기', '말하기'라는 행동을 할 수 있음
  - 이를 코드로 바꾸면

  ```java
  public class Human
  {
    private string name;
    private int sex;
    private int age;

    public void eat();
    public void walk();
    public void talk();
  }

  Human student = new Human();
  Human teacher = new Human();
  ```

  - 클래스는 Human객체의 blueprint라고 보면 됨
  - 상태는 클래스의 멤버 변수가 되고
  - 행동은 클래스의 멤버 함수가 됨
  - student와 teacher은 서로 다르지만 사람이라는 공통된 특성을 지님
    - 각각 new Human()을 해줌으로써 특성이 다른 두 사람이 만들어진다
    - new 해주는 걸 인스턴스화 해준다고 함

### 1.3. 복잡해진 OOP

- 무언가를 만들 때는 단순한 부분은 최대한 단순하게 만들고 복잡해야만 하는 부분만 복잡하게 만들어야 함
- 일부 프로그래머들이 단순했던 개체지향 개념을 쓸데없이 복잡하게 만들기 시작 함
  - 일부 개체지향 분석과 디자인(OOAD)
  - 일부 디자인 패턴들

#### 1.3.1. 지양해야될 개체지향 개념 예시

![image](https://user-images.githubusercontent.com/22488593/174023663-981c262e-b916-44c4-983c-c21af161c263.png)

- 개체 B의 숫자를 증가시키고 싶으면 어떻게 해야할까?

#### 1.3.2. 복잡한 방법

- 호출자라는 다른 개체를 만듦
  - '명령'이라는 멤버와 '명령 추가', '명령 수행' 두 가지의 함수만 가지고 있음
- '증가명령'이라는 또 다른 개체를 생성
  - '개체 B'라는 멤버와 '실행'이라는 함수를 가짊
    ![image](https://user-images.githubusercontent.com/22488593/174024519-9de4ebba-592f-4149-a7b3-3457bb7c610a.png)
- '호출자' 개체의 '멤버'에 '증가명령'개체를 '명령 추가'로 추가해 줌
- 그리고 '명령 수행' 함수를 실행한다
- 그러면 '증가명령'의 '실행' 이라는 함수가 실행 됨
- '실행'은 '개체 B'의 '증가'라는 함수를 호출해 줌
- 정말 복잡하다
  - 유지보수성과 가독성이 떨어짐
  - 하지만 웹프로그래밍에서 이렇게 해야할 때가 있다
- 이렇게 하는 이유
  - 옛날에는 오브젝트를 한번 만들어 놓으면 그 오브젝트를 절때 변경하지 말고 이것을 포함한 다른 오브젝트를 만들어 계속 크기를 키워가라는 이야기가 있었음
  - 이미 존재하는 오브젝트를 고치면 버그를 만들 수 있기 때문
- 근데 요즘은 이렇게 하지 않고 그냥 고친다

#### 1.3.3. 직관적인 방법

- 그냥 '개체 B'의 '증가'함수를 호출해주면 된다
  ![image](https://user-images.githubusercontent.com/22488593/174026464-4fc62c37-1651-4943-b0c7-a09f4a2f14c8.png)

Vector 클래스를 한 번 만들어보자

```c++
  class Vector
  {
    int mX;
    int mY;
  }
```

- 멤버 변수는 앞에 m을 붙여주는게 c++의 관습
  - 이렇게 하면 장점이 매개변수가 x, y가 있을 때 this.x 를 하지 않고도 구분이 가능함

## 2. 접근 제어자(Access Modifier)

- C++의 기본 접근권한은 private

```c++
 class Vector
  {
    int mX; // private 멤버 변수
    int mY; // private 멤버 변수
  }
```

- Java에서 접근권한 제어 키워드를 생략하면 public도 private도 아님
  - 그 변수는 해당 패키지 내에서 접근 가능
    - Java에서는 friend 키워드가 없어서 이게 생겼다고 함

### 2.1. C++의 접근 제어자(Access Modifier)

- public
  - 누구나 접근 가능
- protected
  - 자식 클래스에서 접근 가능
- private
  - 해당 클래스에서만 접근 가능(개체에서가 아님)
    - 내 자식 클래스는 내 나이를 못 보지만, 나와 같은 부모 클래스인 다른 부모는 내 나이를 볼 수 있음
- 클래스 헤더를 구성할 때 제어자 별로 멤버들을 그룹 짓는게 특징임

```c++
class SomeClass
{
public:
  int PublicMember;
protected:
  int mProtectedMember;
private:
  int mPrivateMember1;
  int mPrivateMember2;
}
```

## 3. 개체 생성, 스택/힙

- Java의 개체생성

  ```Java
  // 스택 메모리에 개체생성 불가능

  // 힙(heap) 메모리에 만들기 (느림)
  Vector a = new Vector();
  ```

- C++의 개체생성

  ```c++
  // 스택 메모리에 만들기 (빠름)
  Vector a;

  // 힙 메모리에 만들기 (느림)
  Vector* b = new Vector();
  ```

- C++은 힙 메모리에 개체를 생성하면 포인터에 저장해야한다

### 3.1 스택에 개체 생성

```c++
Vector a;
```

![image](https://user-images.githubusercontent.com/22488593/174066168-fec69096-0841-41b9-95bb-2e31c94c6453.png)

- mX, mY 각각 4바이트로 스택에 8바이트를 차지한다

### 3.2 힙에 개체 생성

```c++
Vector* b = new Vector();
```

![image](https://user-images.githubusercontent.com/22488593/174066474-1934a895-fdd4-4ace-b06b-e1d8980120bf.png)
![image](https://user-images.githubusercontent.com/22488593/174066519-2dff9b08-a247-424e-96a6-16249a44b329.png)

- 힙 메모리에 8바이트가 할당되고 스택에는 그것의 주소가 담긴 포인터가 있음
- 그림에서 스택은 힙 메모리 주소 5120를 갖고 있다

### 3.3 스택

- 미리 예약된 로컬 메모리 공간
  - 컴파일 옵션으로 스택 크기를 정해줄 수 있음
  - 보통 1MB 이하로 함
  - exe파일을 실행시키면 이 1MB 메모리를 할당 해주고 스택으로 사용하는 것
- 함수 호출과 반환이 이 메모리에서 일어남
  - A함수를 호출하면 A함수범위만큼 메모리를 차지함(스택 포인터가 올라감)
  - A함수가 B함수를 호출하면 A함수범위 위에 B함수가 메모리를 차지함(스택 포인터가 올라간다)
  - B함수의 호출이 끝나면 B함수범위만큼 스택 포인터를 내림
  - A함수의 호출이 끝나면 A함수범위만큼 스택 포인터를 내린다
  - 즉, 함수가 호출될때마다 스택에 쌓이고 반환되면 스택에서 제거됨
- 단순히 스택 포인터를 옮김
  - 메모리를 할당 및 해제할 필요가 없음
  - 스택에서 요소를 제거하려면 일일이 지울 필요 없이 스택 포인터만 아래로 내리면 됨
  - 변수와 매개변수를 위해 필요한 크기는 컴파일 도중에 알 수 있음 (?)
  - 스택에 힙보다 빠른 이유임
- 스택에 큰 개체를 많이 넣으면
  - 스택 오버플로우(overflow)가 발생할 수 있음
- 성능이 느려 질 수도 있음
  - 우리가 스택 크기를 1MB로 잡는다 하면 그것을 다쓰고있지 않는이상 그 1MB전체가 메모리에 들어가 있지 않다
  - 쓰는 부분만 메모리에 들어가 있고 나머지는 하드웨어에 스왑파일로 남아 있다
    - 페이징 기법
  - 나중에 메모리 전체를 사용할 때 그것을 불러오는데 성능이 느려질 수 있다
    - OS의 문제

### 3.4. 힙

- 컴파일 때 컴파일러가 알고 있는 메모리 공간이 아님 (?)
- 프로그램이 실행될 때 OS가 제공하는 방대한 메모리 공간
  - OS가 달라질 수 있고 하드웨어 즉 메모리 크기가 달라질 수 있어 가변적임
- 힙에 할당 하려면 할당하려는 크기에 맞는 비어 있고 연속된 메모리 블록을 찾아야 함
  - 메모리 전체를 다 둘러봐야해서 느리다
- 프로그래머가 메모리를 직접 할당 및 해제해야 함
  - 컴파일러는 힙 메모리를 얼만큼 필요한지 알 길이 없음
  - 프로그램 실행 중에 메모리가 할당 된다
  - 해제 안하면 메모리 누수 발생

### 3.5. 어디서 힙/스택을 쓸까

- 기본적으로 스택을 쓰는게 좋음
  - 오브젝트의 크기가 충분히 작고 어떤 범위 안에서만 쓸 때
- 힙을 쓰면 좋은 경우는
  - 실행중에 새로만드는 일이 많을 때
  - 한 함수 안에서 안끝나고 다른함수로 전달하는 일이 많을 때
  - 반환하는 오브젝트가 너무 클 때 힙에 오브젝트를 만들어 그 주소만 반환하면 됨
    - 큰 오브젝트를 복사해서 반환하는 것보다 낫다

### 3.6. RAII 원칙

- Resource Acqusition is Initialization
- new 한 함수가 new 개체를 지워준다는 원칙
  - new를 반환해버리면 호출자는 그것을 지워야 하는지 아닌지 잘 모르고 메모리 누수가 발생할 수 있어서
- 함수에서 new를 하고 이를 반환하는 건 안좋은 습관이다
  - Factory 패턴은 RAII가 적용되지 않음
    - 호출자가 create()로 오브젝트를 만들고 삭제해줘야 함

## 4. 개체 배열 생성, 개체 소멸

### 4.1 개체 배열(Array)

- Java

```java
Vector[] list = new Vector[10];
```

- C++

```c++
Vector* list = new Vector[10];
```

- 둘이 같은 걸까?
- Java의 new Vector\[10]은 Vector 포인터 10개를 만드는 것과 같다
  ![image](https://user-images.githubusercontent.com/22488593/174206418-600acfff-8dc2-48d2-8891-aec456568849.png)
- C++의 new Vector\[10]는 실제 Vector 객체 10개를 만들어 준다
  ![image](https://user-images.githubusercontent.com/22488593/174206531-518424a4-ce81-40d8-8404-65566f610853.png)

#### 4.1.1 벡터 포인터 배열 만들기

- Java

```java
// 10개의 "포인터"를 힙에 만듦
Vector[] a = new Vector[10];
```

- C++

```c++
// 10개의 포인터를 힙에 만듦
Vector** list = new Vector*[10];
```

- Vector\*\*에서 첫번째 것은 '배열'을 의미하고 뒤에 것은 '벡터포인터'를 의미함 따라서 \*\*는 '벡터 포인터 배열'

### 4.2 개체 소멸

- Java는 가비지 컬렉터가 개체를 언젠가 소멸시켜준다
- C++

```c++
Vector* a = new Vector;
Vector* list = new Vector[10];
// ...

delete a; // 메모리가 즉시 해제 됨
a = NULL; // a가 지워진 메모리라는 것을 NULL로 표시

delete[] list; // []를 반드시 넣을 것
list = NULL;
```

- C++는 메모리 해제를 안 하면 메모리에 평생 남아 있음 delete로 메모리를 해제시켜줘야 함
- delete 하고 NULL을 넣어준 이유는 다른 곳에서 a를 사용해버릴 수 있기 때문
- new배열을 해제할 때 []를 꼭 붙여주자
  - []를 붙여야 배열요소전체의 소멸자를 호출시켜줌

### 4.3 가비지컬렉터 vs 메모리 직접 해제

- 가비지컬렉터
  - 편함
  - 안전함
  - 느리다
- 메모리 직접 해제
  - 불편함
  - 안전하지 않음
  - 빠르다
- 가비지컬렉터가 느린 이유
  - 프로그램의 오브젝트를 다 돌면서 쓰이는지 아닌지 확인하고 메모리 해제해서

## 5. 멤버 변수 초기화

- Java

```java
public class Vector
{
 private int x;
 private int y;
}

Vector a = new Vector();
```

- Java는 멤버 변수를 0으로 초기화해준다
  - new Vector() 하면 x와 y가 0으로 초기화 됨
- C++

```c++
class Vector
{
private:
  int mX;
  int mY;
}

Vector a;
Vector* b = new Vector();
```

- x와 y가 0으로 초기화되지 않고 가비지 값을 가지고 있음
- 멤버변수를 초기화해주는 과정이 없어서 Java보다 더 빠름

### 5.1. Java가 멤버 변수를 0으로 초기화하는 방법

```java
Vector a = new Vector();
```

- Java 내부적으로 C로 구현된 초기화 과정을 코드로 짠다면 이렇게 될 것이다
  ```c
  void *ptr = malloc(sizeof(Vector)); // vector의 크기만큼 할당한 다음
  memset(ptr, 0, sizeof(Vector));  // 0로 memset 해줌
  a = (Vector*)ptr;
  ```
  - ptr의 모든 메모리를 0으로 memset하면 모든 멤버들이 0이 될까?
    - 그렇다.
    - integer 0의 비트 패턴은 0이고 reference type이 null일 경우 비트 패턴도 0이다.
    - IEEE754에 따르면 실수값 0.0은 비트 패턴이 0임

## 7. 생성자(Constructor), 초기화 리스트(Initializer List)

생성자는 객체가 생성될 때 자동적으로 호출되는 특수한 멤버함수이다

- Java의 생성자
  ```java
  public class Vector
  {
    private int x;
    private int y;
    // 매개변수 없는 생성자
    // 멤버가 0으로 초기화되어있으니 안 만들어도 상관없음
    public Vector()
    {
      x = 5;
      x = 1;
    }
  }
  ```
- C++의 생성자
  ```c++
  class Vector
  {
  public:
    Vector()
    {
      mX = 0;
      mY = 0;
    }
  private:
    int mX;
    int mY;
  }
  ```
  - c++ 멤버가 생성자에서 초기화해줘야 함
    - 하지만 위 방법은 잘못된 방법
      - 멤버 초기화는 초기화 리스트를 사용해야 함

### 7.1. Java클래스와 C++클래스가 private, public 순서가 다른 이유

- Java는 private, public 순서로 클래스를 구성함
  - Java에는 헤더파일이 없고 라이브러리를 사용할 때 헤더파일을 살펴보지 않음
  - 클래스를 만드는 사람의 편의성을 위해 주로 데이터가 있는 private를 public보다 앞에 위치시킴
- C++은 public, private 순서로 클래스를 구성함
  - C++ 라이브러리를 사용할 때 헤더파일을 먼저 보는데 내가 사용할 수 있는 public 부분을 주로 보기 때문에 public을 private보다 먼저 위치시킴

### 7.2. 초기화 리스트(Initializer List)

- 위의 생성자 예제는 대입을 이용해서 멤버를 초기화 했고 이는 잘못된 거임
- 초기화 리스트를 이용해서 멤버를 초기화하는게 옳음 방법임

```c++
class Vector
{
public:
  Vector()
  : mX(0)   // : 초기화 리스트 시작
  , mY(0)   // , 초기화 리스트에 넣을 멤버를 추가
  {
  }
private:
   int mX;
   int mY;
}
```

### 7.3. 초기화 리스트를 사용해야 하는 이유

- 대입은 오브젝트가 만들어지고 난 이후 실행이 됨
- 초기화 리스트는 오브젝트가 만들어 질 때 초기화가 되는 거임
- 이를 이용하여 **상수**나 **참조 변수**도 초기화 가능
  - 상수와 참조 변수 모두 선언 시 바로 초기화해줘야 함
  - 초기화 리스트를 사용하면 상수와 참조 변수가 선언될 때 초기화 할 수 있음

```c++
class X
{
  const int mNameSize;
  AnotherClass& mAnother;

  // 올바른 방법
  X(AnotherClass& another)
     : mNameSize(20)
     , mAnother(another)
  {
  }

  // 이렇게 하면 에러가 남
  X(AnotherClass& another)
  {
     mNameSize = 20;
     mAnother = another;
  }
}
```

### 7.4. 올바른 Vector클래스

```c++
// vector.h
class Vector
{
public:
  Vector();
  Vector(int x, int y);
private:
  int mX;
  int mY;
}

// vector.cpp
Vector::Vector()  // 클래스 구현
  : mX(0)
  , mY(0)
{
}

Vector::Vector(int x, int y)
  : mX(x)
  , mY(y)
{
}
```

- 매개변수가 없는 생성자는 안전을 위해 멤버를 0으로 초기화해주는게 좋음
  - 초기화 안된 변수는 쓰레기 값을 가지고 있어서 이를 사용하면 어떤 문제가 발생할 지 모름

## 8. 기본 생성자, 컴파일러가 하는 일

### 8.1. 기본 생성자

- 기본 생성자란 매개변수를 받지 않는 생성자를 의미
- 클래스에 생성자를 일부러 안 만들어 주면 컴파일러가 자동으로 기본 생성자를 만들어 줌
  ```c++
  Vector() {}   // 기본 생성자
  ```
- 이렇게 자동으로 만들어진 생성자는
  - 멤버 변수를 초기화하지 않음
  - 하지만 해당 개체가 포함하는 모든 개체의 생성자를 호출
- Java의 경우 객체를 포함하면 그것의 reference를 가지고 있으므로 null로만 초기화 해주면 됨
- 하지만 C++은 객체 그 자체를 포함하고 있는 경우가 있음 이 때는 그 객체의 생성자를 호출함

### 8.2. 컴파일러가 하는 일

- 사례1 - Vector 클래스에서 생성자가 없는 경우
  - 컴파일러가 기본 생성자를 만들어 준다
    ![image](https://user-images.githubusercontent.com/22488593/174223735-00152a7b-9fd2-4d2d-84e2-c4c86e9ac7cc.png)
- 사례2 - Vector 클래스에 생성자가 있는 경우
  - 컴파일러가 기본 생성자를 만들어주지 않는다
    ![image](https://user-images.githubusercontent.com/22488593/174223865-3d8e3425-8a2d-4be5-a2d2-153cf40d489f.png)

## 9. 생성자 오버로딩(Overloading), 소멸자(Destructor)

### 9.1 생성자 오버로딩

- 여러 개의 생성자를 만들 수 있음
- 같은 이름이지만 인자의 개수나 자료형이 다름
- 기본 생성자(매개변수가 없음)
  ```c++
  Vector() : mX(0), mY(0) {}
  Vector a; // 기본 생성자 호출
  ```
- 매개변수를 가지는 생성자

  ```c++
  // 2개의 매개변수를 가지는 생성자
  Vector(int x, int y)
    : mX(x)
    , mY(y)
  {
  }

  Vector a(1, 3); // 매개변수 목록이 일치하는 생성자 호출
  ```

### 9.2 소멸자(Destructor)

- 개체가 지워질 때 호출됨
- Java는 소멸자가 없다
  - 자동으로 가비지를 수집하기 때문에 소멸자 없음
- C++ 소멸자가 있음
  - C++ 클래스는 그 안에서 동적으로 메모리를 할당할 수도 있음
  - 그런 경우 필히 소멸자에서 메모리를 직접 해제해 줘야 한다

```c++
// vector.h
class Vector
{
public:
  ~Vector();
private:
  int mX;
  int mY;
};

// vector.cpp
Vector::~Vector()
{
}
```

- C++의 소멸자는 언제 호출될까
  - Stack에 개체를 만드는 경우 Stack에서 소멸될 때 소멸자가 호출
  - heap에 개체를 만드는 경우(new) delete 할 때 소멸자가 호출됨
- 소멸자는 오버로딩이 없음
  - 매개변수가 없는 하나의 상태로만 존재
- "가상 소멸자"라는 개념도 있음

### 9.3. 클래스 안에서의 동적 메모리 할당

- std::string 클래스를 직접 구현해보자

```c++
// MyString.h
class MyString
{
public:
  MyString();
private:
  char* mChars;
  int mLength;
  int mCapacity;
};

// MyString.cpp
MyString::MyString()
  : mLength(0)
  , mCapacity(15)
{
  mChars = new char[mCapacity + 1];
}

// main.cpp
void Foo()
{
    MyString name;
}

int main()
{
  Foo();
}
```

- 위 코드를 실행하면 메모리에서 어떤 일이 일어날까?
  ![image](https://user-images.githubusercontent.com/22488593/174230261-5be9a27d-ebc6-43e0-8db9-eaab791f51b6.png)
- 스택의 Foo()영역 안에 MyString 인스턴스인 name이 있음
  - 각각 mLength, mCapacity, mChars
- Foo()가 반환되면 무슨 일이 일어날까?
  ![image](https://user-images.githubusercontent.com/22488593/174230545-61b3705e-2c83-4395-b52d-4dc6e479d99e.png)
- Foo() 영역은 반환됬지만 여전히 힙 메모리에 mChars가 가리키던 메모리가 남아 있는 상황
  - 메모리 누수 발생!
- 메모리 누수가 발생하지 않기 위해서는 소멸자로 할당한 메모리를 해제해줘야 함

```c++
// MyString.h
class MyString
{
public:
  MyString();
  ~MyString();
private:
  char* mChars;
  int mLength;
  int mCapacity;
};

// MyString.cpp
MyString::MyString()
  : mLength(0)
  , mCapacity(15)
{
  mChars = new char[mCapacity + 1];
}

MyString::~MyString()
{
  delete[] mChars;
  // mCapacity == 0; 굳이 할 필요 없음
  // mChars == NULL; 굳이 할 필요 없음
}
```

- 소멸자를 추가해 메모리 누수가 일어나지 않는 코드

## 10. const 멤버 함수

- Vector 클래스의 멤버 함수

```c++
  class Vector
  {
  public:
    void SetX(int x);
    void SetY(int y);
    int GetX() const;
    int GetY() const;
    void Add(const Vector& other);
  private:
    int mX;
    int mY;
  }
```

- getter와 setter를 추가해보았음
- GetX()와 GetY()뒤에 const는 뭘까?

### 10.1. const란?

- 바꿀 수 없는 것을 말함
- const 변수는 초기화 후 대입을 할 수 없다
- const 메서드는 해당 개체 안의 어떠한 것도 바꿀 수 없음
  - 개체 안의 멤버 변수를 바꿀 수 없음

```c++
// const 변수
const int LINE_SIZE = 20;
LINE_SIZE = 10; // 컴파일 에러

// const 메서드
int GetX() const;
```

### 10.2. const 멤버 함수

- 멤버 변수가 변하는 것을 방지

```c++
int GetX() const
{
  return mX;
}

void AddConst(const Vector& other) const
{
  mX = mX + other.mX;  // 컴파일 에러
  mY = mY + other.mY   // 컴파일 에러
}
```

- 함수를 수정했는데 const가 있어서 컴파일 에러가 난다면 함수가 왜 const 메서드인지 다시 한번 생각해보는게 좋다
  - 고치면 안되는 함수일 수 있다

## 11. 구조체(Struct) vs 클래스(Class)

- C에서는 구조체(struct)를 데이터를 한 군데에 모아서 그룹 짓는 용도로 사용했다
  - C에서는 함수는 그룹화 할 수 없음
- C++에서는 데이터와 함수를 함께 그룹화할 수 있는 class라는 개념이 생김
- 그래도 C++에서는 구조체가 여전히 존재

### 11.1 구조체 vs 클래스

- C++에서 구조체와 클래스는 동일한 개념
  - C 구조체와 달리 데이터와 함수 같이 그룹화 시킬 수 있음
- C++에서 구조체와 클래스의 차이는 기본 접근권한이 다르다는 것 뿐임
  - 구조체 : public
  - 클래스 : private

```c++
// 구조체
struct Vector
{
  int X;  // public 멤버 변수
  int Y;  // public 멤버 변수
};

// 클래스
class Vector
{
  int mX;  // private 멤버 변수
  int mY;  // private 멤버 변수
}
// 위 코드는 안 좋은 코딩 스타일을 가지고 있음
// private 멤버 변수면 앞에 private라고 표시해 주는게 맞다
```

### 11.2 구조체에 관한 코딩표준

- C++에서는 구조체를 클래스 처럼 쓸 수 있음
  - 하지만 절대 그러지 말 것
  - 구조체는 C 스타일로 쓰자
- struct는 순수하게 데이터뿐이여야 함 (Plain Old Data, POD)
  - Plain Old Data는 int, float과 같은 단순한 옛날 데이터를 말함
    - 메모리 카피를 가능하게 하기 위하여
    - Animal\*과 같은 개체 포인터를 갖고 있으면 카피하기 힘듦
  - 사용자가 선언한 생성자나 소멸자 X
  - static아닌 private/protected 멤버 변수 X
    - 멤버 변수는 전부다 public으로
  - 가상 함수 X
  - 메모리 카피가 가능함
    - memcpy()를 사용하여 struct를 char[]로, 혹은 반대로 복사할 수 있음
  - 표준은 회사마다 다르며 어느 회사는 생성자와 소멸자는 허용하고 함수 생성만 막는 경우도 있음
    - 이 경우 메모리 카피는 불가능함
