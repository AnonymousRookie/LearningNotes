### 32-III. 从上到下打印二叉树 III

```
请实现一个函数按照之字形顺序打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右到左的顺序打印，第三行再按照从左到右的顺序打印，其他行以此类推。

 

例如:
给定二叉树: [3,9,20,null,null,15,7],

    3
   / \
  9  20
    /  \
   15   7
返回其层次遍历结果：

[
  [3],
  [20,9],
  [15,7]
]
 

提示：

节点总数 <= 1000
```

### 解题思路

- 初始层从0层开始，即奇数层要反着遍历
- 当为偶数层时，子节点从尾部压入，先压入左节点，后压入右节点。并从头部取出，
- 当为奇数层是，子节点从头部压入，先压入右节点，后压入左节点。并从尾部取出


```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> ret;
        if (!root) {
            return ret;
        }

        deque<TreeNode*> q;
        q.push_back(root);

        int levelIndex = 0;
        while (!q.empty()) {
            int curLevelSize = q.size();
            ret.push_back(vector<int>());

            for (int i = 0; i < curLevelSize; ++i) {
                if (levelIndex & 1) {
                    TreeNode* node = q.back();
                    q.pop_back();
                    ret.back().push_back(node->val);
                    if (node->right) q.push_front(node->right);
                    if (node->left) q.push_front(node->left);
                }
                else {
                    TreeNode* node = q.front();
                    q.pop_front();
                    ret.back().push_back(node->val);
                    if (node->left) q.push_back(node->left);
                    if (node->right) q.push_back(node->right);
                }
            }
            ++levelIndex;
        }

        return ret;
    }
};
```
