# Lab4

## 명세서

```
COMP3200 실습 4
본 실습은 컴퓨터에서 해야 하는 실습입니다. 코드 작성이 끝났다면 깃 저장소에 커밋 및 푸쉬를 하고 슬랙을 통해 자동으로 채점을 받으세요.

점(Point)은 x, y 좌표를 가지는 2D 개체입니다. 이 점 여러 개를 이으면 하나 이상의 직선(Polyline)을 만들 수 있습니다. 본 실습에서는 Point와 PolyLine이라는 두 개의 C++ 클래스를 작성해야 합니다.

1. 프로젝트 준비
Lab4.sln을 비주얼 스튜디오에서 엽니다.
Point.h 파일을 프로젝트에 추가합니다.
추가한 헤더 파일에 아래 내용을 추가합니다.
#pragma once

namespace lab4
{
	class Point
	{
	public:
		Point(float x, float y);
		~Point();

		Point operator+(const Point& other) const;
		Point operator-(const Point& other) const;
		float Dot(const Point& other) const;
		Point operator*(float operand) const;

		float GetX() const;
		float GetY() const;
	};
}
Point.cpp 파일을 프로젝트에 추가합니다.
추가한 cpp 파일에 아래 미구현된 코드를 추가합니다.
#include "Point.h"

namespace lab4
{
	Point::Point(float x, float y)
	{
	}

	Point::~Point()
	{
	}

	Point Point::operator+(const Point& other) const
	{
		return Point(0.f, 0.f);
	}

	Point Point::operator-(const Point& other) const
	{
		return Point(0.f, 0.f);
	}

	float Point::Dot(const Point& other) const
	{
		return 0.0f;
	}

	Point Point::operator*(float operand) const
	{
		return Point(0.f, 0.f);
	}

	float Point::GetX() const
	{
		return 0.0f;
	}

	float Point::GetY() const
	{
		return 0.0f;
	}
}
PolyLine.h 파일을 프로젝트에 추가합니다.
추가한 헤더 파일에 아래 내용을 추가합니다.
#pragma once

#include "Point.h"

namespace lab4
{
	class PolyLine
	{
	public:
		PolyLine();
		PolyLine(const PolyLine& other);
		~PolyLine();

		bool AddPoint(float x, float y);
		bool AddPoint(const Point* point);
		bool RemovePoint(unsigned int i);
		bool TryGetMinBoundingRectangle(Point* outMin, Point* outMax) const;

		const Point* operator[](unsigned int i) const;
	};
}
PolyLine.cpp 파일을 프로젝트에 추가합니다.
추가한 cpp 파일에 아래 미구현된 코드를 추가합니다.
#include <cstring>
#include <cmath>
#include "PolyLine.h"

namespace lab4
{
	PolyLine::PolyLine()
	{
	}

	PolyLine::PolyLine(const PolyLine& other)
	{
	}

	PolyLine::~PolyLine()
	{
	}

	bool PolyLine::AddPoint(float x, float y)
	{
		return false;
	}

	bool PolyLine::AddPoint(const Point* point)
	{
		return false;
	}

	bool PolyLine::RemovePoint(unsigned int i)
	{
		return false;
	}

	bool PolyLine::TryGetMinBoundingRectangle(Point* outMin, Point* outMax) const
	{
		return false;
	}

	const Point* PolyLine::operator[](unsigned int i) const
	{
		return new Point(0.0f, 0.0f);
	}
}
2. Point와 PolyLine 클래스 구현에 대한 요구 사항
보편적 규칙
Point 클래스는 x, y 좌표를 가지는 2D 개체입니다.
PolyLine이 가질 수 있는 Point의 최대 개수는 10개입니다.
double을 사용하지 말고, float를 사용하세요. double을 사용해서 점수가 깎인다면 그건 본인의 잘못입니다!
STL 컨테이너 (vector, queue, map, 등등) 들을 쓰지 마세요. 쓰면 0점 뜨니까 헛수고 하지 마세요 :)
2.1 Point::operator+()
요소(멤버)별 덧셈을 수행합니다.
예시>
Point p1(1.3f, 2.1f);
Point p2(3.4f, 4.5f);

Point p3 = p1 + p2; // p3는 [4.7f, 6.6f]
2.2 Point::operator-()
요소(멤버)별 뺄셈을 수행합니다.
예시>
Point p1(1.4f, 2.0f);
Point p2(1.2f, 4.0f);

Point p3 = p1 - p2; // p3는 [0.2f, -2.0f]
2.3 Point::operator*()
스칼라 곱셈을 수행합니다.
예시>
Point p1(1.2f, 2.5f);

Point p2 = p1 * 4.0f; // p2는 [4.8f, 10.0f]
Point p3 = 5.0f * p1; // p3는 [6.0f, 12.5f]
2.4 Point::Dot()
두 점의 내적(dot product)을 수행합니다. (https://ko.wikipedia.org/wiki/%EC%8A%A4%EC%B9%BC%EB%9D%BC%EA%B3%B1)
예시>
Point p1(1.0f, 2.0f);
Point p2(1.0f, 4.0f);

float dotProduct = p1.Dot(p2); // dotProduct는 9.0f
2.5 Point::GetX()
점의 X 좌표를 반환합니다.
예시>
Point p1(1.5f, 2.3f);
float x = p1.GetX(); // x는 1.5f
2.6 Point::GetY()
점의 Y 좌표를 반환합니다.
예시>
Point p1(1.5f, 2.3f);
float y = p1.GetY(); // y는 2.3f
2.7 PolyLine::AddPoint()
점 하나를 PolyLine 개체에 추가합니다.
시그내처가 다른 두 개의 AddPoint() 메서드가 있다는 점에 유의해주세요. 하나는 인자로 float 값 두 개를, 다른 하나는 상수 포인터(constant pointer)를 받습니다.
점이 PolyLine에 추가되면 true를 반환합니다. 추가된 점의 라이프사이클(수명)은 PolyLine이 관리하지만, PolyLine 개체를 소멸시키거나 RemovePoint()같은 메서드를 호출하여 점을 소멸시키지 않는 한 여전히 클래스 밖에서도 점을 가리키는 포인터를 사용할 수 있습니다. 만약 PolyLine에 점을 추가할 수 없었다면 false를 반환합니다.
예시>
PolyLine pl;

pl.AddPoint(1.0f, 2.0f);
pl.AddPoint(new Point(2.0f, 3.0f));
pl.AddPoint(2.2f, 1.9f); // pl은 [1.0f, 2.0f], [2.0f, 3.0f], [2.2f, 1.9f]
pl.AddPoint(5.2f, 8.9f);
pl.AddPoint(2.2f, 1.4f);
pl.AddPoint(10.1f, 11.9f);
pl.AddPoint(7.5f, 1.9f);
pl.AddPoint(6.6f, 4.5f);
pl.AddPoint(3.1f, 0.9f);
pl.AddPoint(0.1f, 0.1f); // 10번째 점. 이때까지 AddPoint()는 true를 반환.

pl.AddPoint(2.2f, 1.9f); // 11번째 점 추가 시도. AddPoint()는 false를 반환하고 이 점을 PolyLine 개체에 추가하지 않아야 함.
2.8 PolyLine::RemovePoint()
색인 위치에 있는 점을 제거합니다.
성공적으로 제거되면 true를, 아니라면 false를 반환합니다.
예시>
PolyLine pl;
pl.AddPoint(1.0f, 2.0f);
pl.AddPoint(new Point(2.0f, 3.0f));
pl.AddPoint(2.2f, 1.9f);

pl.RemovePoint(1); // pl은 [1.0f, 2.0f], [2.2f, 1.9f]. true를 반환.

pl.RemovePoint(3); // 4번째 점이 존재하지 않기 때문에 pl은 변하지 않음. false를 반환.
2.9 PolyLine::TryGetMinBoundingRectangle()
PolyLine을 구성하는 모든 점들을 감쌀 수 있는 최소 크기의 사각형(최소 경계 사각형)을 구합니다.
http://www.yaldex.com/games-programming/FILES/08fig40.gif 참고
최소 경계 사각형의 꼭짓점 중 x ,y 좌표 값이 가장 작은 점이 outMin, 가장 큰 점이 outMax가 되어야 합니다.
최소 경계 사각형을 구할 수 없다면 false를 반환합니다.
예시>
PolyLine pl;
pl.AddPoint(1.4f, 2.8f);
pl.AddPoint(3.7f, 2.5f);
pl.AddPoint(5.5f, 5.5f);
pl.AddPoint(-2.9f, 4.1f);
pl.AddPoint(4.3f, -1.0f);
pl.AddPoint(6.2f, 4.4f);

Point minP(0.f, 0.f);
Point maxP(0.f, 0.f);

pl.TryGetMinBoundingRectangle(&minP, &maxP); // min: [-2.9f, -1.0f], max: [6.2f, 5.5f]
2.10 PolyLine::operator[]()
색인 위치에 있는 점을 가져옵니다.
예시>
PolyLine pl;
pl.AddPoint(1.7f, 2.4f);
pl.AddPoint(3.9f, 2.1f);
pl.AddPoint(5.3f, 5.5f);
pl.AddPoint(-2.1f, 4.0f);

pl[0]; // [1.7f, 2.4f]
pl[3]; // [-2.1f, 4.0f]
pl[6]; // NULL
2.11 필요한 경우 소멸자, 복사 생성자 그리고 대입 연산자를 구현하세요
소멸자, 복사 생성자 그리고 대입 연산자를 직접 구현해야 할지 곰곰이 생각해 보세요.

Point와 PolyLine 클래스가 메모리를 할당하나요?
얕은 복사가 필요한가요 아니면 깊은 복사가 필요한가요?
정말로 힙 메모리 할당이 필요한가요?
복사 생성자를 구현한다면 그에 대응하는 대입 연산자도 반드시 구현해 주세요.
3. 본인 컴퓨터에서 테스트하는 법
예를 들어 main.cpp에서 아래와 같은 코드를 통해 자신의 코드를 테스트할 수 있습니다.
#include <cassert>
#include <cmath>

#include "Point.h"
#include "PolyLine.h"

using namespace lab4;

int main()
{
	const double EPSILON = 0.0009765625;

	Point p1(2, 3);
	Point p2(-1, 4);

	Point p3 = p1 + p2;

	assert(std::abs(p3.GetX() - 1) <= EPSILON);
	assert(std::abs(p3.GetY() - 7) <= EPSILON);

	Point p4 = p2 - p1;

	assert(std::abs(p4.GetX() - (-3)) <= EPSILON);
	assert(std::abs(p4.GetY() - 1) <= EPSILON);

	float dotProduct = p1.Dot(p2);

	assert(std::abs(dotProduct - 10) <= EPSILON);

	Point p5 = p1 * 5;

	assert(std::abs(p5.GetX() - 10) <= EPSILON);
	assert(std::abs(p5.GetY() - 15) <= EPSILON);

	Point p6 = 2 * p2;

	assert(std::abs(p6.GetX() - (-2)) <= EPSILON);
	assert(std::abs(p6.GetY() - 8) <= EPSILON);

	/* ----------------------- */

	PolyLine pl1;
	pl1.AddPoint(1, 2);
	pl1.AddPoint(3, 2);
	pl1.AddPoint(5, 5);
	pl1.AddPoint(-2, 4);
	pl1.AddPoint(4, -1);
	pl1.AddPoint(6, 4);

	bool bRemoved = pl1.RemovePoint(4);

	assert(bRemoved);

	Point minP(0.f, 0.f);
	Point maxP(0.f, 0.f);

	pl1.TryGetMinBoundingRectangle(&minP, &maxP);

	assert(minP.GetX() == -2);
	assert(minP.GetY() == 2);
	assert(maxP.GetX() == 6);
	assert(maxP.GetY() == 5);

	return 0;
}
4. 커밋, 푸쉬 그리고 빌드 요청
이건 어떻게 하는 지 이제 다 아시죠? :)
```

## 설명

- 개체를 가리키는 포인터 배열을 정적으로 만들건지 동적으로 만들건지 결정해야 함
  - AddPoint할 때 들어오는 Point의 수명을 PolyLine이 관리할려면 포인터 배열을 힙에 만들어서 복사가 안되게끔 해야 한다
  - 동적 포인터 배열 만드는 법
    ```c++
    Point const** mPoints;
    mPoints = new Point const* [10];
    ```
- 메소드 오버로딩 함수들을 구현해야 함
  - \+ \- \* \[\] 등등
- 소멸자, 복사생성자, 대입연산자를 직접 구현해야 함
- float이 lhs인 경우의 \* 오버로딩 함수를 구현해야 함
  - 클래스 밖의 함수가 클래스 멤버에 접근 가능하도록 friend 키워드를 사용해야 함
