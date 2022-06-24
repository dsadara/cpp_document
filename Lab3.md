# Lab3

## 명세서

```
COMP3200 실습 3
본 실습은 컴퓨터에서 해야 하는 실습입니다. 코드 작성이 끝났다면 깃 저장소에 커밋 및 푸쉬를 하고 슬랙을 통해 자동으로 채점을 받으세요.

POCU 아카데미는 임금 계산을 목적으로 각 직원들의 근무 시간을 기록하는 소프트웨어를 개발 중입니다. 각 직원들은 특정 일수 동안 한 프로젝트에 배정되고 하루가 끝날 때마다 그날 몇 시간을 일했는지 기록합니다.

본 실습에서는 근무 시간 기록 소프트웨어에 사용할 C++ 클래스 하나를 작성해야 합니다.

1. 프로젝트 준비
Lab3.sln을 비주얼 스튜디오에서 엽니다.
TimeSheet.h 파일을 프로젝트에 추가합니다. (모르겠다면 실습 1 참고)
추가한 헤더 파일에 아래 내용을 추가합니다.
#pragma once
#include <string>

namespace lab3
{
    class TimeSheet
    {
    public:
        TimeSheet(const char* name, unsigned int maxEntries);
        void AddTime(int timeInHours);
        int GetTimeEntry(unsigned int index) const;
        int GetTotalTime() const;
        float GetAverageTime() const;
        float GetStandardDeviation() const;
        const std::string& GetName() const;

    private:
       // 필요에 따라 private 변수와 메서드를 추가하세요.
    };
}
TimeSheet.cpp 파일을 프로젝트에 추가합니다.
추가한 cpp 파일에 아래 미구현된 코드를 추가합니다.
#include "TimeSheet.h"

namespace lab3
{
    TimeSheet::TimeSheet(const char* name, unsigned int maxEntries)
    {
    }

    void TimeSheet::AddTime(int timeInHours)
    {
    }

    int TimeSheet::GetTimeEntry(unsigned int index) const
    {
        return 0;
    }

    int TimeSheet::GetTotalTime() const
    {
        return 0;
    }

    float TimeSheet::GetAverageTime() const
    {
        return 0.0f;
    }

    float TimeSheet::GetStandardDeviation() const
    {
        return 0.0f;
    }

    const std::string& TimeSheet::GetName() const
    {
        return "temporary";
    }
}
2. TimeSheet.h 구현에 대한 요구 사항
보편적 규칙
double을 사용하지 말고, float를 사용할 것. double을 사용해서 점수가 깎인다면 그건 본인의 잘못입니다!
C 함수인 malloc()과 free()는 쓸 수 없습니다. (new와 delete가 있는데 굳이 왜? -_-)
STL 컨테이너 (vector, queue, map, 등등) 들을 쓰지 마세요. STL 컨테이너를 쓰는 다른 헤더들도 쓰지 마세요 (iostream, iomanip, etc, 등등). 쓰면 0점 뜨니까 헛수고 하지 마세요 :)
2.1 AddTime() 메서드
각 직원은 매일 본인이 일한 시간을 기록합니다. 이 때 호출하는 함수가 AddTime() 메서드 입니다.
직원이 하루에 근무할 수 있는 시간은 최소 1시간, 최대 10시간입니다. 만약 유효한 범위를 벗어나는 시간을 기록하려고 하면 AddTime() 메서드는 아무 일도 하지 않습니다.
예시>
TimeSheet p("Pope", 3);

p.AddTime(3); // [ 3 ]
p.AddTime(11); // [ 3 ]
p.AddTime(2); // [ 3, 2 ]
p.AddTime(-1); // [ 3, 2 ]
p.AddTime(0); // [ 3, 2 ]
p.AddTime(6); // [ 3, 2, 6 ]
p.AddTime(4); // [ 3, 2, 6 ]
2.2 GetTimeEntry() 메서드
사장님은 일 단위로 직원이 근무한 시간을 조회할 수 있습니다. 이 때, GetTimeEntry() 메서드를 호출합니다.
GetTimeEntry() 함수는 색인 위치에 시간 값이 존재하지 않으면 -1을 반환해야 합니다.
예시>
TimeSheet p("Pope", 10);

p.AddTime(2);
p.AddTime(3);
p.AddTime(10);

p.GetTimeEntry(2); // returns 10
p.GetTimeEntry(6); // returns -1
p.GetTimeEntry(0); // returns 2
2.3 GetTotalTime() 메서드
봉급을 주려면 직원이 총 몇 시간을 일했는지 알아야 겠죠?. GetTotalTime() 메서드는 현재 프로젝트에서 직원이 그동안 기록한 총 시간을 반환해야 합니다.
예시>
TimeSheet p("Pope", 10);

p.AddTime(2);
p.AddTime(3);
p.AddTime(10);

p.GetTotalTime(); // 15 반환
2.4 GetAverageTime() 메서드
회사는 직원의 평균 근무 시간에 대한 통계를 내고자 합니다. 그러므로 GetAverageTime() 메서드는 직원이 하루에 평균 몇 시간을 일하는지 반환해야 합니다.
예시>
TimeSheet p("Pope", 10);

p.AddTime(4);
p.AddTime(7);
p.AddTime(6);
p.AddTime(5);
p.AddTime(1);
p.AddTime(2);

p.GetAverageTime(); // 4.1667 반환
2.5 GetStandardDeviation() 메서드
회사는 직원의 근무 시간이 얼마나 들쭉날쭉 한 지 통계를 내고 싶어합니다. 표준 편차를 구하면 되겠군요? GetStandardDeviation() 메서드는 근무시간 기록표에 기록된 시간의 표준 편차를 반환해야 합니다.
예시>
TimeSheet p("Pope", 10);

p.AddTime(4);
p.AddTime(7);
p.AddTime(6);
p.AddTime(5);
p.AddTime(1);
p.AddTime(2);

p.GetStandardDeviation(); // 2.11476 반환
2.6 GetName() 메서드
GetName() 메서드는 해당 근무시간 기록표의 대상인 직원의 이름을 반환합니다.
예시>
TimeSheet p("Pope", 7);

p.GetName(); // "Pope" 반환
2.7 복사 생성자와 대입 연산자
함수 시그내처는 일부러 보여 드리지 않습니다. 직접 작성해 보세요.
2.8 소멸자
함수 시그내처는 일부러 보여 드리지 않습니다. 직접 작성해 보세요.
3. 본인 컴퓨터에서 테스트 하는 법
main.cpp 파일을 추가하고 main() 함수를 만듭니다.
다음은 main.cpp 파일의 예시입니다.
#include <iostream>

#include "TimeSheet.h"

int main()
{
    lab3::TimeSheet employee1("John", 10);
    employee1.AddTime(4);
    employee1.AddTime(7);
    std::cout << "Employee: " << employee1.GetName() << " / AverageTime: " << employee1.GetAverageTime() << " / TotalTime: " << employee1.GetTotalTime() << std::endl;

    return 0;
}
4. 커밋, 푸쉬 그리고 빌드 요청
이건 어떻게 하는 지 이제 다 아시죠? :)
```

## 설명

- C++ 스타일 동적할당\/해제인 new와 delete를 사용하여 실행 중에 결정되는 갯수만큼의 int 배열을 만들어야 함
- 복사 연산자, 대입 연산자, 소멸자를 직접 구현해야 함
- 필요한 데이터들을 담을 private 멤버 변수 또는 도우미 함수들을 직접 구현해야 함

* implicit casting이 일어나지 않도록 직접 캐스팅 해 줘야 함

### 복사 생성자

- 복사할 오브젝트에 있는 것과 같이 동적 메모리 할당 그리고 나머지 멤버는 대입

### 대입 연산자

- 자기자신을 대입할 때 early return 방법
  ```c++
  void TimeSheet::operator=(const TimeSheet& rhs)
  {
      // 나 자신을 대입하는 경우 early return
      if (this == &rhs)
      {
          return;
      }
  }
  ```
  - 기존 메모리 할당을 해제하고 복사할 오브젝트와 같은 크기의 메모리를 할당
  - 이외 복사할 오브젝트의 멤버들은 다 대입하면 됨

### 소멸자

- 동적 할당으로 만든 배열이라면 delete시 대괄호\(\[\]\) 꼭 붙이기
- 포인터 배열이고 가리키는 대상이 heap 메모리에 저장되어 있다면 배열의 개별 대상도 delete 필요함
  ```c++
  // 객체를 가리키는 포인터 배열
  object *obj[] = new * object[10];
  // 소멸자
  for (int i = 0; i < 10; i++)
  {
      delete obj[i];
  }
  delete obj;
  ```

### 멤버 구현

- 동적 int 배열
  - 실행 중에 Work Time들의 수가 결정되므로 동적으로 배열을 생성해야 한다
    ```c++
    // TimeSheet.h
    int* mWorkTimes;
    // TimeSheet.cpp
    // 생성자에서
    mWorkTimes = new int[입력받은 갯수];
    ```
- 배열에서 현재 비어있는 index를 가리키는 변수가 필요
  - Add 하면 \++해주고 배열경계를 체크해줌
- Name을 저장할 std::string 객체
  - Name은 메모리를 많이 차지 하지 않을 것이고 GetName()함수에만 쓰임
  - 스택에 string 객체를 만들어 줌
  - GetName()시 복사되어 나간다
  - 생성자 사용하여 const char\* 대입
