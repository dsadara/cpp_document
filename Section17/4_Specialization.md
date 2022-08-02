# 템플릿 특수화

템플릿 특수화는 특정한 템플릿 매개변수를 받도록 템플릿 코드를 커스터마이즈 한 것 입니다.
템플릿을 작성할 때 다른 타입들은 괜찮은데 어떤 타입 하나만 따로 코드를 작성해야할 때가 있습니다.
이때 해당 타입 전용으로 특수화를 할 수 있습니다. 
대표적인 예는 벡터(vector)입니다.
벡터의 구현을 살펴보면 다음과 같습니다.

```c++
template <class T, class Allocator>
class std::vector<T, Allocator> {}  // 모든 형을 받는 제네릭 vector

template <class Allocator>
class std::vector<bool, Allocator>  // bool을 받도록 특수화된 vector
```

첫번째 vector는 모든 형에 대한 vector이고,
두번째는 bool형을 위한 특수화된 벡터입니다.
bool형 벡터를 만들면 두번째 버전의 vector를 실행하겠다는 것입니다.   
그렇다면 왜 bool에 대해서 템플릿을 따로 만들었을까요?
그 이유는 bool의 특성을 이용해서 메모리를 아끼기 위해서입니다.
bool은 1비트만으로도 표현이 가능합니다.
1 바이트에 bool형 8개를 넣는 비트플래그 방식으로 메모리를 아낄 수 있습니다.
당연히 bool형 vector함수는 다른 로직으로 작동할 것입니다.  
그래서 bool형을 위한 특수화된 vector를 구현한 것입니다.

# 템플릿 특수화 종류

## 전체 템플릿 특수화

전체 템플릿 특수화는 템플릿 매개변수 두개 다 특수화를 하는 것입니다.
따라서 템플릿 매개변수 리스트가 비어 있습니다.      
다음코드는 거듭제곱을 구하는 power()함수입니다.
정수와 실수는 거듭제곱을 구하는 방법이 다르고,
밑(base)와 지수(exponent) 둘다 실수 매개변수를 받아야 합니다.
이때 전체 템플릿 특수화를 사용해서 템플릿 매개변수 둘다 실수로 특수화 시킬 수 있습니다. 
```c++
template<typename VAL, typename EXP>
VAL Power(const VAL value, EXP exponent) {}  // 모든 형을 받는 제네릭 power()

template<>
float Power(float value, float exp) // float을 받도록 특수화된 power()
```

## 부분 템플릿 특수화

부분 템플릿 특수화는 템플릿 매개변수 중 하나만 특수화한 경우입니다.
앞서 본 vector의 bool 특수화가 이에 해당합니다.
부분 템플릿 특수화는 특수화를 하는 매개변수만 비워둡니다. 

```c++
template <class T, class Allocator>
class std::vector<T, Allocator> {}  // 모든 형을 받는 제네릭 vector

template <class Allocator>
class std::vector<bool, Allocator>  // bool을 받도록 특수화된 vector 
```

