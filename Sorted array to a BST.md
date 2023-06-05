# Sorted array to a BST ([GFG](https://practice.geeksforgeeks.org/problems/array-to-bst4443/1))
Given a sorted array. Convert it into a Height balanced Binary Search Tree (BST). Find the preorder traversal of height balanced BST. If there exist many such balanced BST consider the tree whose preorder is lexicographically smallest.  
Height balanced BST means a binary tree in which the depth of the left subtree and the right subtree of every node never differ by more than 1.
```
Input: nums = {1,2,3,4,5,6,7}
Ouput: {4,2,1,3,6,5,7}
Explanation: The preorder traversal of the following
BST formed is {4,2,1,3,6,5,7} :
        4
       / \
      2   6
     / \  / \
    1   3 5  7
```
**Preorder traversal is root-left-right. So , since the array is sorted left of mid will always be less than mid and right will be greater. So we need to push the mid from left in vector and when all elements from the left are over we move to right and push the mid elements.**

## Solution:
```cpp
    vector<int>res;
    void func(vector<int> &nums, int s,int e){
        if(s>e){
            return;
        }
        int mid = (s+e)/2;
        res.push_back(nums[mid]);
        
        func(nums,s,mid-1);
        func(nums,mid+1,e);
    }
    vector<int> sortedArrayToBST(vector<int>& nums) {
        int n = nums.size()-1;
        
        func(nums,0,n);
        return res;
    }
};
```
# Binary Tree from preorder and postorder [IMP] ([CN](https://www.codingninjas.com/codestudio/problems/construct-binary-tree-from-inorder-and-postorder-traversal_1266106?topList=love-babbar-dsa-sheet-problems&leftPanelTab=0))
Given **inorder** and **postorder** traversals of a Binary Tree in the arrays **in[]** and **post[]** respectively. The task is to construct the binary tree from these traversals.
```
Input: N = 8
in[] = 4 8 2 5 1 6 3 7
post[] =8 4 5 2 6 7 3 1
Output: 1 2 4 8 5 3 6 7 
Explanation: For the given postorder and
inorder traversal of tree the  resultant
binary tree will be
           1
       /      \
     2         3
   /  \      /  \
  4    5    6    7
   \
     8
```


## Solution for postorder:
 - We know that in Inorder traversal mid is the root and left and right
   to it are left and right child respectively.
 - Now for having a unique tree we need Inorder sequence along with pre
   or post order.
 - We need to iterate through pre or post array and try to find the
   element in inorder array.
 - If we find the element then we mark its index and try of find left
   and right child using inorder property in 1
 - postidx is a global var and used to maintain the postorder index

**One important observation is, that we recursively call for the right subtree before the left subtree as we decrease the index of the postorder index whenever we create a new node. So for left child the root element in postorder array is outside the range of inorder array.**
```cpp
int postidx;
Node* postIn(int in[],int post[],int s,int e){
    if(s>e){
        return NULL; 
    }
    
    Node *root = new Node(post[postidx--]);
    int index = 0;
    
    for(int j=s;j<=e;j++){
        
        if(in[j]==root->data){
            index = j;
            break;
        }
    }
    root->right = postIn(in,post,index+1,e);  // for postorder we need to mark right first
    root->left = postIn(in,post,s,index-1);
    
    return root;
}
Node *buildTree(int in[], int post[], int n) {
    // Your code here
    postidx = n-1;
    return postIn(in,post,0,n-1);
}
```
## Solution for preorder:

```cpp
class Solution {
    int preidx;
    TreeNode* preIn(vector<int>& p, vector<int> &i, int s,int e){
        if(s>e){
            return NULL;
        }
        
        TreeNode* root = new TreeNode(p[preidx++]);
        
        int index = 0;
        for(int j=s;j<=e;j++){
            if(i[j]==root->val){
                index = j;
                break;
            }
        }
        
        root->left = preIn(p,i,s,index-1); // left child should be 
        root->right = preIn(p,i,index+1,e);
        return root;
    }
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int n = preorder.size();
        preidx = 0;
        return preIn(preorder,inorder, 0,n-1);
    }
};
```
