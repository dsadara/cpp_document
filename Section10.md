# Section10: 캐스팅(형변환, Casting)

> 중간고사 대비 주제
>
> 1. static_cast vs reinterpret_cast

# 1. C 스타일 캐스팅
## 1.1 암시적(implicit) 캐스팅
* 컴파일러가 형을 변환해 줌
  * 형 변환이 허용되고
  * 프로그래머가 명시적으로 형 변환을 안할 경우
```c++
int number1 = 3;
long number2 = number1; // 암시적 캐스팅
```
## 1.2 명시적(explicit) 캐스팅
* 프로그래머가 형 변환을 위한 코드를 직접 작성
* C++에선 static_cast const_cast dynamic_cast reinterpret_cast가 있다

## 1.3 C스타일 캐스팅의 애매한 점
```c++
int score = (int)someVariable;
```
* 캐스팅을 하긴 하는데 정확히 어떤 캐스팅을 하는지 모르겠음
* C++스타일 4개의 캐스팅 중 하나를 함
  * 명확하지 못하다
* 명백한 실수를 컴파일러가 캐치하지 못하기도 함
  * 컴파일러는 프로그래머가 실수를 의도한건지 아닌지 알 수 없음
  * 이를 방지하기 위해 캐스팅을 세분화를 함
  * C++ 캐스팅이 이 문제를 해결
## 2. static_cast
```c++
float number1 = 3.f;
int number2 = static_cast<int>(number1);
Animal* myPet = new Cat(2, "Coco");
Cat* myCat = static_cast<Cat*>(myPet);
```
1. 값을 유지하면서 두 숫자형 타입간의 변환
  * 반올림 오차는 제외
  * 이진수 표기는 달라질 수 있다
    * 정수형에서 부동소수점형으로 변환할 때 달라짐 
2. 개체 포인터 변환
  * 베이스 클래스 포인터에 파생 클래스가 담아 있을 때 파생 클래스로 포인터를 변환
  * 변수형 체크 후 베이스 클래스를 파생 클래스로 변환
    * 상속 관계를 확인
  * 컴파일 시에만 형 체크 가능
  * 실행 도중 크래시 날 수 있음
  * 예시 1
    ```c++
    Animal* myPet = new Cat(2, "Coco");

    Cat* myCat = static_cast<Cat*>(myPet);  // OK

    Dog* myDog = static_cast<Dog*>(myPet);  // 컴파일은 됨. 그러나 위험
    myDog->GetDogHouseName(); // myDog은 실제로 이 함수를 가지고 있지 않아 크래시가 날 수 있음, 아니면 같은 순서의 Cat 멤버함수가 호출될 수도 있음
    ```
  * 예시 2
    ```c++
    Animal* myPet = new Cat(2, "Coco);
    
    House* myHouse = static_cast<House*>(myPet);    // 컴파일 에러
    myHouse->GetAddress();
    ```
* static_cast는 컴파일 중 결정이 된다
## 3. reinterpret_cast
가장 위험한 캐스팅
### 3.1 reinterpret_cast란?
* 연관 없는 두 포인터 형 사이의 변환을 허용 
  * Cat\*형과 House\*형
  * char\*형과 int\*형
* 포인터와 포인터 아닌 변수 사이의 형 변환을 허용
  * Cat\*형과 unsigned int형 
* **이진수 표기가 달라지지 않음**
  * A형의 이진수 표기를 그냥 B형인 것처럼 해석
### 3.2 reinterpret_cast의 문제
```c++
int* signedNumber = new int(-10);
unsigned int* unsignedNumber = reinterpret_cast<unsigned int*>(signedNumber); 
```
![image](https://user-images.githubusercontent.com/22488593/174940516-8aea5d9e-e220-4830-8e82-088a43e88677.png)
* signed int에서 음수는 MSB가 1로 세트되어 있음   
그래서 이를 unsigned로 바꾸면 값이 엄청 커질거라 예상할 수 있음
  ![image](https://user-images.githubusercontent.com/22488593/174941055-b58806c2-c34e-4dbb-b5d9-a03ee50dd229.png)
* 예상치 못한 값이 unsignedNumber에 들어가게 된다
### 3.3 static_cast vs reinterpret_cast
```c++
int* signedNumber = new int(-10);

// 컴파일 에러. 유효하지 않은 형 변환
unsigned int* unsignedNumber1 = static_cast<unsigned int*>(signedNumber);

// 컴파일 됨. 허나 값은 더 이상 -10이 아님.
unsigned int* unsignedNumber2 = reinterpret_cast<unsigned int*>(signedNumber);
```
* 위의 문제를 의도하고싶으면 reinterpret_cast를 쓰고
* 의도한게 아니라면 static_cast를 써서 컴파일러가 이를 막아줌
### 3.4. reinterpret_cast 사용법
![image](https://user-images.githubusercontent.com/22488593/174941912-f0e84ba0-ef45-40fd-8341-d8d102dd6eb5.png)
### 3.5. reinterpret_cast은 어디서 사용할까?
* 포인터의 주소를 int로 출력하고 싶을 때 사용
```c++
Animal* myPet = new Cat(2, "Coco");
unsigned int myPetAddr = reinterpret_cast<unsigned int>(myPet);
cout << "address: " << hex << myPet;
```
  * 64비트 컴파일러는 포인터의 크기가 8바이트이므로 int에 다 들어가지 않을 수도 있다
* 포인터들을 직렬화 할 때 쓰인다
  * 포인터들의 상대적 위치인 오프셋을 int로 저장하면 새로 프로그램을 시작할 때 불러오면 포인터들을 다시 사용할 수 있다 
* 
## 4. const_cast
하지 말아야 할 캐스팅 
```c++
void Foo(const Animal* ptr)
{
    // BAD CODE
    Animal* animal = const_cast<Animal*>(ptr);
    animal->SetAge(5;)
}
```
* const Animal\*로 들어온 매개변수를 Animal\*로 캐스팅하는 나쁜 코드
  * 이는 함수의 시그니처를 무시한 좋지 않은 캐스팅임
  * 포인터의 주소가 변경되면 안되는 주소의 경우 위험할 수도 있다
    * 코드가 엄청 길면 const로 들어온 줄도 모르고 바꿔버릴 수 있다
## 4.1 const_cast란?
* const_cast로 형을 바꿀 순 없다
  * const를 빼는 일 밖에 할 수가 없다 
* const 또는 volatile 애트리뷰트를 제거할 때 사용 
  ```c++
  Animal* myPet = new Cat(2, "Coco");
  const Animal* petPtr = myPet;

  Animal* myAnimal1 = (Animal*)petPtr;  // OK
  Cat* myCat1 = (Cat*)petPtr;           // OK, 둘다 C 스타일이기 때문에 다 된다

  Animal* myAnimal2 = const_cast<Animal*>(petPtr);    // OK
  Cat* myCat2 = const_cast<Cat*>(petPtr);             // 컴파일 에러
  ```
* 포인터 형에 사용할 때만 말이 됨
  * 값 형은 언제나 복사되니까 const쓰는게 의미가 없다
* const_cast를 코드에서 쓰려고 하면 무언가 잘못된 거다
### 4.2 const_cast를 사용할 때는?
* 써드파티 라이브러리가 const를 제대로 사용하지 않을 때
```c++
void WriteLine(char* ptr);        // 뭔가 별로인 외부 라이브러리, const로 값 변경을 방지했으면 좋겠음

void MyWriteLine(const char* ptr)   // 우리 프로그램에 있는 함수
{
    WriteLine(const_cast<char*>(ptr));
}
```
### 4.3 const_cast 사용법
![image](https://user-images.githubusercontent.com/22488593/174949739-23738bb2-ea99-4dd3-9668-2a5e99ddd2ac.png)
```c++
// const Animal* petPtr;
Animal* animal = const_cast<Animal*>(petPtr);
```
## 5. dynamic_cast
* static\_cast와 비슷하지만 컴파일 중이 아닌 실행 중에 캐스팅이 된다
```c++
Animal* myPet = my Cat();

// 컴파일 됨 그리고 NULL을 반환함
Dog* myDog = dynamic_cast<Dog*>(myPet);  // static_cast와 달리 캐스팅이 안되고 NULL을 반환해준다

// 컴파일 됨 GetHouseName()은 실행되지 않음
if (myDog)
{
  myDog->GetHouseName();
}
```
### 5.1. dynamic_cast란?
* 실행 중에 형을 판단
* 포인터 또는 참조형을 캐스팅할 때만 쓸 수 있음
  * 다형성을 하려면 포인터나 참조 밖에 안됨  
* 호환되지 않는 자식형으로 캐스팅하려 하면 NULL을 반환
  * 호환되지 않음을 실행 시 체크 
  * 따라서 dynamic_cast가 static_cast보다 안전
* RTTI가 꺼져 있으면 static_cast랑 동일하게 동작 
### 5.2. RTTI(Real-Time Type Information, 실시간 타입정보)
* RTTI는 각 오브젝트나 데이터의 타입정보를 typeid를 통해 실시간으로 불러올 수 있음 dynamic_cast는 이를 이용해서 캐스팅할지 결정함
* dynamic_cast를 사용하려면 컴파일 중에 RTTI를 켜야 함
* 그런데 C\++ 프로젝트에서 성능의 이유로 RTTI를 끄는 것이 보통임
* RTTI의 오버헤드를 줄이기 위해 특정 타입만을 위한 RTTI 비슷한 걸 만들 수 있다
  * 클래스의 멤버로 mType을 넣고 typeID 연산자처럼 getter를 만들 수 있다
### 5.3. dynamic_cast 사용법
![image](https://user-images.githubusercontent.com/22488593/174959540-7fcf233e-19f0-43c2-83f4-297afbf65e30.png)
## 6. 캐스팅 규칙
* 제일 안전한 것에서 가장 위험한 것 순으로 사용
1. 기본적으로 static_cast 사용
  * reinterpret_cast<Cat*> 대신 static_cast<Cat*>
    * 만약 Cat이 Animal이 아니라면 컴파일러가 에러를 뱉음
    * static_cast를 쓰면 컴파일러가 잡아주니까 일단 안심을 하게 됨
    * 하지만 reinterpret_cast가 나오면 열심히 볼 것   
3. reinterpret_cast를 쓸 것
  * 포인터와 비포인터 사이의 변환
    * 이것을 해야할 일이 있다
  * 서로 연관이 없는 포인터 사이의 변환은 그 데이터형이 맞다고 **정말 확신**할 때만 할 것   
5. 내가 변경권한이 없는 외부 라이브러리를 호출할 때만 const_cast를 쓸 것
