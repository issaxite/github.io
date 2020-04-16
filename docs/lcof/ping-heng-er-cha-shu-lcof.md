# 面试题55 - II. 平衡二叉树

输入一棵二叉树的根节点，判断该树是不是平衡二叉树。如果某二叉树中任意节点的左右子树的深度相差不超过1，那么它就是一棵平衡二叉树。

 

示例 1:

给定二叉树 [3,9,20,null,null,15,7]
```
    3
   / \
  9  20
    /  \
   15   7
```
返回 true 。

示例 2:

给定二叉树 [1,2,2,3,3,null,null,4,4]
```
       1
      / \
     2   2
    / \
   3   3
  / \
 4   4
```
返回 false 。

 

限制：

1 <= 树的结点个数 <= 10000
注意：本题与主站 110 题相同：https://leetcode-cn.com/problems/balanced-binary-tree/

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/ping-heng-er-cha-shu-lcof

## 1.深度优先遍历（后序）

```js
/**
 * Definition for a binary tree node.
 * function TreeNode(val) {
 *     this.val = val;
 *     this.left = this.right = null;
 * }
 */
/**
 * 深度优先（后序递归遍历）计算左右子树的深度
 * 判断左右子树深度差是否超过1
 * @param {TreeNode} root
 * @return {boolean}
 */
var isBalanced1 = function(root) {
  let res = true;
  dfs(root);
  function dfs(node) {
    if (!node) return 0;
    const lDeep = dfs(node.left) + 1;
    const rDeep = dfs(node.right) + 1;
    if (Math.abs(lDeep - rDeep) > 1) {
      res = false;
    }
    return Math.max(lDeep, rDeep);
  }
  return res;
};

var isBalanced = isBalanced1;
```