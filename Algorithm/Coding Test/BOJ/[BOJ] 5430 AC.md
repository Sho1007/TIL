# 5430 AC (c++)

## 문제
https://www.acmicpc.net/problem/5430

## 접근 방식
문자열
1. R과 D로 이루어진 명령
    * 결국 R의 갯수가 짝수면 그대로고 홀수면 앞뒤가 바뀐다.
2. D는 맨 앞의 숫자 삭제
    * 그 때 그 때 삭제하는게 아닌 앞에서 삭제할 갯수와 뒤에서 삭제할 갯수를 구한 뒤 마지막에 처리
3. 마지막으로 string으로 감싸서 출력
    * 이 때 정상 순서면, 앞에서 삭제한 갯수 ~ (n-뒤에서 삭제한 갯수-1)까지를 넣으면 된다.


## 문제 풀이
```c++
#include <iostream>
#include <string>
#define endl '\n'
using namespace std;

int T, n;
string p, x;
int arr[100'000] = {};

void CheckNum(bool& isFront, int& frontNum, int& endNum)
{
    for (int i = 0; i < p.length(); ++i)
    {
        // Find R 
        int j = i;
        while (j < p.length() && p[j] == 'R')
        {
            j++;
        }
        // R이 있는 경우
        if (j != i)
        {
            // R이 홀수개인 경우
            if ((j - i) % 2 != 0)
            {
                isFront = !isFront;
            }
        }
        // Find D
        int k = j;
        while (k < p.length() && p[k] == 'D')
        {
            k++;
        }
        // D가 있는 경우
        if (k != j)
        {
            if (isFront) frontNum += k - j;
            else endNum += k - j;
        }
        i = k - 1;
    }
}

void SplitX()
{
    int pos = 0;
    x = x.substr(1, x.length() - 2);
    int start = 0;
    int next = x.find(',', start);
    while (next != string::npos)
    {
        string str = x.substr(start, next - start);
        if (str.length() > 1)
            arr[pos++] = stoi(str);
        else
            arr[pos++] = str[0] - '0';
        
        start = next + 1;
        next = x.find(',', start);
    }
    string str = x.substr(start, next - start);
    if (str.length() > 1)
        arr[pos++] = stoi(str);
    else
        arr[pos++] = str[0] - '0';
}

int main()
{
    ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);

    cin >> T;

    while (T-- > 0)
    {
        cin >> p >> n >> x;

        bool isFront = true;
        int frontNum = 0, endNum = 0;
        CheckNum(isFront, frontNum, endNum);

        if (frontNum + endNum > n)
        {
            cout << "error" << endl;
            continue;
        }

        
        if (n == 0) cout << "[]" << endl;
        else
        {
            SplitX();
            string answer = "[";
            if (isFront)
            {
                for (int i = frontNum; i < n - endNum; ++i)
                {
                    answer += to_string(arr[i]);
                    if (i != n - endNum - 1)
                        answer += ',';
                }
            }
            else
            {
                for (int i = n - endNum - 1; i >= frontNum; --i)
                {
                    answer += to_string(arr[i]);
                    if (i != frontNum)
                        answer += ',';
                }
            }
            answer += ']';

            cout << answer << endl;
        }
    }

    return 0;
}
```

## 다시 생각해볼 점
1. 시간을 더 줄여보고 싶어서 여러 시도를 해봤지만 더 늘어났다.
    * string 에서 split 한 뒤 숫자로 변형 후 나중에 필요한 갯수만큼 넣어줌 (28ms)
    * string 에서 배열로 넣어주지 않고 바로 answer string 에 더해줌 (388ms)
    이 때 앞에서부터 뒤로 더해주는건 큰 문제가 없는데 answer = num + ',' + answer 즉 뒤에서 앞으로 가면서 더해주는 부분에서 answer을 리사이징하고 다시 더해주는게 시간을 많이 잡아먹는 것 같음
2. 결국 처음 푼게 54위에 안착 (욕심이 나서 더 건드려봤지만 시간을 줄이지 못했다.)