---
title: "[LeetCode] Letter combinations of a phone number"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Letter combinations of a phone number
  - LeetCode
last_modified_at: 2019-07-16T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Letter combinations of a phone number](https://leetcode.com/problems/letter-combinations-of-a-phone-number)

어떻게 풀까?
-
모든 가능한 경우의 조합을 표시하는 것이다. 

재귀적으로 호출하여 푸는 방법과, Queue를 두어 반복적으로 풀어내는 방법이 있다. 재귀적인 방법이 좀 더 직관적이고 쉬워보인다. 


재귀적인 방법
-
~~~java
class Solution {
  private static final String[] mapping = new String[]{"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};

    public List<String> letterCombinations(String digits) {
      List<String> ret = new ArrayList<>();
    if(digits.length() == 0) return ret;
      combination("", digits, 0, ret);
      return ret;
    }

    public void combination(String prefix, String digits, int offset, List<String> ret){
      if(offset >= digits.length()){
        ret.add(prefix);
        return;
      }
      String letters = mapping[digits.charAt(offset) - '0'];
      for(int i=0; i<letters.length(); i++){
        combination(prefix+letters.charAt(i), digits, offset+1, ret);
      }
    }
}
~~~


반복적인 방법
-
반복적인 방법은 조금 어렵다. 반복할 대상을 Queue(List)에 넣어 빼가며 결과를 만든다.

조합이 된 각 문자는 digits의 length만큼의 길이가 되야하기 때문에, 이 경우가 될 때까지 Queue에 뺐다 넣었다를 반복하며 조합을 진행한다.

~~~java
public List<String> letterCombinations(String digits) {
  List<String> ret = new LinkedList<String>();
  if(digits.isEmpty()) return ret;
  String[] mapping = new String[]{"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
  ret.add("");
  while(ret.peek().length()!=digits.length()){
    String remove = ret.remove();
    String map = mapping[digits.charAt(remove.length())-'0'];
    for(char c: map.toCharArray()){
      ret.addLast(remove+c);
    }
  }
  return ret;
}
~~~