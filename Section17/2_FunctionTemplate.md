# 함수 템플릿

함수 템플릿의 기본 적인 형태는 다음과 같습니다. 

```c++
template <class <type_name>> <function_declaration>;
template <typename <type_name>> <function_declaration>;
```

첫번째와 두번째 즉 typename과 class의 차이는 사실상 없습니다.
class라고 클래스 타입만 받는 것이 아니고 int 형도 받을 수 있습니다.
보통은 typename을 많이 쓴다고 합니다.

함수 템플릿을 코드로 작성하면 다음과 같은 형태가 됩니다.

```c++
template <class T>
T Add(T a, T b)
{
    // ...
}

template <typename T>
T Add(T a, T b)
{
    // ... 
}
```

함수 템플릿을 호출할 때 다음과 같이 템플릿 매개변수를 생략할 수도 있습니다.

```c++
Add<int>(3, 10);
Add(3, 10);
```

