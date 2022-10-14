# C 스타일 Enum
기존에 C에서 쓰던 Enum은 int형 숫자에 이름을 붙여주는 것이었습니다.
Enum의 문제점은 타입 체킹이 되지 않아 다른 타입의 Enum끼리의 비교 시 비교가 된다는 문제점이 있습니다.
다음의 코드를 봅시다. 

```c++
// Main.cpp
enum eScoreType
{
    Assignment1,    // 0
    Assignment2,    // 1
    Assignment3,    // 2
    Midterm,        // 3
    Count,          // 4
}

enum eStudyType
{
    Fulltime,   // 0
    Parttime,   // 1
};

// Main.cpp
int main()
{
    eScoreType type = Midterm;
    eStudyType studyType = Fulltime;

    int num = Assignment3;

    if (type == Fulltime) // 다른 타입끼리 비교 시 에러가 나는게 옳음
    {
        // 이하 코드 생략
    }

    return 0;
}

```

eScoreType인 Midterm과 eStudyType인 Fulltime을 비교 시 같은 int 이므로 컴파일 에러가 나지 않습니다.
해결책은 C++11의 enum class를 사용하면 됩니다.
enum class를 사용하면 enum 타입 체킹이 가능해집니다. 
다음 코드는 enum class를 사용한 코드입니다.   

```c++
// Main.cpp
enum class eScoreType   // enum class 사용
{
    Assignment1,    // 0
    Assignment2,    // 1
    Assignment3,    // 2
    Midterm,        // 3
    Count,          // 4
}

enum class eStudyType
{
    Fulltime,   // 0
    Parttime,   // 1
};

// Main.cpp
int main()
{
    eScoreType type = eScoreType::Midterm;
    eStudyType studyType = eStudyType::Fulltime;

    int num = eScoreType::Assignment3;  // 에러 

    if (score == eStudyType::Fulltime) // 에러
    {
        // 이하 코드 생략
    }

    return 0;
}

```

다음과 같은 에러가 납니다. 

![image](https://user-images.githubusercontent.com/22488593/183056580-ac09465b-224d-4e48-a725-55e2e3c3c086.png)

enum class를 정리하자면   

# enum class

* C++11에서 제대로 지원하기 시작했습니다.
* 정수형으로의 암시적 캐스팅이 없습니다.
* 자료형 검사를 해줍니다.
* 또한 enum에 할당할 바이트 양도 정할 수도 있습니다.

# enum class 바이트 수 정하기

다음 코드는 uint8_t를 사용해서 8비트 정수를 집어넣겠다고 명시한 enum class입니다. 

```c++
// Class.h
#include <cstdint>

enum class eScoreType : uint8_t
{
    Assignment1,
    Assignment2,
    Assignment3,
    Midterm,
    Final = 0x100, // 컴파일 시 경고가 뜸, 8비트 unsigned int의 범위를 넘어서기 때문
}
```
8비트 unsigned 정수만 들어갈 수 있으므로 0x100 즉 256을 넣으면 범위를 넘어섭니다.
범위를 넘으면 컴파일러가 다음과 같은 경고 메시지를 줍니다.

![image](https://user-images.githubusercontent.com/22488593/183329024-c02f0d17-aa85-4bf8-b587-a58ed23b5cb2.png)

enum class 요소를 반복문에 사용시에는 아래와 같이 int로 정적 캐스팅해줘야 합니다.

![image](https://user-images.githubusercontent.com/22488593/183059587-5f255a76-bb13-4299-9ad8-befa2c855c56.png)