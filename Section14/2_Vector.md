# 벡터(Vector)란?

벡터란 STL Container에 속한 자료구조 중 하나입니다.
벡터를 간단히 말하면 **크기를 자동으로 늘려주는 동적 배열**입니다.
벡터에 저장되는 모든 요소들은 연속된 메모리 공간에 위치합니다.
따라서 어떤 요소든지 순회할 필요 없이 한번에 접근이 가능합니다. (O(1))  
벡터는 [템플릿]()을 사용하기 때문에 어떤 자료형이라도 넣을 수 있습니다.
벡터는 요소 수가 증가함에 따라 자동으로 메모리를 관리해 줍니다.

### 벡터 만들기 예시

```c++
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> scores;
    scores.reserve(2);  // 두 개의 공간을 잡아주기

    scores.push_back(30); // 30 삽입
    socres.push_back(50); // 50 삽입

    scores.pop_back();  // 50 삭제

    std::cout << "Current capacity : " << scores.capacity() << std::endl;  // 용량, 담을 수 있는 갯수
    std::cout << "Current size : " << scores.size() << std::endl; // 크기, 현재 담겨 있는 갯수
}
```

# 벡터 변수 만드는 법

```
방법1
std::vector<<type>><name>;

방법2
std::vector<<type>><name>(const vector& x);

방법3
std::vector<<type>><name>(<size>);
```

- 방법1은 빈 벡터를 만들어 줍니다.
- 방법2는 x라는 벡터와 같은 크기 및 데이터를 갖는 vector를 생성합니다.  
  이는 [복사 생성자]()를 사용하는 방법입니다.
- 방법3은 크기를 지정하여 vector를 생성합니다.  
  이 때 모든 요소의 값은 0으로 초기화 됩니다. 사실 방법3은 선호되지 않는 방법이라고 합니다. 동일한 방법을 사용하면 C#의 List<T>는 size만큼 용량을 설정해줍니다. 즉, 요소들을 초기화하지 않으며 요소 10개를 포함하는 공간을 만들어줍니다. 사용자가 직접 요소를 넣는 C#의 방식이 더 좋다고 합니다. 따라서 방법3 말고 방법1과 reserve()함수로 용량을 설정해주는 방식이 좋습니다.

## 벡터 생성 코드

```c++
// 방법 1
std::vector<int> scores;  // int를 요소로 하는 빈 벡터 생성
std::vector<string> names;  // string을 요소로 하는 빈 벡터 생성
std::vector<Cat> myCats;  // 객체를 요소로 하는 빈 벡터 생성
std::vector<Cat*> myCats; // 포인터를 요소로 하는 빈 벡터 생성

// 방법 2
std::vector<int> scores;

scores.push_back(1);
scores.push_back(2);

std::vector<int> scores1(scores); // scores의 사본

// 방법 3
std::vector<int> scores(10);  // 10개의 요소가 0으로 초기화 됨

std::vector<Cat> myCats(4);
```

# 벡터 요소 삽입/삭제

## 삽입

```
push_back(<data>);
```

push_back()함수는 vector의 맨 뒤에 요소를 추가합니다.

## 삭제

```
pop_back();
```

pop_back()함수는 vector의 맨 뒤에 있는 요소를 제거합니다.

## 삽입/삭제 코드

```c++
  // push_back()
  scores.push_back(30); // 30 삽입
  names.push_back("Coco");  // "Coco" 삽입
  myCats.push_back(myNewCat); // myNewCat 개체 삽입

  // pop_back()
  scores.pop_back();  // 맨 뒤에 있는 30 반환
```

# 벡터 용량(capacity)와 크기(size)

![image](https://user-images.githubusercontent.com/22488593/180711219-35c43068-1d43-42dc-89db-1049d6a9754f.png)

## 벡터의 용량

```
  capacity();
```

capacity()함수는 벡터에 할당된 요소 공간 수를 반환합니다.

## 벡터의 크기

```
  size();
```

size()함수는 실제로 들어 있는 요소 수를 반환합니다.

# 벡터의 용량 늘리기

벡터의 용량을 늘리려면 어떻게 해야 할까요?

## reserve()

```
  reserve(<size>);
```

reserve() 함수는 벡터의 용량을 늘려줍니다.
용량이 증가시키려면 새로운 저장 공간을 재할당하고 기존의 요소들을 모두 새 공간으로 복사해야 합니다.  
불필요한 재할당을 막기 위해 벡터를 생성한 직후에 이 함수를 호출하는게 좋습니다.

## 벡터 용량/크기 확인 코드

```c++
// 벡터 생성, 용량 설정
std::vector<int> scores;
scores.reserve(10);

// 생성 직후 용량과 크기
scores.capacity();  // 10반환
scores.size();  // 0 반환

// 요소 삽입 후 크기
scores.push_back(10);
scores.size();  // 1 반환
```
