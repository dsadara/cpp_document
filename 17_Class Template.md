# 클래스 템플릿

클래스도 템플릿으로 만들 수 있습니다.
함수 템플릿과 같이 클래스 시그니처 위에 템플릿 매개변수와 템플릿임을 표시해주면 됩니다. 

# 클래스 템플릿의 필요성 
다음과 같은 MyIntArray라는 클래스가 있다고 합시다.
클래스를 살펴보니 정수를 넣는 배열 기능을 하는 것 같습니다. 
```c++
// MyIntArray.h
class MyIntArray
{
public:
    bool Add(int data);
    MyIntArray();

private:
    enum { MAX = 3}; // 상수대신 사용되는 enum 입니다.
    int mSize;
    int mArray[MAX];
}
```

```c++
#include "MyIntArray.h"

int main()
{
    MyIntArray scores;

    scores.Add(10); // true 반환
    scores.Add(20); // true 반환
    scores.Add(30); // true 반환
    scores.Add(40); // false 반환
    return 0;
}
```

그럼 float 배열을 만들고 싶으면 어떡할까요?
float에 대해서 MyFloatArray.h와 MyFloatArray.cpp를 또 만들어주는 것은 너무 힘듭니다.
이때 클래스 템플릿을 사용하면 됩니다.
여기서 주의해야할 점은 **함수 구현체는 헤더 파일에 위치해야 한다는 점입니다.**

# 함수 구현체 위치

다음의 코드를 봅시다. MyArray 클래스 템플릿을 구현해줬습니다. 
```c++
// MyArray.h
#pragma once

template<typename T>
class MyIntArray
{
public:
    bool Add(T data);
    MyIntArray();

private:
    enum { MAX = 3}; 
    int mSize;
    T mArray[MAX];
}
```

```c++
// MyArray.cpp
#include "MyArray.h"

template<typename T>
bool MyArray<T>::Add(T data)
{
    if (mSize >= Max)
    {
        return false;
    }

    mArray[mSize++] = data;
    return true;
}

template<typename>
MyArray<T>::MyArray()
    : mSize(0)
{
}
```

```c++
// Main.cpp
#include "MyArray.h"

int main()
{
    MyArray<int> scores;

    scores.Add(10); // true 반환
    scores.Add(20); // true 반환
    scores.Add(30); // true 반환
    scores.Add(40); // false 반환
    return 0;
}
```

겉보기에는 문제 없어보입니다.
컴파일 하면 아래와 같은 에러 메시지가 뜹니다. 

![image](https://user-images.githubusercontent.com/22488593/182091876-6aa8d482-363b-4b3e-97cd-a5a547319ed4.png)   

에러 메시지가 뜬 이유는
컴파일러가 Main.cpp을 컴파일할 때 MyArray.h만 인클루드 하기 때문에 MyArray.cpp를 찾지 못해서 그렇습니다.
Main.cpp와 MyArray.cpp는 나중에 링킹(Linking) 단계에서 연결이 됩니다.
템플릿은 컴파일 시 수행되므로 에러가 나는 것입니다.

# 해결책
해결책은 다음과 같이 함수 구현체를 헤더파일에 위치시키는 겁니다.

```c++
// MyArray.h
template<typename T>
class MyIntArray
{
public:
    bool Add(T data);
    MyIntArray();

private:
    enum { MAX = 3}; 
    int mSize;
    T mArray[MAX];
}

template<typename T>
bool MyArray<T>::Add(T data)
{
    if (mSize >= Max)
    {
        return false;
    }

    mArray[mSize++] = data;
    return true;
}

template<typename>
MyArray<T>::MyArray()
    : mSize(0)
{
}
```

# 템플릿 매개변수 생략

개체를 선언할 때 템플릿 매개변수를 명시해야 합니다.
함수 템플릿의 경우 템플릿 매개변수만으로 어떤 타입인지 알 수 있었지만,
개체를 선언할 때는 컴파일러가 이게 어떤 타입인지 알 길이 없습니다. 

```c++
MyArray<int> scores;    // 올바름
MyArray scores  // 에러 
```

# 클래스 템플릿 활용 

## 클레스 템플릿으로 고정크기벡터 만들기

[벡터]()는 크기가 정해지지 않았고 메모리를 무한히 증가시킬 수 있었습니다.
이때문에 생기는 문제점도 있었습니다.   
벡터의 메모리 문제가 없는 고정크기벡터를 다음과 같이 만들 수 있습니다.
```c++
// FixedVector.h
template<typename T, size_t N>
class FixedVector
{
public:
    // public 메서드들
private:
    T mArray[N];
}

// main.cpp
FixedVector<int, 16> numbers;
```

# 두 개의 템플릿 매개변수

```c++
template <class <type_name>, class <type_name>>
template <typename <type_name>, typename <type_name>>
```

함수 템플릿과 클래스 템플릿은 두 개의 템플릿 매개변수를 받을 수 있습니다.

```c++
// 두 개의 템플릿 매개변수를 사용한 함수 템플릿
template <typename T, typename U>
void Print(const T& a, const U& b)
{
    std::cout << a << " / " << b << std::endl;
}
```

```c++
// 두 개의 템플릿 매개변수를 사용한 클래스 템플릿
template <typename T, typename U>
class MyClass
{
private:
    T mX;
    U mY;
}
```

## 템플릿으로 std::pair 구현하기

[맵]()에서 키와 값을 하나의 매개변수로 넣으려고 std::pair을 사용했었습니다.
클래스 템플릿으로 std::pair을 구현할 수 있습니다.

```c++
// MyPair.h
template<typename T, typename U>
class MyPair
{
public:
    const T& GetFirst() const;
    const U& GetSecond() const;

    Mypair(const T&, cpmst U& second);
private:
    T mFirst;
    U mSecond;
};

template<typename T, typename U>
const T& MyPair<T, U>::GetFirst() const
{
    return mFirst;
}

template<typename T, typename U>
const U& Mypair<T, U>::GetSecond() const
{
    return mSecond;
}

template<typename T, typename U>
MyPair<T, U>::MyPair(const T& first, const U& second)
    : mFirst(first)
    , mSecond(second)
{
}
```

```c++
// Main.cpp
std::vector<MyPair<std::string, int> > scores;

scores.emplace_back(MyPair<std::string, int>("Coco", 10));
scoroes.emplace_back(MyPair<std::string, int>("Mocha", 100));

for (std::vector<MyPair<std::string, int>>::iterator it = scores.begin(); it != scores.end(); ++it)
{
    std::cout << it->GetFirst() << " : " << it->GetSecond() << std::endl;
}

```