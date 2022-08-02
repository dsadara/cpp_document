# STL 알고리즘(Algorithm)

STL 알고리즘은 STL 컨테이너에 공용적으로 적용 되는 알고리즘입니다.
요소 범위 즉 처음요소부터 마지막 요소 **전 까지**에서 쓸 수 있는 함수들입니다.
배열 또는 몇몇 STL 컨테이너에 쓸 수 있습니다.
반복자를 통해 컨테이너에 접근하는데 컨테이너의 크기를 변경하지 않습니다.
따라서 추가 메모리 할당도 없습니다. 

# STL 알고리즘의 유형

## \<algorithm\> 라이브러리

1. 변경 불가 순차(sequence) 연산   
요소를 순회하지만 요소를 변경하지 않는 연산입니다. find()와 for_each()가 있습니다. 
2. 변경 가능 순차 연산
요소를 순회하고 요소를 변경하는 연산입니다. copy()와 swap()이 있습니다.
3. 정렬 관련 연산
오름차순 또는 내림차순으로 정렬하는 연산입니다. sort()또는 merge()가 있습니다.

## \<numeric\> 라이브러리

1. 범용 수치 연산
요소의 총합을 구하는 accumulate()가 있습니다. 

# copy() 

```c++
template<class _InIt, class _OutIt>
_OutIt copy(_InIT _First, _InIt _Last, _OutIt _Dest);
```

copy() 함수는 매개변수로 반복자 3개를 넣어줍니다.
_First에서 _Last 전까지의 요소를 _Dest에 복사해줍니다.
주의할 점은 _Last까지가 아닌 _Last 전까지만 복사한다는 점입니다.

```c++
//std::vector<int> scores;
//std::vector<int> copiedScores;
//std::vector<std::string> names;
//std::vector<std::string> copiedNames;

// int vector
std::copy(scroes.begin(), scores.end(), copiedScores.begin());  // end()는 포함 안시키고 그 전까지만 복사

// string vector
std::copy(names.begin(), names.begin() + 2, copiedNames.begin());   // begin() + 1까지만 복사 
```

# 결론

STL 알고리즘은 사실 많이 쓰이지는 않습니다. 정렬 알고리즘 정도 쓰인다고 합니다. 
find()는 컨테이너마다 있어서 그것을 사용하는 편입니다.
copy()나 swap()도 직접 만들어서 사용할 수 있습니다.
반복자를 좋아하지 않는 사람은 for문을 만들어서 직접 알고리즘을 짜는 사람도 많다고 합니다. 