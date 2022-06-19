# Section8: 개체지향 프로그래밍2
> 중간고사 대비 주제
>
> 1. 복사 생성자 및 대입 연산자
> 2. 연산자 오버로딩
### 오브젝트 복사시 고려할 점
* Java에서 오브젝트의 사본을 만들기 위해서는 Clone()함수를 사용함
* Clone() 함수를 구현해본다고 하면 생각해봐야 할 점 이 있다
  1. 오브젝트가 가진 값들이 모두 value type이면 새로운 오브젝트를 만들고 그대로 대입해주면 되는데
  2. 오브젝트가 가진 값에 reference type이 있으면 두가지 선택지가 있다
    * reference type값의 원본을 두 오브젝트끼리 공유를 하는 방법이 있고
      * 이 경우를 얕은 복사(shallow copy)가 된다 하고 (원본의 변경이 복사본에 영향을 미친다)    
    * reference type값의 원본을 복사해서 새 오브젝트값에 넣어주는 방법이 있다
      * 이 경우를 깊은 복사(deep copy)가 된다고 한다 (원본의 변경이 복사본에 영향을 미치지 않는다)
* C++에서는 오브젝트 복사를 생성자 단에서 지원해준다
## 1. 복사(Copy) 생성자
* 같은 클래스에 속한 다른 개체를 이용하여 새로운 개체를 초기화
  * 같은 크기, 같은 데이터의 개체
  * <class-name> (const <class-name>&);
  ```c++
  Vector(const Vector& other);
  
  Vector a;   // 매개변수 없는 생성자를 호출
  Vector b(a);  // 복사 생성자를 호출
  ```
### 1.1 암시적(implicit) 복사 생성자
  * 코드에 복사 생성자가 없는 경우, 컴파일러가 암시적 복사 생성자를 자동생성
  ```c++
  // Vector.h
  class Vector
  {
  private:
    int mX;
    int mY;
  }
  ```
  * 아래와 같이 암시적으로 기본 생성자와, 복사 생성자를 만들어준다
  ```c++
  // Vector.obj
  class Vector
  {
  public: 
    Vector() {}
    Vector(const Vector& other)
      : mX(other.mX)
      , mY(other.mY)
  {
  }
  private:
    // ...
  }
  ```
  * 암시적 복사 생성자는 **얕은 복사(shallow copy)**를 수행
      * 멤버 별 복사
  * 각 멤버의 값을 복사 함
  * 개체인 멤버변수는 그 개체의 복사 생성자가 호출됨
    ```c++
    Vector(const Vector& other)
      : mX(other.mX)
      , mY(other.mY)
    {
    }
    ```
      * 개체에다가 같은 클래스의 다른 개체를 삽입하므로 복사 생성자가 호출된다
### 1.1.1 포인터는 얕은 복사가 된다 
* 클래스에 포인터 형 변수가 있다면 어떻게 될까?
* 예: ClassRecord 클래스
```c++
// ClassRecord.h
class ClassRecord
{
public:
  ClassRecord(const int* scores, int count);
  ~ClassRecord();
private:
  int mCount;
  int* mScores;
}
  
// ClassRecord.cpp
ClassRecord::ClassRecord(const int* scores, int count)
    : mCount(count)
{
  mScores = new int[mCount];
  memcpy(mScores, scores )
}
  
ClassRecord::~ClassRecord()
{
  delete[] mScores;  
}
```
* 암시적 복사 연산자의 모습
```c++
// ClassRecord.cpp
ClassRecord::ClassRecord(const int* scores, int count)
    : mCount(count)
{
  mScores = new int[mCount];
  memcpy(mScores, scores )
}
  
// 암시적 복사 생성자
ClassRecord::ClassRecord(const ClassRecord& other)
  : mCount(other.mCount)
  , mScores(other.mScores)
{
}
  
// Main.cpp
ClassRecord classRecord(scores, 5);
ClassRecord* classRecordCopy = new ClassRecord(classRecord);
delete classRecordCopy;
```
  * 암시적 복사 연산자를 사용하면 mScores를 같이 공유한다
  ![image](https://user-images.githubusercontent.com/22488593/174463416-1da32ac2-68a8-401f-a173-0f6804e7521d.png)
  * 이 상태에서 delete classRecordCopy를 실행하면 소멸자가 실행되어 mScores를 지워버린다
  * classRecord는 지워버린 메모리를 가리키고 있는 상태
  ![image](https://user-images.githubusercontent.com/22488593/174463464-a781ecf9-580b-461f-b968-ff450f5c8df1.png)
  * 개체끼리 데이터를 공유하는 얕은 복사가 위험한 이유
### 1.1.2. 사용자가 만든 복사 생성자
  * 클래스 안에서 동적으로 메모리를 할당하고 있다면? 얕은 복사가 일어나 위험할 가능성이 매우 높음
  * 이때는 직접 복사 생성자를 만들어서 깊은 복사(deep copy)를 할 것
     * 포인터 변수가 가리키는 실제 데이터까지도 복사
```c++
ClassRecord::ClassRecord(const ClassRecord& other)
  : mCount(other.mCount)
{
  mScores = new int[mCount];
  memcpy(mScores, other.mScores, mCount * sizeof(int));
}
  
// Main.cpp
// scores에 5개의 점수가 들어있다고 가정하자
ClassRecord classRecord(scores, count);
ClassRecord* classRecordCopy = new ClassRecord(classRecord);
delete classRecordCopy;
```
* 이 복사생성자를 호출하면 어떤 일이 일어날까?
![image](https://user-images.githubusercontent.com/22488593/174464007-d0e5de99-3e79-4429-a68a-e70a6eb0743b.png)
* 더 이상 mScores의 원본을 공유하지 않음
* delete를 실행하면 문제가 더 이상 발생하지 않는다
![image](https://user-images.githubusercontent.com/22488593/174464032-a3405e68-04d2-407b-9892-acee4f96061e.png)
## 2. 함수 오버로딩(overloading)
## 3. 연산자(operator) 오버로딩
## 4. friend 키워드
## 5. 연산자 오버로딩과 const
## 6. 연산자 오버로딩을 남용하지 말 이유
## 7. 암시적 함수들을 제거하는 법 
