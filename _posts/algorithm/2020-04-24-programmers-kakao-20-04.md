---
layout: post
title: "[프로그래머스] 가사 검색 - 2020 KAKAO BLIND RECRUITMENT[4/7]"
tags: 알고리즘 프로그래머스 카카오기출
---

## 문제
[프로그래머스 - 가사 검색](https://programmers.co.kr/learn/courses/30/lessons/60060)

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
    public int[] solution(String[] words, String[] queries) {
        final int N = 10001;

        List<String>[] list = new ArrayList[N];
        List<String>[] rlist = new ArrayList[N];

        for(int i=0; i<N; i++) {
            list[i] = new ArrayList<String>();
            rlist[i] = new ArrayList<String>();
        }

        for(String word: words) {
            int idx = word.length();
            list[idx].add(word);
            rlist[idx].add(new StringBuilder(word).reverse().toString());
        }

        for(int i=0; i<N; i++) {
            Collections.sort(list[i]);
            Collections.sort(rlist[i]);
        }

        int[] ans = new int[queries.length];
        for(int i=0; i<queries.length; i++) {
            String query = queries[i];
            int j = query.length();

            char l = query.charAt(0);
            char r = query.charAt(query.length() - 1);
            if(l == '?' && r == '?') {
                ans[i] = list[j].size();
            } else if(l == '?') {
                query = new StringBuilder(query).reverse().toString();
                ans[i] = upper(rlist[j], query) - lower(rlist[j], query);
            } else if(r == '?') {
                ans[i] = upper(list[j], query) - lower(list[j], query);
            }
        }

        return ans;
    }

    public int upper(List<String> list, String query) {
        int low = 0;
        int high = list.size();
        while (low < high) {
            int mid = low + (high - low) / 2;
            if (compare(list.get(mid), query) <= 0) {
                low = mid + 1;
            } else {
                high = mid;
            }
        }
        return low;
    }

    public int lower(List<String> list, String query) {
        int low = 0;
        int high = list.size();
        while(low < high) {
            int mid = low + (high - low) / 2;
            if(compare(list.get(mid), query) >= 0) {
                high = mid;
            } else {
                low = mid + 1;
            }
        }
        return low;
    }

    public int compare(String word, String query) {
        for(int i=0; i<word.length(); i++) {
            char w = word.charAt(i);
            char q = query.charAt(i);
            if(q == '?') {
                return 0;
            }
            if(w > q) {
                return 1;
            } else if(w < q) {
                return -1;
            }
        }
        return 987654321;
    }
}
```