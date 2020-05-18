---
layout: post
title: "[프로그래머스] 괄호 변환 - 2020 KAKAO BLIND RECRUITMENT[2/7]"
tags: 알고리즘 프로그래머스 카카오기출
---

## 문제
[프로그래머스 - 괄호 변환](https://programmers.co.kr/learn/courses/30/lessons/60058)

---
## 설명
균형잡힌 괄호 문자열이 주어졌을 때, 주어진 규칙에 따라 이를 올바른 괄호열로 만드는 문제다.

---
## 풀이
재귀적으로 문제를 풀이할 수 있다. 문제의 설명을 그대로 재귀적으로 구현하면 된다. 문제에서 v가 공백 문자열이 될 수 있기 때문에 반복문의 범위를 설정할 때 주의하자.

---
## 코드
```java
import java.util.*;

class Solution {
    public String solution(String p) {
        return dfs(p);
    }

    public String dfs(String p) {
        if(p.length() == 0) {
            return p;
        }

        for(int i=2; i<=p.length(); i+=2) {
            String u = p.substring(0, i);
            if(!balanced(u)) continue;
            String v = p.substring(i, p.length());
            if(correct(u)) {
                return u + dfs(v);
            } else {
                return "(" + dfs(v) + ")" + toggle(u);
            }
        }
        return "";
    }

    public String toggle(String s) {
        StringBuilder sb = new StringBuilder();
        for(int i=1; i<s.length() - 1; i++) {
            char token = s.charAt(i);
            if(token == '(') {
                sb.append(')');
            } else {
                sb.append('(');
            }
        }
        return sb.toString();
    }

    public boolean balanced(String p) {
        int ret = 0;
        for(char token: p.toCharArray()) {
            if(token == '(') {
                ret++;
            } else {
                ret--;
            }
        }
        return ret == 0;
    }

    public boolean correct(String p) {
        Deque<Character> s = new ArrayDeque<>();
        for(char token: p.toCharArray()) {
            if(token == '(') {
                s.push(token);
            } else {
                if(s.isEmpty()) {
                    return false;
                }
                s.pop();
            }
        }
        return s.isEmpty();
    }
}
```