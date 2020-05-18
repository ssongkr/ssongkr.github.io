---
layout: post
title: "[프로그래머스] 문자열 압축 - 2020 KAKAO BLIND RECRUITMENT[1/7]"
tags: 알고리즘 프로그래머스
---

## 문제
[프로그래머스 - 문자열 압축](https://programmers.co.kr/learn/courses/30/lessons/60057)

---
## 설명
문자열에서 같은 값이 연속해서 나타나는 것을 그 문자의 개수와 반복되는 값으로 표현하여 더 짧은 문자열로 줄여서 표현하는 문제다.

---
## 풀이
문자열 패턴의 길이를 통해 압축된 문자열의 길이를 구할 수 있다. 마지막에 남은 문자열의 길이를 더해줘야함에 주의하자. 남은 문자열의 길이는 원본 문자열의 길이에 부분 문자열의 길이를 모듈러 연산하여 구할 수 있다.

---
## 코드
```java
class Solution {

	public int solution(String s) {
		int answer = 987654321;
		for(int i=1; i<=s.length(); i++) {
			answer = Math.min(answer, compress(s, i));
		}
		return answer;
	}
	
	public int compress(String s, int len) {
		int ret = 0;
		int count = 1;
		String cur = s.substring(0, len);
		for(int i=len; i+len<=s.length(); i += len) {
			String next = s.substring(i, i + len);
			if(cur.equals(next)) {
				count++;
			} else {
				ret += len;
				if(count > 1) {
					ret += Integer.toString(count).length();
				}
				cur = next;
				count = 1;
			}
		}
		ret += len;
		if(count > 1) {
			ret += Integer.toString(count).length();
		}
		ret += s.length() % len;
		return ret;
	}
}
```