# Diameter of a binary tree ([LeetCode](https://leetcode.com/problems/diameter-of-binary-tree/description/))
We have to find the diameter i.e the longest path in the tree form one leaf to another 
We can have 3 cases:

 1. Include main root for longest path **(h of lst + h of rst + 1)**
 2. Longest path in left sub tree  **(diameter(root->left))** - Traverse
    to left for longest path
 3. Longest path in right sub tree **(diameter(root->right))** -
    Traverse to right for longest path

We will find maximum by taking the max of these three cases
**Sub Problem:**
Min Sub problem in this a tree with one node with 0 diameter
For full binary tree of 3 nodes, the diameter is the **height of rst + height of lst + 1** i.e 3
## Solution 1: O(N**2)
```cpp
struct Node
{
    int data;
    struct Node* left;
    struct Node* right;

    Node(int x){
        data = x;
        left = right = NULL;
    }
};

class Solution {
  public:
    // Function to return the diameter of a Binary Tree.
    int height(Node* root){
        if(root==NULL){
            return 0;
        }
        
        int l = height(root->left);
        int r = height(root->right);
        
        return max(l,r)+1;
    }
    
    int diameter2(Node* root) {
        // Your code here
        if(root==NULL){
            return 0;
        }
        
        int h1 = height(root->left);
        int h2 = height(root->right);
        
        int a = h1+h2+1;
        int b = diameter2(root->left);
        int c = diameter2(root->right);
        
        return max(a,max(b,c));
    }
    
    int diameter(Node* root) {
        // Your code here
        int ans = diameter2(root);
        return ans;
    }
};
```



## Solution 2:  O(N)

### Instead of finding the height and diameter separately, we can make a class or struct with height and diameter as class variables and solve the question with the same logic.

```cpp
struct Node
{
    int data;
    struct Node* left;
    struct Node* right;

    Node(int x){
        data = x;
        left = right = NULL;
    }
}; 
class Tree{
    public:
        int height;
        int diameter;
};

Tree fastDiameter(Node* root){
    Tree t;
    if(root==NULL){
        t.height = t.diameter = 0;
        return t;
    }
    
    Tree lst = fastDiameter(root->left);
    Tree rst = fastDiameter(root->right);
    
    t.height = max(lst.height,rst.height)+1;
    t.diameter = max(lst.height + rst.height +1, max(lst.diameter,rst.diameter));
    return t;
}

int res(Node* root){
    Tree ans = fastDiameter(root);
    return ans.diameter;
}

class Solution {
  public:
    // Function to return the diameter of a Binary Tree.
    int diameter(Node* root) {
        // Your code here
        return res(root);
    }
};
```
