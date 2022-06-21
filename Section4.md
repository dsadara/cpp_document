# Section4: 일부 새로운 C++ 기능

> 중간고사 대비 주제
>
> 1. 참조, 포인터
> 2. 얕은 복사 vs 깊은 복사

## 1. Bool 데이터형

- c 에서 불리언값은 0 또는 0이 아닌 값(1) 이었다.
- 매크로로 불리언값의 가독성을 높이기도 했다
  ```c++
   #define TRUE (1)
   #define FALSE (0)
  ```

```c++
// 만약 student가 아니면
if (IsStudent() == 0)
{
  // ...
}
// 만약 student라면
if (IsStudent() == TRUE)
{
  // ...
}
```

- C++에서는 bool 데이터형을 지원해 줌
  - 참: true, 거짓: false

```c++
// 만약 student가 아니면
if (IsStudent() == false)
{
  // ...
}
// 만약 student라면
if (IsStudent() == true)
{
  // ...
}
```

> ## 값에 의한 호출
>
> ```c++
> void swap(int arg1, int arg2)
> {
>   int temp = arg1;
>   arg1 = arg2;
>   arg2 = temp;
> }
> int main()
> {
>   int num1 = 10;
>   int num2 = 10;
>   swap(num1, num2);
>   /..
> }
> ```
>
> - 위 코드를 실행하면 num1와 num2가 Swap될까?
> - 안 됨. num1, num2가 swap함수에 복사해서 들어갔기 때문  
>   복사한 것을 바꿔도 원본은 바뀌지 않음
> - 스택 프레임을 살펴보면 이를 확인 가능  
>   ![image](https://user-images.githubusercontent.com/22488593/173720851-8ecfa666-ca9f-4eef-bc6f-a65ab0f6a1d8.png)
> - num1, num2값은 복사 됬고 복사된 값끼리 swap이 됬음
>
> ## 참조에 의한 호출
>
> ```c++
> void swap(int* arg1, int* arg2)
> {
>   int temp = *arg1;
>   *arg1 = *arg2;
>   *arg2 = temp;
> }
>
> int main()
> {
>   int num1 = 10;
>   int num2 = 20;
>   swap(&num1, &num2);
>   // ...
> }
> ```
>
> - num1와 num2가 swap이 됨
> - swap에 num1와 num2의 참조 즉 주소를 전달했기 때문에 원본을 변경 가능
> - 스택프레임을 살펴보면 num1, num2의 주소값인 4100, 4096이 swap 함수의 매개변수로 들어감  
>   ![image](https://user-images.githubusercontent.com/22488593/173722145-c45bd047-5197-44ab-9a60-49415f144554.png)

## 2. 참조(Reference)

포인터를 더 안전하게 쓰기 위해 c++에 추가된 기능

### 2.1. C/C++은 값/참조에 의한 호출은 모든 타입에 똑같이 동작한다

- Java는 기본 타입(Primitive Type)은 값에 의한 호출, 참조 타입(Reference Type)은 참조에 의한 호출이 실행된다.
- C/C++은 모든 타입에 참조에 의한 호출을 할지 값에 의한 호출을 할 지 정해줄 수 있다.
  - 포인터(참조)의 유무의 차이

```c++
struct Mystruct // 8 bytes
{
  int number;
  int NotNumber;
};

void Increase(MyStruct argument)
{
  argument.Number = argument.Number + 1;
}

int main()
{
  MyStruct mystruct;
  myStruct.Number = 100;
  Increase(myStruct);
}
```

- myStruct.Number은 100이 됨
- Increase()에 myStruct를 넣으면 복사가 되어 들어 간다
  - 이는 값에 의한 호출
- struct가 아닌 class를 전달해도 값에 의한 호출이 된다.

```c++
void Increase(MyStruct* argument)
{
  argument->Number = argument-> Number + 1;
}

int main()
{
  MyStruct myStruct;
  myStruct.Number = 100;

  Increase(&myStruct);
}
```

- myStruct.Number은 101이 된다
- Increase()에 myStruct의 주소값을 넣어주어서 원본을 바꿀 수 있음
  - 참조에 의한 호출

### 2.2. 참조

- 별칭이다
  ```c++
  int number = 100;
  int& reference = number;
  ```
  - number를 reference라는 별칭을 붙이는 것과 같음
  - 타입 옆에 &를 붙여줌
    - &number와 같이 주소를 불러오는 연산자와는 다른 것
- NULL이 될 수 없음
  ```c++
  int & reference = NULL; // error
  ```
- 초기화 중에 반드시 선언되어야 함
  ```c++
  int& reference; // error
  ```
- 참조하는 대상을 바꿀 수 없음

  ```c++
  int number1 = 100;
  int number2 = 200;

  int& reference = number1;
  reference = number2;  // 세 변수 값이 모두 200이 됨
  ```

- 함수 매개변수로서의 참조

```c++
void Swap(int& number1, int& number2)
{
  int temp = number1;
  number1 = number2;
  number2 = temp;
}
```

- 참조로 구현한 swap 함수임
- 장점은 매개변수가 NULL인지 체크를 안해도 됨
  - 참조는 NULL을 가리킬 수 없기 떄문
- 그리고 우리가 소유하지 않은 메모리를 장소를 가리킬 수 없음
  - 포인터처럼 \*(number++) 이런짓을 할 수 없기 때문

### 2.3. 컴퓨터는 참조가 뭔지 알까?

- 모름
- 포인터와 참조는 같은 어셈블리 명령어를 생성함
- 컴파일러는 참조를 포인터로 바꿔 줌. 기계가 이해할 수 있도록
- 참조는 오직 인간을 위한 것임

## 3. 어떤 매개변수가 출력결과인지 알기 좋은 방법

```c++
struct Vector
{
  int X;
  int Y;
  int Z;
}

bool TryDivide(Vector& a, Vector& b, Vector& c)
```

- 매개변수 a, b, c 셋중에 하나는 출력 결과인데 어떤게 출력결과인지 알기 힘들다

### 3.1 시도

- 매개변수 이름을 더 잘 지으면 해결할 수 있을까?
  ```c++
    bool TryDivide(Vector& result, Vector& a, Vector& b);
  ```
- 하지만 호출자가 여전히 실수할 수 있음

  ```c++
    TryDivide(a, b, result);
  ```

- 읽기전용 매개변수를 상수화 하면 해결할 수 있을까?
  ```c++
    bool TryDivide(Vector& result, const Vector& a, const Vector& b);

    const Vector a;
    const Vector b;
    Vector result;

    TryDivide(result, a, b); // OK
  ```
- 괜찮아 보이지만 호출자는 여전히 이런 짓을 할 수 있음
  ```c++
    bool TryDivide(Vector& result, const Vector& a, const Vector& b);

    Vector a;
    Vector b;
    Vector c;

    TryDivide(a, b, c); // 어떤게 출력결과?
  ```

### 3.2 참조를 이용한 방법

- 읽기전용 매개변수는 상수 참조로
- 출력결과용 매개변수는 포인터로
  ```c++
    bool TryDivide(Vector* result, const Vector& a, const Vector& b);
    TryDivide(&a, b, c);
  ```
- 포인터 매개변수에는 &가 앞에 붙으니 구분이 된다
- 변수 a가 NULL이 되면 안 됨
  - 이때는 함수 내에 assert함수를 넣어 NULL이 되는 경우를 잡으면 됨

### 3.3 C#에서의 방법

- out 키워드를 이용해서 출력 결과임을 표시해 줌
  ```c#
  bool TryDivide(out Vector result, ...);
  ```
- 함수 내에서 out 변수에 값을 대입해주지 않으면 컴파일 오류
