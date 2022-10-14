# 1260 DFS와 BFS (c++)

## 문제
https://www.acmicpc.net/problem/1260

## 접근 방식
구현, BFS, DFS, 정렬
1. 노드를 입력받고
2. 노드의 인접노드를 정렬
3. 시작노드를 기준으로 DFS, BFS 하며 순서 출력 (visisted 배열 초기화 필요)

## 문제 풀이
```c++
#include <iostream>
#include <vector>
#include <algorithm>
#include <queue>

#define endl '\n'

using namespace std;

int N, M, V;

bool visited[1'001] = {};

struct Node
{
    vector<int> adj;
};

vector<Node> v;

void Print()
{
    for (int i = 1; i <= N; ++i)
    {
        cout << i << " : ";
        for (int j = 0; j < v[i].adj.size(); ++j)
        {
            cout << v[i].adj[j] << " ";
        }
        cout << endl;
    }
}

void DFS(int start)
{
    if (visited[start]) return;
    visited[start] = true;
    cout << start << " ";

    for (int i = 0; i < v[start].adj.size(); ++i)
    {
        DFS(v[start].adj[i]);
    }
}

void BFS(int start)
{
    queue<int> q;
    q.push(start);

    while (!q.empty())
    {
        int now = q.front();
        q.pop();

        if (visited[now]) continue;
        visited[now] = true;
        cout << now << " ";
        for (int i = 0; i < v[now].adj.size(); ++i)
        {
            q.push(v[now].adj[i]);
        }
    }
    cout << endl;
}

int main()
{
    ios::sync_with_stdio(0); cin.tie(0); cout.tie(0);

    cin >> N >> M >> V;

    v = vector<Node>(N + 1);

    for (int i = 0; i < M; ++i)
    {
        int num1, num2;
        cin >> num1 >> num2;

        v[num1].adj.push_back(num2);
        v[num2].adj.push_back(num1);
    }

    for (int i = 1; i <= N; ++i)
    {
        sort(v[i].adj.begin(), v[i].adj.end());
    }

    DFS(V);
    cout << endl;
    fill(visited, visited + 1'001, 0);
    BFS(V);

    return 0;
}
```

## 다시 생각해볼 점
1. 인접노드를 정렬하는 과정이 필요했다. (번호가 작은 순으로 방문하라는 조건)
2. visited 노드를 초기화 시켜줘야했다. (DFS 후 BFS도 실행해야 하므로)
3. 노드에 따로 int num을 저장할 필요는 없었다. 위치 = 번호였기 때문
4. 참조 없이 생각나는데로 구현했는데 통과했다. BFS 와 DFS 를 잘기억하고 있는 것 같다.
