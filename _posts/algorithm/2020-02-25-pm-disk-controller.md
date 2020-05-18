---
title: "[프로그래머스] 디스크 컨트롤러"
layout: post
tags: 알고리즘 프로그래머스
---

## 문제
[프로그래머스 - 디스크 컨트롤러](https://programmers.co.kr/learn/courses/30/lessons/42627?language=java)

## 풀이
현재 시간을 기준으로 소요되는 시간이 가장 짧은 작업을 선택하면 된다.

소요 시간이 짧은 작업을 먼저 처리할 수록 대기시간이 짧아지기 때문이다.

## 코드

```java
import java.io.*;
import java.util.*;

class Solution {
	int solution(int[][] jobs) {
        int answer = 0;
        Arrays.sort(jobs, new Comparator<int[]>() {
			@Override
			public int compare(int[] o1, int[] o2) {
				return Integer.compare(o1[0], o2[0]);
			}
		});
        
        PriorityQueue<int[]> pq = new PriorityQueue<>(512, new Comparator<int[]>() {
			@Override
			public int compare(int[] o1, int[] o2) {
				return Integer.compare(o1[1], o2[1]);
			}
		});
        
        int sum = 0;
        int idx = 0;
        int time = 0;
        while(idx < jobs.length) {
        	while(idx < jobs.length && jobs[idx][0] <= time) {
        		pq.offer(jobs[idx]);
        		idx++;
        	}
        	if(pq.isEmpty()) {
        		time++;
        	} else {
        		int[] cur = pq.poll();
        		int waiting = time - cur[0];
        		int runtime = cur[1];
        		sum += waiting + runtime;
        		time += runtime;
        	}
        }
        while(!pq.isEmpty()) {
    		int[] cur = pq.poll();
    		int waiting = time - cur[0];
    		int runtime = cur[1];
    		sum += waiting + runtime;
    		time += runtime;
        }
        answer = sum / jobs.length;
        return answer;
    }
}
```