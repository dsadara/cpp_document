# Section9: 개체지향 프로그래밍3

> 중간고사 대비 주제
>
> 1. 다형성
> 2. 추상 함수
> 3. 추상 소멸자
> 4. 추상 클래스

## 1. 상속
* 동물 예시
```c++
// Animal.h
class Animal
{
public:
  Animal(int age);
private:
  int mAge;
};

// Cat.h
class Cat : public Animal
{
public:
  Cat(int age, const char* name);
private:
  char* mName;
};

// Cat.cpp
Cat::Cat(int age, const char* name)
    : Animal(age)     // 초기화 리스트에서 부모 생성자를 호출해 줌
{
  size_t size = strlen(name) + 1;
  mName = new char[size];
  strcpy(mName, name);
}
```
  * \"cat\" is a \"Animal\" 상속관계 라고 함 
### 1.1. 상속이란
* 다른 클래스의 특성들을 내려 받음
  * 베이스(base) 클래스
    * 부모 클래스 
  * 파생(derived) 클래스
    * 자식 클래스
* 파생 클래스의 개체는 다음의 것들을 가짐
  * 베이스 클래스의 멤버 변수
  * 베이스 클래스의 멤버 메서드
  * 자신의 생성자와 소멸자
* 파생클래스는 멤버 변수 및 메서드 추가가능
### 1.2. 파생 클래스 정의하는 법
![image](https://user-images.githubusercontent.com/22488593/174575407-977384c1-9ecd-41ce-b1ee-bea838ba893a.png)
```c++
class Cat : public Animal {};
class Honda : private Car {};
class AndroidPhone : protected Phone {};
```
* 보통 베이스 클래스의 접근제어자는 public으로 많이 사용한다
### 1.3. 파생 클래스의 접근 제어자
* public 상속
  * 부모 클래스의 public 메서드를 나와 나를 이용하는 외부 사람이 public으로 접근 가능함, protected는 protected, private는 private
  * public으로 상속 받으면 부모의 접근 제어자를 그대로 따르는 것이라고 생각하면 됨
* private 상속
  * 부모가 public, protected 메서드를 private으로만 접근가능
* protected 상속
  * 부모가 public 메소드를 protected로만 접근 가능
![image](https://user-images.githubusercontent.com/22488593/174576270-51c8b3cf-37b3-4801-8334-db7896cbe3dd.png)
#### 1.3.1. 예\: public 상속과 private 상속
```c++
// Animal.h
class Animal
{
public:
  Animal(int age);
  void Move();
private:
  int mAge;
};

// Cat.h

// Animal을 public으로 상속 
class Cat : public Animal
{
public:
  Cat(int age, const char* name);
private:
  char* mName;
}

// Animal을 private로 상속
class Cat : private Animal
{
public:
  Cat(int age, const char* name);
private:
char* mName;
}
```
* 이것을 호출하면 어떻게 될까?
  ```c++
  Cat myCat;
  myCat.Move();
  ```
* public 상속은 myCat.Move()가 호출 됨
* private 상속은 myCat.Move()가 호출 안 됨
  * Animal의 public 함수가 private로 바뀌었기 때문 
### 1.4. 상속 시 메모리 뷰
```c++
// Animal.cpp
Animal::Animal(int age)
  : mAge(age)
{
}

// Cat.cpp
Cat::Cat(int age, const string& name)
  : Animal(age)
{
  size_t size = strlen(name) + 1;
  mName = new char[size];
  strcpy(mName, name);
}

// main.cpp
Cat* myCat = new Cat(2, "Mew");
```
* 위 코드를 실행하면 메모리에서 어떤 일이 일어날까?
1. 먼저 부모님의 생성자를 호출하고 부모를 초기화 함
  ![image](https://user-images.githubusercontent.com/22488593/174580938-6b7d23f8-6511-4b2c-97cc-6d125e9731f3.png)
  * 부모님의 mAge가 2로 초기화 됨
2. 그 다음 자식을 초기화 함
  ![image](https://user-images.githubusercontent.com/22488593/174581082-8640c9ed-d6e1-403a-ba86-b8bee1556370.png)
  * 메모리를 할당하고 mName에 주소값이 들어간다
* 항상 부모의 메모리가 앞에 위치해 있다
  * 부모의 생성자를 먼저 호출하는 이유
  * 이 특성으로 부모 포인터로 자식을 저장할 수 있는 이유
* Cat은 언제부터 자신의 메모리가 시작해야하는지 알고 있음
  * animal의 멤버 바이트 수 이후가 Cat 메모리의 시작임
  * Cat은 Animal의 헤더파일을 포함하고 있어 animal 멤버 바이트 수 를 알 수 있다 
## 2. 다형성(Polymorphism)
* Poly: 여러가지
* morp: 모습
* 즉, 여러가지 모습으로 변할 수 있다는 것을 의미 
### 2.1 멤버 함수의 메모리
#### 2.1.1 예시 
```c++
class Animal
{
public:
  // ...
  int GetAge();
private:
  int mAge;
};

class Cat : public Animal
{
public:
  // ...
  const char* GetName();
private:
  char* mName;
};

// main.cpp
// 각 값은 스택 또는 힙에 위치에 있음
Cat* myCat = new Cat(5, "Coco");
Cat* youtCat = new Cat(2, "Mocha");

myCat->GetName();   // 어디에 저장되어 있을까?
yourCat->GetName();
```
* myCat과 yourCat의 메모리 뷰
![image](https://user-images.githubusercontent.com/22488593/174705528-7258439f-3a2a-4fee-a489-21cde6e82e51.png)
  * mAge, mName이 저장되어 있음
  * 멤버함수는 어디에 저장되어 있을까?
#### 2.1.2. 멤버 함수의 메모리
* 멤버 함수도 메모리 어딘가에 위치해 있음
  * 모든 것이 메모리 어딘가에 위치해 있다
* 그런데 각 개체마다 멤버 함수의 메모리가 잡혀 있을까?
  ```c++
  myCat->GetName();   
  youtCat->GetName();   // 이 둘의 동작은 완전히 일치한다 그렇다면?
  ```
* 그 대신 각 멤버 함수는 컴파일 시에 딱 한 번만 메모리에 "할당" 됨
  * 저수준에서는 멤버 함수는 전역 함수와 그다지 다르지 않음
  * 함수에 개체의 오브젝트를 매개변수로 전달해 줌
#### 2.1.3. 멤버 함수의 저수준 뷰
```c++
// main.cpp
myCat->GetName();
// 어셈블리어
// mov  ecx, dword ptr[myCat] // myCat 주소를 저장
// call Cat::GetName(0A16C7h) // GetName()메소드 호출

yourCat->GetName();
// 어셈블리어
// mov  ecx, dword ptr[yourCat] // yourCat 주소를 저장
// call Cat::GetName(0A16C7h)   // 
```
* 함수의 메모리 뷰
   ![image](https://user-images.githubusercontent.com/22488593/174709239-3f199d48-a5df-462e-896f-579afe4d8008.png)
  * GetAge(), GetName()에 객체 매개변수가 추가되어 있다
  * myCat yourCat 둘 다 같은 메모리 0x0A16C7을 호출하는 모습을 볼 수 있다
### 2.2. 함수 오버라이딩
* Animal의 종류에 따라 말하는게 다르다
  ![image](https://user-images.githubusercontent.com/22488593/174709912-3e346162-c570-4d4c-a35e-d76368928dca.png)
  * 말을 하는 행위는 어느 Animal에나 있지만 무슨 말을 할지는 각 자식이 결정하게 하는 것
  * 오버라이딩(override)라는 말 자체가 무언가를 덮어 쓴다는 뜻
* 이를 코드로 짜면
```c++
void Animal::Speak()
{
  std::cout << "Animal speaking" << std::endl;
}

void Cat::Speak()
{
  std::cout << "Meow" << std::endl;
}

void Dog::Speak()
{
  std::cout << "Woof" << std::endl;
}
```
#### 2.2.1 Java vs C++ 상속 차이
```java
public class Animal
{
  // "An animal is speaking"을 출력
  public void Speak() {}
}
public class Cat extends Animal
{
  // "Meow"을 출력
  public void Speak() {}
}
Cat myCat = new Cat(2, "Coco");
myCat.Speak();  // Meow 출력
Animal yourCat = new Cat(5, "Mocha");
yourCat.Speak();  // Meow 출력
```
```c++
class Animal
{
public:
  void Speak(); // "An animal is speaking" 출력
}
class Cat : public Animal
{
public:
  void Speak(); // "Meow"을 출력 
}

Cat* myCat = new Cat();
myCat->Speak();   // Meow 출력 
Animal* yourCat = new Cat();
yourCat->Speak();   // "An animal is speaking" 출력
```
* Java는 부모가 자식을 담고 있을 때 오버라이드 함수를 호출하면 자식의 것이 호출된다
* C++은 부모의 것이 호출됨
* Java는 실체를 따라가고 C++은 무늬를 따라 호출이 된다
## 3. 정적 바인딩
## 4. 동적 바인딩
## 5. 가상 소멸자
## 6. 다중(Multiple) 상속
## 7. 추상(Abstract) 클래스
## 8. 인터페이스(Interface) 
