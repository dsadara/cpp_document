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
불필요한 재할당을 막기 위해 벡터를 생성한 직후에 reserve()를 호출하는게 좋습니다.

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

# 요소 하나에 접근하기

```
operator[](size_t n);
```

벡터는 배열과 같이 첨자 연산자 (\[\])로 지정된 위치(n)의 요소 하나에 접근할 수 있습니다.
벡터의 첨자 연산자는 요소를 참조로 반환합니다.
주의 할 점은 벡터에서만 첨자 연산자를 쓸 수 있다는 것입니다.
벡터는 데이터가 한 공간에 순차적으로 들어가 있어서 가능한 것입니다.
데이터가 순차적으로 들어가 있지 않은 맵(map) 이나 딕셔너리(dictionary)는 첨자 연산자로 요소에 바로 접근할 수 없습니다.
그래서 맵에서 어떤 요소에 접근하려면 처음부터 순회하면서 요소를 찾아야 합니다.
STL 컨테이너를 순회할 때는 반복자(iterator)를 이용하는게 표준 방식입니다.

## 첨자 연산자 예시

```c++
scores[i] = 3;

std::cout << names[i] << " ";

std::cout << myCats[i].GetScore() << "";
```

### 첨자 연산자로 벡터의 요소 출력하기 예시

```c++
#include <vector>

int main()
{
    std::vector<int> scores;
    scores.reserve(2);

    scores.push_back(30);
    scores.push_back(50);

  for (size_t i = 0; i < scores.size(); ++i)
  {
      std::cout << scores[i] << " ";
  }
}
```

# 반복자(iterator)

반복자는 vector 뿐만 아니라 STL 컨테이너의 모든 요소들을 순회하는데 사용됩니다.
반복자는 어떤 요소를 가리키고 있다는 점에서 포인터와 비슷합니다. \* 연산자와 \-\> 연산자가 반복자 클래스에 오버라이딩 되어 있어서 포인터 같이 사용할 수 있습니다.

```
vector<<type>>::iterator<name>
```

## 반복자 - begin() / end()

```c++
vector<int>::iterator bIter = scores.begin();
```

begin()은 벡터의 첫 번째 요소를 가리키는 반복자를 반환합니다.

```c++
vector<int>::iterator bIter = scores.begin();
```

end()는 벡터의 마지막 요소 버로 뒤의 요소를 가리키는 반복자를 반환합니다.
C 스타일 문자열에서 마지막 요소 다음에 널 포인터가 있는 것과 비슷한 개념입니다.

![image](https://user-images.githubusercontent.com/22488593/181665346-0e1ff3b3-a4b7-4206-838f-2cbbea983d50.png)

## 반복자로 벡터 요소 출력하기 예시

```c++
#include <iostream>
#include <vector>

int main()
{
    std::vector<int> scores;
    scores.reserve(2);

    scores.push_back(30);
    scores.push_back(50);

    for (std::vector<int>::iterator iter = scores.begin(); iter != scores.end(); ++iter)
    {
        std::cout << *iter << " ";
    }
}
```

# 역방향 반복자(reverse iterator)

end()에서부터 begin()까지 순회하고자 하면 마지막 요소 뒤를 가리키므로 순회를 할 수 없습니다.
이럴 때는 역방향 반복자를 사용하면 됩니다.
역방향 반복자는 rbegin()과 rend()가 있습니다.

![image](https://user-images.githubusercontent.com/22488593/181666792-9f2105b7-2610-409f-9efc-641adbb813a9.png)

## rbegin(), rend()

```
reverse_iterator rbegin();
```

rbegin()은 벡터의 마지막 요소를 가리키는 역방향 반복자를 반환합니다.

```
reverse_iterator rend();
```

rend()는 벡터의 첫 번째 요소 앞의 요소를 가리키는 역방향 반복자를 반환합니다.

### rbegin(), rend() 사용 예시

```c++
std::vector<int>::reverse_iterator reversedBeginIt = scores.rbegin();
std::vector<int>::reverse_iterator reversedEndIt = scores.rend();
```

# 특정 위치에 요소 삽입하기

insert()를 사용하면 특정 위치에 요소를 삽입 할 수 있습니다.

```c++
std::vector<int> scores;

scores.reserve(4);
scores.push_back(10);   // 10
scores.push_back(50);   // 10, 50
scores.push_back(38);   // 10, 50, 38
scores.push_back(100);  // 10, 50, 38, 100

std::vector<int>::iterator it = scores.begin();

it = scores.insert(it, 80);   // 80, 10, 50, 38, 100
```

첫번째 매개변수에 위치에 해당하는 반복자를 넣고, 두번째 매개변수에는 삽입할 요소를 넣습니다.
위 예시의 경우 첫번째 부분에 80이 대입됩니다.
반환값은 대입한 부분이 반환됩니다.
위 경우 80을 가리키는 반복자가 반환되겠네요.

## Insert()의 복사/재할당 문제

위 삽입 예제에서는 메모리상 어떤 일이 일어날까요?
우선 10, 50, 38을 대입하면 다음과 같습니다.

![image](https://user-images.githubusercontent.com/22488593/181672325-6f5771dc-adb2-44f3-ba62-b18c2cb00125.png)

스택에는 아래부터 각각 힙 메모리 주소, 용량, 크기가 들어가 있습니다.
힙에는 4개의 공간이 있고 10, 50, 38이 대입되어 있습니다.
여기서 scores.insert(it, 80); 을 하면 어떤 일이 일어날까요?

![image](https://user-images.githubusercontent.com/22488593/181672706-ef81361c-7af7-4f74-98ec-6c598ab0d8b3.png)

스택에 크기를 나타내는 변수가 4로 바뀝니다.
10, 50, 38이 한 칸씩 뒤로 복사가 되고 첫 번째 요소에 80이 삽입이 됩니다.
만약에 데이터가 3개가 아니라 5MB 정도를 차지한다고 하면 복사하는데 시간이 많이 걸릴 수 있습니다.

또 다른 문제인 재할당 문제를 살펴봅시다.
이번엔 10, 50, 38, 100을 먼저 대입하고 80을 첫번째 요소에 삽입한다고 합시다.

![image](https://user-images.githubusercontent.com/22488593/181673960-57ac03a5-1e56-4a3e-aee5-22504bcdc712.png)

80을 대입하려고 하면 용량을 초과하므로 새로운 메모리를 힙에 재할당해줍니다.
80을 대입하면 아래와 같습니다.

![image](https://user-images.githubusercontent.com/22488593/181674124-11853b11-e2f6-42e6-b7ab-26dbcea7c455.png)

다른 메모리로 재할당도 해야하고 요소들도 복사해야하고 시간이 오래 걸립니다.
그래서 이런 경우를 피하기 위해 벡터 생성시 reserve()로 용량을 넉넉히 잡아두는 것이 좋습니다.

# 특정 위치에 있는 요소 삭제하기

erase(iterator)는 반복자 위치에 있는 것을 지우고 그 위치를 반환합니다.

```c++
std::vector<int> scores;
scores.reserve(4);
scores.push_back(10); // 10
scores_push_back(50); // 10, 50, 38
scores_push_back(38); // 10, 50, 38, 100
scores_push_back(100);

std::vector<int>::iterator it;
it = scores.begin();

it = scores.erase(it);    // 50, 38, 100
```

위 코드에서는 erase()를 실행 후 복사는 일어났지만 재할당을 일어나지 않았습니다.

![image](https://user-images.githubusercontent.com/22488593/181674836-b9513f15-6dd5-4164-ae4e-34b746c32e41.png)

여기에서 첫번째 요소를 삭제하면 아래와 같이 됩니다.

![image](https://user-images.githubusercontent.com/22488593/181674884-5ceeb3f2-5f8c-409a-a4c5-5a7046b3d56a.png)

요소들을 한 칸씩 앞으로 복사하므로 역시 시간이 오래 걸립니다.
배열의 순서가 중요하지 않다면 삭제 시 복사하지 않는 방법이 있습니다.
10을 지우고 제일 끝에 원소인 100을 갖다 넣으면 됩니다.
복사 시간을 최소화하기 위해 EraseUnordered()라는 이름으로 이러한 함수를 만들어 줄 수 있습니다.
