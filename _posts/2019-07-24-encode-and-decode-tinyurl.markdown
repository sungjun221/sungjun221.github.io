---
title: "[LeetCode] Encode and Decode TinyURL"
categories: 
  - Algorithm
tags:
  - Algorithm
  - Encode and Decode TinyURL
  - LeetCode
  - JavaScript
last_modified_at: 2019-07-24T12:04:24-04:00
toc: true
---

문제정보
-
- [LeetCode - Encode and Decode TinyURL](https://leetcode.com/problems/encode-and-decode-tinyurl)


어떻게 풀까?
Shorten URL 서비스를 제공하는 문제이다. 

Date객체를 이용해 36진수의 값을 생성하여 Shorten URL로 사용하고 원래 값과 매핑한다.


문제풀이(JavaScript)
-
~~~javascript
let urls = {};

/**
 * Encodes a URL to a shortened URL.
 *
 * @param {string} longUrl
 * @return {string}
 */
let encode = function(longUrl) {
    let s = Date.now().toString(36);
    urls[s] = longUrl;
    return "http://tinyurl.com/" + s;
};

/**
 * Decodes a shortened URL to its original URL.
 *
 * @param {string} shortUrl
 * @return {string}
 */
let decode = function(shortUrl) {
    return urls[shortUrl.split('com/')[1]];
};

/**
 * Your functions will be called as such:
 * decode(encode(url));
 */
}
~~~