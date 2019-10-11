# Trees

+ [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal)
+ [Symmetric Tree](#symmetric-tree)

## Binary Tree Inorder Traversal

https://leetcode.com/problems/binary-tree-inorder-traversal/

### Iteratively

```C++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        TreeNode* currentNode = root;
        stack<TreeNode*> treeTops;
        vector<int> res;
        while (currentNode != NULL || !treeTops.empty()) {
          while (currentNode) {
            treeTops.push(currentNode);
            currentNode = currentNode->left;
          }
          currentNode = treeTops.top();
          treeTops.pop();
          res.push_back(currentNode->val);
          currentNode = currentNode->right;
        }
      return res;
    }
};
```

### Recursively

```C++
class Solution {
public:
    void RecursiveTraversal(TreeNode* node, vector<int>& res) {
      if (node) {
        RecursiveTraversal(node->left, res);
        res.push_back(node->val);
        RecursiveTraversal(node->right, res);
      }
    }
    vector<int> inorderTraversal(TreeNode* root) {
      vector<int> res(0);
      RecursiveTraversal(root, res);
      return res;
    }
};
```

## Symmetric Tree

https://leetcode.com/problems/symmetric-tree/

### Recursively

```C++
class Solution {
public:
    bool isEqual(TreeNode* first, TreeNode* second) {
      if (!first && !second)
        return true;
      if (!first || !second)
        return false;
      return (first->val == second->val) &&
              isEqual(first->left, second->right) &&
              isEqual(first->right, second->left);
    }
    bool isSymmetric(TreeNode* root) {
       return isEqual(root, root);
    }
};
```
