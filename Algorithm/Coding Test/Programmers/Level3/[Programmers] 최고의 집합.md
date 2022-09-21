# 최고의 집합 (c++)

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/12938

## 접근 방식
n개의 자연수로 s를 만들 때, 곱이 최대가 되는 집합을 구해라  
예를 들어 8을 2개의 자연수로 만들려면 {1,7}, {2,6}, {3,5}, {4,4} 가 있는데 이 중에서 곱이 제일 큰건 {4,4}이다.  
즉 n을 s로 나눈 값으로 이루어진 집합의 곱이 최고가 된다.

## 문제 풀이
```c++
#include <string>
#include <vector>

using namespace std;

vector<int> solution(int n, int s) {
    vector<int> answer;
    
    if (s / n == 0) return {-1};
    
    while (n > 0)
    {
        int div = s / n;
        answer.push_back(div);
        n--;
        s -= div;
    }
    
    return answer;
}
```
1. s를 n으로 나누었을 때 몫이 1보다 작다면 n개의 자연수로 표현할 수 없으므로 -1을 리턴
2. n이 0이 될때까지 s / n을 구해서 vector에 넣어주고, n은 1만큼, s는 s/n 만큼 빼준다.

## 다시 생각해볼 점
처음엔 완전탐색으로 풀어볼까 했지만, s를 n개의 원소로 나타내는것만 해도 엄청난 시간이 걸릴 것 같았다.   
집합의 원소가 s/n에 가까울수록 곱이 최대가 된다는 걸 알고 있었고,   
다음 원소는 다시 s- (s/n)을 n-1로 나눈 수가 된다는 것을 깨달았다. (아마 점화식?)    
while을 이용해서 n이 0이 될때까지 (더 추가할 원소의 갯수가 0) 반복해줬다.