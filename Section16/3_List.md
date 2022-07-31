# 리스트(List)란?

![image](https://user-images.githubusercontent.com/22488593/182006250-5aef8530-61cb-46fb-8a4e-d9dbad242f34.png)

STL Container에서 리스트란 양방향 연결 리스트(Doubly Linked List)를 말합니다.
벡터와 달리 요소들이 각각 다른 메모리에 위치합니다.
각 요소는 다음요소, 이전요소를 가리킵니다. 
첫번째 요소를 가리키는 머리(front)와 마지막 요소를 가리키는 꼬리(back)가 존재합니다.
따라서 리스트는 양쪽 끝에서 삽입/제거가 가능합니다.   
여담이지만 요즘 CPU 아키텍쳐는 메모리 캐쉬가 많이 들어가 있어 메모리가 붙어 있는 벡터를 쓰는 것이 빠르다고 합니다.
연결 리스트말고 벡터를 쓰도록 합시다. 

# 요소 삽입하기

```c++
iterator insert(iterator position, const value_type& value);
```

insert()는 postion 반복자가 가리키는 위치에 새 요소를 삽입합니다.

```c++
void push_front(const value_type& value);
```

push_front()는 제일 처음에 새 요소를 삽입합니다. 

```c++
void push_back(const value_type& value);
```

push_back()은 제일 마지막에 새 요소를 삽입합니다.

```c++
std::list<int> scores;
std::list<int>::iterator it = scores.begin();

scores.insert(it, 99);      // 99
scores.push_front(10);      // 10, 99
scores.push_back(50);       // 10, 99, 50

```

# 처음 또는 마지막 요소 제거하기

```c++
void pop_front();
```

pop_front()는 첫 번째 요소를 제거합니다.

```c++
void pop_back();
```

pop_back()은 마지막 요소를 제거합니다. 


```c++
std::list<int> scores;  // 10, 99, 50

scores.pop_front();     // 99, 50
scores.pop_back();      // 99
```

# 특정 요소 제거하기

```c++
iterator erase(iterator position);
```

erase()는 반복자 postion이 가리키는 위치의 요소들을 제거합니다. 

```c++
void remove(const value_type& value);
```

remove()는 value와 값이 같은 요소를 전부 제거합니다.

```c++
std::list<int> scores = {20, 30, 40, 30, 25, 30, 70, 96};
std::list<int>::iterator it = scores.begin();

scores.erase(it);   // 30, 40, 30, 25, 30, 70, 96
scores.remove(30);  // 40, 25, 70, 96
```

# 리스트의 다른 메서드들

그밖에 리스트의 다른 메서드들은
* 정렬하기
* 두 리스트 합치기
* 한 리스트에서 빼 내어 다른 리스트에 넣기
* 중복인 요소들 제거하기
가 있다.

# 리스트의 장점과 단점

리스트는 삽입과 제거시 메모리 복사가 일어나지 않고 O(1)으로 속도가 빠르다.
또 큐와 스택과 달리 요소들을 꺼내지 않고 삽입/제거가 가능하다.
단점은 탐색이 O(N)으로 느린 편이다.
메모리가 불연속적을 되어 있어 요소에 한번에 접근이 불가능하기 때문이다. 

# 그 밖의 컨테이너들

지금까지 Vector, Map, Queue, Stack, List를 살펴 보았습니다.
그 밖에 컨테이너는 무엇이 있는지 간단히 알아봅시다.

* 멀티셋(multi-set)   
기존 셋과 달리 멀티 셋은 중복 키를 허용합니다.
키 자체가 요소이기 때문에 요소를 수정하면 안됩니다.
* 멀티맵(multi-map)   
멀티 맵은 중복 키를 허용합니다.
* 덱(deque)   
Double-ended queue의 약자로 양쪽 끝에서 요소 삽입과 삭제가 모두 가능합니다.
* 우선순위 큐(priority-queue)   
우선순위 큐는 요소를 넣을 때 우선순위를 같이 넣습니다.
요소를 넣으면 우선순위에 따라 자동으로 정렬됩니다. 