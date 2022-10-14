# auto 키워드

auto 키워드는 자료형을 자동으로 추론해줍니다.
JavaScript에 있는 동적인 형과는 다릅니다. 
자료형이 실행중에 바뀌지 않고 컴파일 시 결정이 되기 떄문입니다. 
컴파일 시 자료형을 결정하기 위해서는 반드시 auto 변수를 초기화해야 합니다. 

```c++
auto x; // 에러
auto x = "Chris"; // 올바름
```

# JavaScript vs C++

```javascript
var x;  //OK. x가 정의되지 않음
x = "Chris";    //OK. x 는 문자열
x = 50; //OK. x는 숫자 
```

```c++
auto x = "Chris";   // x는 const char*
x = 50; // 에러
```

# auto로 포인터와 참조 받기

데이터형은 크게 3가지 입니다. 값, 참조, 포인터.
값을 받을 때는 **auto** 를 쓰고, 
포인터를 받을때는 **auto** 또는 **auto\*** 를 쓰고,
참조를 받을 때는 **auto\&** 를 씁니다.   
여기서 특이한 점은 값형과 포인터형 받을때 둘 다 **auto** 를 써도 된다는 점입니다.   

```c++
// Main.cpp
Cat* myCat = new Cat("Coco", 2);
auto myCatPtr = myCat;  // auto는 myCat과 동일한 Cat*가 됩니다. 
```
컴파일러가 값형인지 포인터형인지 구분할 수 있기 때문입니다.   
하지만 우리가 보기에는 구분이 안 갈 수도 있습니다.   
다음 코드는 포인터일까요 아닐까요?
```c++
auto name = object.GetName();
```
이처럼 포인터를 받을 때 **auto**를 쓰는 것은 가독성이 좋지 않습니다.
따라서 **auto\***를 쓰는 것이 좋습니다.   
```c++
auto* name = object.GetName();
```

# auto로 const 받기 
다음 코드를 봅시다.
a는 const를 이어 받을까요?
```c++
const int b = 1;
auto& a = b;    // const를 이어받을까요>
```
a는 const를 이어받습니다.   
const를 받는 이유는 컴파일러가 알아낼 수 있기 때문입니다.   
하지만 이것 역시 가독성이 떨어질 수 있습니다.
다음 코드를 봅시다.   
```c++
auto& name = object.GetName();  
```
name이 const를 받을지 안 받을지 겉만보면 알 수 없습니다.   
역시 **const를 받을 때는 const auto\&를 쓰는게 좋습니다**. 
```c++
const auto& name = object.GetName();
```

# auto와 함수 반환형
auto 키워드는 함수가 반환하는 걸 저장하는 데 유용할 수 있습니다. 
함수의 반환형이 뭐든 간에 이를 저장할 수 있고,   
함수 반환형이 변해도 코드를 변경할 필요 없습니다.   
다음 코드를 봅시다.
```c++
// MyMath.h
double Add(double a, double b)
{
    return a + b;
}

// Main.cpp
int main()
{
    auto result = Add(10.0, 30.0);

    return 0;
}
```
반환형이 double에서 float으로 바뀌어도 auto 부분을 변경할 필요 없습니다.   
근데 과연 함수 반환형을 auto를 받는 것에는 문제가 없을까요?
다음의 코드를 봅시다.
```c++
auto minValue = object.GetMinValue();
if (minValue < -3)
{
    // ...
}
```
위 코드의 문제점은 minValue가 unsigned int인지 int인지 아니면 다른 것인지 알 수 없다는 점입니다.   
unsigned int형이면 -3과 비교를 한다는게 옳지 않습니다.
그리고 int형이라고 해도 함수의 반환값을 언제든지 unsigned int형으로 바꿀 수 있습니다. 

# auto는 엄청나게 좋을까요?
auto 키워드가 타이핑을 줄여준다는 점에서는 좋다고 할 수 있습니다.   
하지만 위와 같이 가독성이 떨어진다는 문제점이 있습니다.   
auto를 써야 하냐 안 써야 하냐는 논란이 있는 주제라고 할 수 있습니다.   
그럼에도 불구하고 auto를 꼭 쓰면 좋은 케이스가 있습니다.

# auto를 쓰면 좋은 케이스
## auto와 반복자
반복문에 반복자를 쓰는 경우 auto를 쓰는 것이 타이핑을 많이 줄여줍니다.   
```c++
for (std::vector<int>::const_iterator it = v.begin(); it != v.end(); ++it)
{
    // 이하 코드 생략 
}
```
위 코드에서 반복자의 선언 부분이 너무 기므로 auto로 변경해줍니다.   
v가 어떤 STL 컨테이너인지는 반복문 위에 나타나 있을테니 반복문 선언을 auto로 대체해도 상관없어보입니다.
```c++
for (auto it = v.begin(); it != v.end(); ++it)
{
    // 이하 코드 생략
}
```
이렇게 코드가 간결해졌습니다. 
## auto로 템플릿형 받기
템플릿형도 auto로 간결하게 해줄 수 있습니다.
```c++
MyArray<int>* a = new MyArray<int>(10);

auto* a = new MyArray<int>(10);
```
auto로 대체해도 new 옆을 보면 어떤 템플릿인지 알 수 있습니다.   
템플릿 매개변수가 3개 이상일 경우를 생각하면 auto로 템플릿형 받는 것이 좋아보입니다.

# auto 베스트 프렉티스
이는 POCU 아카데미의 Comp3200 수업에서의 표준입니다. 업계 전반의 코딩 표준은 아닙니다.
1. **auto** 보다 실제 자료형 사용을 권장
    ```c++
    Class* myClass = new Class("COMP3200");
    // ...
    const auto* name = myClass->GetStudentName(id);
    const char* name = myClass->GetStudentName(id);  // 더 나음 
    ```
2. 예외: 템플릿 매개변수와 반복자에는 **auto** 사용
    ```c++
    std::vector<int> scores;
    // 여기서 10, 50을 삽입

    int sum = 0;
    for (auto it = scores.begin(); it != scores.end(); ++it)
    {
        sum += *it;
    }
    ```
3. 포인터를 받을 때는 **auto** 보다 **auto\***를 사용
    ```c++
    Cat* myCat = new Cat("Coco", 2);
    // ...
    auto myCat2 = myCat;    // myCat을 복사하는 것으로 착각할 수도 있다.
    auto* myCat2 = myCat;   // 더 나음
    ```
4. const변수를 받을 때는 **auto\&**보다 **const auto\&**를 사용
    ```c++
    Class* myClass = new Class("COMP3200");
    // ...
    auto& name = myClass->GetStudentName(id);
    const auto& name = myClass->GetStudentName(id); // 더 나음
    ```

1. 
