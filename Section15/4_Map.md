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
대입 연산자를 사용할 경우 맵에 키가 없으면 새 요소를 삽입해주고,
키가 이미 있으면 그 값을 새로운 값으로 덮어씁니다.

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

키가 문자열이므로 알파벳 순서대로 출력이 된다.
