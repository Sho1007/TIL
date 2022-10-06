# 단속카메라 (c++)

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/42884

## 접근 방식
Greed
1. 출입한 시간 기준으로 정렬
2. 현재 주행 구간이 다음 주행 구간을 포함한다면 -> 다음 주행 구간의 어디를 골라도 둘다 만족하므로 continue
3. 현재 주행 구간이 다음 주행 구간과 겹친다면 -> 다음 주행구간을 현재 주행구간과 겹치는 구간으로 축소 -> 다음 주행 구간에서 조건 처리 후 어디에 설치해도 현재 구간과 같이 만족
4. 겹치지 않는 경우 -> 현재 구간의 출차 지점에 설치

## 문제 풀이
```c++
#include <vector>
#include <algorithm>

using namespace std;

int solution(vector<vector<int>> routes) {
    int answer = 0;
    
    // 들어온 순서대로 정렬
    sort(routes.begin(), routes.end(), [](vector<int> a, vector<int> b)->bool{ if (a[0] == b[0]) return a[1] < b[1]; return a[0] < b[0]; });
    
    int recentPos = -1;
    for (int i = 0; i < routes.size(); ++i)
    {
        // 최근 설치한 cctv가 있는지 확인
        if (recentPos != -1)
        {
            // 있다면 현재 주행 구간을 체크 가능한지 확인 후 가능하면 continue
            if (routes[i][0] <= recentPos && recentPos <= routes[i][1])
            {
                continue;
            }
        }
        
        // 다음 값이 없다면 출차 구간에 설치
        if (i == routes.size()-1)
        {
            answer++;
            recentPos = routes[i][1];
            continue;
        }

        // 현재 구간이 다음 구간을 포함한다면 continue
        if (routes[i][0] <= routes[i+1][0] && routes[i+1][1] <= routes[i][1])
            continue;
        else
        {
            // 포함하지 않는다면 
            // 겹치는지 확인
            // 1) 겹친다면 다음값을 줄임
            if (routes[i+1][0] <= routes[i][1])
            {
                routes[i+1][1] = routes[i][1];
            }
            // 2) 겹치지 않는다면 그냥 출차지역에 설치
            else
            {
                answer++;
                recentPos = routes[i][1];
            }
        }
    }
    
    return answer;
}
```
## 다시 생각해볼 점
'최소한' '최대한' 이런식으로 극값을 구하라고 한다면 일단 그리디 알고리즘을 생각해볼 필요가 있는 것 같다.   
이 경우는 최대한 겹치는 구간에 cctv를 설치하면 그 수를 줄일 수 있다.   
어떻게 겹치는지를 확인해 본 결과 포함인지 아닌지, 겹치는지 안겹치는지, 이미 설치된 cctv로 체크가 가능한지 의 케이스를 확인할 수 있었다.

-> 놀랍게도 출차 값을 기준으로 정렬하고 진입지점 -1 로 기준점을 설정한뒤 기준점보다 크다면 해당 지점에 설치하고 기준점으로 삼는 방법이면 훨씬 간단하게 풀 수 있었다.   
-> 그리디를 활용할 때는 기준을 어디로 어떻게 잡을 것인지를 항상 고민해보자.