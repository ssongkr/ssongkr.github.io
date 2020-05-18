---
title: "[프로그래머스] 카카오 인턴십 코딩 테스트 실전 모의고사 [3/5]"
layout: post
tags: 알고리즘 백준
---

## 문제
[프로그래머스](https://programmers.co.kr/learn/challenges)

---
## 설명
유저의 아이디와 밴을 할 수 있는 아이디가 주어졌을때, 유저를 밴할 수 있는 경우의 수를 모두 구하는 문제다. 

---
## 풀이
밴 가능한 아이디를 통해 밴을 할 수 있는 유저의 경우의 수를 미리 계산해놓았다. 그 후 DFS를 통해 가능한 경우의 수를 완전탐색 하였다. 주의할 점은 서로 다른 밴 아이디를 통해 같은 유저들을 밴하는 경우도 있기 때문에 비트마스킹을 통해 중복 처리를 막았다.


## 코드
```java
import java.util.*;

public class Test03 {
	
	static class Solution {
		
		static boolean[] visit;
		static boolean[] cache = new boolean[1<<8];
		static boolean[][] possible = new boolean[10][10];
		static String[] bannedId, userId;
		
	    public int solution(String[] user_id, String[] banned_id) {
	    	for(int i=0; i<user_id.length; i++) {
	    		for(int j=0; j<banned_id.length; j++) {
	    			if(canBan(user_id[i], banned_id[j])) {
	    				possible[i][j] = true;
	    			}
	    		}
	    	}
	    	userId = user_id;
	    	bannedId = banned_id;
	    	visit = new boolean[user_id.length];
	    	return dfs(0, 0);
	    }
	    
	    public int dfs(int j, int mask) {
	    	if(j == bannedId.length) {
	    		if(!cache[mask]) {
	    			cache[mask] = true;
	    			return 1;
	    		}
	    		return 0;
	    	}
	    	
	    	int ret = 0;
	    	for(int i=0; i<userId.length; i++) {
	    		if(possible[i][j] && !visit[i]) {
	    			visit[i] = true;
	    			ret += dfs(j + 1, mask | (1 << i));
	    			visit[i] = false;
	    		}
	    	}
	    	return ret;
	    }
	    
	    public boolean canBan(String user, String banned) {
	    	if(user.length() != banned.length()) {
	    		return false;
	    	}
	    	for(int i=0; i<user.length(); i++) {
	    		if(banned.charAt(i) == '*') continue;
	    		if(user.charAt(i) != banned.charAt(i)) {
	    			return false;
	    		}
	    	}
			return true;
		}
	}
	
	public static void main(String[] args) {
		Solution sol = new Solution();
		String[] user_id = {"frodo", "fradi", "crodo", "abc123", "frodoc"};
		String[] banned_id = {"*rodo", "*rodo", "******"};
		System.out.println("ANS: " + sol.solution(user_id, banned_id));
	}
}
```