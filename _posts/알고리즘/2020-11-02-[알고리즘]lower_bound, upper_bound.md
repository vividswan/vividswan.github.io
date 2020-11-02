---
title: "[알고리즘] lower_bound, upper_bound"
tags:
  - 알고리즘
date: 2020-11-02T08:06:00-05:00
key: "[알고리즘] lower_bound, upper_bound"
---

# lower_bound, upper_bound

<!--more-->

lower_bound, upper_bound를 int 타입과 pair<>에서도 사용해봅시다.<br>

## algorithm 헤더

lower_bound, upper_bound를 사용하기 위해선 `algorithm` 라이브러리가 필요합니다.<br>

> #include <algorithm>

## 선언

> lower_bound(시작 이터레이터, 끝 이터레이터, 지정 값)
<br>

> upper_bound(시작 이터레이터, 끝 이터레이터, 지정 값)
<br>

## 리턴 값

vector에서 lower_bound, upper_bound를 사용할 때의 리 값을 알아봅시다.<br>

> lower_bound<br>

lower_bound는 vector의 시작 이터레이터와 끝 이터레이터 사이에서 `지정한 값 이상`을 가지는 최초의 이터레이터를 반환합니다.<br>

> upper_bound<br>

upper_bound는 vector의 시작 이터레이터와 끝 이터레이터 사이에서 `지정한 값보다 더 큰 값`을 가지는 최초의 이터레이터를 반환합니다.<br>

## 인덱스 반환

vector를 기준으로 첫 이터레이터 값을 빼주면 인덱스가 반환됩니다.<br>

> lower_bound(v.begin(), v.end(), num) - v.begin();<br>

> upper_bound(v.begin(), v.end(), num) - v.begin();<br>

## 예제

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
int main(){
    vector<int> adj= {2,4,6,1,7,8,3,1,5,7,9,3,2,1};
    sort(adj.begin(),adj.end());
    for(int i=0; i<adj.size();i++) cout << adj[i] << ' ';
    // 1 1 1 2 2 3 3 4 5 6 7 7 8 9
    cout << '\n';
    int lowBoundIndex = lower_bound(adj.begin(),adj.end(), 4) - adj.begin();
    int upperBoundIndex = upper_bound(adj.begin(),adj.end(),4) - adj.begin();
    cout << lowBoundIndex << ' ' << upperBoundIndex << '\n';
    // 7, 8
    return 0;
}
```

## pair<int,int>를 적용한 예제

비교의 기준이 될 bool을 반환하는 함수를 만들고 그 함수를 네 번째 인자로 추가하고, 세 번째 인자에는 `make_pair()`로 지정 값을 pair로 선언해 줍니다.<br>

```cpp
#include <iostream>
#include <vector>
#include <algorithm>
using namespace std;
bool comp(const pair<int, int> &a, const pair<int,int> &b) {
    return a.first < b.first;
}
int main(){
    vector<pair<int,int>> adj= {{1,3},{2,5},{2,9},{9,20},{10,25},{5,2},{7,2},{8,9}};
    sort(adj.begin(),adj.end());
    for(int i=0; i<adj.size();i++) cout << adj[i].first << ' ' << adj[i].second << " | ";
    // 1 3 | 2 5 | 2 9 | 5 2 | 7 2 | 8 9 | 9 20 | 10 25 | 
    cout << '\n';
    int lowBoundIndex = lower_bound(adj.begin(),adj.end(), make_pair(5,0),comp) - adj.begin();
    int upperBoundIndex = upper_bound(adj.begin(),adj.end(),make_pair(5,0),comp) - adj.begin();
    cout << lowBoundIndex << ' ' << upperBoundIndex << '\n';
    // 3, 4
    return 0;
}
```