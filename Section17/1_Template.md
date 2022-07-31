# 템플릿(template)이란?

템플릿은 컴파일 시 템플릿 매개변수 타입에 대한 함수나 클래스를 생성해주는 기능입니다.
즉 하나의 함수나 클래스만 작성해도 여러 타입의 함수나 클래스를 자동으로 생성해줍니다.  
템플릿이 나온 이후 사람들이 최적화를 명목으로 많이 남용을 했지만,
가독성이 떨어지거나 컴파일 시간이 길어진다는 문재점들이 생겨 사용이 줄어드는 추세였고,
C+11 이후에 [constexpr]()이라는 것이 나오면서 요즘은 정말 필요한 곳에만 템플릿이 사용이 됩니다.  
템플릿은 Java나 C#의 제네릭(generic) 메서드/클래스와 비슷합니다.

## STL Container에서 사용되었던 템플릿

```c++
// Main.cpp
#include <vector>

int main()
{
    std::vector<int> scores;
    scores.push_back(10);   // 10
    scores.push_back(50);   // 50

    return 0;
}
```

# 템플릿의 사용처?

[다형성]()은 런타임 중에 어떤 형이냐에 따라 행동이 달라지는 거였습니다.
템플릿을 사용하면 컴파일중에 다형성의 동작을 할 수 있습니다.  
예를들어 두 정수를 더하는 함수가 있다고 합시다.

```c++
int Add(int a, int b)
{
    return a + b;
}
```

만약에 두 실수를 더하고 싶다면 double이나 float에 대한 Add()함수를 다시 작성해야 합니다.
템플릿 함수를 작성하면 Add() 한 개만 작성하면 됩니다.

```c++
// MyMath.h
template <typename T>   // 또는 template <class T>
T Add(T a, T b)
{
    return a + b;
}

// Main.cpp
int main()
{
    std::cout << Add<int>(3, 10) << std::endl;
    std::cout << Add<float>(3.14f, 10.14f) << std::endl;

    return 0;
}
```

이런식으로 템플릿 매개변수에 int와 float을 넣으면 컴파일러가 int에 대한 함수와 float에 대한 함수를 자동으로 만들어줍니다.  
템플릿을 이용한 트릭도 있습니다.  
실행 시 돌리면 느린 함수들을 컴파일 시 미리 돌려놓아 함수의 결과값을 미리 평가(evaluate)해놓은 다음
실행 시 이를 상수로 사용할 수 있습니다.
예를들어 템플릿을 재귀함수와 같이 동작하게 하여 컴파일 시 재귀 결과값을 구할 수 있습니다.
이는 [템플릿 메타프로그래밍]()을 참고하시면 됩니다.
