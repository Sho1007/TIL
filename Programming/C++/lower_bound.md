# lower_bound / upper_bound

## Header
>\<algorithm>

## Usage
```c++
ForwardIt lower_bound(ForwardIt first, ForwardIt last, const T& value);
ForwardIt lower_bound(ForwardIt first, ForwardIt last, const T& value, Compare comp);

ForwardIt upper_bound(ForwardIt first, ForwardIt last, const T& value);
ForwardIt upper_bound(ForwardIt first, ForwardIt last, const T& value, Compare comp);
```

## Description
> lower_bound(first, last, x) 는 (first, last) 범위에서 x **이상**인 첫 번째 원소의 위치를 반환     
upper_bound(first, last, x) 는 (first, last) 범위에서 x를 **초과**하는 첫 번째 원소의 위치를 반환

##  Example
```c++
#include <algorithm>
#include <vector>
#include <iostream>

using namespace std;

int main()
{
    // Array
    int arr[6] = { 1, 2, 3, 4, 5, 6 };
    cout << lower_bound(arr, arr + 6, 4) - arr << '\n';  // 3
    cout << upper_bound(arr, arr + 6, 4) - arr << '\n';  // 4

    // Vector
    vector<int> v = { -2, -2, -1, -1, 0, 0, 0, 1, 1 };
    cout << lower_bound(v.begin(), v.end(), 0) - v.begin() << '\n';  // 4
    cout << upper_bound(v.begin(), v.end(), 0) - v.begin() << '\n';  // 7


    return 0;
}
```


## Caution
>! (first, last) 범위 내의 원소들이 *오름차순*으로 정렬되어 있어야한다. (이진탐색으로 구현되어 있기  때문)   
! Random Access Iterator가 있는 경우에만 O(log n)   
! map, set 과 같이 Random Access Iterator가 없는 경우 클래스 내의 lower_bound 메소드를 써야 O(log n)

## Reference
https://torbjorn.tistory.com/269    
https://chanhuiseok.github.io/posts/algo-55/