# assert
static_assert를 살펴보기 앞서 assert가 뭔지 살펴봅시다.
assert는 실행 중에 가정이 맞는지 평가하는 함수였습니다.
프로그램 실행 중 가정이 틀리면 즉 조건이 false면 실행을 멈춥니다.
assert함수는 디버그 빌드에서만 작동합니다.
assert함수의 단점은 실패한 assert를 보려면 반드시 프로그램을 실행해야한다는 점입니다.
이게 왜 단점이 되냐면 프로그램 실행 시 모든 코드 경로가 실행되었다고 장담할 수 없기 때문입니다.
그리고 컴파일 시 확인되는 조건을 굳이 실행시켜서 확인하는 것은 비효율적입니다. 
```c++
// Class.cpp
#include <cassert>
const Student* Class::GetStudentInfo(const char* name)
{
    assert(name != NULL);   // name이 NULL이 아님을 가정. 즉 name이 NULL이면 실행을 멈춤
    // 이하 코드 생략
}
```

이러한 assert의 단점을 해결하기위해 컴파일 중에 가정을 확인하는 static_assert가 나왔습니다. 

# static_assert
static_assert는 컴파일 중에 assertion 평가를 합니다.
즉 실행 중이 아닌 컴파일 중 assert()가 실행된다고 보시면 됩니다.
실패하면 즉 조건 false면 컴파일 에러가 나므로 컴파일을 할 수 없습니다.
static_assert는 많은 경우에 유용합니다.
그리고 여담이지만 강사분께서 C++11중 가장 좋아하는 기능이라고 합니다.  

# static_assert 사용처
## 예시1: 구조체의 크기
Student 구조체를 74바이트라고 가정하고 코드를 작성했었는데,
어느날 Student에 int형 currentSemester를 추가하면 Student는 78바이트로 기존 코드에 문제가 생길 수 있습니다.   
이를 방지하는데 static_assert()를 사용할 수 있습니다.
위 코드의 경우 Student는 74바이트가 아니므로 컴파일 에러가 납니다. 
```c++
// Class.h
struct Student
{
    char name[64];  // 64바이트
    char id[10];    // 10바이트
    int currentSemester;    // 4바이트 
    // 총 78바이트임
};

class Class
{
public:
    // ...
    const Student* GetStudentInfo(const char* name);
};
```

```c++
// Class.cpp
const Student* Class::GetStudentInfo(const char * name)
{
    static_assert(sizeof(Student) == 74, "Student structure size mismatch");    // 74바이트가 아니면 컴파일 에러, 메시지 출력
    // 이하 코드 생략
}
```

![image](https://user-images.githubusercontent.com/22488593/182806434-aa1330a8-ee5a-4ebc-9edc-cd2fc60bcedf.png)

## 예시2: Version 확인하기

클래스에 버전에 해당하는 static 변수가 있다고 합시다.
버전이 다를 경우 컴파일이 안되게 할 수 있습니다. 

```c++
// Class.h
class Class
{
public:
    const static int Version = 1;
    // ...+
};
```
```c++
// Main.cpp
#include "Class.h"

static_assert(Class::Version > 1, "You need higher version than 1");
```

![image](https://user-images.githubusercontent.com/22488593/182806565-9b6f5d11-632a-44d5-a668-4ecf002f1151.png)


## 예시3: 배열의 길이
컴파일시 길이가 정해지는 배열의 경우 배열의 길이를 체크할 수 있습니다.
```c++
// Student.h
class Student
{
public:
    static const int MAX_SCORES = 10;
    int GetScore(int index);
    // ...
private:
    int mScores[MAX_SCORES];    // 컴파일 시 배열의 길이가 정해집니다.
}
```

```c++
// Student.cpp
int Student::GetScore(int index)
{
    static_assert(sizeof(mScores) / sizeof(mScores[0]) == MAX_SCORES, "The size of scores vector is not 10");
    // 이하 코드 생략 
}
```

sizeof(mScores)는 mScores 배열이 컴파일 시 정해질 때만 바이트 수를 반환합니다.
컴파일 시 정해지지 않으면 mScores포인터를 넣는 것이라서 포인터 사이즈만 반환이 됩니다.

# static_assert 베스트 프렉티스
최대한 assert보다 static_assert를 사용합시다.
static_assert를 사용하면 컴파일 중에 모든 문제를 알 수 있고,
컴파일러처럼 생각하는데 도움이 된다고 합니다.
예를들어 static_assert시 컴파일이 안되는 이유를 살펴보면 실행중에 결정이 되는 조건, 데이터값이 뭔지 알 수 있고 컴파일러처럼 생각할 수 있다고 합니다. 
