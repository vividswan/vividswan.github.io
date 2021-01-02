---
title: "[Programmers] 자물쇠와 열쇠(2020 KAKAO BLIND RECRUITMENT)"
tags:
  - Programmers
date: 2021-01-02T08:06:00-05:00
key: "[Programmers] 자물쇠와 열쇠(2020 KAKAO BLIND RECRUITMENT)"
---

## [Programmers] 자물쇠와 열쇠(2020 KAKAO BLIND RECRUITMENT) JAVA

<!--more-->

문제 : [자물쇠와 열쇠(2020 KAKAO BLIND RECRUITMENT)](https://programmers.co.kr/learn/courses/30/lessons/60059)<br>


## 문제 설명

크기가 `N*N`인 자물쇠와 크기가 `M*M`인 열쇠가 주어집니다.<br>
자물쇠와 열쇠는 각각 홈인 부분과 돌기인 부분이 있습니다.<br>
이때 자물쇠보다 크기가 항상 작거나 같은 열쇠를 90도씩 회전하거나 이동시켜서 자물쇠와 열쇠 서로의 홈과 돌기 부분을 맞추는 것이 문제입니다.<br>
solution 메서드에서 맞출 수 있는 경우의 수가 있다면 `true`, 맞출 수 있는 경우의 수가 존재하지 않는다면 `false`를 `return` 해야 합니다.<br>

---

## Solution

### 완전 탐색

만약 어떠한 방법을 써도 맞출 수 없다면 false를 return해야 하기 때문에 모든 경우의 수를 확인해야 합니다.<br>
또 한, 자물쇠의 최대 크기가 20*20(400)으로 충분히 작은 크기이므로 완전 탐색으로 모든 경우의 수를 살펴봐야 합니다.<br>
자물쇠의 크기를 3배 확대한 map을 만든 뒤 가운데에 원래의 자물쇠의 숫자를 넣어줍니다.<br>
그 후 열쇠의 맵을 90도씩 4번 회전하며 한 칸 한 칸 확인하면 모든 경우의 수를 확인할 수 있습니다.<br>
이때 홈은 0, 돌기 부분은 1이므로 확장된 자물쇠에 열쇠 값을 더했을 때, 가운데에 있는 자물쇠 크기의 배열의 모든 수가 1이라면 열쇠로 자물쇠가 열리는 경우입니다.<br>


---

## Description

- 가운데 자물쇠 배열의 값이 모두 1인지 확인하는 것은 `isSuccsess` 메서드를 이용했습니다.
- 90도씩 회전시키는 것은 `rotMap` 메서드에서 구현했습니다.

---

```java
class Solution {

    public static boolean isSuccsess(int[][] map){
        int size= map.length/3;
        boolean result = true;
        for(int i=size;i<2*size;i++){
            for(int j=size;j<2*size;j++){
                if (map[i][j]!=1) result=false;
            }
        }
        return result;
    }

    public static int[][] rotMap(int[][] map){
        int mapSize = map.length;
        int[][] returnMap = new int[mapSize][mapSize];
        for(int i=0;i<mapSize;i++){
            for(int j=0;j<mapSize;j++){
                returnMap[i][j] = map[mapSize-j-1][i];
            }
        }
        return returnMap;
    }

    public boolean solution(int[][] key, int[][] lock) {
        int m = key.length;
        int n = lock.length;

        int[][] map = new int[n*3][n*3];
        for(int i=0;i<n;i++){
            for(int j=0;j<n;j++){
                map[n+i][n+j] = lock[i][j];
            }
        }

        for(int z=0;z<4;z++){
            key = rotMap(key);
            for(int x=0;x<2*n;x++){
                for(int y=0;y<2*n;y++){
                    for(int i=0;i<m;i++){
                        for(int j=0;j<m;j++){
                            map[x+i][y+j]+=key[i][j];
                        }
                    }
                    if(isSuccsess(map)) return true;
                    for(int i=0;i<m;i++){
                        for(int j=0;j<m;j++){
                            map[x+i][y+j]-=key[i][j];
                        }
                    }
                }
            }
        }

        return false;
    }
}
```