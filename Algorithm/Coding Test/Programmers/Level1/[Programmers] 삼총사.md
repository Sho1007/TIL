# [1차] 프렌즈4블록 (c++)
## 문제 
https://school.programmers.co.kr/learn/courses/30/lessons/131705

## 접근 방식
순서가 있는 백트래킹
1. 주어진 배열을 돌면서 숫자를 하나씩 빈 배열에 넣는다.
2. 배열의 숫자가 3개가 되면 더해보고 0이되면 answer++
3. 배열의 숫자를 하나 빼고 다시 1번부터 반복


## 문제 풀이
#### 메인함수
```c++
#include <string>
#include <vector>

using namespace std;

int answer = 0;

void Func(vector<int>& number, vector<int>& three, int start = 0)
{
    if (three.size() == 3)
    {
        if (three[0] + three[1] + three[2] == 0)
            answer++;
        
        return;
    }
    
    for (int i = start; i < number.size(); ++i)
    {
        three.push_back(number[i]);
        Func(number, three, i + 1);
        three.pop_back();
    }
}

int solution(vector<int> number) {
    vector<int> v;
    Func(number, v, 0);
    
    return answer;
}
```

## 다시 생각해볼 점
1. 백트래킹 문제라는걸 생각해내는게 중요.
2. 순서가 있는 경우 시작지점을 정해주고
3. 순서가 없는 경우 주어진 배열에서 원소를 제거했다가 삽입해야함