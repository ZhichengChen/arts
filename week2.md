# Algorithm

## [11. 盛最多水的容器](https://leetcode-cn.com/problems/container-with-most-water/)

### 描述

给定 n 个非负整数 a1，a2，...，an，每个数代表坐标中的一个点 (i, ai) 。在坐标内画 n 条垂直线，垂直线 i 的两个端点分别为 (i, ai) 和 (i, 0)。找出其中的两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。

说明：你不能倾斜容器，且 n 的值至少为 2。

![](https://aliyun-lc-upload.oss-cn-hangzhou.aliyuncs.com/aliyun-lc-upload/uploads/2018/07/25/question_11.jpg)

图中垂直线代表输入数组 [1,8,6,2,5,4,8,3,7]。在此情况下，容器能够容纳水（表示为蓝色部分）的最大值为 49。

 

示例:

```
输入: [1,8,6,2,5,4,8,3,7]
输出: 49
```

### 解决方案

```javascript
/**
 * @param {number[]} height
 * @return {number}
 */
var maxArea = function(height) {
    var sum = 0;
    var l = 0;
    var distance = 0;
    for (var i=0;i<height.length;i++) {
        for (var j=i+1;j<height.length;j++) {
            var temp = Math.min(height[i],height[j]);
            // if (temp<l && (j-i)<distance) {
            //     break;
            // }
            var tempNum = temp*(j-i);
            if (tempNum>sum) {
                distance = j-i;
                l=temp;
                sum = tempNum;
            }
        }
    }
    return sum; 
};
```



题目不难，嵌套 for 遍历所有的情况，但是之前提交超出时间限制，后来几乎同样的代码提交却通过了，难道和网网速有关系？

## Review

[What you need to know to become a full-stack serverless developer](https://www.freecodecamp.org/news/what-you-need-to-become-a-full-stack-serverless-developer/)

serverless 很火，大势所趋吗？node 让前端拓展到了全栈，serverless 让前端更专业的处理后端

## Technique

这周和后端联调的时候，发现了 chrome 的一个有用的功能，inspect network 里面右击选择 copy，copy as curl。可以将网络连接拷贝成 curl 字符串，不仅可以在终端里运行，参数也一目了然。

# Share

分享一个程序员直播编程的网站 https://belly.io/