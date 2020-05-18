---
title: "[프로그래머스] 카카오 인턴십 코딩 테스트 실전 모의고사 [2/5]"
layout: post
tags: 알고리즘 프로그래머스 카카오테스트
---

## 문제
[프로그래머스](https://programmers.co.kr/learn/challenges)

---
## 풀이
중괄호 문자를 모두 공백문자로 대체한다. StringTokenizer를 이용하여 콤마로 토큰을 구분하면 숫자만 남게 된다. 남은 숫자들의 개수를 세고, 개수대로 정렬을 하면 정답을 얻을 수 있다.


## 코드
```java

import java.util.*;
class Solution {
	static class Pair implements Comparable<Pair> {
		int val, cnt;
		public Pair(int val, int cnt) {
			this.val = val;
			this.cnt = cnt;
		}

		@Override
		public int compareTo(Pair o) {
			return Integer.compare(o.cnt, cnt);
		}
	}
	
	public int[] solution(String s) {
		int[] count = new int[100001];
		s = s.replace("{", "").replace("}", "");
		StringTokenizer st = new StringTokenizer(s, ",");
		int max = 0;
		while(st.hasMoreTokens()) {
			int n = Integer.parseInt(st.nextToken());
			count[n]++;
			max = Math.max(max, n);
		}
		
		PriorityQueue<Pair> pq = new PriorityQueue<>();
		for(int i=0; i<=max; i++) {
			if(count[i] == 0) continue;
			pq.offer(new Pair(i, count[i]));
		}
		
		int[] answer = new int[pq.size()];
		int idx = 0;
		while(!pq.isEmpty()) {
			answer[idx++] = pq.poll().val;
		}
		
		return answer;
	}
}
```