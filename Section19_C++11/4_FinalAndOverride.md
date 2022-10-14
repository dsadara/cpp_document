# 상속의 문제점

C++에서 상속의 주체는 내가 아니고 다른 곳 입니다.
내 클래스를 다른 곳에서 마음데로 상속을 할 수 있습니다.
이는 다른 곳에서 내 클래스의 멤버에 마음대로 접근할 수 있고,
내 클래스 메모리도 마음대로 접근할 수 있다는 뜻입니다.
또 상속으로 생기는 메모리 문제를 방지하기 위해 가상(virtual) 함수와 가상 소멸자를 사용했습니다.
클래스의 상속을 막고 싶을 때는 final 키워드를 사용하면 됩니다. 

# final 키워드
클래스나 가상 함수를 파생 클래스에서 오버라이딩 못하도록 하려면 final 키워드를 사용하면 됩니다.
컴파일 중 에러를 확인할 수 있습니다.
가상 함수에만 final 을 사용할 수 있습니다.

```c++
// Animal.h
class Animal final  // 클래스 상속 방지
{
public:
    virtual void SetWeight(int weight);
    // ...
};

// Dog.h
#include "Animal.h"
class Dog : public Animal   // Animal을 상속하면 에러가 납니다
{
public:
    virtual void SetWeight(int weight);
    // ...
};
```

위 코드를 실행하면 다음과 같은 컴파일 에러가 납니다.   

![image](https://user-images.githubusercontent.com/22488593/183009990-62f335a2-2f73-4d36-8238-cf53590e16a2.png)   

부모에게 한번 상속 받은 자식에 final 키워드를 사용할 수도 있습니다.

```c++
// Dog.h
#include "Animal.h"

// Animal 클래스는 final 클래스가 아님
class Dog final : public Animal
{
public:
    virtual void SetWeight(int weight) override;    // override는 부모 함수를 오버라이딩 한다는 키워드
    // ...
};

// Corgi.h
#include "Dog.h"

class Corgi : public Dog    // 에러
{
public:
    // ... 
};
```
다음과 같은 컴파일 에러가 납니다.   
![image](https://user-images.githubusercontent.com/22488593/183011696-350f7ed6-03de-4f99-8dc1-f1f5c3ffdc15.png)

final 키워드로 함수의 상속 즉 함수의 재정의도 막을 수 있습니다. 
```c++
// Animal.h
class Animal
{
public:
    virtual void SetWeight(float weight) final;
    // ...
};

// Dog.h
#include "Animal.h"

class Dog : public Animal
{
public:
    virtual void SetWeight(int weight) override;    // 에러 
    Dog();
    ~Dog();
}
```

아래와 같은 에러 메시지가 뜹니다.

![image](https://user-images.githubusercontent.com/22488593/183011516-d256df57-01fb-4185-ba92-7a83f0c600d7.png)

# 잘못된 가상 함수 오버라이딩

```c++
// Animal.h
class Animal
{
public:
    virtual void SetWeight(float weight);
    // ...
};

// Dog.h
#include "Animal.h"

class Dog : public Animal
{
public:
    virtual void SetWeight(int weight); // Animal::SetWeight(float weight)를 오버라이딩 하지 않음
    // ... 
}
```

위 경우 Animal::SetWeight(float weight)가 오버라이딩 되지 않습니다.
실수로 매개변수로 int weight로 바꿨기 때문입니다.
컴파일러는 매개변수가 int weight인 SetWeight()함수를 새로 만들어줍니다.
의도는 오버라이딩이었는데 새로운 함수를 만들었습니다.
오버라이딩한다는 의도를 컴파일러에게 알리기 위해서는 override 키워드를 사용하면 됩니다.

```c++
// Animal.h
class Animal
{
public:
    virtual void SetWeight(float weight);
    // ...
};

// Dog.h
#include "Animal.h"

class Dog : public Animal
{
public:
    virtual void SetWeight(float weight) override;  // OK
    
    virtual void SetWeight(int weight) override;    // 컴파일 에러
}
```

override 키워드를 사용한 경우, float weight를 매개변수로 한 함수는 그대로 오버라이딩 해주고,
int weight를 사용한 함수는 컴파일 에러가 납니다.
부모 함수와 자료형이 달라 오버라이딩 할 수 없다는 것입니다.
override 키워드를 정리하자면,   

# override 키워드
잘못된 가상 함수 오버라이딩을 막으려면 override 키워드를 사용합니다.
당연히 가상 함수가 아니면 쓸 수 없고, 컴파일 도중에 올바르게 오버라이딩 했는지 검사합니다.   
