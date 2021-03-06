# 面试题31. 栈的压入、弹出序列

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

 

示例 1：

输入：pushed = [1,2,3,4,5], popped = [4,5,3,2,1]
输出：true
解释：我们可以按以下顺序执行：
push(1), push(2), push(3), push(4), pop() -> 4,
push(5), pop() -> 5, pop() -> 3, pop() -> 2, pop() -> 1
示例 2：

输入：pushed = [1,2,3,4,5], popped = [4,3,5,1,2]
输出：false
解释：1 不能在 2 之前弹出。
 

提示：

0 <= pushed.length == popped.length <= 1000
0 <= pushed[i], popped[i] < 1000
pushed 是 popped 的排列。
注意：本题与主站 946 题相同：https://leetcode-cn.com/problems/validate-stack-sequences/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/zhan-de-ya-ru-dan-chu-xu-lie-lcof

## 使用真实stack模拟

```js
/**
 * 使用真实stack模拟
 * 0.定义辅助栈stack
 * 1.持续按照pushed的顺序入栈stack，直到stack的栈顶元素与poped的栈底元素相同，
 * 2.则按照poped顺序出栈stack直到stack的栈顶元素不再与poped的栈低元素相同，然后重复1->2之道pushed的元素使用完
 * 3.如果poped不为空，则stack按照poped继续出栈，直到poped使用完返回true，否则返回false；
 * @param {number[]} pushed
 * @param {number[]} popped
 * @return {boolean}
 */
var validateStackSequences = validateStackSequences2;
function validateStackSequences1(pushed, popped) {
  const stack = [];
  let isValid = true;
  while (pushed.length || popped.length) {
    if (stack && stack[stack.length - 1] === popped[0]) {
      stack.pop();
      popped.shift();
    } else if (pushed.length) {
      stack.push(pushed.shift());
    }

    if (!pushed.length && stack[stack.length - 1] !== popped[0]) {
      isValid = false;
      break;
    }
  }
  return isValid;
};

function validateStackSequences2(pushed, popped) {
  const stack = [];
  let isValid = true;
  let i = 0;  // pushed idx
  let j = 0;  // poped idx
  while (i < pushed.length || j < popped.length) {
    if (stack && stack[stack.length - 1] === popped[j]) {
      stack.pop();
      j++;
    } else if (i < pushed.length) {
      stack.push(pushed[i]);
      i++;
    }

    if (i >= pushed.length && stack[stack.length - 1] !== popped[j]) {
      isValid = false;
      break;
    }
  }
  return isValid;
};
```