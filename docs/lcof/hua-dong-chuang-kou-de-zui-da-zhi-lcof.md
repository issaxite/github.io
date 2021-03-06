# 面试题59 - I. 滑动窗口的最大值

给定一个数组 nums 和滑动窗口的大小 k，请找出所有滑动窗口里的最大值。

示例:

输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7] 
解释: 
```
  滑动窗口的位置                 最大值
---------------               -------
[
  1  3  -1] -3  5  3  6  7       3
  1 [3  -1  -3] 5  3  6  7       3
  1  3 [-1  -3  5] 3  6  7       5
  1  3  -1 [-3  5  3] 6  7       5
  1  3  -1  -3 [5  3  6] 7       6
  1  3  -1  -3  5 [3  6  7       7
]
```

提示：

你可以假设 k 总是有效的，在输入数组不为空的情况下，1 ≤ k ≤ 输入数组的大小。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/hua-dong-chuang-kou-de-zui-da-zhi-lcof


## 暴力法
```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
function maxSlidingWindow(nums, k) {
  if (!nums.length) return [];
  let left = 0;
  let right = k - 1;
  const res = [];
  while (right < nums.length) {
    // [left, right]
    let max = -Infinity;
    for (let i = left; i <= right; i++) {
      max = Math.max(max, nums[i]);
    }
    res.push(max);
    left++;
    right++;
  }
  return res;
};
```

## 双端队列
```js
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
function maxSlidingWindow(nums, k) {
  if (!nums.length) return [];
  const res= [];
  let curMaxNumIdx;
  const deQueue = new DeQueue(nums, k);

  curMaxNumIdx = deQueue.getLeftHead();
  res.push(nums[curMaxNumIdx]);

  for (let i = k; i < nums.length; i++) {
    deQueue.push(i);
    curMaxNumIdx = deQueue.getLeftHead();
    res.push(nums[curMaxNumIdx]);
  }
  return res;
}

/**
 * 维护一个队列，成员是nums中元素的idx，并且这些idx都是当前
 * 并且这些idx都是在滑动中
 */
function DeQueue(nums, k) {
  this._queue = [];
  this._nums = nums;
  this._k = k;
  this._init();
}

DeQueue.prototype.push = function(idx) {
  this._clean(idx);
  this._queue.push(idx);
}

DeQueue.prototype.getLeftHead = function() {
  return this._queue[0];
}

DeQueue.prototype._init = function() {
  for (let i = 0; i < this._k; i++) {
    this.push(i);
  }
}

DeQueue.prototype._clean = function(idx) {
  const queueLen = this._queue.length;
  const leftHead = this._queue[0];
  const minValidIdx = idx + 1 - this._k;
  // 队列为空，不需要清理无用元素
  if (!queueLen) return;

  // ps: 每次调用_clean只会调用一次
  // 当前滑动窗口中index的取值范围是：[minValidIdx, idx]
  // 如果deQueue的中的元素已经不再取值范围内就已经无用，出队清理掉
  if (leftHead < minValidIdx) {
    this._queue.shift();
  }

  // PS: 调用多次更新序列
  // 背景：nums[idx]将会进入deQueue，现在这里的逻辑是在入队新元素前的整理工作
  // deQueue中的元素要求是：一个由大到小的有序列
  // 如果队尾（队列中最小）元素指向的nums的真实值比nums[idx]还小，那就没用，要清理掉
  let rightHead = this._queue[this._queue.length - 1];
  while (this._queue.length && this._nums[idx] >= this._nums[rightHead]) {
    this._queue.pop();
    rightHead = this._queue[this._queue.length - 1];
  }
}
```