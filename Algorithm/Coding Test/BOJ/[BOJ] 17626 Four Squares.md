# 17626 Four Squares (c++)

## 문제
https://www.acmicpc.net/problem/17626

## 접근 방식
DP
1. N을 만들 수 있는 제곱수들의 합 중 최소
2. N - sqrt(N) ~ 1까지의 수의 제곱 , 그렇게 뺀 수를 만들 수 있는 최소한의 제곱 수들의 수 (반복)

## 문제 풀이
```c++
#include <iostream>
#include <cmath>
#define endl '\n'
using namespace std;

int N;
int arr[50'001] = {};

int DP(int num)
{
    //cout << num << endl;
    if (num == 0) return 0;
    if (arr[num] == 0)
    {
        int maxNum = sqrt(num);
        int cnt = 0;
        int minCnt = 987'654'321;
        for (int i = maxNum; i > 0; --i)
        {
            //cout << "   " << maxNum << endl;
            //if (num / 2 > pow(i, 2)) break;
            cnt = DP(num - pow(i, 2)) + 1;
            if (cnt < minCnt)
                minCnt = cnt;
        }

        arr[num] = minCnt;
    }

    return arr[num];
}

int main()
{
    ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);

    cin >> N;

    cout << DP(N) << endl;

    return 0;
}
```

## 다시 생각해볼 점
1. 이런 류의 DP를 더 빨리 풀 수 있는 방법을 찾아봐야겠다.
