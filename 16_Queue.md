# 큐(Queue)란?

큐는 선입 선출(First-in first-out) 자료구조입니다.
즉 먼저 들어간 요소가 먼저 나옵니다.

![image](https://user-images.githubusercontent.com/22488593/182004528-20932bea-a0cf-4c3c-b633-11176cd512a1.png)

# 큐 만들기

```
queue<<type>><name>
```

큐를 생성합니다. [템플릿]()을 사용하기 때문에 \<type\>에 어떤 자료형이든 넣을 수 있습니다.


```c++
std::queue<int> studentIDQueue;
std::queue<std::string> studentNameQueue;

std::queue<StudentInfo> studentInfoQueue;
```

# 요소 삽입하기 

```c++
void push(const value_type& val);
```

push()는 큐의 맨 뒤에 새 요소를 삽입합니다.

```c++
studentIDQueue.push(1234);  //  정수 삽입

studentNameQueue.push("Lulu");  // 문자열 삽입

studentInfoQueue.push(StudentInfo("Coco", 1234));   // 객체 삽입
```

# 요소 제거하기 

```c++
void pop();
```

pop()은 가장 먼저 삽입되었던 요소를 제거합니다.
여기서 주의할 점은 pop()시 삭제하는 요소를 반환하지 않는다는 점입니다.
요소를 반환시키고 싶으면 front()함수를 사용하면 됩니다. 

# front(), back()

![image](https://user-images.githubusercontent.com/22488593/182004676-1a693a29-5d2a-4d07-b22a-3ef938c0fa91.png)

```c++
value_type& front();
```

front()는 가장 먼저 삽입되었던 요소를 참조로 반환합니다.

```c++
value_type& back();
```

back()은 가장 마지막에 삽입되었던 요소를 참조로 반환합니다.

```c++
// "Coco"와 "Mocha"를 넣는다

std::string front = studentNameQueue.front();   // "Coco"
std::string back = studentNameQueue.back(); // "Mocha"
```

# size(), empty()

```c++
size_type size();
```

size()는 큐에 들어 있는 요소의 수를 반환합니다.

```c++
bool empty();
```

큐가 비어 있으면 true를 그렇지 않으면 false를 반환합니다.

```c++
int size = studentNameQueue.size();

bool bEmpty = studentNameQueue.empty();
```

# 예시: 큐 만들기

```c++
#include <queue>

int main()
{
    std::queue<std::string> studentNameQueue;
    studentNameQueue.push("Coco");
    studentNameQueue.push("Mocha");

    while (!studentNameQueue.empty())
    {
        std::cout << "Waiting student " << studentNameQueue.front() << std::endl;
        studentNameQueue.pop();
    }

    return 0;
}
```