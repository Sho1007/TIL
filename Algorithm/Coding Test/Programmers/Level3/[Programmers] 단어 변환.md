# 단어 변환 (c++)

## 문제
https://school.programmers.co.kr/learn/courses/30/lessons/43163

## 접근 방식
DFS
1. 단어들이 1글자만 다르다면 변환이 가능하다. (인접노드)
2. begin 에서 target 으로의 최소 변환 횟수 (최단거리)

## 문제 풀이
```c++
#include <string>
#include <vector>

#include <map>

using namespace std;

map<string, vector<string>> m;
map<string, int> costMap;

bool CheckDifCount(string a, string b)
{
    int count = 0;
    for (int i = 0; i < a.length(); ++i)
    {
        if (a[i] != b[i])
            count++;
    }
    
    if (count == 1) return true;
    
    return false;
}

void Init(string begin, vector<string>& words)
{
    words.push_back(begin);
    
    for (int i = 0; i < words.size(); ++i)
    {
        costMap[words[i]] = -1;
        
        // words 안의 단어들끼리 등록
        for (int j = i + 1; j < words.size(); ++j)
        {
            if (CheckDifCount(words[i], words[j]))
            {
                m[words[i]].push_back(words[j]);
                m[words[j]].push_back(words[i]);
            }
        }
    }
}

void Calc(string now, int cost)
{
    if (costMap[now] == -1 || costMap[now] > cost)
    {
        costMap[now] = cost;
        for (int i = 0; i < m[now].size(); ++i)
        {
            Calc(m[now][i], cost + 1);
        }
    }
}

int solution(string begin, string target, vector<string> words) {
    
    Init(begin, words);

    Calc(target, 0);
    
    if (costMap[begin] == -1) return 0;
    
    return costMap[begin];
}
```
1. Init에서 words vector에 begin을 포함시키고 인접 노드들을 계산한다.
2. Calc에서 target을 시작노드로 한 거리를 구한다. (각 인접노드들을 순회하면서 더 작은값을 저장시킴)
3. costMap[begin]이 -1 이라면, 변환 불가이므로 0을 리턴, 아니라면 저장된 최소값을 리턴한다.

## 다시 생각해볼 점
처음에 단어의 변환으로 접근했을 때는 어떻게 시작해야할지 막막했다.  
하지만 결국 begin 에서 target 까지의 최단거리를 구하는 문제였고,    
단어의 변환은 인접노드를 구하는 조건이라고 생각하니 문제가 단순해졌다.  
문제가 진짜로 원하는 바가 무엇인지 최대한 단순화 시켜서 생각하는 능력이 필요한 것 같다.