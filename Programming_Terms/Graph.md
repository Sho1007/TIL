# 그래프 간선의 종류
* 트리 간선 (Tree edge)
    * 정점 v 가 간선 (u,v)를 통해 처음 발견되었다면, (u,v)는 트리 간선이다.
    * 간단하게는, 스패닝 트리에 포함된 간선을 말한다.

* 순행 간선 (Forward edge)
    * 선조에서 자손으로 연결되는 간선

* 교차 간선 (Cross edge)
    * 서로 다른 스패닝 트리에 있는 두 정점 간의 간선

* 역행 간선 (Back edge)
    * 스패닝 트리의 자손에서 선조로 연결되는 간선
    * 방향 그래프에서 자기 순환 간선들은 역행 간선으로 간주한다.
    * 역행 간선은 해당 DFS의 사이클 유무와 동치이다.

# DFS(Depth First Search)
	깊이 우선 탐색 알고리즘
* 모든 노드를 방문하고자 할 때 사용
* BFS 보다 좀 더 간단함
* 단순 검색 속도는 BFS 보다 느림
* 재귀 알고리즘 형태



## DFS Spanning Tree
	그래프에서 DFS를 어느 정점 u에 대해 실행했을 때, u를 루트로 하는 트리



