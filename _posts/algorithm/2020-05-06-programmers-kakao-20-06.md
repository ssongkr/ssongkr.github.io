---
layout: post
title: "[프로그래머스] 외벽점검 - 2020 KAKAO BLIND RECRUITMENT[6/7]"
tags: 알고리즘 프로그래머스 카카오기출
---

## 문제
[프로그래머스 - 외벽점검](https://programmers.co.kr/learn/courses/30/lessons/60062)

---
## 설명
길이가 N인 레스토랑과 상태를 점검해야하는 외벽, 그리고 외벽을 점검할 수 있는 친구들의 이동거리가 주어졌을때, 외벽을 점검할 수 있는 최소 인원을 구하는 문제다.

---
## 풀이
최소 인원을 이용해 외벽을 점검해야하기 때문에, dist 배열을 내림차순으로 정렬하였다.

탐색을 위해 비트마스킹과 메모이제이션을 이용하였다. 메모이제이션 배열에는 현재 점검한 외벽의 상태와 외벽을 점검한 친구의 수가 주어졌을때, 추가적으로 외벽을 점검하기 위해 필요한 친구의 수를 저장한다. 그리고 비트를 이용하여 외벽의 상태를 점검했는지 확인하면 문제를 해결할 수 있다.

이 문제는 비트마스킹을 사용하지 않고 단순 깊이우선탐색을 이용해도 문제를 풀이할 수 있다. 하지만 테스트 케이스가 큰 경우 약 2초의 시간이 걸리기 때문에, 한 방향으로만 검사하는 등 중복을 최소화 해야한다.

---
## 코드
```java
import java.util.*;

class Solution {
    static int n;
    static int[] weak, dist;
    static int[][] cache;

    static final int INF = 987654321;

    public int solution(int k, int[] w, int[] d) {
        n = k;
        weak = w;
        dist = d;
        Arrays.sort(dist);
        dist = reverse(dist);
        cache = new int[dist.length+1][1 << weak.length];
        for(int i=0; i<dist.length+1; i++) {
            Arrays.fill(cache[i], -1);
        }
        int answer = dp(0, 0);
        return answer == INF ? -1 : answer;
    }

    public int dp(int di, int mask) {
        if(cache[di][mask] != -1) {
            return cache[di][mask];
        }
        if(mask == (1 << weak.length) - 1) {
            return cache[di][mask] = 0;
        }
        if(di == dist.length) {
            return INF;
        }

        int ret = INF;
        for(int i=0; i<weak.length; i++) {
            if((mask & (1 << i)) == 0) {
                ret = Math.min(ret, dp(di + 1, mask | bit(i, dist[di])) + 1);
            }
        }
        return cache[di][mask] = ret;
    }

    public int bit(int i, int maxDist) {
        int cur = weak[i];
        int bit = (1 << i);
        while(true) {
            i = (i + 1) % weak.length;
            int next = weak[i];
            int curDist = getDist(cur, next);
            if(cur == next || curDist > maxDist) {
                break;
            }
            bit |= (1 << i);
        }
        return bit;
    }

    public int getDist(int cur, int next) {
        if(next > cur) {
            return next - cur;
        }
        return n - cur + next;
    }

    public int[] reverse(int[] arr) {
        int[] temp = new int[arr.length];
        for(int i=0; i<arr.length; i++) {
            temp[i] = arr[arr.length-1-i];
        }
        return temp;
    }
}
```