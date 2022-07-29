# 맵(map)이란?

맵은 STL Container에 속해있는 자료구조중에 하나입니다.
키(key)와 값(value)의 쌍들을 요소로 가지고 있습니다.
여기서 키는 중복될 수 없습니다.
Java의 맵은 해시 맵이지만 C++의 맵은 이진 탐색 트리 기반입니다.
해시맵의 요소 접근은 O(1)에 가능하지만 C++의 맵은 O(logN)으로 느린 편입니다.
C++의 맵은 키 기준으로 자동으로 정렬된다는 장점이 있습니다.
자동으로 정렬되는 이유는 이진 탐색 트리를 중위 순회하면 정렬된 순서가 나오기 때문입니다.
이진 탐색 트리 구조를 유지하기 위해 삽입 시에 O(logN)이 걸린다는 단점도 있습니다.

# 맵 만들기

```
map<<key_type>, <value_type>><name>
```

이렇게 하면 빈 map을 생성합니다

```
map<<key_type>, <value_type>><name>(const map& x)
```

이렇게 하면 x라는 map과 같은 크기(size)와 데이터를 갖는 map을 생성합니다.
복사 생성자를 이용한 것입니다.

```c++
std::map<std::string, int> simpleScoreMap;
std::map<StudentInfo, int> complexScoreMap;

std::map<std::string, int> copiedSimpleScoreMap(simpleScoreMap);
```

# std::pair<key, value>

두 데이터를 한 단위로 저장하는 구조입니다.
맵에 요소를 삽입할 때 키와 값을 한번에 넣기위해 사용됩니다.
정확히는 std::map::insert() 함수의 매개변수로 쓰입니다.
첫 번째 데이터를 가져올때는 first를 사용, 두번째는 second을 사용하면 됩니다.
first와 second는 각각 std::map클래스의 멤버객체입니다.
첫번째 데이터는 first에 두번째 데이터는 second에 저장되어 있는 것이죠.

```c++
std::par<std::string, int> student1("Coco", 10);

student1.first; // "Coco" 가져오기
student1.second; // 10 가져오기

simpleScoreMap.insert(std::pair<std::string, int>("Mocha", 100));
```

# insert()

```
std::pair<<iterator>, bool> insert(const value_type& value)
```

insert()함수는 map에 새 요소를 삽입해줍니다.
삽입된 위치를 나타내는 반복자와, 삽입 결과를 나타내는 bool을 같이 반환합니다.
맵의 특성상 같은 키를 중복으로 삽입할 수 없습니다.

```c++
// <iterator, true>를 반환
simpleScoreMap.insert(std::pair<std::string, int>("Mocha", 100));
// <iterator, false>를 반환
simpleScoreMap.insert(std::pair<std::string, int>("Mocha", 0));
```

여담이지만 이렇게 반환값과 성공여부 두가지를 반환하는 테크닉은 예외처리에서도 사용이 됩니다.
함수의 반환값과 에러코드를 반환해서 호출자가 에러를 처리하는 방식으로 말이죠.

# operator[]

```
mapped_type& operator[](const key& key);
```

map도 vector와 같이 첨자 연산자로 요소에 접근할 수 있습니다.
map의 경우 키에 대응하는 값을 참조로 반환합니다.
맵에 키가 없으면 새 요소를 삽입해주고, 키가 이미 있으면 그 값을 새로운 값으로 덮어씁니다.

```c++
std::map<std::string, int> simpleScoreMap;

simpleScoreMap["Coco"] = 10;    // 새 요소를 삽입한다
simpleScoreMap["Coco"] = 50;    // "Coco"의 값을 덮어쓴다.
```

## 맵을 정렬된 순서로 출력하는 코드

```c++
simpleScoreMap.insert(std::pair<std::string, int>("Mocha", 100));
simpleScoreMap.insert(std::pair<std::string, int>("coco", 50));

for (std::map<std::string, int>::iterator it = simpleScoreMap.begin(); it != simpleScoreMap.end(); ++it)
{
    std::cout << "(" << it->first << ", " << it->second << ")" << std::endl;
}
```

키가 문자열이므로 알파벳 순서대로 출력이 됩니다.

# find()

``` c++
iterator find(const key& key);
```

find() 함수를 이용해서 키에 매칭되는 요소를 찾을 수 있습니다.
map에서 key를 찾으면 그에 대응하는 값을 가리키는 반복자를 반환합니다. 
찾지 못하면 end()를 반환합니다.
반복자가 nullptr일 수 없으므로 end()를 대신 반환합니다. 
첨자 연산자(\[\])를 find() 대신해서 사용할 수 있을것 같지만, key가 없으면 새로 삽입하기 때문에 key가 없는 요소도 반환해 버리는 문제가 있습니다.


```c++
std::map<std::string, int>::iterator it = simpleScoreMap.find("Coco");  // "Coco"에 해당하는 요소를 반환합니다
```

# swap(), clear()

```c++
void swap(map& other);
```

swap() 함수는 두 map의 키와 값을 서로 맞바꿉니다.
벡터의 swap()과 마찬가지로 메모리 포인터만 교환하면 되니까 비용이 많이 드는 연산은 아닙니다.

```c++
void clear();
```

clear() 함수는 map을 비워줍니다.

```c++
std::map<std::string, int> scoreMap;        // ("Mocha", 10), ("Coco", 40)
std::map<std::string, int> anotherScoreMap;     // ("Nana", 100)

scoreMap.swap(anotherScoreMap); // scoreMap: ("Nana", 100)
                                // anotherScoreMap: ("Mocha", 10), ("Coco", 40)
anotherScoreMap.clear();        // anotherScoreMap: empty
```

# erase()

erase()는 map의 요소들을 제거해줍니다.
반복자나 키를 넣어서 하나의 요소만 삭제할 수 있고,
반복자 두개를 넣어서 둘 사이의 요소들을 모두 삭제할 수 있습니다.

```c++
std::map<std::string, int>::iterator foundIt = simpleScoreMap.find("Coco");
simpleScoreMap.erase(foundIt);

simpleScoreMap.erase("Coco");
```

# 객체를 키로 사용할 때

StudentInfo라는 객체가 있다고 하고 map의 키로 사용한다고 합시다.

```c++
// StudentInfo.h
class StudentInfo
{
public:
    // 생성자들
private:
    std::string mName;
    std::string mStudentID;
};
```

map은 항상 정렬되어 있는데 어떤 기준으로 정렬할지 모호하다는 문제가 있습니다.
mName으로 정렬할 수도 있고 mStudentID로 정렬할 수 도 있습니다.

정렬기준을 어떤 식으로 만들어 주면 될까요?
첫번째로는 두 개체를 비교하는 연산자 \<의 오버라이딩 함수를 만들어 줄 수 있습니다.

```c++
// StudentInfo.cpp
bool StudentInfo::operator<(const StudentInfo& other) const
{
    if (mName == other.mName)
    {
        return mStudentID < other.mStudentID;
    }

    return mName < other.mName;
}
```

또 다른 방법은 map을 만들 때 비교함수(comparer)을 넣을 수 있습니다.

```c++
struct StudentInfoComparer
{
    bool operator()(const Student& left, const StudentInfo& right) const;
    {
        return (left.getName() < right.getName());
    }
};

// Main.cpp
std::map<StudentInfo, int, StudentInfoComparer> Scores;
```

두번째 방법은 남이 만든 클래스인데 수정 권한이 없을 경우 사용하면 된다.
그 외에는 첫번째 방법을 사용하는게 좋다. 

# 맵의 장점과 단점

맵의 장점은 std::list나 std::vector보다 탐색 속도가 빠르다는 점입니다.
그리고 맵은 자동 정렬된다는 장점을 있지만 해시맵처럼 O(1)이 아닙니다.
이러한 문제 때문에 C++11에는 정렬이 안되는 맵인 std::unordered_map이 존재합니다.

# 셋(set)이란?

set은 map과 거의 같지만 맵과 달리 키(key)만 존재하고 값(value)는 존재하지 않습니다.
맵과 같이 정렬되는 컨테이너이고 중복되지 않은 키를 요소로 저장합니다.
역시 내부도 이진 탐색 트리 기반으로 되어 있어 탐색 시 O(logN)입니다.

```c++
#include <set>

int main()
{
    std::set<int> scores;
    scores.insert(20);
    scores.insert(100);

    for (std::set<int>::iterator it = scores.begin(); it != scores.end(); ++it)
    {
        std::cout << *it << std::endl;  // 20\n 100\n 출력
    }
    
    return 0;
}
```