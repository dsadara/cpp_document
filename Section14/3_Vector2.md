# 두 벡터 교환하기

```
swap(vector& other);
```

swap()함수로 같은 자료형의 두 벡터를 맞바꿀 수 있습니다.
메모리 주소값만 맞바꾸면 되므로 재할당과 복사가 일어나지 않아 빠릅니다.

```c++
vector<int> scores;     // 10, 20, 30, 100
vector<int> anotherScores;  // 40, 50, 60

scores.swap(anotherScores); // scores: 40, 50, 60
                            // anotherScores: 10, 20, 30, 100
```

# 요소 대입하기

```
assign(size_t n, <data>);
```

assign()함수는 n 개의 \<data\> 값을 벡터에 넣습니다.

```c++
vector<int> anotherScores;
anotherScores.assign(7, 100);   // 100, 100, 100, 100, 100, 100, 100
```

# 벡터의 크기 변경하기

```
resize(size_t n);
```

resize()는 벡터의 크기를 바꿉니다.
새 크기가 벡터의 기존 크기보다 작으면 초과분이 제거되고,
새 크기가 벡터의 용량보다 크면 재할당이 일어나고 0으로 초기화 해줍니다.  
resize()와 reserve()의 차이는 reserve는 용량을 정해주는 것이고,
resize는 크기를 정해주는 것이라고 생각하면 됩니다. (용량이 작을 경우 resize가 용량을 늘려주기도 합니다)
그리고 resize는 0으로 대입해주는 반면 reserve는 용량만큼의 메모리만 확보해줍니다.

## 벡터 축소하기 예제

```c++
std::vector<int> scores;

scores.reserve(3);

scores.push_back(30);   // 30
scores.push_back(100);  // 30, 100
scores.push_back(100);  // 30, 100, 70

scores.resize(2);   // 30, 100

for (int i = 0; i < scores.size(); ++i)
{
    std::cout << scores[i] << " ";  // "30 100 "
}
```

새 크기가 기존 크기보다 작아서 마지막 요소 70이 날아갔습니다.

# 벡터에서 모든 요소 제거하기

```
clear();
```

clear()는 벡터 요소들을 지워주고 크기를 0으로 설정해줍니다.
용량은 변하지 않습니다.

```c++
scores.clear();
```

# 객체(object) 벡터

벡터에는 객체(object) 요소도 대입할 수 있습니다.

```c++
// Score.h
class Score
{
public:
    // ...
private:
    int mScore;
    string mClassName;
}

// Main.cpp
int main()
{
    vector<Score> scores;
    scores.reserve(4);

    scores.push_back(Score(30, "C++"));
    scores.push_back(Score(59, "Algorithm"));
    scores.push_back(Score(87, "Java"));
    scores.push_back(Score(41, "Android"));
    // ...
}
```

객체를 scores에 push_back()을 하면 객체의 복사가 일어납니다.
따라서 scores에는 객체의 포인터나 참조가 아닌 실제 객체가 들어가 있습니다.
vector 생성 시 템플릿에 Score 포인터를 넣으면 객체의 복사가 일어나지 않게 할 수 있습니다.
객체의 크기가 1MB 라고 크다고 하면 이때는 객체를 복사하는데 부담이 큽니다.
또 객체를 대입하다가 용량이 꽉찬 경우 객체 전체를 복사해야 하는 일이 생깁니다.
이때 포인터 벡터를 사용하면 좋습니다.  
주의할 점은 메모리 누수가 이러나지 않도록 원본 객체의 수명을 잘 관리해주어야 합니다.

# 포인터 벡터

객체 벡터에서 객체 포인터 벡터로 바꾸어 봅시다.

```c++
int main()
{
    vector<Score*> scores;

    scores.reserve(2);

    scores.push_back(new Score(30, "C++"));
    scores.push_back(new Score(87, "Java"));
    scores.push_back(new Score(41, "Android"));

    scores.clear();

    return 0;
}
```

이번에는 객체의 포인터를 대입해야하므로 객체를 new로 생성해주었습니다.
scores.clear()를 해도 힙에 저장된 객체들은 지워지지 않습니다.
그래서 이 경우 모든 요소에 대해 delete를 호출해 직접 객체들을 해제해야 합니다.  
이런식으로 하면 장점이 대입 시 재할당 시 복사할 양이 적어지는 장점이 있습니다.
객체의 크기가 클 때는 포인터 벡터를 사용하는게 좋습니다.

## 포인터 벡터 삭제하기

```c++
int main()
{
    vector<Score*> scores;

    scores.reserve(2);

    scores.push_back(new Score(30, "C++"));
    scores.push_back(new Score(87, "Java"));
    scores.push_back(new Score(41, "Android"));

    for (vector<Score*>::iterator iter = scores.begin()l iter != scores.end(); ++iter)
    {
        delete *iter;
    }

    scores.clear();

    return 0;
}
```

여기까지 벡터의 내용은 끝났고 벡터의 장점과 단점에 대해 알아보겠습니다.

# 벡터의 장점

벡터의 장점은 순서에 상관 없이 요소에 임의적으로 접근이 가능합니다.
앞에서부터 요소를 찾을 필요 없이 색인으로 바로 요소에 접근이 가능하다는 뜻입니다.
그리고 벡터는 빠르게 삽입 및 삭제를 할 수 있습니다.
요소의 복사 없이 벡터의 제일 마지막 위치에 삽입 삭제가 가능하기 때문입니다.
마치 스택과 같이 말이죠.

# 벡터의 단점

벡터에 중간에 요소를 삽입할 때나 삭제할 때는 복사가 일어나기 때문에 느립니다.
그리고 용량이 꽉 차면 재할당 및 요소 복사에 드는 비용이 많습니다.
여기서 비용이란 메모리와 시간비용 둘 다를 말합니다.
그래서 이 비용을 줄이기 위해 reserve()로 필요한 만큼 사이즈를 잡아주는게 좋습니다.
