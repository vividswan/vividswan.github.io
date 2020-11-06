---
title: "[AOJ] BOARDCOVER"
tags:
  - BOJ
date: 2020-11-06T08:06:00-05:00
key: "[AOJ] BOARDCOVER"
---

## [ALGOSPOT Online Judge(AOJ)] BOARDCOVER C++

<!--more-->

문제 : [AOJ BOARDCOVER](https://www.algospot.com/judge/problem/read/BOARDCOVER)<br>

## 문제 설명

테스트 케이스마다 H*W 크기의 게임판이 주어집니다.<br>
게임판의 각 자라니는 검은 칸이나 흰 칸으로 되어있는데, L자 모양의 블록으로 흰 칸이 안 남도록 채워야 합니다.<br>
L자 모양의 블록을 자유롭게 회전할 수 있지만, 블록끼리 겹치거나 검은 칸을 덮으면 안 됩니다.<br>
이때 L자 모양의 블록으로 모든 흰 칸을 채울 수 있는 경우의 수를 구하는 문제입니다.<br>

---

## Solution

### 완전 탐색

게임판의 가장 위쪽, 왼쪽의 판부터 시작해서 오른쪽, 아래쪽으로 내려가며 완전 탐색으로 해결해야 합니다.<br>
게임판의 한 칸을 기준 삼아 순회하면서 L자 모양의 블록이 그 칸을 기준으로 회전할 수 있는 네 가지 경우를 넣어주며 경우의 수를 계산합니다.<br>
H, W가 최대 20이므로 400칸이 나오고 이를 3으로 나누면 경우의 수가 크게 느껴지지만, 하나의 L자 블록을 넣으면 단순한 칸 수 보다 넣을 수 있는 경우의 수가 더 줄어들므로 더 적은 경우의 수로 해결할 수 있습니다.<br>

---

## Description

- `nextMove` 배열을 이용하여 L자 블록이 회전할 수 있는 네 가지 경우를 계산했습니다.<br>
- `vector<vector<int>>& map`로 배열을 함수에 직접 넣어줬습니다.<br>
- `isOk`의 `value`가 1인 경우는 L자 블록을 추가하는 경우, -1인 경우는 블록을 삭제하는 경우입니다.<br>

---

```cpp
#include <iostream>
#include <vector>
using namespace std;
vector<vector<int>> map;
int nextMove[4][3][2] = {{{0,0},{1,0},{1,1}},{{0,0},{1,0},{1,-1}},{{0,0},{0,1},{1,1}},{{0,0},{0,1},{1,0}}};
bool isOk(vector<vector<int>>& map, int y, int x, int type ,int value );
int chkMap(vector<vector<int>>& map);
int c, h, w;
int main(){
    cin >> c;
    while(c--){
        map.clear();
        cin >> h >> w;
        map.resize(h+1);
        for(int i=1;i<=h;i++){
            map[i].push_back(0);
            for(int j=1; j<=w;j++){
                char nowLot;
                cin >> nowLot;
                if(nowLot=='#') map[i].push_back(1);
                else map[i].push_back(0);
            }
        }
        int result = chkMap(map);
        cout << result << '\n';
    }
    return 0;
}
bool isOk(vector<vector<int>>& map, int y, int x, int type ,int value ){
    bool isOk = true;
    for(int i=0;i<3;i++){
        int ny = y + nextMove[type][i][0];
        int nx = x + nextMove[type][i][1];
        if(ny<1||nx<1||ny>h||nx>w) isOk = false;
        else if((map[ny][nx]+=value) > 1) isOk = false;
    }
    return isOk;
}
int chkMap(vector<vector<int>>& map){
    int y = -1;
    int x = -1;
    for(int i=1;i<=h;i++){
        for(int j=1;j<=w;j++){
            if(map[i][j]==0){
                y=i;
                x=j;
                break;
            }
        }
        if(y!=-1) break;
    }
    if(y ==-1) return 1;
    int ret = 0;
    for(int i=0;i<4;i++){
        if(isOk(map, y,x,i,1)) ret += chkMap(map);
        isOk(map,y,x,i,-1);
    }
    return ret;
}
```