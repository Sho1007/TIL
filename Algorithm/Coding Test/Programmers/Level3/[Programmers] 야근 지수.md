# 야근 지수 (c++)

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/12927

## 접근 방식
벡터의 각 숫자를 제곱해서 더한 총합이 야근 지수이다.   
그 야근 지수가 최소가 되게 하려면 가장 큰수를 줄여야한다.   
매번 가장 큰 수가 앞으로 나오게 하는 방법은 우선순위 큐이다.

## 문제 풀이
```c++
#include <string>
#include <vector>
#include <queue>
#include <iostream>

using namespace std;

priority_queue<int> pq;

long long solution(int n, vector<int> works) {
    long long answer = 0;
    
    for (int i = 0; i < works.size(); ++i)
    {
        pq.push(works[i]);
    }
    
    for (int i = 0; i < n; ++i)
    {
        int num = pq.top(); pq.pop();
        if (num > 0) num--;
        pq.push(num);
    }
    
    while (!pq.empty())
    {
        int num = pq.top();
        answer += num * num;
        pq.pop();
    }
    
    return answer;
}
```
1. 우선순위 큐에 벡터의 각 원소들을 입력
2. 업무량 N만큼 반복하며 최대값에서 1을 빼서 다시 입력
3. 남아있는 원소들을 제곱하여 answer에 더함
4. answer 출력

## 다시 생각해볼 점
최대값을 줄이는 것이 관건이라 처음엔 매 반복순간마다 정렬을 할까 생각했었다.   
하지만 시간이 너무 걸릴 것 같아서 어떻게 평등하게 줄일 수 있을까를 생각하다가 totalWork - N한다음
그 total work 를 works.size()로 나누면 되지 않을까 생각했다.   
그리고 나눈 나머지만큼은 + 1을 더해서 제곱하려고 했는데 풀리지 않았다.   
결국 힌트를 찾아보니 우선순위 큐 (매 순간 최대값이 Top에 올라오는 큐)를 활용하는 문제였다.   
우선순위 큐를 활용해서 풀었더니 쉽게 풀렸다.   
하지만 이렇게 매번 큐의 값을 갱신해야 할 때 주의할 점은 우선순위 큐는 임의접근이 안되기 때문에  
pq.size()로 for 문을 돌지 말고 !pq.empty() 일때까지 while 문으로 접근 해야한다는 것이다.

