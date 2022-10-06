# 야근 지수 (c++)

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/42898

## 접근 방식
DP   
처음엔 BFS 로 풀려고 했다. 목적이에 도달할 때마다 정답을 +1 씩해주려고 했는데 풀리지 않았다.   
그러다 생각해낸게 DP의 현재 값은 이전 값을 이용해서 계산된다.   
여기서 이전값이란 현재 칸에 도달할 수 있는 칸들의 가짓 수의 합이다.
dp[i][j] = dp[i-1][j] + dp[i][j-1] 인 것이다.   
(왼쪽과 위가 모두 존재한다는 가정 하에)   
출발점에서 목적지로의 탐색이 아니라 목적지에서 출발지로의 재귀형 역탐색이었다.

## 문제 풀이
```c++
#include <string>
#include <vector>
#include <queue>
#include <iostream>
#include <algorithm>

using namespace std;

int arr[101][101];

queue<pair<int, int>> q;

int dy[2] = {0, -1};
int dx[2] = {-1, 0};

int N, M, answer = 0;

int DP(int y, int x)
{
    if (arr[y][x] == 0)
    {
        int nextY, nextX;
        int sum = 0;
        for (int i = 0; i < 2; ++i)
        {
            nextY = y + dy[i];
            nextX = x + dx[i];
            
            if (nextY < 1 || nextX < 1) continue;
            if (arr[nextY][nextX] == -1) continue;
            
            sum += DP(nextY, nextX);
            sum %= 1'000'000'007;
        }
        arr[y][x] = sum;
    }
    
    return arr[y][x];
}

void Print()
{
    for (int i = 1; i <= N; ++i)
    {
        for (int j = 1; j <= M; ++j)
            cout << arr[i][j] << " ";
        
        cout << endl;
    }
}

int solution(int m, int n, vector<vector<int>> puddles) {
    
    M = m;
    N = n;
    
    for (int i = 0; i < puddles.size(); ++i)
    {
        arr[puddles[i][1]][puddles[i][0]] = -1;
    }
    
    arr[1][1] = 1;
    
    answer = DP(N, M);
    
    //Print();
    
    
    return answer;
}
```
1. 최대 범위만큼 2차배열을 미리 선언한다.
2. 지역변수 n, m을 전역변수로 변환해준다.
3. 웅덩이가 있는 칸을 배열에 표시한다.
4. 시작지점의 값을 입력한다.
5. DP 알고리즘 수행
    1. 왼쪽과 위쪽으로 갈 수 있는지 체크하고
    2. 가려고 하는 칸이 범위 밖이거나 웅덩이면 패스
    3. DP(왼쪽 칸) + DP(윗쪽 칸) 해준다. 각 값들을 1'000'000'007 로 나눠준다.
    4. 해당 값을 arr[i][j]에 입력하고 리턴
6. arr[N][M] 의 값을 리턴해준다.

## 다시 생각해볼 점
최단거리의 가짓 수를 구하라고 했지만 우하향, 오른쪽 아래 끝에 존재하는 목적지는 다 최단거리를 계산할 필요가 없게 만들어준다.   
결국 목적지인 오른쪽 아래 모서리에 도달하려면 우향 혹은 하향 할 수 밖에 없기 때문에다.   
현재 칸의 값을 계산하려면 윗쪽과 왼쪽의 값을 더하면 되기 때문에 DP 역시 가능하게 해준다.   
길찾기, 좌표 문제는 언뜻 보면 BFS 문제로 보일 수 있지만, 이전 값을 이용해서 현재 값을 계산하는 문제는 DP 문제라는걸 확실히 파악할 필요가 있다.