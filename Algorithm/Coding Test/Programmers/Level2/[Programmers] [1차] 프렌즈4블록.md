# [1차] 프렌즈4블록 (c++)
## 문제 
https://school.programmers.co.kr/learn/courses/30/lessons/17679#
## 접근 방식
> 문제에서 주어진 처리방법 대로 차근차근 구현해나가면 되는 문제였다.    
> 이런 문제는 규칙을 얼마나 코드로 정확하게 구현하는지가 관건

## 문제 풀이
#### 메인함수
```c++

int solution(int m, int n, vector<string> board) {
    
    do
    {
        // 초기화
        v.clear();
        // 보드 확인
        CheckBoard(board);
        // 지우기
        RemoveBoard(board);
        // 칸 내리기
        DropDownBoard(board);
        
    } while (v.size() > 0);
    
    return answer;
}
```
> 지워야할 부분이 있는지 체크 후 있다면 보드에 변화가 생겼으니 한번 더 반복한다. (없다면 끝냄)   
> 지워야할 부분을 우선 벡터에 넣고 나중에 다 같이 지워준 이유는 지워야하는 블럭들 중에 겹치는 부분이 생겨도 정상적으로 인식하게끔 하기 위함이다.

#### 보드 확인
```c++
bool CheckFour(vector<string>& board, int startY, int startX)
{
    return board[startY][startX] != '0'&& board[startY][startX] == board[startY+1][startX] && board[startY][startX] == board[startY][startX+1] && board[startY][startX] == board[startY+1][startX+1];
}

void CheckBoard(vector<string>& board)
{
    // 지워질 부분 찾기
    for (int i = 0; i < board.size()-1; ++i)
    {
        for (int j = 0; j < board[i].size()-1; ++j)
        {
            // 우하향 하며 4개가 같은 그림인지 확인
            if (CheckFour(board, i, j))
            {
                v.push_back({i, j});
                v.push_back({i, j+1});
                v.push_back({i+1, j});
                v.push_back({i+1, j+1});
            }
        }
    }
}
```
> 4개가 같은지 체크할 때는 지워졌을 때의 문자 '0'인지를 같이 확인해줬다. ('0'이라면 지울필요 X)     
> 보드에서 지울 부분을 체크할 때는 마지막 열과 행은 체크하지 않았다. (CheckFour에서 우하향하며 건드리기 때문에)

### 보드 지우기
```c++
void RemoveBoard(vector<string>& board)
{
    for (int i = 0; i < v.size(); ++i)
    {
        if (board[v[i].first][v[i].second] != '0')
        {
            answer++;
            board[v[i].first][v[i].second] = '0';
        }
    }
}
```
> 간단하게 '0'인지 확인 후 아니라면 정답 카운트를 늘려주고 '0'으로 지워줬다.

### 칸 내리기
``` c++
void DropDownColumn(vector<string>& board, int startY, int startX, int zeroCount)
{
    for (int i = startY; i >= zeroCount; --i)
    {
        board[i][startX] = board[i-zeroCount][startX];
    }
    for (int i = 0; i < zeroCount; ++i)
    {
        board[i][startX] = '0';
    }
}

void DropDownBoard(vector<string>& board)
{
    for (int i = board.size()-1; i > 0; --i)
    {
        for (int j = 0; j < board[i].size(); ++j)
        {
            if (board[i][j] == '0')
            {
                // 0의 갯수를 셈
                int zeroCount = 0;
                for (int k = i; k >= 0; --k)
                {
                    if (board[k][j] == '0')
                        zeroCount++;
                    else
                        break;
                }
                // 위칸이 전부 0인 경우 (줄 내리기 불필요)
                if (zeroCount == i + 1)
                    continue;
                // 0인 갯수만큼 내려야함
                DropDownColumn(board, i, j, zeroCount);
            }
        }
    }
}
```
> 이 문제의 핵심 구현부인 칸 내리기 함수이다. 우선 보드를 아래서부터 훑으면서 '0'인 부분이 존재하는지 확인해준다.   
>그리고 존재한다면 위로 몇개의 '0'이 있는지 확인해준다 (그만큼 내려야하기 때문에)   
>이 때 만약 윗칸이 전부 '0'이라면 칸 내리기를 실행하지 않는다. (아마 이 조건문을 뺏다면 성능저하가 심했을 것이다.)  
>마지막으로 '0'인 칸의 갯수만큼 줄을 내려준다.  
>(여기서 나는 처음에 else break를 넣지 않아서 중간에 문자가 존재하는데도 위에 있는 '0'의 총 갯수만큼 칸을 내렸다가 문제를 틀렸었다.)

## 다시 생각해볼 점
> 이런 문제를 풀 때는 구현을 차근차근해 나가야하기 때문에 필요한 기능단위로 함수화 시키는 것이 중요한 것 같다.  
> 이렇게 기능들을 함수화시켜서 나열하면 나중에 로직이 틀렸을 때도 해당 부분만 쉽게 고칠 수 있다.    
> 또한 상기했던 else break를 빼먹었던 것처럼 아주 사소한 부분을 놓치면 코드 전체가 틀리게 된다.     
> 코드의 전체 작성이 끝난 후 로직대로 진행되는지 한번 검토할 필요성이 있다.