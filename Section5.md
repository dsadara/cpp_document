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