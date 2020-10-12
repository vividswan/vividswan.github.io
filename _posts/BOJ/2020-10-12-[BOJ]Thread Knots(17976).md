---
title: "[BOJ] Thread Knots(17976)"
tags:
  - BOJ
date: 2020-10-12T08:06:00-05:00
key: "[BOJ] Thread Knots(17976)"
---

## [백준(BOJ)] Thread Knots(17976) C++

<!--more-->

문제 : [BOJ_17976번 Thread Knots](https://www.acmicpc.net/problem/17976)<br>

## 문제 설명

### 이분탐색

n 개의 thread가 주어지고, 이 thread 하나하나마다 점을 찍어야 합니다.<br>
이때 점의 x좌표의 간격의 최솟값이 최대가 되는 거리를 구하는 것이 문제입니다.<br>
점의 범위가 0부터 1000000000이기 때문에 완전 탐색으로 점의 간격을 파악하기는 어려우므로, 이분 탐색을 이용해야 합니다.<br>

---

## Solution

우선 thread의 범위를 벡터로 입력받은 뒤, 벡터를 정렬합니다.<br>
순서대로 정렬한 값을 순회하면서 현재 이분 탐색의 mid 값의 거리를 최솟값으로 각 thread마다 점을 찍을 수 있는지 확인합니다.<br>
확인을 위해선 우선 첫 번째 thread의 시작점에 첫 번째 점을 찍어줘야 합니다.<br>
첫 번째 점이 왼쪽의 있을수록 점들 간의 거리의 최댓값이 커질 수 있습니다.<br>
그 후 다음 thread부터 각 thread를 순회합니다.<br>
이때 마지막으로 찍은 점 + mid 값이 다음 thread의 끝 값을 넘어가면 성립하지 않으므로 다시 right 값을 낮추고 다시 이분 탐색합니다.<br>
만약 다음 thread의 끝 값 보다 마지막으로 찍은 점 + mid 값이 작다면, 마지막 점 + mid 값과 다음 thread의 시작 점 중 최댓값의 위치에 다음 점을 찍고 다시 순회합니다.<br>
또 한 가장 큰 점이 1000000000이기 때문에 mid 연산에서 int 값을 넘어갈 수 있으므로 long long형으로 선언해 줍니다.<br>

---

## Description

- 벡터 값은 sort로 정렬했습니다.<br>
- pair를 sort로 정렬 시, pair의 첫 번째 값 기준으로 정렬됩니다.<br>
- 해당 mid 값이 성립하는지는 `isBossible` 함수에서 확인했습니다.<br>

---

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
long long n, le, ri,ans;
vector<pair<long long,long long>> adj;
bool isPossible(long long val);
int main(){
    cin >> n;
    for(int i=0;i<n;i++){
        long long st,ed;
        cin >> st >> ed;
        adj.push_back({st,st+ed});
        ri=max(ri,st+ed);
    }
    sort(adj.begin(),adj.end());
    while(le<=ri){
        long long mid = (le+ri)/2;
        if(isPossible(mid)) {
            ans=mid;
            le =mid+1;
        }
        else ri = mid-1;
    }
    cout << ans << '\n';
    return 0;
}
bool isPossible(long long val){
    long long now = adj[0].first;
    for(int i=1;i<n;i++){
        if(now+val > adj[i].second) return false;
        else {
            now = max(adj[i].first,now+val);
        }
    }
    return true;
}
```
