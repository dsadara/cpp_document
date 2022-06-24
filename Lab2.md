# Lab2

## 명세서

```
COMP3200 실습 2
본 실습은 컴퓨터에서 해야 하는 실습입니다. 코드 작성이 끝났다면 깃 저장소에 커밋 및 푸쉬를 하고 슬랙을 통해 자동으로 채점을 받으세요.

수많은 소프트웨어 애플리케이션에서 사람과 컴퓨터 간의 상호작용이 필요합니다. 사용자는 데이터와 그 데이터를 처리하는 방법을 제공하고(입력), 컴퓨터는 처리한 데이터를 시각적인 형태로 반환(출력)합니다. 본 실습에서는 두 개의 C++ 함수를 구현해야 합니다. 두 함수는 정수 또는 부동 소수점 수들을 읽어 간단한 수학적 연산을 수행한 뒤, 그 결과값들을 사람이 읽기 편한 서식에 맞추어 출력합니다.

주니어(신입, 초급) 개발자는 이렇게 문서 출력 형식을 맞추는 것처럼 지루한 일을 배정받는 일이 흔합니다. 아직 시스템 기획 및 설계를 할 정도의 기술력이 없기 때문이죠. 또한 이런 단순 반복 작업들은 더 큰 책임을 맡길 수 있을 만큼 꼼꼼한 개발자인지 판단하는 좋은 방법이기도 합니다. 대부분의 주니어들은 인터미디엇(중급) 수준이 되기 전까지 한동안 이런 종류의 일을 하며 고생합니다. 이번 실습을 통해서 이런 일을 시켜서 정말 죄송하게 생각합니다만 실습에서 입출력 같은 지루한 일을 시키는 건 이게 처음이자 마지막이니 이해해 주세요.

1. 프로젝트 준비
비주얼 스튜디오에서 Lab2.sln을 엽니다.
프로젝트에 Lab2.h 파일을 추가합니다. (혹시 방법을 모르신다면 실습 1을 참고해주세요)
방금 추가한 헤더 파일에 아래 내용을 추가합니다.
#pragma once

#include <iostream>

namespace lab2
{
	// in: 여기서 사용자 입력을 읽으세요. cin에서 읽는 게 아닙니다.
	// out: 여기에 출력을 쓰세요. cout에 쓰는 게 아닙니다.
	void PrintIntegers(std::istream& in, std::ostream& out);
	void PrintMaxFloat(std::istream& in, std::ostream& out);
}
Lab2.cpp 파일을 프로젝트에 추가합니다.
아래 내용을 cpp 파일에 추가합니다.
#include "Lab2.h"

namespace lab2
{
    void PrintIntegers(std::istream& in, std::ostream& out)
    {

    }

    void PrintMaxFloat(std::istream& in, std::ostream& out)
    {

    }
}
2. PrintIntegers()와 PrintMaxFloat() 구현
일반적인 규칙
PrintIntegers()와 PrintMaxFloat()는 반드시 첫 번째 매개변수 in에서 입력을 읽어 와야 합니다. std::cin을 사용하면 0점 처리 합니다.
PrintIntegers()와 PrintMaxFloat()는 in에서 반드시 EOF 까지 입력을 읽어 와야 합니다.
PrintIntegers()와 PrintMaxFloat()는 반드시 두 번째 매개변수 out에 출력을 해야 합니다. std::cout을 사용하면 0점 처리 합니다.
PrintIntegers()과 PrintMaxFloat()는 반드시 각 함수의 스펙에 따라 출력 포맷을 맞춰야 합니다.
부동 소수점 값을 다룰 때에는 반드시 double 대신 float를 써야 합니다.
2.1. PrintIntegers() 함수 구현
일련의 정수들을 입력 받아 각 정수를 8진수, 10진수, 16진수로 출력합니다. 출력 형식은 아래와 일치해야 합니다.

.........oct........dec......hex⤶
------------.----------.--------⤶
..........64.........52.......34⤶
.........146........102.......66⤶
...........1..........1........1⤶
........4516.......2382......94E⤶
기호

⤶: 줄 바꿈 문자

.: 스페이스

위 출력은 아래 입력의 결과입니다.

52 102
      1  abc        2382
입력값 오류처리
입력에는 최소한 하나의 정수가 있을 것입니다.
입력은 다음으로만 구성됩니다.
양의 정수
공백
문자열
모든 문자열은 무시해야 합니다.
2.2. PrintMaxFloat() 함수 구현
일련의 부동 소수점 수들을 입력 받아서 각 부동 소수점 수를 아래 형식에 따라 출력해야 합니다. 또한 마지막 줄에는 가장 큰 수를 출력해야 합니다.

.....+.........2.230⤶
.....-......1912.872⤶
.....+......2323.000⤶
.....+.........1.000⤶
max:.+......2323.000⤶
위 출력은 아래 입력의 결과입니다.

2.23
              -1912.87233125
    2323 def 1
입력값 오류처리
입력에는 최소한 하나의 숫자가 있을 것입니다.
입력은 다음으로만 구성됩니다.
부동 소수점 수 (양수, 음수 모두 포함)
정수 (양수, 음수 모두 포함)
공백
문자열
모든 문자열은 무시해야 합니다.
3. 본인 컴퓨터에서 테스트 하는 법
main.cpp 파일을 추가하고 main() 함수를 만듭니다.
아래와 같이 cin과 cout을 매개변수로 전달하여 로컬에서 함수들을 테스트할 수 있습니다.
lab2::PrintIntegers(std::cin, std::cout);

main.cpp 파일은 아마 아래처럼 되겠죠.

#include "Lab2.h"

#include <iostream>

using namespace std;

int main()
{
	lab2::PrintIntegers(cin, cout);
	lab2::PrintMaxFloat(cin, cout);

	return 0;
}
제대로 테스트를 하려면 명령 프롬프트(command line)에서 입력 재지정(input redirection)을 사용해야 할 수도 있습니다.
cmd> Lab2 < testinput.txt

4. 커밋, 푸쉬 그리고 빌드 요청
이건 어떻게 하는 지 이제 다 아시죠? :)
```

## 설명

- 입력버퍼에 있는 숫자들을 whitespace로 구분하여 하나씩 입력받아 지정된 포맷에 맞춰 출력하는 예제
- 숫자 또는 문자열이 입력되는데 문자열 입력 시 또는 EOF시 오류처리가 필요
- 입력값 오류처리
  - 입력버퍼에 문자열이 있으면 failbit 세트됨
    - failbit clear해주고 문자열을 따로 입력해줘야 함
  - EOF 상태여도 failbit 발생함
    - 이 때는 그냥 종료

```c++
void PrintIntegers(std::istream& in, std::ostream& out)
{
    int num;  // 숫자 입력 변수
    string trash;   // 버리는 문자열

    while (true)
    {
        in >> num // in에 입력
        if (in.fail())   // fail bit 세트되면
        {
            if (in.eof())   // End of File이면
            {
                break;      // 탈출
            }
            // 아니면 문자열이 입력되어 failbit이 발생한 상태
            in.clear()      // 먼저 failbit 클리어 하고
            in >> trash;    // 문자열 입력
        }
        else    // fail bit이 세트 안되면
        {
            out << num << std::endl;  // 원하는 포맷대로 출력
        }
    }
}
```
