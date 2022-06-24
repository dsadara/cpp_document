# Lab5

# 명세서

```
COMP3200 실습 5
본 실습은 컴퓨터에서 해야 하는 실습입니다. 코드 작성이 끝났다면 깃 저장소에 커밋 및 푸쉬를 하고 슬랙을 통해 자동으로 채점을 받으세요.

'바니의 완벽한 잔디밭(Barney's Perfect Lawn)'은 고객의 마당에 완벽한 잔디밭을 조성해 주는 스타트업 기업으로, 최근 고객의 수가 10배씩 늘며 사업이 번창하고 있습니다. 이 회사의 대표인 바니는 갑작스럽게 증가한 수요에 대응하기 위해서 수작업으로 처리하던 다음의 일들을 자동화하기로 결정했습니다.

잔디밭 넓이에 따라 필요한 최소한의 롤잔디 및 울타리 자재의 수 계산
잔디밭 조성에 쓰이는 잔디 및 울타리 자재의 총가격 계산
바니는 잔디밭의 넓이와 모양에 따른 가격과 자재 수량을 계산해 내는 프로그램의 제작을 의뢰하기 위해 여러분에게 연락을 해 왔습니다.

4개의 구체적인 클래스, RectangleLawn, SquareLawn, CircleLawn, EquilateralTriangleLawn을 구현하는 것이 본 실습입니다.

1. 프로젝트 준비하기
비주얼 스튜디오에서 Lab5.sln을 엽니다.
프로젝트에 eFenceType.h 파일을 추가합니다.
추가한 헤더 파일에 아래 내용을 추가합니다.
#pragma once

namespace lab5
{
	enum eFenceType
	{
		RED_CEDAR,
		SPRUCE
	};
}
프로젝트에 eGrassType.h 파일을 추가합니다.
추가한 헤더 파일에 아래 내용을 추가합니다.
#pragma once

namespace lab5
{
	enum eGrassType
	{
		BERMUDA,
		BAHIA,
		BENTGRASS,
		PERENNIAL_RYEGRASS,
		ST_AUGUSTINE
	};
}
프로젝트에 Lawn.h 파일을 추가합니다.
추가한 헤더 파일에 아래 내용을 추가합니다.
#pragma once

#include "eGrassType.h"

namespace lab5
{
	class Lawn
	{
	public:
		Lawn();
		virtual ~Lawn();

		virtual unsigned int GetArea() const = 0;

		unsigned int GetGrassPrice(eGrassType grassType) const;
		unsigned int GetMinimumSodRollsCount() const;
	};
}
프로젝트에 Lawn.cpp 파일을 추가합니다.
추가한 cpp 파일에 아래 미구현된 코드를 추가합니다.
#include "Lawn.h"

namespace lab5
{
	Lawn::Lawn()
	{
	}

	Lawn::~Lawn()
	{
	}

	unsigned int Lawn::GetGrassPrice(eGrassType grassType) const
	{
		return 0;
	}

	unsigned int Lawn::GetMinimumSodRollsCount() const
	{
		return 0;
	}
}
프로젝트에 IFenceable.h 파일을 추가합니다.
추가한 헤더 파일에 아래 내용을 추가합니다.
#pragma once

#include "eFenceType.h"

namespace lab5
{
	class IFenceable
	{
	public:
		virtual unsigned int GetMinimumFencesCount() const = 0;
		virtual unsigned int GetFencePrice(eFenceType fenceType) const = 0;
	};
}
2. 구현하기
보편적 규칙
이번 실습에서 기본적으로 제공되는 파일은 인터페이스와 추상 클래스뿐입니다. 아래 나열된 구체적인 클래스들은 여러분이 직접 구현해야 합니다. 이 때, 필요에 따라 IFenceable과 Lawn으로부터 상속을 받으세요.
CircleLawn
RectangleLawn
SquareLawn
EquilateralTriangleLawn
구체 클래스들의 이름은 꼭 위에 있는 이름과 같아야 합니다. 그렇게 하지 않으면 여기저기서 컴파일 에러가 나니까 주의해 주세요!
계산을 할 때는 float 대신 double을 사용하고 반환값은 정수로 해주세요. 그러려면 반올림을 하는 게 일반적이지만 상황에 따라서는 무조건 올림을 할 일도 있을 겁니다. 언제 그래야 하는지는 알아서 판단하세요. ;)
필요한 경우 복사 생성자와 소멸자를 구현하세요.
잔디밭의 모든 치수는 미터를 사용합니다.
모든 클래스에 공통적으로 적용되는 정보
'바니의 완벽한 잔디밭'이 조성 가능한 잔디밭의 모양은 다음과 같습니다.
원형(Circle)
직사각형(Rectangle)
정사각형(Square)
정삼각형(EquilateralTriangle)
울타리를 칠 수 있는 잔디밭의 모양은 다음에 한합니다. (힌트: IFenceable)
직사각형(Rectangle)
정사각형(Square)
정삼각형(Equilateral Triangle)
잔디 가격
버뮤다(Bermuda) - 800 센트(cents) / m2
바이아(Bahia) - 500 센트 / m2
겨이삭(Bent grass) - 300 센트 / m2
호밀풀(Perennial Ryegrass) - 250 센트 / m2
세인트 어거스틴(St Augustine) - 450 센트 / m2
울타리 자재 가격
연필향나무(Red Cedar) - $6 / m
가문비나무(Spruce) - $7 / m
롤잔디 1개의 넓이는 0.3m2입니다.
울타리 자재 1개의 폭(너비)은 25cm입니다.
파이(PI) 값으로는 3.14를 사용하세요.
2.1 Lawn::GetArea() 구현하기
GetArea()는 순수 가상 함수죠. 따라서 각 구체 클래스에서 이 함수를 구현해야 합니다.
GetArea()가 반환하는 넓이의 단위는 m2입니다.
2.2 Lawn::GetGrassPrice() 구현하기
잔디밭 조성 면적을 가득 메우는 데 소요되는 잔디의 총가격을 반환합니다. 이 가격은 '바니의 완벽한 잔디밭'이 고객에게 청구하는 금액이며, 쓰고 남은 잔디에 대한 비용은 청구하지 않습니다.
GetGrassPrice()가 반환하는 가격의 화폐 단위는 달러입니다. (참고: 1달러는 100센트입니다)
예시>
Lawn* lawn = new RectangleLawn(10, 20); // 10m, 20m
unsigned int grassPrice = lawn->GetGrassPrice(BERMUDA); // 잔디 가격: 1600 (달러)
2.3 Lawn::GetMinimumSodRollsCount() 구현하기
잔디밭을 조성할 때 필요한 최소 롤잔디 개수를 반환합니다.
예시>
Lawn* lawn = new RectangleLawn(10, 20);
unsigned int sodRollsCount = lawn->GetMinimumSodRollsCount(); // 667개의 롤잔디가 필요
2.4 IFenceable::GetMinimumFencesCount() 구현하기
잔디밭에 울타리를 칠 때 필요한 최소 울타리 자재 수를 반환합니다.
이 클래스는 인터페이스입니다. 인터페이스 내에서 메서드를 구현하면 안 되니 IFenceable을 상속하는 각 구체 클래스에서 구현해 주세요.
2.5 IFenceable::GetFencePrice() 구현하기
잔디밭에 울타리를 치는 데 소요되는 울타리 자재의 총가격을 반환합니다. 이 가격은 '바니의 완벽한 잔디밭'이 고객에게 청구하는 금액이며, 쓰고 남은 울타리 자재에 대한 비용은 청구하지 않습니다.
GetFencePrice()가 반환하는 가격의 화폐 단위는 달러입니다. (참고: 1달러는 100센트입니다)
이 클래스는 인터페이스입니다. 인터페이스 내에서 메서드를 구현하면 안 되니 IFenceable을 상속하는 각 구체 클래스에서 구현해 주세요.
2.6 RectangleLawn 클래스 구현하기
RectangleLawn 클래스 생성자의 인자는 폭(첫 번째 인자)과 높이 둘뿐입니다.
폭과 높이 매개 변수의 자료형은 unsigned int입니다.
2.7 SquareLawn 클래스 구현하기
SquareLawn 클래스 생성자의 인자는 폭(너비) 하나뿐입니다.
폭 매개 변수의 자료형은 unsigned int입니다.
힌트: SquareLawn이 구체적인 클래스들 중 하나를 상속할 수 있을까요?
2.8 CircleLawn 클래스 구현하기
CircleLawn 클래스 생성자의 인자는 반지름 하나뿐입니다.
반지름 매개 변수의 자료형은 unsigned int입니다.
CircleLawn에는 울타리를 칠 수 없다는 사실을 기억하세요.
2.9 EquilateralTriangleLawn 클래스 구현하기
EquilateralTriangleLawn 클래스 생성자의 인자는 한 변의 길이 하나뿐입니다.
한 변의 길이의 자료형은 unsigned int입니다.
3. 본인 컴퓨터에서 테스트하는 법
프로젝트에 main.cpp를 추가하고, 그 안에 테스트 코드를 작성하세요. 아래는 테스트 코드의 한 예입니다.
#include <cassert>

#include "Lawn.h"
#include "IFenceable.h"
#include "RectangleLawn.h"
#include "CircleLawn.h"
#include "SquareLawn.h"
#include "EquilateralTriangleLawn.h"

using namespace lab5;

int main()
{
	Lawn* lawn = new RectangleLawn(10, 20);
	unsigned int area = lawn->GetArea();
	unsigned int grassPrice = lawn->GetGrassPrice(BERMUDA);
	unsigned int sodRollsCount = lawn->GetMinimumSodRollsCount();

	assert(area == 200);
	assert(grassPrice == 1600);
	assert(sodRollsCount == 667);

	RectangleLawn* rectangleLawn = (RectangleLawn*)lawn;

	unsigned int fencePrice = rectangleLawn->GetFencePrice(RED_CEDAR);
	unsigned int fencesCount = rectangleLawn->GetMinimumFencesCount();

	assert(fencePrice == 360);
	assert(fencesCount == 240);
	
	// 위와 같은 방식으로 다른 구체 클래스들도 테스트할 수 있음
	return 0;
}
4. 커밋, 푸쉬 그리고 빌드 요청
이건 어떻게 하는 지 이제 다 아시죠? 
```


# 설명
* 순수 추상 클래스를 이용한 인터페이스의 함수를 자식에서 구현
* 복사 생성자와 소멸자를 구현
* implicit casting이 일어나지 않게 static_cast처리 하기
* virtual 키워드가 필요한 곳에 virtual 붙이기
    * 소멸자, 공통함수들