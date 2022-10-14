# 기존 map의 문제점

std::map은 이진트리를 기반으로 한 자동으로 정렬되는 컨테이너입니다.
따라서 요소 삽입과 제거시 이진트리를 유지하기 위해 O(logN)의 시간이 소요되었습니다. 

# Unordered_map

std::unordered_map은 Hashmap 기반의 [맵(Map)]()입니다. 
이진 트리기반의 std::map보다 삽입, 삭제 속도가 O(1)으로 월등히 빠릅니다.
unordered_map의 특징은 아래와 같습니다. 

## unordered_map 특징 정리 
* 키와 값의 쌍(pair)들을 저장합니다.
* 키는 중복 불가합니다.
* 자동으로 정렬되지 않는 컨테이너입니다. 
    * 요소는 해쉬 함수가 생성하는 색인(index) 기반의 버킷(bucket)들로 구성됩니다.
    * 해쉬맵(hashmap)이라고도 합니다.

## unordered_map 사용법
std::map과 동일하게 사용하면 됩니다. 

```c++
#include <iostream>
#include <string>
#include <unordered_map>

int main()
{
    std::unordered_map<std::string, int> scores;

    scores["Nana"] = 60;
    scores["Mocha"] = 70;
    socres["Coco"] = 100;

    for (auto it = scores.begin(); it != scores.end(); ++it)
    {
        std::cout << it->first << ": " << it->second << std::endl;
    }

    return 0;
}
```

# 요소 삽입
unordered_map 삽입 시 내부적으로 어떤 일이 일어나는지 살펴봅시다.
Coco라는 키에 100이라는 값을 삽입한다고 합시다.
```c++
scores["Coco"] = 100;
```
Coco는 먼저 해쉬 함수에 들어갑니다.

![image](https://user-images.githubusercontent.com/22488593/183339993-bb47d375-33e8-4458-841f-c7faa9419667.png)

해쉬 함수의 버킷은 사이즈를 6이라고 합시다.

![image](https://user-images.githubusercontent.com/22488593/183340223-aca50409-8278-4232-b6ee-2b816a8b12d1.png)

해쉬 함수는 0에서 5사이의 수를 반환해야 합니다.
그래야 6개의 버킷 중 하나에 들어갈 수 있기 때문입니다.
여기서는 다음과 같이 간단한 해시 함수를 사용합니다.
각 문자의 아스키코드 값을 더해서 이를 버킷의 수인 6으로 나눕니다.

![image](https://user-images.githubusercontent.com/22488593/183340753-df502380-a273-4221-abe4-b78287fe7135.png)

해시 함수의 반환값은 4가 나오고 이는 몇 번째 버킷에 들어갈지를 나타내는 인덱스입니다.

![image](https://user-images.githubusercontent.com/22488593/183340855-5dbc727e-60c8-4e65-b78d-59a869b1502c.png)

4번 인덱스에 100값이 성공적으로 들어갔습니다.

![image](https://user-images.githubusercontent.com/22488593/183340906-4685f86f-a169-4e3e-8a14-0a747005bf72.png)

# 요소 접근 

요소 접근도 요소 삽입과 같은 과정을 거쳐서 버킷에 있는 값을 가져옵니다. 

```c++
auto found = scores.find("Coco");
```

![image](https://user-images.githubusercontent.com/22488593/183341290-c1ce377d-931c-426c-82b8-ccbfe455e415.png)

# 해쉬 충돌

위에서 본 해시 함수의 문제점은 무엇일까요?
Coco가 아닌 ocoC를 키로 넣으면 같은 해쉬값이 나옵니다.
즉 해쉬 충돌이 일어납니다. 해쉬 충돌은 어떻게 해결할까요?
**공간 늘리기**와 **좋은 해시 함수**를 사용하기 두 가지 방법이 있습니다.

## 공간 늘리기
공간 늘리기도 두가지 방법이 있습니다.
**배열을 사용하는 것**과 **링크드 리스트**를 사용하는 방법이 있습니다.   
배열을 사용하는 방법은 각 버킷마다 배열을 넣는 방식입니다.
예를들어 각 버킷마다 배열 4개를 넣고, 요소를 삽입할 때마다 배열 사이즈를 체크해
4개 이상이면 더 이상 넣지 못하게 하면 됩니다.   
링크드 리스트를 사용하는 방법은 버킷에 요소를 삽입할 때마다 링크드 리스트의 노드를 추가해주는 방식입니다.
이 경우 요소를 해쉬 충돌 없이 무한히 삽입할 수 있지만, 요소를 찾는데 시간이 오래 걸립니다.
그림으로 보면 다음과 같습니다.   

![image](https://user-images.githubusercontent.com/22488593/183342524-2f307ce6-74fd-4f42-b4fb-62055ab51a69.png)

# 해시 함수

좋은 해시 함수를 사용하면 해시 충돌을 줄일 수 있습니다.
어떤 해시 함수를 고르면 좋을까요?
해시 함수는 암호학적 해시 함수와 비 암호학적 해시 함수로 나뉩니다.
지금과 같은 경우는 속도가 빠르고 해시 충돌이 적은 비암호학적 해시 함수를 사용하는 것이 좋겠지요.
해시 함수들의 속도와 충돌횟수를 비교한 아래의 링크를 참고해보시면 됩니다.   
[https://softwareengineering.stackexchange.com/questions/49550/which-hashing-algorithm-is-best-for-uniqueness-and-speed]   

그리고 버킷 사이즈를 소수(Prime Number)로 정하면 해시 충돌을 줄일 수 있다고 합니다.
그 이유는 자연에서도 발견되는 사실이고 수학적으로도 설명이 되어 있다고 합니다.

# std::map vs std::unordered_map
일반 map하고 unordered_map을 마지막으로 정리해보도록 하겠습니다.

## std::map
* 자동으로 정렬되는 컨테이너
* 키-값 쌍들을 저장
* 이진 탐색 트리 기반
* 탐색 시간: O(logN)
* 삽입과 제거가 빈번하면 느림

## std::unordered_map
* 자동으로 정렬되지 않는 컨테이너
* 키-값 쌍들을 저장
* 해쉬 테이블 기반
* 탐색 시간: 
    * O(1), 해쉬 충돌이 없을 경우 
    * O(n), 최악의 경우
* 버킷 때문에 메모리 사용량 증가

# unordered_set

std::set은 값(value)이 없고 키(key)만 존재하는 std::map입니다. 
set역시 map과 같이 삽입, 삭제시 느리다는 단점이 있습니다.
std::unordered_set은 해쉬 테이블 기반으로 삽입, 삭제가 O(1)으로 빠릅니다.
다음은 unordered_set을 사용하는 예시입니다. set과 같이 사용하면 됩니다.   

```c++
#include <iostream>
#include <string>
#include <unordered_map>

int main()
{
    std::unordered_set<std::string> names;

    names.insert("Victor");
    names.insert("Lulu");
    names.insert("Mocha");

    for (auto it = names.begin(); it != names.end(); ++it)
    {
        std::cout << *it  << std::endl;
    }

    return 0;
}
```

# unordered_set vs set
unordered_set과 set을 정리해보겠습니다.
# std::set
* 자동 **정렬되는** 컨테이너
* 키드를 저장
* 이진 탐색 트리
* 탐색 시간: O(logN)
* 삽입과 제거가 빈번하면 느림
# std::unordered_set
* 자동 정렬되지 않는 컨테이너
* 키들을 저장
* 해쉬 테이블
* 탐색 시간:
    * O(1), 해쉬 충돌이 없을 경우
    * O(n), 최악의 경우
* 버킷 때문에 메모리 사용량 증가
