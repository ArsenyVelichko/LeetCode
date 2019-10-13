# Trees

+ [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal)
+ [Symmetric Tree](#symmetric-tree)
+ [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree)
+ [Same Tree](#same-tree)
+ [Invert Binary Tree](#invert-binary-tree)
+ [Path Sum](#path-sum)
+ [Subtree of Another Tree](#subtree-of-another-tree)
+ [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal)

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

## Invert Binary Tree

https://leetcode.com/problems/invert-binary-tree/

```C++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
      if (root) {      
        TreeNode* tmp = invertTree(root->left);
        root->left = invertTree(root->right);
        root->right = tmp;
      }
      return root;
    }
};
```

## Path Sum

https://leetcode.com/problems/path-sum/

```C++
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
      if (root) {
        if (!root->left && !root->right && sum - root->val == 0)
          return true;
        else if (!hasPathSum(root->left, sum - root->val))
          return hasPathSum(root->right, sum - root->val);
        else
          return true;
      }
      return false;
    }
};
```

## Subtree of Another Tree

https://leetcode.com/problems/subtree-of-another-tree/submissions/

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
    bool isSubtree(TreeNode* s, TreeNode* t) {
       if (!s)
         return false;
       if (isSameTree(s, t))
         return true;
       if(!isSubtree(s->left, t))
         return isSubtree(s->right, t);
       else
         return true;
    }
};
```

# Binary Tree Level Order Traversal

https://leetcode.com/problems/binary-tree-level-order-traversal/

```C++
class Solution {
public:
    void LevelTraversal(TreeNode* root, vector<vector<int>>& levelOrder, int level) {
      if (root) {
        if (level >= levelOrder.size())
          levelOrder.resize(level + 1);
        levelOrder[level].push_back(root->val);
        LevelTraversal(root->left, levelOrder, level + 1);
        LevelTraversal(root->right, levelOrder, level + 1);
      }
    }
    vector<vector<int>> levelOrder(TreeNode* root) {
        vector<vector<int>> res(0);
        LevelTraversal(root, res, 0);
        return res;
    }
};
```
