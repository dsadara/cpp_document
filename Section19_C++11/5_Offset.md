# offsetof 매크로
```c++
#define offsetof(type, member) // implementation-defined
```
offsetof은 매크로의 일종입니다.   
특정 멤버가 본인을 포함한 자료 구조의 시작점에서부터 몇 바이트만큼 떨어져 있는지 알려줍니다.   
직렬화(serialize)나 역직렬화(deserialize)를 할때 유용하게 사용됩니다.

# 예시: 멤버들의 상대적 위치(offset) 구하기

다음의 예시는 각 멤버가 Student 구조체 메모리 시작 부분에서 몇 바이트 떨어져 있는지 출력합니다. 

```c++
// Main.cpp
struct Student
{
    const char* ID;
    const char* Name;
    int CurrentSemester;
};

int main()
{
    std::cout << "ID offset: " << offsetof(Student, ID) << std::endl;
    std::cout << "Name offset: " << offsetof(Student, Name) << std::endl;
    std::cout << "CurrentSemester offset: " << offsetof(Student, CurrentSemester) << std::endl;

    return 0;
}
```

![image](https://user-images.githubusercontent.com/22488593/183022803-4e41d9eb-426b-4c53-8131-dd2994992839.png)

ID는 메모리 시작 부분에 위치하고, Name은 4바이트에, CurrentSemester은 8바이트에 위치합니다. 