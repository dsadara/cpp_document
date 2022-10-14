# std::array

std::array는 C++11에서 새로 추가된 STL Container중 하나입니다. 
std::array를 여기서는 편의상 어레이라고 부르겠습니다.
어레이는 벡터와 유사하지만 배열과 같이 용량(Capacity)이 고정되어 있습니다.
단순히 C 스타일 배열을 객체로 감싸서 추상화한 것이라고 보면 됩니다.
요소 수(size)를 기억하지 않아서 아쉬운 점이 많은 컨테이너 입니다.
어레이에 장점도 있는데 어레이 범위를 넘는 요소에 접근하지 못하게 경계체크를 해준다는 것입니다.  
다음의 코드를 실행하면 어떤 것이 출력될까요?

```c++
#include <array>
// ...
std::array<int, 3> numbers;

numbers[0] = 1;
std::cout << numbers.size() << std::endl;   // ??
std::cout << numbers.max_size() << std::endl;   // ??
```

요소의 수와 관계없이 numbers.size()는 3이 출력되고,
max_size()역시 3이 출력됩니다.   

# 어레이가 생겨난 이유
어레이가 생겨난 이유는 고정 배열에 std::algorithm을 사용하기 위해서
그리고 반복자를 사용할 수 있어서라고 추측할 수 있습니다.   
하지만 [범위 기반 for문]()을 사용하면 반복자를 사용할 필요가 없기도 합니다.
그래서 강사님은 어레이보다 fixedVector를 직접 만들어서 사용하는 것을 선호하십니다.