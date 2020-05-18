---
layout: post
title: "[프로그래머스] 기둥과 보 설치 - 2020 KAKAO BLIND RECRUITMENT[5/7]"
tags: 알고리즘 프로그래머스 카카오기출
---

## 문제
[프로그래머스 - 기둥과 보 설치](https://programmers.co.kr/learn/courses/30/lessons/60061)

---
## 설명
가사의 단어와 쿼리가 주어졌을 때, 쿼리에 맞는 단어가 몇개인지 구하는 문제다.

---
## 풀이
해싱과 이분탐색을 이용하여 문제를 풀이하였다.

 1. 문제의 길이가 최대 10000이기 때문에, 문자열을 길이별로 저장한다. 
 2. 이분탐색을 이용해 lower bound와 upper bound를 검색할 수 있는 단어의 개수를 찾는다.

주의할점은 쿼리의 시작이 ?라면 이분탐색을 진행할 수 없기 때문에, word와 쿼리를 모두 뒤집에서 이분탐색을 진행한다. ?가 알파벳 사이에 오는 경우가 없기 때문에 가능하다.

이 문제는 일반적으로 트라이로 해결 가능하다고 한다. 다음에 트라이를 이용한 풀이도 올려봐야겠다.

---
## 코드
```java
import java.util.*;

class Solution {

    static class Info implements Comparable<Info> {
        int x, y, t;
        public Info(int x, int y, int t) {
            this.x = x;
            this.y = y;
            this.t = t;
        }
        @Override
        public int compareTo(Info i) {
            if(x == i.x) {
                if(y == i.y) {
                    return Integer.compare(t, i.t);
                }
                return Integer.compare(y, i.y);
            }
            return Integer.compare(x, i.x);
        }
    }

    static int N;
    static boolean[][][][] map;

    static PriorityQueue<Info> pq = new PriorityQueue<Info>();

    public int[][] solution(int n, int[][] build_frame) {

        N = n + 2;
        map = new boolean[N+2][N+2][N+2][N+2];

        for(int[] frame: build_frame) {
            int x = frame[0] + 1;
            int y = frame[1];
            int type = frame[2];
            int command = frame[3];
            if(command == 0) { // 제거
                if(type == 0) { // 기둥 
                    map[x][y][x][y+1] = false;
                    if(!valid(x, y, type)) {
                        map[x][y][x][y+1] = true;
                    }
                } else {
                    map[x][y][x+1][y] = false;
                    if(!valid(x, y, type)) {
                        map[x][y][x+1][y] = true;
                    }
                }
            } else {
                if(canInstall(x, y, type)) {
                    pq.add(new Info(x, y, type));
                    if(type == 0) {
                        map[x][y][x][y+1] = true;
                    } else {
                        map[x][y][x+1][y] = true;
                    }
                }
            }
        }

        Queue<Info> q = new LinkedList<Info>();
        while(!pq.isEmpty()) {
            Info info = pq.poll();
            if(info.t == 0) {
                if(map[info.x][info.y][info.x][info.y+1]) {
                    q.offer(info);
                    map[info.x][info.y][info.x][info.y+1] = false;
                }
            } else {
                if(map[info.x][info.y][info.x+1][info.y]) {
                    q.offer(info);
                    map[info.x][info.y][info.x+1][info.y] = false;
                }
            }
        }

        int[][] answer = new int[q.size()][3];
        int idx = 0;
        while(!q.isEmpty()) {
            Info info = q.poll();
            answer[idx++] = new int[] { info.x - 1, info.y, info.t };
        }

        return answer;
    }

    private boolean valid(int x, int y, int type) {
        for(Info i: pq) {
            if(i.t == 0) {
                if(!map[i.x][i.y][i.x][i.y+1]) {
                    continue;
                }
            } else {
                if(!map[i.x][i.y][i.x+1][i.y]) {
                    continue;
                }
            }
            if(!canInstall(i.x, i.y, i.t)) {
                return false;
            }
        }
        return true;
    }

    public boolean canInstall(int x, int y, int t) {
        if(t == 0) { // 기둥 
            if(isBottom(y) || hasVer(x, y-1) || hasHor(x-1, y) || hasHor(x, y)) {
                map[x][y][x][y+1] = true;
                return true;
            }
        } else {
            if(hasVer(x, y-1) || hasVer(x+1, y-1) || (hasHor(x-1, y) && hasHor(x+1, y))) {
                map[x][y][x+1][y] = true;
                return true;
            }
        }
        return false;
    }

    public boolean isBottom(int y) {
        return y == 0;
    }

    public boolean hasHor(int x, int y) {
        return map[x][y][x+1][y];
    }

    public boolean hasVer(int x, int y) {
        return map[x][y][x][y+1];
    }
}
```