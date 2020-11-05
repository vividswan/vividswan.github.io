---
title: "[AOJ] PICNIC"
tags:
  - BOJ
date: 2020-11-05T08:06:00-05:00
key: "[AOJ] PICNIC"
---

## [ALGOSPOT Online Judge(AOJ)] PICNIC C++

<!--more-->

문제 : [AOJ PICNIC](https://www.algospot.com/judge/problem/read/PICNIC)<br>

## 문제 설명

n 명의 학생이 주어지고, m 개의 가능한 짝의 수가 주어집니다.<br>
m 개로 주어진 조건에 맞게 짝을 지을 수 있는 모든 경우의 수를 구하는 문제입니다.<br>
가능하다고 주어지지 않은 학생들 끼린 짝이 될 수 없고, 모든 학생들은 각자의 짝이 있어야 합니다.<br>

---

## Solution

### 완전 탐색

학생의 수가 최대 10명으로 완전 탐색으로 모든 경우의 수를 확인하여 풀 수 있습니다.<br>
최악의 경우 최대 학생 수 10명이 자신을 제외한 9명과 모두 짝이 될 수 있는 경우인데, 이 경우는 `9*7*5*3*1`이므로 시간제한 안에 해결할 수 있습니다.<br>
해당 학생이 짝을 찾았는지의 유무인 `isFind` 배열을 만들어 이 배열로 재귀를 계속 돌며 결과를 확인해 주는 방식으로 해결했습니다.<br>

---

## Description

- 테스트케이스의 수인 c가 주어지므로, 새로운 테스트 케이스마다 초기화를 해주어야 합니다.<br>
- <string.h>의 memset() 함수를 통해 테스트 케이스마다 초기화를 했습니다.<br>

---

```cpp
#include <iostream>
#include <cstring>
using namespace std;
bool isFriend[10][10];
bool isFind[10];
int c, n, m;
int recursion(bool isFind[10]);
int main(){
    cin >> c;
    while(c--){
        memset(isFriend,false,sizeof(isFriend));
        memset(isFind,false,sizeof(isFind));
        cin >> n >> m;
        for(int i=1;i<=m;i++){
            int st, ed;
            cin >> st >> ed;
            isFriend[st][ed] = true;
            isFriend[ed][st] = true;
        }
        int result = recursion(isFind);
        cout << result << '\n';
    }
}
int recursion(bool isFind[10]){
    int firstSearch = - 1;
    for(int i=0;i<n;i++){
        if(!isFind[i]){
            firstSearch = i;
            break;
        }
    }
    if(firstSearch== -1) return 1;
    int ret = 0;
    for(int i=firstSearch+1;  i<n; i++){
        if(!isFind[i] && isFriend[firstSearch][i]){
            isFind[firstSearch] = true;
            isFind[i] = true;
            ret += recursion(isFind);
            isFind[firstSearch] = false;
            isFind[i]=false;
        }
    }
    return ret;
}
```