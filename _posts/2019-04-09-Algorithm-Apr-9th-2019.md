---
title: "Algorithm April 9th 2019"
header:
  teaser: /assets/images/algorithm-logo.png
  overlay_image: /assets/images/algorithm-logo-horizon.png
  overlay_filter: 0.2
categories:
  - Algorithm
tags:
  - Swift
  - Algorithm
---



> [problem 1260 DFS BFS](https://www.acmicpc.net/problem/1260)

```swift
var N = 0 // number of vertex
var M = 0 // number of edge
var V = 0 // start vertex
var visited: [Int] = Array(repeating: 0, count: 1001)
var connected: [[Bool]] = Array(repeating: Array(repeating: false, count: 1001), count: 1001)

func readData() {
    let line = readLine()!.components(separatedBy: " ").map({ Int($0)! })
    N = line[0]
    M = line[1]
    V = line[2]
    
    for _ in 1...M {
        let line = readLine()!.components(separatedBy: " ").map({ Int($0)! })
        let v1 = line[0]
        let v2 = line[1]
        connected[v1][v2] = true
        connected[v2][v1] = true
        // there is no direction
    }
}

func DFS(_ vertex: Int) {
    visited[vertex] = 1
    print(vertex, terminator: " ")
    
    for i in 1...N {
        if visited[i] == 0 && connected[vertex][i] {
            DFS(i)
        }
    }
}

func BFS(_ vertex: Int) {
    var queue = [Int]()
    queue.append(vertex)
    visited[vertex] = 1
    
    while !queue.isEmpty {
        let vertex = queue.removeFirst()
        print(vertex, terminator: " ")
        
        for i in 1...N {
            if visited[i] == 0 && connected[vertex][i] {
                queue.append(i)
                visited[i] = 1
            }
        }
    }
}

func solution() {
    DFS(V)
    visited = Array(repeating: 0, count: 1001)
    print("")
    BFS(V)
}

readData()
solution()
```



### DFS : Depth-first Search

특정 목적지까지 갈 수 있는 길의 개수가 대표적인 예이다.
재귀함수를 이용하여 푸는 것이 좋다.



방향이 없는 그래프이기 때문에 2차원 배열 connected에서 대칭으로 연결됐음을 표시해준다.

```swift
func readData() {
    ...
    
    for _ in 1...M {
        let line = readLine()!.components(separatedBy: " ").map({ Int($0)! })
        let v1 = line[0]
        let v2 = line[1]
        connected[v1][v2] = true
        connected[v2][v1] = true
        // there is no direction
    }
}
```



DFS, BFS 모두 완전탐색임을 생각하며 모든 정점을 검사하도록 한다.
for문을 사용하여 전체 탐색을 하였다.

```swift
func DFS(_ vertex: Int) {
    visited[vertex] = 1
    print(vertex, terminator: " ")
    
    for i in 1...N {
        if visited[i] == 0 && connected[vertex][i] {
            DFS(i)
        }
    }
}
```



### BFS: Breadth-first Search

너비우선 탐색은 최단거리를 찾을 때에 적합하다.
queue를 이용하여 탐색한다.

queue에 방문할 노드를 집어넣고 dequeue를 하여 방문 -> dequeue한 노드가 방문할 수 있는 노드들을 다시 enqueue하는 방식으로하여 queue가 empty가 될 때까지 진행한다.

while - for - if 

```swift
func BFS(_ vertex: Int) {
    var queue = [Int]()
    queue.append(vertex)
    visited[vertex] = 1
    
    while !queue.isEmpty {
        let vertex = queue.removeFirst()
        print(vertex, terminator: " ")
        
        for i in 1...N {
            if visited[i] == 0 && connected[vertex][i] {
                queue.append(i)
                visited[i] = 1
            }
        }
    }
}
```



> [problem 2178 미로 탐색](https://www.acmicpc.net/problem/2178)

BFS로 접근해야하는 문제이며 4 방향으로 움직일 수 있다.
input 받는 것부터 조금 까다롭다.

마찬가지로 BFS 이기 때문에 queue를 이용하며

while+dequeue(방문) - for(탐색) - if+enqueue(방문할 노드 고르기) 을 생각하자.

```swift
import Foundation

var N = 0
var M = 0

var path = [[Bool]]()
var accumulationCount = [[Int]]()

func readData() {
    let line = readLine()!.components(separatedBy: " ").map({ Int($0)! })
    N = line[0]
    M = line[1]
    
    path = Array(repeating: Array(repeating: false, count: M + 1), count: N + 1)
    accumulationCount = Array(repeating: Array(repeating: 0, count: M + 1), count: N + 1)
    
    for i in 1...N {
        let mazeLine = Array(readLine()!).map({ Int(String($0))! })
        for j in 0..<M {
            if mazeLine[j] == 1 {
                path[i][j + 1] = true
            }
        }
    }
}

var dx = [-1, 1, 0, 0]
var dy = [0, 0, -1, 1]

func solution(x: Int, y: Int) {
    var queue = [(Int, Int)]()
    queue.append((x, y))
    accumulationCount[x][y] = 1
    
    while !queue.isEmpty {
        let coordinate = queue.removeFirst()
        let cur_x = coordinate.0
        let cur_y = coordinate.1
        
        for i in 0..<4 {
            let next_x = cur_x + dx[i]
            let next_y = cur_y + dy[i]
            if next_x >= 1 && next_y >= 1 && next_x <= N && next_y <= M {
                if accumulationCount[next_x][next_y] == 0 && path[next_x][next_y] {
                    queue.append((next_x, next_y))
                    accumulationCount[next_x][next_y] = accumulationCount[cur_x][cur_y] + 1
                }
            }
        }
    }
    print(accumulationCount[N][M])
}

readData()
solution(x: 1, y: 1)
```

