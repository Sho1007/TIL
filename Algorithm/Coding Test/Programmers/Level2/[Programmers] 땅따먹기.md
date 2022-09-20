# 땅따먹기 (c++)
## 문제 
https://school.programmers.co.kr/learn/courses/30/lessons/12913

## 접근 방식
Greedy + DP   
한칸씩 차지하며 내려오는 순서가 존재한다.     
마지막 칸까지 왔을 때 최대가 되는 값을 구하려한다.    
매 칸에서 직전 단계의 최대값 (나와 동일한 열의 칸을 제외한) 을 구하면 되지 않을까 생각했다.   
각 칸에서의 최대값을 저장하면 (dp), 맨 마지막 행에서의 최대값이 답이 된다.

## 문제 풀이
```c++
#include <iostream>
#include <vector>

using namespace std;

int answer = 0;

// dp로 사용할 최대범위의 배열을 미리 생성
int arr[100'000][4] = {};

int solution(vector<vector<int> > land)
{
    for (int i = 0; i < land.size(); ++i)
    {
        for (int j = 0; j < 4; ++j)
        {
            // 첫번째 줄이면 그냥 그 값을 그대로 입력
            if (i == 0)
                arr[i][j] = land[i][j];
            else
            {
                // 두번째 줄부터는
                int maxNum = 0;
                for (int k = 0; k < 4; ++k)
                {
                    // 이전 행의 값들 중 나와 동일한 열의 값을 제외한 최대값을 구한다.
                    if (k == j) continue;
                    if (maxNum < arr[i-1][k])
                        maxNum = arr[i-1][k];
                }
                // 그 값에 현재 위치의 값을 더하면 현재칸까지의 최대값이 나온다.
                arr[i][j] = maxNum + land[i][j];
            }
        }
    }
    
    for (int i = 0; i < 4; ++i)
    {
        // 마지막 행에서의 최대값이 답이 된다.
        if (answer < arr[land.size()-1][i])
            answer = arr[land.size()-1][i];
    }

    return answer;
}
```
## 다시 생각해볼 점
처음에는 재귀 완전탐색을 이용해서 풀어보려고 했다.  
모든 칸을 돌았을 때의 값들 중 최대값을 구하는 식으로 코드를 짰더니 당연하게도 시간초과가 나왔다.    
여기서의 오류는 굳이 불필요하게 하위 작업들을 계속 반복했다는 점이다.   
DP로 매 순간의 최대값을 저장하면, 바로 윗 행의 최대값만 찾으면 되기때문에 하위 작업이 불필요하게 된다.    
DP가 왜 완전탐색보다 효율적인지를 다시금 깨닫게 해주는 문제였다.