---
layout: post
title: "[프로그래머스] 자물쇠와 열쇠 - 2020 KAKAO BLIND RECRUITMENT[3/7]"
tags: 알고리즘 프로그래머스 카카오기출
---

## 문제
[프로그래머스 - 자물쇠와 열쇠](https://programmers.co.kr/learn/courses/30/lessons/60059)

---
## 설명
N * N 크기의 자물쇠와 M * M 크기의 열쇠가 주어졌을 때, 열쇠를 이동하고 회전하여 자물쇠를 열 수 있는지 확인하는 문제다.

---
## 풀이
비교를 쉽게 하기 위해서 N + M * 2 크기의 2차원 배열을 만들어 자물쇠를 중앙에 배치시켰다. 자물쇠와 열쇠를 더했을 때 자물쇠의 영역이 모두 1이 되면 자물쇠가 열린다.

---
## 코드
```java
class Solution {

    static int N, M, K;
    static int[][] plock;

    public boolean solution(int[][] key, int[][] lock) {
        M = key.length;
        K = lock.length;
        N = K + (M * 2);
        plock = new int[N][N];
        for(int i=0; i<K; i++) {
            for(int j=0; j<K; j++) {
                plock[i+M][j+M] = lock[i][j];
            }
        }
        return solve(key);
    }

    public boolean solve(int[][] key) {
        for(int k=0; k<4; k++) {
            for(int i=0; i<N-M; i++) {
                for(int j=0; j<N-M; j++) {
                    add(key, i, j, 1);
                    if(unlocked()) {
                        return true;
                    }
                    add(key, i, j, -1);
                }
            }
            key = rotate(key);
        }
        return false;
    }

    public void add(int[][] key, int di, int dj, int sign) {
        for(int i=0; i<M; i++) {
            for(int j=0; j<M; j++) {
                plock[i+di][j+dj] += key[i][j] * sign;
            }
        }
    }

    public boolean unlocked() {
        for(int i=0; i<K; i++) {
            for(int j=0; j<K; j++) {
                if(plock[i+M][j+M] != 1) {
                    return false;
                }
            }
        }
        return true;
    }

    public int[][] rotate(int[][] key) {
        int[][] next = new int[M][M];
        for(int i=0; i<M; i++) {
            for(int j=0; j<M; j++) {
                next[j][M-i-1] = key[i][j];
            }
        }
        return next;
    }

}
```