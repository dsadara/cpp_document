# 기존에 사용해왔던 반복문

STL의 요소들을 한번씩 훑어야 할 때 일반 for문과 반복자 for문을 사용해야 했습니다.

```c++
// 일반 for문
int scores[3] = {10, 20, 30};

for (int i = 0; i < 3; ++i)
{
    std::cout << scores[i] << " "<< std::endl;
}
```

```c++
// 반복자를 사용한 for문
std::map<std::string, int> scoreMap;

scoreMap["Lulu"] = 10;
scoreMap["Coco"] = 50;
scoreMap["Mocha"] = 100;

for (auto it = scoreMap.begin(); it != scoreMap.end(); ++it)
{
    std::cout << it->first << ": " << it->second << std::endl;
}
```

범위 기반 반복문을 사용하면 for문이 더 간결해집니다.
범위 기반 반복문은 요소를 값으로 또는 참조로 읽어올 수 있습니다.   

```c++
int scores[3] = {10, 20, 30}; 

for (int score : scores)     // 요소를 복사해서 읽어오기
{
    std::cout << score << " "<< std::endl;
}
```

```c++
std::map<std::string, int> scoreMap;

scoreMap["Lulu"] = 10;
scoreMap["Coco"] = 50;
scoreMap["Mocha"] = 100;

for (auto& score : scoreMap)    // 요소를 참조로 읽어오기
{
    std::cout << score.first << ": " << score.second << std::endl;
}
```

범위 기반 for문을 정리하겠습니다.   

# 범위 기반 for 반복

범위 기반 for문의 형태는 아래와 같습니다.
```c++
for (<range_declaration> : <range_expression>)
{
    // 이하 코드 생략
}
```
범위 기반 for문은 값, 참조, const 참조 3가지 형태로 작성할 수 있습니다. 

## 값으로 그리고 참조로
```c++
// {["Lulu", 10], ["Chris", 50], ["Monica", 100]}
std::map<std::string, int> scoreMap;

for (auto score : scoreMap) // 값으로 복사해서 
{
    score.second -= 10;
    std::cout << score.first << ": " << score.second << std::endl;
}

for (auto& score : scoreMap) // 참조로 
{
    std::cout << score.first << ": " << score.second << std::endl;
}
```
첫번째 반복문에서 복사된 score의 second에 10을 감소시켜 원본이 수정되지 않았습니다.
두번째 참조를 사용한 반복문에서 원본 그대로의 값이 출력됩니다.
따라서 출력값은 다음과 같습니다.

![image](https://user-images.githubusercontent.com/22488593/183387805-5e4aa276-ab66-4d91-ba43-7cb9b28eccf4.png)

## 참조로 그리고 const로

```c++
// {["Lulu", 10], ["Chris", 50], ["Monica", 100]}
std::map<std::string, int> scoreMap;

for (auto& score : scores) // 값으로 복사해서 
{
    score.second -= 10;
    std::cout << score.first << ": " << score.second << std::endl;
}

for (const auto& score : scores) // 참조로 
{
    std::cout << score.first << ": " << score.second << std::endl;
}
```

원본을 수정할 일이 없으면 두번째 반복문과 같이 const를 붙이는 것이 좋습니다.   

# 범위 기반 for 반복문 특징
범위 기반 for문의 특징은 다음과 같습니다.   
* for 반복문을 더 간단하게 쓸 수 있습니다.
* 이전에 for 반복보다 가독성이 더 높습니다.
* STL 컨테이너와 C 스타일 배열 모두에서 작동합니다.
* auto 키워드를 범위 기반 for반복에 쓸 수 있습니다. 
* 단점은 역순회를 할 수 없다는 점입니다. 
    * 하지만 for문을 쓸때는 거의 대부분이 모든 요소를 순회하는 케이스가 많습니다. 그래서 범위 기반 for문이 유용합니다.

# 번외 - for_each()문
C++03에 나온 for_each()문은 컨테이너의 각 요소마다 함수를 실행하는 알고리즘입니다.
매개변수로 람다 식(Lambda Expression)을 사용하기 때문에 가독성이 높지는 않습니다.
다음은 for_each()를 사용하는 예시입니다. 

```c++
#include <algorithm>
#include <iostream>
#include <string>
#include <unordered_map>

int main()
{
    std::unordered_map<std::string, int> scores;

    scores["Lulu"] = 70;
    scores["Chris"] = 50;
    scores["Monica"] = 100;

    auto printElement = [](std::unordered_map<std::string, int>::value_type& item)
    {
        std::cout << item.first << ": " << item.second << std::endl;
    };

    for_each(scores.begin(), scores.end(), printElement);

    return 0;
}
```

위와같이 for_each()문은 가독성이 좋지 못하므로 범위 기반 for문을 사용하는 것이 좋습니다. 