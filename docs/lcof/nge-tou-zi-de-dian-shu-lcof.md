# 面试题60. n个骰子的点数

把n个骰子扔在地上，所有骰子朝上一面的点数之和为s。输入n，打印出s的所有可能的值出现的概率。

 

你需要用一个浮点数数组返回答案，其中第 i 个元素代表这 n 个骰子所能掷出的点数集合中第 i 小的那个的概率。

 

示例 1:

输入: 1
输出: [0.16667,0.16667,0.16667,0.16667,0.16667,0.16667]
示例 2:

输入: 2
输出: [0.02778,0.05556,0.08333,0.11111,0.13889,0.16667,0.13889,0.11111,0.08333,0.05556,0.02778]
 

限制：

1 <= n <= 11

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/nge-tou-zi-de-dian-shu-lcof

## 1.动态规划穷举

```js
/**
 * 状态：dp[n][i]:掷出第n个骰子，总点数是i的所有次数
 * 最优子结构：每枚骰子6个点数，dp[n][i]的次数可以由 dp[n-1][i-1] + 1或dp[n-1][i-2] + 2或...或dp[n-1][i-6] + 6构成
 * 状态转移方程：dp[n][i] = sum(dp[n-1][i-j]), 1 <= j <= 6
 * @param {number} n
 * @return {number[]}
 */
var twoSum = twoSumDp2;
function twoSumDp1(n) {
  let dp = new Array(n + 1).fill();
  dp = dp.map(() => new Array(7).fill(0));
  for (let i = 1; i <=  6; i++) {
    dp[1][i] = 1;
  }

  dpCount(n);
  const res = [];
  const all = Math.pow(6, n);
  for (let i = n; i <= n * 6; i++) {
    if (!dp[n][i]) continue;
    const tmp = dp[n][i] / all;
    res.push(tmp);
  }
  function dpCount(n) {
    for (let i = 2; i <= n; i++) {
      for (let j = i; j <= i * 6; j++) {
        dp[i][j] = 0;
        for (let num = 1; num <= 6; num++) {
          if (j - num <= 0) break;
          const sub = dp[i - 1][j - num] || 0;
          dp[i][j] += sub;
        }
      }
    } 
  }
  return res;
};
```

## 2.优化动态规划

使用一个长度是2的二维数值存储dp元素，因为计算`dp[i][j]`只是依赖`dp[i - 1][j - num]`

```js
function twoSumDp2(n) {
  const dp = [[0, 1, 1, 1, 1, 1, 1], []];

  dpCount(n);
  const res = [];
  const all = Math.pow(6, n);
  for (let i = n; i <= n * 6; i++) {
    res.push(dp[0][i] / all);
  }
  function dpCount(n) {
    for (let i = 2; i <= n; i++) {
      for (let j = i; j <= i * 6; j++) {
        dp[1][j] = 0;
        for (let num = 1; num <= 6; num++) {
          if (j - num <= 0) break;
          dp[1][j] += dp[0][j - num] || 0;
        }
      }
      dp[0] = dp[1];
      dp[1] = [];
    } 
  }
  return res;
};
```