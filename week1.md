# Algorithm

## [10. 正则表达式匹配](https://leetcode-cn.com/problems/regular-expression-matching/)

### 描述

给你一个字符串` s` 和一个字符规律 `p`，请你来实现一个支持` '.' `和 `'*' `的正则表达式匹配。

```
*'.' 匹配任意单个字符
'*' 匹配零个或多个前面的那一个元素
```

所谓匹配，是要涵盖**整个**字符串 `s`的，而不是部分字符串。

说明:

* `s` 可能为空，且只包含从 `a-z` 的小写字母。
* `p` 可能为空，且只包含从 `a-z` 的小写字母，以及字符 `.` 和 `*`。

示例 1:

```
输入:
s = "aa"
p = "a"
输出: false
解释: "a" 无法匹配 "aa" 整个字符串。
```


示例 2:

```
输入:
s = "aa"
p = "a*"
输出: true
解释: 因为 '*' 代表可以匹配零个或多个前面的那一个元素, 在这里前面的元素就是 'a'。因此，字符串 "aa" 可被视为 'a' 重复了一次。
```


示例 3:

```
输入:
s = "ab"
p = ".*"
输出: true
解释: ".*" 表示可匹配零个或多个（'*'）任意字符（'.'）。
```


示例 4:

```
输入:
s = "aab"
p = "c*a*b"
输出: true
解释: 因为 '*' 表示零个或多个，这里 'c' 为 0 个, 'a' 被重复一次。因此可以匹配字符串 "aab"。
```


示例 5:

```
输入:
s = "mississippi"
p = "mis*is*p*."
输出: false
```

### 说明

提交了 33 次，历时 1 年，真的是太难了，解决方案并没有很优雅。使用 if 区分不同情况，递归判断 `'*'` 的贪婪情况，cache 缓存递归结果。

### 解决方案

```javascript
/**
 * @param {string} s
 * @param {string} p
 * @return {boolean}
 */
var cache = {};
var isMatch = function(s, p) {
    var sIndex = 0; //匹配的字符串的字符位置
    var matchLength = 0; //匹配成功的长度
    var currentChar = ''; //当前匹配的字符
    var zeroMatch = false; //*是否匹配零次
    console.log('输入',s,p);
    if (cache[s+'-'+p]!==undefined) {
        return cache[s+'-'+p];
    }
    function nextIs(j) {//判断是否包含
        //!arguments[2] && console.log('判断是否包含');
        //!arguments[2] && console.log('isNext:' + j);
        var findCurrentChar = -1;
        for (var i = j+1;i<p.length;i++) {//如果后面的匹配模式包含当前字符
            if (p[i] === s[sIndex]) {
                findCurrentChar = i;
                break;
            } 
        }
        //!arguments[2] && console.log('findCurrentChar:'+findCurrentChar);
        if (findCurrentChar===-1) {
            return false;
        }
        var allZero = true;
        for (var i=j+1;i<p.length;i+=2) {
            if (p[i+1] && p[i+1]!=='*') {
                allZero = false;
                break;
            }
        }
        //!arguments[2] && console.log('allZero:'+allZero);
        return allZero;
    }
  for (var i=0;i<p.length;i++) {//遍历 pattern
      if (zeroMatch) { //非贪婪模式，匹配零次
          !arguments[2] && console.log('zero jump');
          zeroMatch = false;
          continue;
      }
      !arguments[2] && console.log('p:'+p[i]);
      !arguments[2] && console.log("sIndex:"+sIndex);
      if (s.length===sIndex) {//匹配失败
          !arguments[2] && console.log('王多余');
          if (i<p.length-1&&p[i+1]==='*') {
              continue;
          }
          if (p[i]==='*') {
              continue;
          }
          matchLength = 0;
      }
      for (var j=sIndex;j<s.length;j++) {//遍历字符串
          !arguments[2] && console.log('s:'+s[j]);
          if (!sIndex && (s[j]===p[i] || p[i]==='.')) {//字符串里找到开始的部分
              if (p[i+1]==='*') {
                  if (p.substring(i+2) && isMatch(s, p.substring(i+2), true)) {//判断是否 0 匹配
                      zeroMatch = true;
                      break;
                  }
              }
              matchLength++;
              currentChar = s[j];
              if (p[i]==='.') {
                  currentChar = '.';
              }
              sIndex++;
              !arguments[2] && console.log('初次匹配到第'+sIndex+'个'+s[j]);
              break;
          }
          
          if (sIndex && p[i]===s[j]) { //匹配字符
              !arguments[2] && console.log('匹配字符');
              if (i<p.length-2 && p[i+1]==='*' && isMatch(s.substring(sIndex), p.substring(i+2), true)) {
                  console.log('0匹配');
                  zeroMatch=true;
                  break;
              }
              matchLength++;
              currentChar = s[j];
              sIndex++;
              !arguments[2] && console.log('匹配到第'+sIndex+'个'+s[j]);
              break;
          }
          if (sIndex && p[i]==='.') { //匹配.
              !arguments[2] && console.log('匹配.');
              if (p[i+1]==='*' && p.substring(i+2) && isMatch(s.substring(sIndex), p.substring(i+2), true)) {//判断是否 0 匹配
                  zeroMatch = true;
              } else {
                  matchLength++;
                  currentChar = '.';
                  sIndex++;
                  !arguments[2] && console.log('匹配到第'+sIndex+'个'+s[j]);
              }
              break;
          }
          if (sIndex && p[i]==='*') { //匹配*
              !arguments[2] && console.log('匹配*');
              if (isMatch(s.substring(sIndex), p.substring(i+1), true)) {
                  currentChar = p[i+1];
                  break;
              }
              if (p[i+1]===currentChar) {
                  for (var k=j;k<s.length-1;k++) {
                      if (s[k+1]!=currentChar) {
                          break;
                      } else {
                          !arguments[2] && console.log('匹配相同字符');
                          matchLength++;
                          sIndex++;
                      }
                  }
                  if (currentChar==='.') {
                    currentChar=p[i];
                  }
                  break;
              }
              if (currentChar==='.' || currentChar===s[j]) {
                  if (currentChar==='.' && p.substring(i+1) && isMatch(s.substring(sIndex), p.substring(i+1), true)) {
                    !arguments[2] && console.log(p[i+1] + '匹配'+s[sIndex]);
                    i++;
                    sIndex++;
                    matchLength++;
                    break;
                  }
                  // if (i<p.length-1 && nextIs(i) ) {//如果下个匹配模式（或者n个0匹配后的模式）为当前字符则匹配 0 次
                  //     !arguments[2] && console.log('如果下个匹配模式（或者n个0匹配后的模式）为当前字符则匹配 0 次');
                  //     break;
                  // }
                  matchLength++;
                  sIndex++;
                  !arguments[2] && console.log('匹配到第'+sIndex+'个'+s[j]);
                  continue;
              }
              
              break;
          }
          if (i<p.length-1 && p[i+1] === '*') {//是否匹配0次
              !arguments[2] && console.log('0匹配');
              zeroMatch=true;
              break;
          }
          arguments[2] && console.log(false);
        cache[s+'-'+p] = false;
          return false;
      }
    }
    !arguments[2] && console.log('最终length'+matchLength);
    if (matchLength===0 && s.length===0) {
        var match = true;
        for (var i=0;i<p.length;i+=2) {
            if (p[i+1] && p[i+1]!=='*') {
                match = false;
            }
        }
        if (p.length===1) {
            match = false;
        }
        if (p.length%2!==0) {
            match = false;
        }
        arguments[2] && console.log(match);
        cache[s+'-'+p] = match;
        return match;
    }
    arguments[2] && console.log(matchLength===s.length);
    cache[s+'-'+p] = matchLength===s.length;
    return matchLength===s.length;
};
```

## Review

### [The Definitive TypeScript Handbook](<https://www.freecodecamp.org/news/the-definitive-typescript-handbook/>)

TypeScript 赋予了 JS 大型项目的能力，并且对于团队协作友好，是时候好好学一下了。

## Technique

参加了 [【技术干货系列 #4】15 分钟打造低成本，零维护，Serverless应用]([https://www.huodongxing.com/event/6498002426700?utm_source=%e6%90%9c%e7%b4%a2%e6%b4%bb%e5%8a%a8%e5%88%97%e8%a1%a8%e9%a1%b5&utm_medium=&utm_campaign=searchpage&qd=4328363122557](https://www.huodongxing.com/event/6498002426700?utm_source=搜索活动列表页&utm_medium=&utm_campaign=searchpage&qd=4328363122557)) 现场实操了一下，虽然最终没有成功，但是还是学到了一些知识。

## Share 

本周学习了 GraphQL ，这种先进的东西还是需要了解一下的。GraphQL 节约的是服务器和客户端的传输成本，可以用在 node 和 react 之间。

