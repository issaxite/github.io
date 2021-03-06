# 面试题46. 把数字翻译成字符串

给定一个数字，我们按照如下规则把它翻译为字符串：0 翻译成 “a” ，1 翻译成 “b”，……，11 翻译成 “l”，……，25 翻译成 “z”。一个数字可能有多个翻译。请编程实现一个函数，用来计算一个数字有多少种不同的翻译方法。

 

示例 1:

输入: 12258
输出: 5
解释: 12258有5种不同的翻译，分别是"bccfi", "bwfi", "bczi", "mcfi"和"mzi"
 

提示：

0 <= num < 231

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ba-shu-zi-fan-yi-cheng-zi-fu-chuan-lcof

## 1.深度优遍历

```js
/**
 * @param {number} num
 * @return {number}
 */
var translateNum1 = function(num) {
  let count = 0;
  dfs(num + '', 0);
  function dfs(str, start) {
    if (start >= str.length) return ++count;
    const num1 = +(str[start]);
    const num2 = +(str[start] + str[start+1]);
    
    dfs(str, start + 1);
    if (9 < num2 && num2 < 26) {
      dfs(str, start + 2);
    }
  }
  return count;
};
```

## 2.动态规划

```js
/**
 * dp[i]：长度为i字符串的翻译方法数
 * 最优子结构：dp[i-1]:最后单字符翻译，dp[i-2]：最后双字符翻译
 * 状态转移方程：dp[i] = isValid(s[i-1]+s[i-2]) ? dp[i-1] + dp[i-2] : dp[i-1]
 * @param {number} num
 * @return {number}
 */
var translateNum2 = function(num) {
  const str = '_' + num;
  // [0, pos)
  let dp1 = 1;
  let dp2 = 1;
  for (let i = 3; i <= str.length; i++) {
    let tmp = dp2;
    const num = str[i-2] + str[i - 1];
    if (9 < +num && +num < 26) {
      dp2 = dp2 + dp1;
    }
    dp1 = tmp;
  }
  return dp2;
};
```

## 3.动态规划（优化）

```js
/**
 * @param {number} num
 * @return {number}
 */
var translateNum3 = function(num) {
  const str = '_' + num;
  // [0, pos)
  let dp1 = 1;
  let dp2 = 1;
  for (let i = 2; i < str.length; i++) {
    let tmp = dp2;
    const num = str[i-1] + str[i];
    if (9 < +num && +num < 26) {
      dp2 = dp2 + dp1;
    }
    dp1 = tmp;
  }
  return dp2;
};
```