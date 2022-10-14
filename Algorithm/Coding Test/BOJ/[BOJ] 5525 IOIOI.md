# 5525 IOIOI (c++)

## 문제
https://www.acmicpc.net/problem/5525

## 접근 방식
구현, 문자열
1. 처음엔 Pn 을 직접 만들려고 했다. 그런데 N의 최대값이 1'000'000인데 그러면 PN의 길이는 2'000'001이 된다. (너무 긺)
2. 그 다음엔 Pn 을 직접 만들지 않고 어떻게 PN 이 S에 포함되어 있는지 확인할 수 있을까 생각해봤다.
3. 생각해보니 IOIO식으로 계속 다른 문자열이 입력되고, 그 시작과 끝이 'I'라는데에서 단서를 얻었다.
4. 문자를 순차적으로 접근
    1. 만약 배열이 0이고, 문자열의 현재문자가 'I'면 넣기
    2. 만약 배열이 1 이상이면, 배열의 끝문자와 문자열의 현재 문자 비교
        1. 다르면 넣기 (끝문자 'I', 현재 문자 'O'인 경우)
        2. 같으면 배열을 초기화 시키고, 만약 현재 문자가 'I'인 경우에만 넣기 (Pn은 'I'로 시작하니)
    3. 배열의 길이가 Pn의 길이와 같다면, 시작문자와 끝 문자가 'I'인지 확인 (이미 그 사이 문자의 확인은 넣으면서 체크함)
        1. 맞으면 answer++
    4. 배열의 시작지점을 뒤로 한칸 (Size--) 시키고 다시 4-1부터 진행


## 문제 풀이
```c++
#include <iostream>
#include <string>
#include <vector>
#define endl '\n'

using namespace std;

int N, M, answer = 0;

string S;

char frontChar, backChar;
int strSize = 0;

int main()
{
    ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);

    cin >> N >> M >> S;

    for (int i = 0; i < M; ++i)
    {
        if (strSize == 0)
        {
            if (S[i] == 'I')
            {
                frontChar = S[i];
                backChar = S[i];
                strSize++;
            }  
        }
        else
        {
            if (backChar != S[i])
            {
                backChar = S[i];
                strSize++;
            }
            else
            {
                strSize = 0;
                if (S[i] == 'I')
                {
                    frontChar = S[i];
                    backChar = S[i];
                    strSize++;
                }
            }
        }

        if (strSize == (2 * N) + 1)
        {
            if (frontChar == 'I' && backChar == 'I')
            {
                answer++;
            }
            if (frontChar == 'I') frontChar = 'O';
            else frontChar = 'I';

            strSize--;
        }
    }

    cout << answer << endl;

    return 0;
}
```

## 다시 생각해볼 점
1. 위의 접근법으로 해결했으나, 다른 사람들의 처리 결과를 보니 메모리를 적게 씀 (나:4148KB, 다른사람들:3680KB)
2. 배열을 시작부터 최대 크기로 선언해서 그런가 싶어서 queue 를 활용해봄
3. 크기는 줄었으나 실행시간이 늘어남 (4148KB,8ms -> 3696KB,12ms)
4. 생각해보니 배열에 저장할 필요가 없음 그냥 시작문자, 끝문자, 사이즈 만 알고있으면 풀 수 있었음
(이미 접근법에서 사이문자 확인이 필요없다는걸 알았지만 그걸 저장하지 않을 생각을 못해봄)
5. 그렇게 풀어보니 다른 사람들과 같은 메모리, 실행시간이 나옴 (해당 문제 740등에 안착)
6. 다른 사람들도 다 이렇게 풀었는지 해당 메모리, 실행시간이 몇페이지임.. 좀 더 일찍 풀었다면 더 높은 등수였을텐데 아쉽다.
7. 결론 : 저장하지 않아도 될 데이터를 잘 파악해보자!