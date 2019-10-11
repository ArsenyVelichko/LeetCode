# Trees

+ [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal)
+ [Symmetric Tree](#symmetric-tree)
+ [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree)
+ [Same Tree](#same-tree)

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

## Maximum Depth of Binary Tree

https://leetcode.com/problems/maximum-depth-of-binary-tree/

```C++
class Solution {
public:
    int SearchBottom(TreeNode* node, int depth) {
      if (!node)
        return depth;
      return max(SearchBottom(node->left, depth + 1), SearchBottom(node->right, depth + 1));
    }
    int maxDepth(TreeNode* root) {
        return SearchBottom(root, 0);
    }
};
```

## Same Tree

https://leetcode.com/problems/same-tree/

```C++
class Solution {
public:
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (!p && !q)
          return true;
        if (!p || !q)
          return false;
        return (p->val == q->val) &&
          isSameTree(p->left, q->left) &&
          isSameTree(p->right, q->right);
    }
};
```
