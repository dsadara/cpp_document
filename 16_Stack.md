# 스택(Stack)이란?

스택은 후입 선출(Last-in First-out) 자료구조입니다.
마지막에 넣은 요소가 제일 처음 나온다는 뜻입니다.

![image](https://user-images.githubusercontent.com/22488593/182005277-54caeb6a-bb6f-4d7b-9b77-af1584d09e73.png)

# 스택 만들기

```c++
stack<<type>><name>
```

스택을 생성합니다. [템플릿]()을 사용하기 때문에 어떤 자료형이든 넣을 수 있습니다.

```c++
std::stack<int> studentIDStack;

std::stack<std::string> studentNameStack;

std::stack<StudentInfo> studentInfoStack;
```

# 요소 삽입하기

```c++
void push(const value_type& val);
```

push()는 스택의 맨 뒤에 새 요소를 삽입합니다.

```c++
studentIDStack.push(1234);

studentNameStack.push("Coco");

studentInfoStack.push(studentInfo("Coco", 1234));
```

# 요소 제거하기 

```c++
void pop();
```

pop()은 스택 마지막에 저장된 요소를 제거합니다.
삭제 후 반환값이 없습니다. top()으로 반환값을 얻을 수 있습니다.

```c++
std::stack<std::string> studentNameStack;
// "Coco"와 "Mocha"를 넣는다

studentNameStack.pop(); // studentNameStack: "Coco"
```

# top()

![image](https://user-images.githubusercontent.com/22488593/182006031-a51e28fc-0736-446a-b3f3-68c35b35f713.png)

```c++
value_type& top();
```

top()함수는 스택에 가장 마지막에 저장된 요소를 참조로 반환합니다.
스택은 큐와 같이 양쪽의 요소를 꺼낼수 없습니다.
즉 큐와 같이 front(), rear() 함수 두개가 존재하지 않는다는 뜻입니다. 

```c++
// "Coco"와 "Mocha"를 넣는다

std::string top = studentNameStack.top();   // "Mocha"

```

# size(), empty()

```c++
size_type size();
```

size()는 스택에 들어 있는 요소의 수를 반환합니다. 

```c++
bool empty()
```

empty()는 스택이 비어 있으면 true를, 아니면 false를 반환합니다.

```c++
int size = studentNameStack.size();

bool bIsEmpty = studentNameStack.empty();
```

# 예시: 스택 만들기

```c++
#include <stack>

int main()
{
    std::stack<std::string> studentNameStack;
    studentNameStack.push("Coco");
    studentNameStack.push("Mocha");

    while (!studentNameStack.empty())
    {
        std::cout << studentNameStack.top() << std::endl;
        studentNameStack.pop();
    }

    return 0;
}
```