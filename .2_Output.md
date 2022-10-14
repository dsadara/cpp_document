# Section2: 출력(Output)

> 중간고사 대비 주제
>
> 1. cout 출력 포맷팅

## 1. C++에서의 Hello World 출력

C에서의 Hello World 출력

```c
printf("Hello, %s%d\n", "World", 123);  // Hello, World123 출력
```

C++에서의 Hello World 출력

```c++
std::cout << "Hello, " << "World" << 123 << std::endl;  // Hello, World123 출력
```

## 2. namespace

- 여러가지 함수나 클래스들을 하나로 뭉치는 역할
- Java의 패키지나 C#의 네임스페이스와 비슷
- 함수, 클래스의 이름 충돌을 방지할 수 있음
  - get 함수가 있다고 하면 네임스페이스A의 get 함수와 네임스페이스B의 get 함수 둘 다 존재할 수 있다

### 2.1 namespace 예제

```c++
// Hello1.h
void SayHello();

// Hello2.h
void SayHello();

// Main.cpp
#include "Hello1.h"
#include "Hello2.h"

// ..

SayHello();   // 컴파일 오류
```

- 컴파일 오류가 난다
- namespace를 쓰면 이 문제를 해결 가능

```c++
namespace hello
{
  void SayHello();
}

namespace hi
{
  void SayHello();
}

// ...

hello::SayHello();
hi::SayHello();
```

### 2.2 using 지시문

- 타이핑의 양을 줄이는 방법
  - std::cout 대신 cout만 써도 된다.
- Java의 import나 C#의 using과 비슷.

```c++
#include <iostream>

using namespace std;

int main()
{
  cout << "Hello, World!" << endl;
  return0;
}
```

## 3. << 연산자

- 정식명칭은 Insertion연산자
- 일명 밀어넣기 연산자, push연산자, 출력 연산자라고도 함
- \+ 혹은 \- 등과 같은 연산자 중 하나
- 비트쉬프트 연산자와 겹치지 않을까?
  - std::cout에 << 연산자를 쓰면 밀어넣기 연산을 함
  - 이게 되는 이유는 C++에서는 연산자 오버로딩을 통해 연산자의 동작을 바꿀 수 있기 때문

## 4. output Formatting 1

조정자(Manipulator)를 이용한 16진수 출력

```c++
int number = 10;
cout << showbase << hex << number << endl;
```

- showbase : 몇 진수인지 보여주기
- hex : 16진수로 출력

### 4.1 조정자

- showpos/noshowpos
  - +를 출력 또는 안함
  - number는 123 이라고 하자
  ```c++
  cout << showpos << number; // +123
  cout << noshowpos << number; // 123
  ```
- dec/hex/oct
  - 각각 10진법/16진법/8진법으로 출력
  ```c++
  cout << dec << number; // 123
  cout << hex << number; // 7b
  cout << oct << number; // 173
  ```
- uppercase/noupppercase
  - 대문자 또는 대문자로 출력
  ```c++
  cout << uppercase << hex << number; // 7B
  cout << nouppercase << hex << number; // 7b
  ```
- showbase/noshowbase
  - 진법을 표기 또는 안함
  ```c++
  cout << showbase << hex << number; // 0x7b
  cout << noshowbase << hex << number; // 7b
  ```
- left/internal/right
  - 왼쪽정렬/내부정렬/오른쪽정렬
  - 정렬을 하기위해 setw(6)로 컬럼 수를 정해줌
  ```c++
  cout << setw(6) << left << number;     // |-123  |
  cout << setw(6) << internal << number; // |-  123|
  cout << setw(6) << right << number;    // |  -123|
  ```
- showpoint/noshowpoint
  - 소수점 이하를 보여주거나, 안보여줄 수 있으면 보여주지 않는다
  - decimal1의 값이 100.0, decimal2의 값이 100.12라고 하자
  ```c++
  cout << nowshowpoint << decimal1 << " " << decimal2;  // 100 100.12
  cout << showpoint << decimal1 << " " << decimal2;     // 100.000 100.120
  ```
- fixed/scientfic
  - 실수를 고정적 표기법, 과학적 표기법으로 표기해줌
  - number의 값이 123.456789라고 하자
  ```c++
  cout << fixed << number;      // 123.456789
  cout << scientific << number; // 1.234568E+02
  ```
  > - 고정적 표기법: 숫자에 점 찍고 절댓값이 한번에 보이게 한 표기법
  > - 과학적 표기법: 유효한 값 하나를 정수부로 두고 나머지를 소수점으로 만든 다음 몇승을 해야 하는지 표기하는 방법
- boolalpha/noboolalpha
  - 불리언값을 알파벳으로 표기 또는 Cstyle(0 또는 1)로 표기
  - bReady의 값이 true라고 하자
  ```c++
  cout << boolalpha << bReady;     // true
  cout << noboolalpha << bReady;   // 1
  ```

### 4.3 \#include \<iomanip\> 안에 있는 조정자

iomanip에 있는 조정자의 특징은 함수처럼 매개변수를 받음

- setw()
  - 컬럼 수를 정해줌
  - 출력하고 남은 컬럼은 스페이스(space)로 채워줌
    - setfill()로 어떤 문자로 채워줄지 정할 수 있음
- setfill()
  - setw()시 빈 컬럼을 다른 문자로 채워줌

```c++
cout << setfill('*') << setw(5) << number; // **123
```

- setprecision()
  - 소수점을 보여줄 때 유효한 자리 몇자리 까지 보여줄꺼냐
  - number의 값이 123.456789이라고 하자
  - 9자리의 수인 number를 setprecision(7)을 넣어주면 7자리만 보여준다

```c++
cout << number; // 123.456789
cout << setprecision(7) << number; // 123.4568
```

## 5. output Formatting 2

### 5.1 cout 멤버 메서드

- << 연산자와 조정자로 했던 일을 cout 멤버 메서드로도 할 수 있다
- cout.setf() : set flag, 매개변수에 해당하는 플래그를 세트(set) 해줌
  - unsetf() 함수도 있다
- cout.width() : setw(5) 조정자와 같은 역할
  - fill(), precision() 함수들도 있다

![image](https://user-images.githubusercontent.com/22488593/173506528-0994492c-65ac-4932-bfcc-d37ab7c2dec0.png)
