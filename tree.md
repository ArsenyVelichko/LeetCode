# Trees

+ [Binary Tree Inorder Traversal](#binary-tree-inorder-traversal)
+ [Symmetric Tree](#symmetric-tree)
+ [Maximum Depth of Binary Tree](#maximum-depth-of-binary-tree)
+ [Same Tree](#same-tree)
+ [Invert Binary Tree](#invert-binary-tree)
+ [Path Sum](#path-sum)
+ [Subtree of Another Tree](#subtree-of-another-tree)
+ [Binary Tree Level Order Traversal](#binary-tree-level-order-traversal)
+ [Kth Smallest Element in a BST](#kth-smallest-element-in-a-bst)
+ [Validate Binary Search Tree](#validate-binary-search-tree)
+ [Binary Search Tree Iterator](#binary-search-tree-iterator)
+ [Inorder Successor in BST](#inorder-successor-in-bst)
+ [Lowest Common Ancestor of a Binary Search Tree](#lowest-common-ancestor-of-a-binary-search-tree)
+ [Lowest Common Ancestor of a Binary Tree](#lowest-common-ancestor-of-a-binary-tree)

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
    int maxDepth(TreeNode* root) {
        return root == NULL ? 0
          : max(maxDepth(root->left), maxDepth(root->right)) + 1;
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
        return hasPathSum(root->left, sum - root->val) ||
                 hasPathSum(root->right, sum - root->val);
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
       return isSubtree(s->left, t) || isSubtree(s->right, t);
    }
};
```

## Binary Tree Level Order Traversal

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

## Kth Smallest Element in a BST

https://leetcode.com/problems/kth-smallest-element-in-a-bst/

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
    int kthSmallest(TreeNode* root, int k) {
        vector<int> res(0);
        RecursiveTraversal(root, res);
        return res[k - 1];
    }
};
```

### Iteratively

```C++
class Solution {
public:
    int kthSmallest(TreeNode* root, int k) {
        int res;
        stack<TreeNode*> treeTops;
        TreeNode* currentNode = root;
        while (k--) {
          while (currentNode) {
            treeTops.push(currentNode);
            currentNode = currentNode->left;
          }
          currentNode = treeTops.top();
          treeTops.pop();
          res = currentNode->val;
          currentNode = currentNode->right;
        }
        return res;
    }
};
```

## Validate Binary Search Tree

https://leetcode.com/problems/validate-binary-search-tree/

### Iteratively
```C++
class Solution {
public:
    bool isValidBST(TreeNode* root) {
        TreeNode* currentNode = root;
        stack<TreeNode*> treeTops;
        double first = -1.0 / 0;
        int second;
        while (currentNode != NULL || !treeTops.empty()) {
          while (currentNode) {
            treeTops.push(currentNode);
            currentNode = currentNode->left;
          }
          currentNode = treeTops.top();
          treeTops.pop();
          second = currentNode->val;
          if (first >= second)
            return false;
          first = second;
          currentNode = currentNode->right;
        }
      return true;
    }
};
```

### Recursively

```C++
class Solution {
public:
    bool CheckBST(TreeNode* node, const int* lowerBound, const int* upperBound) {
      if (!node)
        return true;
      if (lowerBound && node->val <= *lowerBound ||
          upperBound && node->val >= *upperBound) 
        return false;
      return CheckBST(node->left, lowerBound, &node->val) &&
             CheckBST(node->right, &node->val, upperBound);
    }
    bool isValidBST(TreeNode* root) {  
      return CheckBST(root, NULL, NULL);
    }
};
```

## Binary Search Tree Iterator

https://leetcode.com/problems/binary-search-tree-iterator/

```C++
class BSTIterator {
    stack<TreeNode*> treeTops;
    void PutLeftOrderIn(TreeNode* root) {
      while (root) {
        treeTops.push(root);
        root = root->left;
      }
    }
public:   
    BSTIterator(TreeNode* root) {
      PutLeftOrderIn(root);
    }
    int next() {
      TreeNode* currentNode = treeTops.top();
      treeTops.pop();
      PutLeftOrderIn(currentNode->right);
      return currentNode->val;
    }
    bool hasNext() {
      return !treeTops.empty();
    }
};
```

## Inorder Successor in BST

https://www.lintcode.com/problem/inorder-successor-in-bst/

```C++
class Solution {
public:
    TreeNode * inorderSuccessor(TreeNode * root, TreeNode * p) {
        TreeNode* prevNode = NULL;
        if (!root)
          return root;
        if (p->right) {
          prevNode = p->right;
          while (prevNode->left)
            prevNode = prevNode->left;
          return prevNode;
        }
        while (root->val != p->val) {
          if (root->val > p->val) {
            prevNode = root;
            root = root->left;
          } else
            root = root->right;
        }
        return prevNode;
    }
};
```

## Lowest Common Ancestor of a Binary Search Tree

https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-search-tree/

```C++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
      int leftBound, rightBound;
      if (p->val > q->val) {
        leftBound = q->val;
        rightBound = p->val;
      } else {
        leftBound = p->val;
        rightBound = q->val;
      }
      while (root) {
        if (root->val > rightBound)
          root = root->left;
        else if (root->val < leftBound)
          root = root->right;
        else
          break;
      } 
      return root;
    }
};
```

## Lowest Common Ancestor of a Binary Tree

https://leetcode.com/problems/lowest-common-ancestor-of-a-binary-tree/

```C++
class Solution {
private:
  int PtrsFound;
  TreeNode* resAnc;
public:
    int FindTwoNodes(TreeNode* root, TreeNode* p, TreeNode* q) {
      int currentRes = 0;
      if (!root)
        return 0;
      if (root == p || root == q) {
        PtrsFound++;
        currentRes = 1;
      }
      if (PtrsFound < 2) {
         currentRes += FindTwoNodes(root->left, p, q) + FindTwoNodes(root->right, p, q);
         if (currentRes == 2) {
             resAnc = root;
             return 0;
         }
      }
      return currentRes;
    }
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
       FindTwoNodes(root, p, q);
       return resAnc;
    }
};
```
