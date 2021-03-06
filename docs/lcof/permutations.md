# 46. 全排列

给定一个 没有重复 数字的序列，返回其所有可能的全排列。

示例:

输入: [1,2,3]
输出:
```
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]
```

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/permutations

## 1.深度优先（比较隐含的回溯）

由于传入下一层调用栈的参数都是新值，不会影响当前栈所以不用显式地回溯

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute1 = function(nums) {
  // [left, right]
  const res = [];
  dfs(nums, []);

  function dfs(nums, visited) {
    if (nums.length === 1) {
      visited.push(nums[0]);
      res.push(visited);
      return;
    }
    for (let i = 0; i < nums.length; i++) {
      dfs([...nums.slice(0, i), ...nums.slice(i + 1)], [nums[i], ...visited]);
    }
  }
  return res;
};
```

## 2.显式的回溯

```js
/**
 * @param {number[]} nums
 * @return {number[][]}
 */
var permute2 = function(nums) {
  // [left, right]
  const res = [];
  dfs(nums, [], {}, res);

  function dfs(nums, path, visited, res) {
    if (path.length === nums.length) {
      res.push(path.slice());
      return;
    }
    for (let i = 0; i < nums.length; i++) {
      if (!visited[i]) {
        visited[i] = true;
        path.push(nums[i]);
        dfs(nums, path, visited, res);
        visited[i] = false;
        path.pop();
      }
    }
  }
  return res;
};
```
