---
layout: post
title: "[프로그래머스] 블록 이동하기 - 2020 KAKAO BLIND RECRUITMENT[7/7]"
tags: 알고리즘 프로그래머스 카카오기출
---

## 문제
[프로그래머스 - 블록 이동하기](https://programmers.co.kr/learn/courses/30/lessons/60063)

---
## 설명
2x1 크기의 로봇이 주어졌을 때, 이동명령 및 회전명령을 최소로 사용하여 로봇을 출발지점에서 도착지점까지 이동시키는 문제다.

---
## 풀이
최소 거리를 구하는 문제이므로 BFS를 이용하여 문제를 풀이하였다. 

로봇의 위치와 방향을 이용하여 로봇의 위치정보를 저장하였다. 

코드의 수를 줄이기 위해 로봇의 축을 기준으로만 회전을 하였다. 하지만 이렇게 할 경우 축이 다르지만 위치가 같은 경우를 고려하지 않기 때문에 올바를 답이 나오지 않았다. 큐에 로봇의 다음 위치를 저장할 때 축이 다르고 위치정보가 같은 경우를 함께 저장해 문제를 해결했다.

---
## 코드
```java
import java.util.*;

class Solution {

    static class Pos {
        int row, col, dir;
        public Pos(int row, int col, int dir) {
            this.row = row;
            this.col = col;
            this.dir = dir;
        }

        public Pos other() {
            return new Pos(row + dr[dir], col + dc[dir], (dir + 2) % 4);
        }

        public boolean inside() {
            Pos p = this.other();
            return (row >= 0 && col >= 0 && row < N && col < N)
                && (p.row >= 0 && p.col >= 0 && p.row < N && p.col < N);
        }

        public boolean empty() {
            Pos p = this.other();
            return map[row][col] + map[p.row][p.col] == 0;
        }

        public boolean isDest() {
            Pos p = this.other();
            return (row == N-1 && col == N-1) 
                || (p.row == N-1 && p.col == N-1);
        }
    }

    static int N;
    static int[][] map;
    static int[] dr = { -1, 0, 1, 0 };
    static int[] dc = { 0, 1, 0, -1 };

    public int solution(int[][] board) {

        N = board.length;
        map = board;

        int time = 0;
        Queue<Pos> q = new LinkedList<Pos>();
        boolean[][][] visit = new boolean[N][N][4];
        q.offer(new Pos(0, 0, 1));
        q.offer(new Pos(0, 1, 3));
        visit[0][0][1] = true;
        visit[0][1][3] = true;

        while(!q.isEmpty()) {
            time++;
            int size = q.size();
            while(size-- > 0) {
                Pos cur = q.poll();
                int row = cur.row;
                int col = cur.col;
                int dir = cur.dir;
                for(int i=0; i<4; i++) {
                    int nr = row + dr[i];
                    int nc = col + dc[i];
                    Pos next = new Pos(nr, nc, dir);
                    if(!next.inside() || !next.empty() || visit[nr][nc][dir]) {
                        continue;
                    }
                    if(next.isDest()) {
                        return time;
                    }

                    q.offer(new Pos(nr, nc, dir));
                    visit[nr][nc][dir] = true;

                    Pos o = next.other();
                    if(!visit[o.row][o.col][o.dir]) {
                        q.offer(o);
                        visit[o.row][o.col][o.dir] = true; 
                    }
                }

                int[] list;
                if(dir % 2 == 1) {
                    list = new int[] { 0, 2 };
                } else {
                    list = new int[] { 1, 3 };
                }

                for(int nd: list) {
                    Pos p = new Pos(row + dr[nd], col + dc[nd], dir);
                    if(p.inside() && p.empty() && !visit[row][col][nd]) {
                        if(p.isDest()) {
                            return time;
                        }
                        q.offer(new Pos(row, col, nd));
                        visit[row][col][nd] = true;
                    }
                }
            }
        }
        return time;
    }
}
```