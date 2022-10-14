# NULL 문제

NULL을 쓰면 다음과 같은 문제가 발생할 수 있습니다.

```c++
// Class.h
float GetScore(const char* name);
float GetScore(int id);

// Main.cpp
Class* myClass = new Class("Comp3100");
// ...
int score = myClass->GetScore(NULL);   // GetScore(int id)가 호출이 됨
```

빈 포인터를 의도하고 NULL을 넣었지만, GetScore(const char* name)가 호출되지 않고 GetScore(int id)가 호출이 됩니다.   
NULL은 타입이 정해지지 않은 숫자 0이기 때문입니다. 
NULL은 다음과 같이 정의되어 있습니다.   
```c
#define NULL 0
```
물론 NULL을 널 포인터로 사용할 수 있지만 함수의 매개변수로 넣으면 숫자로 인식을 합니다.
이러한 문제를 해결하기 위해선 nullptr이 생겼습니다.

# nullptr

nullptr은 숫자가 아닌 포인터형에만 사용할 수 있는 null 입니다.
포인터의 null도 여전히 0이지만 컴파일러에게 포인터에 사용하는 null이라고 알려주는 것입니다.
그래서 포인터가 아닌 자료형에 nullptr을 쓰면 컴파일 에러가 납니다. 

```c++
int number = NULL;  // OK
int* ptr = NULL;    // OK

int anotherNumber = nullptr;    // ERROR
int* anotherptr = nullptr;       // OK
```

다음은 nullptr을 사용하는 예제입니다.   
포인터를 비교할 때는 다음과 같이 nullptr을 사용하면 됩니다.   

```c++
// Main.cpp
Class* myClass = new Class("COMP3200");

const Student* student = myClass->GetStudnet("Coco");
if (student != nullptr)
{
    std::cout << student->GetID() << ":" << student->GetName() << std::endl;
}
```