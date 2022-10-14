default와 delete를 살펴보기 앞서 c++의 암시적 생성자 문제를 알아보겠습니다.

# 암시적 생성자 문제1
다음의 코드는 컴파일러가 암시적 생성자를 만들어 냅니다. 
```c++
// Dog.h
class Dog
{
// Dog 생성자 없음 -> 암시적 생성자 생성
private:
    std::string mName;
    int mAge;
}
```

```c++
// Main.cpp
#include "Dog.h"
int main()
{
    Dog myDog;  // 암시적 기본 생성자 호출 
    Dog copiedMyDog(myDog); // 암시적 복사 생성자 호출
    // .. 
}
```

암시적 기본 생성자는 문제가 없을 수 있습니다.
하지만 암시적 복사 생성자를 호출하면,
얕은 복사(Shallow Copy)가 될 가능성이 있습니다.
그밖에도 암시적 생성자는 혼동을 줄 수 있습니다.
저 코드를 작성한 프로그래머가 암시적 생성자를 의도한 지 실수로 빼먹은건지 모르겠고,
암시적 생성자를 호출시 얕은 복사와 같은 문제가 발생할 수 있기 때문입니다.
암시적 생성자를 방지하기 위해 프로그래머가 직접 생성자를 만들어주면 어떨까요?  
```c++
// Dog.h
class Dog
{
public:
    Dog();
    Dog(std::string name);
private:
    std::string mName;
};

// Dog.cpp
#include "Dog.h"

Dog::Dog()
{

}

Dog::Dog(std::string name)
{
    // 이하 코드 생략
}
```
위 코드의 경우 일반 생성자를 일부러 빈 생성자로 만들어주었고 복사 생성자를 직접 만들어주었습니다.
암시적 생성자가 호출되지 않는다는 장점이 있지만 여전히 생성자를 작성하는 것은 번거롭습니다.
이런 배경으로 생긴 것이 C++11의 default 키워드입니다.   
다음은 default 키워드를 사용한 코드입니다.

```c++
// Dog.h
class Dog
{
public:
    Dog() = default;    // default 키워드 사용 
    Dog(std::string name);
private:
    std::string mName;
}

// Dog.cpp
#include "Dog.h"

// 컴파일러가 Dog()를 대신 만들어냅니다

Dog::Dog(std::string name)
{
    // 이하 코드 생략
}
```

기본 생성자와 같은 동작을 하지만 default 키워드가 있어서 프로그래머가 이를 의도했다는 것을 알 수 있습니다.

# default 키워드
default 키워드를 사용하면 컴파일러가 특정한 생성자 연산자 및 소멸자를 만들어 낼 수 있습니다. 
그래서 비어 있는 생성자나 소멸자를 구체화할 필요가 없습니다.
또한 기본 생성자, 연산자 및 소멸자를 더 분명하게 표시할 수 있습니다. 
즉 프로그래머의 의도를 나타낼 수 있습니다.  

# 암시적 생성자 문제2

그렇다면 컴파일러가 암시적 생성자를 만들지 않게 하고 싶을때는 어떻게 할까요?
C++11전에는 지우고 싶은 생성자를 private 멤버에 넣은 방식을 사용했습니다.

```c++
// Dog.h
class Dog
{
public:
    Dog() = default;
    // ...
private:
    Dog(const Dog& other);  // 복사생성자
    // ...
};

// Main.cpp
#include "Dog.h"
int main()
{
    Dog myDog;
    Dog copiedMyDog(myDog); // 에러
}
```

위 코드의 경우 private 멤버에 있는 복사생성자를 호출하면 에러가 납니다.
하지만 이는 private 접근 제어자를 다른 의도로 사용한 것입니다.
따라서 에러 메시지도 아래와 같습니다.   

![image](https://user-images.githubusercontent.com/22488593/183004723-005d3c36-4124-4da1-b650-db98d63cc902.png)

복사생성자가 private 멤버여서 접근하지 못한다고 되어있습니다.
제대로 된 에러 메시지가 아닌 것 같은 느낌입니다.
이런 문제가 있어 C++11이후 delete라는 키워드가 나왔습니다.
다음 코드는 delete를 사용한 코드입니다.

```c++
// Dog.h
class Dog
{
public:
    Dog() = default;
    Dog(const Dog& other) = delete; // 복사 생성자 삭제
    // ...
};

// Main.cpp
#include "Dog.h"
int main()
{
    Dog myDog;
    Dog copiedMyDog(myDog); // 에러 
}
```

이번에는 올바른 에러 메시지가 나옵니다.   

![image](https://user-images.githubusercontent.com/22488593/183005458-acd0a665-87aa-4b0d-8b25-e55298ef3c7b.png)

# delete
컴파일러가 자동으로 생성자를 만들어 주길 원하지 않을 때 delete를 사용합니다.
C++11이전에 private 접근 제어자로 빈 생성자를 만드는 생성자 삭제 방식을 대체할 수 있습니다.
private을 사용하는 방법과 달리 올바른 메시지를 출력합니다. 

# 결론
컴파일러가 자동으로 코드를 생성하는 암시적 방식은 프로그래머의 의도를 알 수 없다는 점에서 좋지 않습니다.
언제나 명확한게 좋으므로 **default/delete** 키워드를 어디에나 넣는 것이 좋습니다. 