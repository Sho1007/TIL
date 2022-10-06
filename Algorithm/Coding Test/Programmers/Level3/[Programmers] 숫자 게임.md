# 단속카메라 (c++)

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/12987

## 접근 방식
우선순위 큐   
처음엔 정렬해서 해결하려 했다.   
그러다 문득 upper_bound 가 생각났고 upper_bound로 풀어보려했다.   
(근데 이건 아무리 이분탐색이지만 매 순간 탐색을 해야해서 시간 초과가 뜬다.)   
생각하다보니 '정렬', 그리고 '항상 최고값을 가져온다' 가 떠올랐고, 우선순위 큐에 두 배열 다 집어넣고 A의 최대값을 하나씩 접근하면 B의 최대값으로 이길 수 있을 때만 answer++ 해줬더니 너무나도 간단하게 풀렸다.

## 문제 풀이
```c++
#include <vector>
#include <queue>

using namespace std;

priority_queue<int> pqB;
priority_queue<int> pqA;

int solution(vector<int> A, vector<int> B) {
    int answer = 0;

    // 두 배열 다 우선순위 큐에 넣어준다.    
    for (int i = 0; i < A.size(); ++i)
    {
        pqA.push(A[i]);
        pqB.push(B[i]);
    }
    
    // 이기든 지든 A를 하나씩 최대값부터 순차접근
    while (!pqA.empty())
    {
        // A의 현재 최대값을 B의 최대값으로 이길 수있다면 answer++ 해주고 B에서 최대값을 pop 해준다.
        if (pqA.top() < pqB.top())
        {
            answer++;
            pqB.pop();
        }
        pqA.pop();
    }
    
    return answer;
}
```
## 다시 생각해볼 점
이 경우 'A팀이 순서를 정했다.' 라는게 나에겐 함정이었다.   
결국 구하고자 하는 건 최대한 많이 이겼을 때의 점수이다.   
(A팀의 순서를 따를필요없이 뒤바꿔도 이기기만 하면 된다는 뜻이다.)  
이기려면 상대방보다 높은 수를 내면 된다. 그런데 효율적으로 이기려면 상대방의 작은 수에 내 높은 수를 써버리면 안됐다.   
거기서 든 생각이 상대도 항상 최대값을 먼저 내고, 내가 이길 수 있을때만 내 최대값을 내면 어떨까 였다.  
결국 아무리 높은 수여도 이길 수 있는 횟수는 1번이고 그렇다면 가급적 상대방의 높은 수를 이기는게 효율적이기 때문이다.   
그래서 처음엔 정렬을 사용하려했다. 하지만 '정렬' '항상 최대값' 을 본 순간 우선순위 큐가 생각났다.   
'정렬' '항상 최대값' 이 필요하면 꼭 우선순위 큐를 생각하자!
