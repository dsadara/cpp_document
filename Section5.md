# Section5: 문자열(string)
> 중간고사 대비 주제
> 1. std::string 클래스
## 1. std::string 클래스
### 1.1. c style 문자열
* c에서는 char 배열에 문자열을 저장했었음
```c++
char line[256];
cin.getline(1ine, 256);
```
* 위 코드는 아래 두 경우에 작동하지 않음 
    1. 아무것도 읽지 못했을 때
    2. 한 줄에 문자가 256자 이상일 때(즉, 버퍼가 충분히 크지 않을 때)
### 1.2. std::string 클래스
* std::string 클래스를 이용한 문자열은 길이가 증가할 수 있음
```c++
#include <string>
std::string firstName;
std::cin >> firstName;
```
* 100개의 문자를 넣든 몇개의 문자를 넣든 일단 다 읽을 수 있음
### 1.3. 대입(Assignment)과 덧붙이기(Appending)
* C에서의 대입과 덧붙이기는 strcpy()함수나 strcat() 함수를 사용했다
    ```c++
    char firstName[20] = "POPE";
    char fullName[20];

    // 대입 - 안전하지 않음
    strcpy(fullName, firstName);
    // 덧붙이기 - 안전하지 않음
    strcat(fullName, " KIM");
    ```
    * 대입하거나 덧붙일때 fullName의 크기를 초과하면 안전하지 않다
    * 허가받지 메모리에 접근하는 메모리 스톰프(Stomp)가 발생
* C++에서의 대입과 덧붙이기는 직관적으로 연산자를 사용
    * 대입은 = 연산자, 덧붙이기는 += 연산자
    ```c++
        string firstName = "POPE";
        string fullName = "John Doe";

        // 대입
        fullName = firstName;
        // 덧붙이기
        fullname += " KIM";
    ```
### 1.4. 문자열 합치기(Concatenation)
* C에서의 문자열 합치기
    ```c
        char firstName[20] = "POPE";
        char lastName[20] = "KIM";
        char fullName[40]; 

        snprintf(fullName, 40, "%s %s", firstName, lastName);
    ```
    * fullName에 40글자만큼 print 해주는 함수
    * 대입하는 문자가 40글자가 넘으면, 넘는 부분을 자르고 마지막에 자동으로 널 문자를 넣어 줌
    * 비교적 안전하지만 직관적이지 않음
* C++에서의 문자열 합치기
    * \+ 연산자를 사용해서 직관적으로 할 수 있음
    ```c++
        string firstName = "POPE";
        string lastName = "KIM";
        string fullName;

        fullName = firstName + " " + lastName;
    ```
### 1.5. 비교(Relational)연산자
* 문자열을 비교할 떄 C에서는 strcmp()를 사용하고 Java에서는 equals()를 사용한다
* C에서의 문자열 비교
    ```c++
    if (strcmp(firstName1, firstName2) == 0)
    {
    }

    if (strcmp(firstName1, firstName2) > 0)
    {
    }
    ```
    * strcmp()는 같으면 0을 반환하는데 이는 좀 헷갈림
* C++에서의 문자열 비교
    * 연산자로 직관적으로 비교 가능
        * \> \< \<\= \>\= \=\= \!\=
    ```c++
    if (firstName1 == firstName2)
    {
    }
    
    // 사전상의 순서를 비교
    if (firstName1 > firstName2)
    {
    }
    ```
### 1.6. string 클래스의 함수들
* size(), length()
    * 문자열의 길이를 반환 
```c++
cout << firstName.size() << endl;
cout << firstName.length() << endl;
```
* c_str()
    * c 스타일 문자열인 const char*를 반환
        * 해당 string이 가지고 있는 문자 배열의 시작주소를 가리키는 포인터를 반환
```c++
string line;
cin >> line;
const char* cLine = line.c_str();
```
### 1.7. string 속의 한 문자에 접근하기
* C는 배열의 첨자 연산자 \[\]를 사용했음
* C++도 첨자 연산자로 한 문자에 접근 가능
    ```c++
    string firstName = "POKE";
    char letter = firstName[1];

    firstName[2] = 'P';
    ```
    * 여기서 firstName[2]이 그저 값을 반환하면 'P'를 대입할 수 없음
    * 참조를 반환해서 대입이 가능한 거임
* at() 함수로도 한 문자에 접근 가능함
    * 잘 사용하지 않는다
    ```c++
    char letter = firstName.at(1);
    firstName.at(2) = 'P';
    ```
### 1.8. 한 줄 읽기
```c++
string mailHeader;
getline(cin, mailHeader);   // 개행문자(\n)를 만날 때까지 cin에서 문자들을 꺼내서 mailHeader에 저장 
getline(cin, mailHeader, '@');  // @ 문자를 만날 때까지 cin에서 문자들을 꺼내서 mailHeader에 저장
```
* 다음의 조건을 만족할 때까지 계속해서 스트림에서 문자들을 꺼내 stirng에 저장
    * end-of-file을 만날 때까지 
    * 구분 문자(delimiter)를 만날 때까지
        * 구분 문자는 버려짐
## 2. std::string이 좋은가요?
### 2.1. sstream
* std::istringstream
    * cin과 비슷: 키보드 대신 string으로부터 읽어옴
    * sscanf()와 비슷
* std::ostringstream
    * cout와 비슷: 콘솔 대신 string에 출력함
    * sprintf()와 비슷
* sstream은 그렇게 많이 쓰이지는 않는다
### 2.2. C헤더를 써도 될까요?
* 현업의 C++ 애플리케이션에서는 여전히 성능 상의 이유로 많은 C 함수들을 사용
* C
    * <string.h>
    * <stdio.h>
    * <ctype.h>
* C++
    * <cstring>
    * <cstdio>
    * <cctype>
### 2.3. string클래스의 내부동작
#### 2.3.1 1단계
* string을 만들어주면 내부적으로 무슨 일이 일어날까?
```c++
string line;   
```
* string 인스턴스에는 4가지 요소가 있음
    * 포인터(ptr), 길이(Size), 용량(Capacity), 문자 배열(char array)
* 우선 문자열을 저장할 문자 배열 16바이트가 힙 메모리에 할당이 됨
* 포인터는(ptr)은 힙 메모리에 존재하는 char array를 가리키는 포인터임
* 길이(Size)는 문자열의 현재 길이를 표현하는데 처음에는 0임
* 용량(capacity)는 문자열을 저장할 수 있는 크기임
    * 문자 배열 16바이트에서 널 문자를 저장할 1바이트를 빼서 15로 설정해줌
![image](https://user-images.githubusercontent.com/22488593/174011639-d4628240-71a4-4d79-bbdf-43d79b98b412.png)
#### 2.3.2 2단계
* 문자열을 대입하면 내부적으로 무슨 일이 일어날까?
```c++
line = "POPE";    
```
* 문자 배열 16바이트 중 4바이트에 "POPE"가 복사되고 5번째 바이트에 널 문자가 들어간다
* 길이가 4인 문자열이 들어갔으므로 길이(size)를 4로 바꿔줌
#### 2.3.3. 3단계
* 용량을 넘어서는 문자열을 덧붙이기하면 어떻게 될까?
```c++
line += "C++ is awesome, isn't";
```
* 현재 용량인 15바이트에는 이 문자열이 들어갈 수 없으므로 두 배 크기의 메모리 공간을 새로 할당해준다
![image](https://user-images.githubusercontent.com/22488593/174013007-f793ecb3-3a5f-4856-954d-781734866e56.png)
#### 2.3.4. 4단계
* 두 배의 새로운 메모리를 할당해주고 기존 16바이트 메모리에 있는 "POPE"를 복사해준다.
    * 복사완료하면 기존의 16바이트 메모리는 해제
* 포인터가 새로운 메모리를 가리키고 5번째 바이트부터 "C++ is awesome, isn't?"을 복사해준다 
![image](https://user-images.githubusercontent.com/22488593/174013686-dde1d182-4fa6-463f-98c9-87b17c3e04d3.png)
### 2.4 과연 std::string은 빠를까?
* 위의 과정을 보면 굉장히 복잡하다
* 힙(heap) 메모리 할당을 느리다
    * 힙은 os가 가지고 있는 메모리를 일일이 다 훑으면서 사용 가능한 메모리 공간을 찾음. 이는 오래 걸릴 수 밖에 없음
* 메모리 단편화(memory fragmentation)문제도 있다
    * 메모리 할당, 해제를 하다보면 중간에 비어 있는 공간이 많아지는데   이 때 큰 메모리를 할당하려고 하면 할당할 공간이 없다(공간이 많이 비어있음에도 불구하고)
* 내부 버퍼의 증가는 멀티 쓰레드 환경에서 안전하지 않을 수도 있음
* 이러한 이유로 여전히 c 스타일의 문자열을 많이 사용한다
    * sprintf와 char[]
### 2.5 c_str()은 새로운 메모리를 만들어서 주는걸까?
* 아니다
* string 문자열은 char array를 관리해주는 클래스라고 보면 됨
* c_str()을 호출하면 char array를 가리키는 포인터만 반환하면 됨
* 근데 string 클래스 밖에서 char array를 수정해주는 건 말이 안된다
    * 문자열을 수정하고 길이(size) 용량(capacity)도 업데이트 해줘야 함
* 그래서 const char*로 반환한다
    * const char* : 포인터는 바꿀 수 있지만, char은 바꿀 수 없음
    * char* const : char은 바꿀 수 있지만, 가리키는 대상(포인터) 바꿀 수 없음
