# Section7:개체지향 프로그래밍1

> 중간고사 대비 주제
>
> 1. 접근 제어자(Access Modifier)
## 1. OOP란?
### 1.1. C++에서의 OOP vs Java에서의 OOP 
* C++, Java에 공통적으로 존재하는 OOP요소
  * 클래스, 개체, 생성자, 함수 오버로딩, 힙에 개체 생성하기  
* C++에만 존재하는 OOP요소
  * 스택에 개체 생성하기, 복사 생성자, 소멸자, 연산자 오버로딩
* C++에서는 OOP와 OOP 아닌 것을 섞어 쓸 수 있음
  * C와의 후방호환성을 가짐
* C++은 언매니지드 언어
  * Java와 달리 메모리를 직접 관리해준다
### 1.2. OOP가 뭘까?
* 사람이 세상을 바라보는 방식
  *  사람이 바라보는 물체는 '상태'와 '행동'으로 나눌 수 있음
    ![image](https://user-images.githubusercontent.com/22488593/174020221-4b0f00a8-8800-4857-a9f9-c3fc7fc0c5d8.png)
  * 사람은 '이름', '성별', '나이'라는 상태를 가지고   
  '먹기', '걷기', '말하기'라는 행동을 할 수 있음
  * 이를 코드로 바꾸면
  ```java
  public class Human
  {
    private string name;
    private int sex;
    private int age;
    
    public void eat();
    public void walk();
    public void talk();
  }
  
  Human student = new Human();
  Human teacher = new Human();
  ```
  * 상태는 클래스의 멤버 변수가 되고
  * 행동은 클래스의 멤버 함수가 됨
  * 여기서는 Human이라는 객체를 만들어 줄 꺼임
  * 클래스는 Human객체의 blueprint라고 보면 됨
  * student와 teacher은 서로 다르지만 사람이라는 공통된 특성을 지님
    * 각각 new Human()을 해줌으로써 특성이 다른 두 사람이 만들어진다
    * new 해주는 걸 인스턴스화 해준다고 함
### 1.3. 복잡해진 OOP
* 무언가를 만들 때는 단순한 부분은 최대한 단순하게 만들고 복잡해야만 하는 부분만 복잡하게 만들어야 함
* 일부 프로그래머들이 단순했던 개체지향 개념을 쓸데없이 복잡하게 만들기 시작 함
  * 일부 개체지향 분석과 디자인(OOAD)
  * 일부 디자인 패턴들
#### 1.3.1. 지양해야될 개체지향 개념 예시
![image](https://user-images.githubusercontent.com/22488593/174023663-981c262e-b916-44c4-983c-c21af161c263.png)
* 개체 B의 숫자를 증가시키고 싶으면 어떻게 해야할까?
##### 1.3.1.1. 복잡한 방법
* 호출자라는 다른 개체를 만듦
  * '명령'이라는 멤버와 '명령 추가', '명령 수행' 두 가지의 함수만 가지고 있음
* '증가명령'이라는 또 다른 개체를 생성 
  * '개체 B'라는 멤버와 '실행'이라는 함수를 가짊
![image](https://user-images.githubusercontent.com/22488593/174024519-9de4ebba-592f-4149-a7b3-3457bb7c610a.png)
* 호출자 개체의 '멤버'에 '증가명령'개체를 '명령 추가'로 추가해 줌
* 그리고 '명령 수행' 함수를 실행한다
* 그러면 '증가명령'의 '실행' 이라는 함수가 실행 됨
* '실행'은 '개체 B'의 '증가'라는 함수를 호출해 줌 
* 정말 복잡하다
  * 유지보수성과 가독성이 떨어짐
  * 하지만 웹프로그래밍에서 이렇게 해야할 때가 있다
* 옛날에는 오브젝트를 한번 만들어 놓으면 그 오브젝트를 절때 변경하지 말고 이것을 포함한 다른 오브젝트를 만들어 계속 크기를 키워가라는 이야기가 있었음
  * 이미 존재하는 오브젝트를 고치면 버그를 만들 수 있기 때문
  * 근데 요즘은 이렇게 하지 않고 그냥 고친다
##### 1.3.1.2. 직관적인 방법
* 그냥 '개체 B'의 '증가'함수를 호출해주면 된다
![image](https://user-images.githubusercontent.com/22488593/174026464-4fc62c37-1651-4943-b0c7-a09f4a2f14c8.png)
* Vector 클래스를 만들어보자
```c++
  class Vector
  {
    int mX;
    int mY;
  }
```
  * 멤버 변수는 앞에 m을 붙여주는게 c++의 관습
  * 매개변수가 x, y일 때 this.x 를 하지 않고도 구분이 가능함
## 2. 접근 제어자(Access Modifier)
* C++의 기본 접근권한은 private
```c++
 class Vector
  {
    int mX; // private 멤버 변수
    int mY; // private 멤버 변수
  }
```
* Java에서 접근권한 제어 키워드를 생략하면 public도 private도 아님
  * 그 변수는 해당 패키지 내에서 접근 가능
    * Java에서는 friend 키워드가 없어서 이게 생겼다고 함 
### 2.1. C++의 접근 제어자(Access Modifier)
* public
  * 누구나 접근 가능
* protected
  * 자식 클래스에서 접근 가능
* private
  * 해당 클래스에서만 접근 가능(개체에서가 아님)
    * 내 자식 클래스는 내 나이를 못 보지만, 나와 같은 부모 클래스인 다른 부모는 내 나이를 볼 수 있음 
* 제어자 별로 C++ 멤버들을 그룹 짓는게 특징임
```c++
class SomeClass
{
public:
  int PublicMember;
protected:
  int mProtectedMember;
private:
  int mPrivateMember1;
  int mPrivateMember2;
}
```
## 3. 개체 생성, 스택/힙
## 4. 개체 배열 생성, 개체 소멸
## 5. 멤버 변수 초기화
## 6. new/delete와 malloc()/free()의 차이
## 7. 생성자(Constructor), 초기화 리스트(Initializer List)
## 8. 기본 생성자, 컴파일러가 하는 일
## 9. 생성자 오버로딩(Overloading), 소멸자(Destructor)
## 10. const 멤버 함수
## 11. 구조체(Struct) vs 클래스(Class)
