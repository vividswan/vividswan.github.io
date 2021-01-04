---
title: "[Programmers] 기둥과 보 설치(2020 KAKAO BLIND RECRUITMENT)"
tags:
  - Programmers
date: 2021-01-05T08:06:00-05:00
key: "[Programmers] 자물쇠와 열쇠(2020 KAKAO BLIND RECRUITMENT)"
---

## [Programmers] 기둥과 보 설치(2020 KAKAO BLIND RECRUITMENT) JAVA

<!--more-->

문제 : [기둥과 보 설치(2020 KAKAO BLIND RECRUITMENT)](https://programmers.co.kr/learn/courses/30/lessons/60061)<br>


## 문제 설명

`N*N`의 벽면이 주어지고 이 벽면에 기둥과 보를 설치, 삭제하는 문제입니다.<br>
기둥은 다른 기둥의 위, 혹은 보의 양 끝 쪽에 설치될 수 있습니다.<br>
보는 양 끝 점 중 한점에 기둥이 있거나, 양 끝 점에 보가 설치되어 있을 시 설치될 수 있습니다.<br>
입력으로 원하는 기둥과 보의 설치, 삭제 유무와 좌표가 주어지며, 출력으로 설치, 삭제가 가능할 때 실행한 뒤의 최종 결과를 출력하는 것이 문제입니다.<br>

---

## Solution

### 구현, 완전 탐색

입력값을 받은 뒤, 이 입력값을 최종 배열에 삽입합니다.<br>
그 후 삽입한 결과(삭제나 설치)가 기둥과 보의 규칙을 위반하지 않는다면 최종 배열에 그대로 두고, 규칙을 위반한다면 다시 설치나 삭제를 해줘야 합니다.<br>
규칙을 지키는 것에 대한 확인은 최종 배열을 완전 탐색하면서 확인해야 합니다.<br>
최종 배열에 남은 기둥과 보를 문제의 규칙대로 정렬한 뒤 2차원 배열에 차례대로 삽입해서 return 해줍니다.<br>

---

## Description

- 양쪽의 보의 설치 유무는 두 가지 boolean 변수 le,ri로 파악했습니다.
- 각각의 기둥과 보는 ArrayList를 만들고 세 가지 변수를 넣어 만들었습니다. (x좌표, y좌표, 보인지 기둥인지)

---

## 소스코드

```java
import java.util.ArrayList;
import java.util.Collections;

class Node implements Comparable<Node>{
    private int x;
    private int y;
    private  int article;

    public int getX() {
        return x;
    }

    public int getY() {
        return y;
    }

    public int getArticle() {
        return article;
    }

    public Node(int x, int y, int article){
        this.x = x;
        this.y = y;
        this.article = article;
    }

    @Override
    public int compareTo(Node other) {
        if(this.x==other.x && this.y == other.y) return Integer.compare(this.article,other.article);
        else if(this.x == other.x) return Integer.compare(this.y, other.y);
        else return Integer.compare(this.x, other.x);
    }
}

class Solution {

    public static boolean chkPossible(){
        for(int i=0;i<result.size();i++){
            ArrayList<Integer> nowNode = result.get(i);
            int x = nowNode.get(0);
            int y = nowNode.get(1);
            int a = nowNode.get(2);

            if(a==0){
                boolean chk = false;
                if(y==0) chk = true;
                for(int j=0; j<result.size();j++){
                    ArrayList<Integer> otherNode = result.get(j);
                    int otherX = otherNode.get(0);
                    int otherY = otherNode.get(1);
                    int otherA = otherNode.get(2);
                    if(otherA==0){
                        if(otherX==x && otherY==y-1) chk = true;
                    }
                    else {
                        if(otherX==x && otherY==y) chk = true;
                        else if(otherX==x-1 && otherY==y) chk = true;
                    }
                }
                if(chk==false) return false;
            }
            else if(a==1){
                boolean chk = false;
                boolean le = false;
                boolean ri = false;
                if(y==0) chk = true;
                for(int j=0; j<result.size();j++){
                    ArrayList<Integer> otherNode = result.get(j);
                    int otherX = otherNode.get(0);
                    int otherY = otherNode.get(1);
                    int otherA = otherNode.get(2);
                    if(otherA==0){
                        if(otherX==x && otherY == y-1) chk = true;
                        if(otherX==x+1 && otherY == y-1) chk = true;
                    }
                    else {
                        if(otherX==x-1 && otherY == y) le = true;
                        if(otherX==x+1 && otherY == y) ri = true;
                    }
                }
                if(le&&ri) chk = true;
                if(chk==false) return false;
            }
        }
        return true;
    }

    public static ArrayList<ArrayList<Integer>> result = new ArrayList<ArrayList<Integer>>();

    public int[][] solution(int n, int[][] build_frame) {

        for(int i=0;i<build_frame.length;i++){
            int x = build_frame[i][0];
            int y = build_frame[i][1];
            int a = build_frame[i][2];
            int b = build_frame[i][3];

            if(b==1){
                ArrayList<Integer> nowNode = new ArrayList<>();
                nowNode.add(x);
                nowNode.add(y);
                nowNode.add(a);
                result.add(nowNode);
                if(!chkPossible()) result.remove(result.size()-1);
            }
            else if(b==0){
                int idx = 0;
                for(int j=0;j<result.size();j++){
                    if(x==result.get(j).get(0)&&y==result.get(j).get(1)&&a==result.get(j).get(2)){
                        idx = j;
                        break;
                    }
                }
                ArrayList<Integer> nowNode = result.get(idx);
                result.remove(idx);
                if(!chkPossible()) result.add(nowNode);
            }


        }

        ArrayList<Node> list = new ArrayList<>();

        for(int i = 0; i<result.size();i++){
            list.add(new Node(result.get(i).get(0),result.get(i).get(1),result.get(i).get(2)));
        }

        Collections.sort(list);

        int[][] answer = new int[list.size()][3];
        for(int i=0;i<list.size();i++){
            answer[i][0] = list.get(i).getX();
            answer[i][1] = list.get(i).getY();
            answer[i][2] = list.get(i).getArticle();
        }
        return answer;
    }
}
```