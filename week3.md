# Algorithm

## [12. 整数转罗马数字](https://leetcode-cn.com/problems/integer-to-roman/)

### 描述

罗马数字包含以下七种字符： `I`， `V`， `X`， `L`，`C`，`D` 和 `M`。

```
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```


例如， 罗马数字 2 写做 `II` ，即为两个并列的 1。12 写做 `XII` ，即为 `X + II` 。 27 写做  `XXVII`, 即为 `XX` + `V `+ `II` 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做 `IIII`，而是 `IV`。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为 `IX`。这个特殊的规则只适用于以下六种情况：

* `I` 可以放在 `V` (5) 和 `X` (10) 的左边，来表示 4 和 9。
* `X` 可以放在 `L` (50) 和 `C` (100) 的左边，来表示 40 和 90。 
* `C` 可以放在 `D` (500) 和 `M` (1000) 的左边，来表示 400 和 900。
* 给定一个整数，将其转为罗马数字。输入确保在 1 到 3999 的范围内。

示例 1:

```
输入: 3
输出: "III"
```


示例 2:

```
输入: 4
输出: "IV"
```


示例 3:

```
输入: 9
输出: "IX"
```



示例 4:

```
输入: 58
输出: "LVIII"
解释: L = 50, V = 5, III = 3.
```


示例 5:

```
输入: 1994
输出: "MCMXCIV"
解释: M = 1000, CM = 900, XC = 90, IV = 4.
```

### 说明

之前做过类似的题目，现在发现直接用 `switch` 更清晰。

### 解决方案

```javascript
/**
 * @param {number} num
 * @return {string}
 */
var intToRoman = function(num) {
    const romanLevel = ['I','X','C','M'];    
    const romanHalfLevel = ['V','L','D'];
    function repeatNum(num,level) {
        var result = '';
        for (var i=0;i<num;i++) {
            result += romanLevel[level];
        }
        return result;
    }
    function getRoman(num,level) {
        switch (num) {
            case 1:
            case 2:
            case 3: return repeatNum(num, level);
            case 4: return romanLevel[level]+romanHalfLevel[level];
            case 5: return romanHalfLevel[level];
            case 6: 
            case 7: 
            case 8: return romanHalfLevel[level]+repeatNum(num-5, level);
            case 9: return romanLevel[level]+romanLevel[level+1];
            case 0: return '';
        }
    }
    let level = num.toString().length-1;
    let result = '';
    for (var i=0;i<num.toString().length;i++) {
        result += getRoman(num.toString()[i]*1,level);
        level--;
    }
    return result;
};
```

## Review

### [How to build a calendar with CSS Grid](<https://www.freecodecamp.org/news/how-to-build-a-calendar-with-css-grid/>)

CSS Grid 这种布局方式还不是很熟悉，float 布局是时候退休啦。

## Technique

学习到了 VS code 的一个小功能，直接新建 html 文件，输入`!`，等到代码提示后输入 `TAB`，就会生成一个 标准 html 文件。

## Share 

[猴子都能懂的 GIT 入门](<https://backlog.com/git-tutorial/cn/>)