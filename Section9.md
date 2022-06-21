# Section9: 개체지향 프로그래밍3

> 중간고사 대비 주제
>
> 1. 다형성
> 2. 추상 함수
> 3. 추상 소멸자
> 4. 추상 클래스

## 1. 상속

- 동물 예시

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

- \"cat\" is a \"Animal\" 상속관계 라고 함

### 1.1. 상속이란

- 다른 클래스의 특성들을 내려 받음
  - 베이스(base) 클래스
    - 부모 클래스
  - 파생(derived) 클래스
    - 자식 클래스
- 파생 클래스의 개체는 다음의 것들을 가짐
  - 베이스 클래스의 멤버 변수
  - 베이스 클래스의 멤버 메서드
  - 자신의 생성자와 소멸자
- 파생클래스는 멤버 변수 및 메서드 추가가능

### 1.2. 파생 클래스 정의하는 법

![image](https://user-images.githubusercontent.com/22488593/174575407-977384c1-9ecd-41ce-b1ee-bea838ba893a.png)

```c++
class Cat : public Animal {};
class Honda : private Car {};
class AndroidPhone : protected Phone {};
```

- 보통 베이스 클래스의 접근제어자는 public으로 많이 사용한다

### 1.3. 파생 클래스의 접근 제어자

- public 상속
  - 부모 클래스의 public 멤버를 나와 나를 이용하는 외부 사람이 public으로 접근 가능함, protected는 protected, private는 private
  - public으로 상속 받으면 부모의 접근 제어자를 그대로 따르는 것이라고 생각하면 됨
- private 상속
  - 부모가 public, protected 멤버를 private으로만 접근가능
- protected 상속
  - 부모가 public 멤버를 protected로만 접근 가능
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

- 이것을 호출하면 어떻게 될까?
  ```c++
  Cat myCat;
  myCat.Move();
  ```
- public 상속은 myCat.Move()가 호출 됨
- private 상속은 myCat.Move()가 호출 안 됨
  - Animal의 public 함수가 private로 바뀌었기 때문

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

- 위 코드를 실행하면 메모리에서 어떤 일이 일어날까?

1. 먼저 부모님의 생성자를 호출하고 부모를 초기화 함
   ![image](https://user-images.githubusercontent.com/22488593/174580938-6b7d23f8-6511-4b2c-97cc-6d125e9731f3.png)

- 부모님의 mAge가 2로 초기화 됨

2. 그 다음 자식을 초기화 함
   ![image](https://user-images.githubusercontent.com/22488593/174581082-8640c9ed-d6e1-403a-ba86-b8bee1556370.png)

- 메모리를 할당하고 mName에 주소값이 들어간다
- 항상 부모의 메모리가 앞에 위치해 있다
  - 부모의 생성자를 먼저 호출하는 이유
  - 이 특성으로 부모 포인터로 자식을 저장할 수 있는 있다
- Cat은 언제부터 자신의 메모리가 시작해야하는지 알고 있음
  - animal의 멤버 바이트 수 이후부터가 Cat 메모리의 시작
  - Cat은 Animal의 헤더파일을 포함하고 있어 animal 멤버 바이트 수를 알 수 있음

## 2. 다형성(Polymorphism)

- Poly: 여러가지
- morp: 모습
- 즉, 여러가지 모습으로 변할 수 있는 상태를 의미
- 부모 클래스의 함수가 자식 클래스에서 여러 모습으로 변화

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

- myCat과 yourCat의 메모리 뷰
  ![image](https://user-images.githubusercontent.com/22488593/174705528-7258439f-3a2a-4fee-a489-21cde6e82e51.png)
  - mAge, mName이 저장되어 있음
  - 멤버함수는 어디에 저장되어 있을까?

#### 2.1.2. 멤버 함수의 메모리

- 멤버 함수도 메모리 어딘가에 위치해 있음
  - 모든 것이 메모리 어딘가에 위치해 있다
- 그런데 각 개체마다 멤버 함수의 메모리가 잡혀 있을까?
  ```c++
  myCat->GetName();
  youtCat->GetName();   // 이 둘의 동작은 완전히 일치한다 그렇다면?
  ```
- 그 대신 각 멤버 함수는 컴파일 시에 딱 한 번만 메모리에 "할당" 됨
  - 저수준에서는 **멤버 함수는 전역 함수**와 그다지 다르지 않음
  - 함수에 개체의 오브젝트를 매개변수로 전달해 줄 뿐임

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
// call Cat::GetName(0A16C7h)   // GetName()메소드 호출
```

- 함수의 메모리 뷰
  ![image](https://user-images.githubusercontent.com/22488593/174709239-3f199d48-a5df-462e-896f-579afe4d8008.png)
  - GetAge(), GetName()에 없던 매개변수가 추가되어 있다
    - 개체를 매개변수로 받음
  - myCat yourCat 둘 다 getName()을 호출 할 때 메모리 0x0A16C7을 호출을 함
    - 멤버 함수가 개체별로 있는게 아닌 전역함수로 어딘가 존재한다는 증거임

### 2.2. 함수 오버라이딩

- Animal의 종류에 따라 말하는게 다르다
  ![image](https://user-images.githubusercontent.com/22488593/174709912-3e346162-c570-4d4c-a35e-d76368928dca.png)
- 함수 오버라이딩(Override)
  - 말을 하는 행위는 어느 Animal에나 있지만 무슨 말을 할지는 각 자식이 결정하게 하는 것
  - 오버라이딩(override)라는 말 자체가 무언가를 덮어 쓴다는 뜻
- 이를 코드로 짜면

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

- Java 코드
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
- C++ 코드

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

- 부모 포인터가 자식을 담고 있는 경우 오버라이딩 함수를 호출하면
  - Java는 자식의 것이 호출된다
  - C++은 부모의 것이 호출됨
- Java는 실체를 따라가고 C++은 무늬를 따라 호출이 됨
* 이것의 이유는 **바인딩**의 차이에 있다
## 3. 정적 바인딩
* 무늬따라 가는 것
* C++는 기본이 정적 바인딩
### 3.1. 정적 바인딩 - 멤버 함수
* Animal의 Speak()와 Cat의 Speak()의 메모리 뷰
![image](https://user-images.githubusercontent.com/22488593/174729943-ef62f712-299d-4997-8d9e-0a2fc42acfe2.png)
```c++
// main.cpp
Cat* myCat = new Cat();
myCat->Speak(); // Call   Cat::Speak(0x0AA16C2)

Animal* yourCat = new Cat();
yourCat->Speak();   // Call   Animal::Speak(0x0AA168D)
```
  * myCat은 Cat의 Speak() 호출, yourCat은 Animal의 Speak() 호출
## 4. 동적 바인딩
* 무늬가 아닌 실체따라 가는 것
### 4.1. C++에서 동적 바인딩을 하려면 가상(virtual) 함수를 사용
```c++
class Animal
{
public:
  virtual void Speak(); // 가상 함수 선언
}
class Cat : public Animal
{
public:
  void Speak();
};

Cat* myCat = new Cat();
myCat->Speak();   // Meow
Animal* yourCat = new Cat();
yourCat->Speak();   // Meow
```
* Animal포인터여도 Cat의 speak()가 호출 된다
### 4.2. 가상함수
* 자식 클래스의 멤버함수가 언제나 호출됨
  * 부모의 포인터 또는 참조를 사용 중이더라도
* 동적(dynamic) 바인딩/늦은(late) 바인딩
  * 실행 중에 어떤 함수를 호출할지 결정한다
  * 당연히 컴파일 중에 어떤 함수를 호출할지 정하는 정적 바인딩보다 느림
* 이를 위해 **가상 테이블**이 생성됨
  * 모든 가상 멤버함수의 주소를 포함   
* Java의 모든 것이 기본적으로 가상 함수임
  * final 키워드로 정적 바인딩으로 만들 수는 있다
* 가상 테이블은 **클래스 마다 하나**  있다
  * 멤버 함수는 개체 마다 하나씩 있는 것이 아니라고 했다
* 개체를 생성할 때, 해당 클래스의 가상 테이블 주소가 함께 저장됨
  * 가상 테이블의 포인터 4바이트가 들어있고 그다음에 개체의 멤버가 저장된 형태
### 4.3. 가상 테이블
* 가상 테이블의 동작을 생각해보면 느림
  * 힙 메모리에서 해당하는 함수를 찾아서 lookup해야 함 
* 가상 테이블의 주소는 알았는데 가서 몇 번째 함수인지 어떻게 알까? 
### 4.3.1. 동적 바인딩 메모리 뷰
* 컴파일 시 만들어지는 것들
  ![image](https://user-images.githubusercontent.com/22488593/174738057-44395052-c096-418a-bbbc-6ccaa35d58b2.png)
    * 코드 섹션에 Animal과 Cat의 Move()와 Speak()이 들어가 있다
    * Animal과 Cat의 가상 테이블이 따로 존재함
    * 여기서 중요한 건 함수의 순서가 다 동일하다는 것임
        * 함수의 순서가 동일해야 가상 테이블에서 어떤 함수가 어디있는지 찾을 수 있다
* 실행 중에 만들어지는 것들
  ![image](https://user-images.githubusercontent.com/22488593/174738953-43211244-3f8c-49b4-9bba-da63e3a8563a.png)
  * MyCat, YourCat 둘 다 가상 테이블의 주소를 가지고 있다
## 5. 가상 소멸자
```c++
Animal* yourCat = new Cat(5, "Mocha");
delete yourCat;
```
  * 이 경우는 Animal, Cat중 누구의 소멸자가 호출되어야 할까?
### 5.1. 비 가상 소멸자
```c++
// Animal.h
class Animal
{
public:
  ~Animal();
private:
  int mAge;
}

// Cat.h
class Cat : public Animal
{
public:
  ~Cat();
private:
  char* mName;
}
```
* 경우 1
  ```c++
  Cat* myCat = new Cat(2, "Coco");
  delete myCat;
  ```
  * Cat의 소멸자가 호출되고 Animal의 소멸자가 자동적으로 호출 된다
* 경우 2
  ```c++
  Animal* yourCat = new Cat(5, "Mocha");
  delete yourCat;
  ```
  * 정적 바인딩이 적용되어 Animal의 소멸자만 호출 된다
    * Cat의 소멸자가 호출되지 않으므로 메모리 누수
* Virtual 키워드를 생략하면 큰일나는 이유임!!
### 5.2. 가상 소멸자
  * 소멸자 앞에 virtual 키워드로 가상 소멸자를 설정해주자
    ![image](https://user-images.githubusercontent.com/22488593/174743182-e3d925cf-f586-43d7-b109-21eed5cd13d6.png)
  * 누군가 Cat도 상속할 수 있으므로 ~Cat()에도 virtual을 붙여줌
#### 5.2.1 가상 소멸자 메모리 뷰
![image](https://user-images.githubusercontent.com/22488593/174743860-3c8005d0-52e7-4f13-b049-f59e1955b884.png)
* Animal 포인터에 저장되어 있지만 Cat의 가상 소멸자 주소를 알고 있으므로 Cat의 소멸자를 호출할 수 있다
### 5.3. **모든 소멸자에 언제나 virtual키워드를 붙일 것**
  * 파생 클래스의 소멸자에도 virtual을 붙여야 하는 이유는
  * 내가 아닌 누군가가 파생 클래스를 상속할 수 있기 때문
  * 폴리몰피즘을 이용하여 부모를 delete하면 메모리 누수가 발생한다
  * 가상 함수는 느림에도 불구하고 넣는게 좋다
## 6. 다중(Multiple) 상속
* C++에만 존재하는 것임
* 다중 상속은 **잘 쓰이지는 않는 개념**임 
![image](https://user-images.githubusercontent.com/22488593/174748492-ba0d50fd-0cdf-408e-a2d5-981ac2b0e09a.png)
* 학생의 속성, 교수의 속성을 가진 조교 클래스
* Student와 Faculty를 다중 상속 받음
### 6.1 어느 부모의 생성자가 먼저 호출될까?
* Student와 Faculty중 누구의 생성자가 먼저 호출될까?
  * 파생 클래스에서 등장한 부모 클래스 순서대로 호출된다
    ```c++
    // Student(), Faculty() 순으로 호출
    class TA : public Student, public Faculty
    {
    };
    
    // Faculty(), Student() 순으로 호출
    class TA : public Faculty, public Student
    {
    };
    ```
  * 초기화 리스트의 순서는 상관 없음
    ```c++
    // Student(), Faculty() 순으로 호출
    class TA : public Student, public Faculty
      : Faculty()
      , Student()
    {
    };
    ```
### 6.2 super()를 쓸 수 없는 이유
* Java처럼 super()를 쓸 수 없는 이유는 다중 상속이 가능하기 때문이다
    ![image](https://user-images.githubusercontent.com/22488593/174749733-16782bc5-488e-4a21-8da7-7f85de4842d8.png)
### 6.3. 문제점1 - 어떤 함수가 호출될까?
  ![image](https://user-images.githubusercontent.com/22488593/174750137-5e576696-93e0-4bb6-8471-65673238d8e1.png)
* 어떤 함수가 호출될 지 모호함
* 해결책 - 우리가 직접 부모 클래스를 특정시켜줌
  ```c++
  myTa->Student::DisplayData();
  ```
### 6.4. 문제점2 - 다이아몬드 문제
![image](https://user-images.githubusercontent.com/22488593/174750658-4c22cad5-c445-431e-a599-b992d98d4d17.png)
* Liger 안에는 Animal이 몇개가 있을까?
  * 2개가 중첩됨
* 해결책 - 가상 베이스 클래스
  * 상속받을 때 virtual을 넣어준다..
   ![image](https://user-images.githubusercontent.com/22488593/174751014-ab6467a6-d9ad-4c59-8c6f-91f67d91c0b6.png)
  * Liger가 Animal 하나만 가질 수 있도록 보장한다 
  * 흔히 쓰일일이 없을 것이다..
### **다중 상속을 최대한 쓰지 말것, 대신 인터페이스를 사용하자**
## 7. 추상(Abstract) 클래스

## 8. 인터페이스(Interface)
