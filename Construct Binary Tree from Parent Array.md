# Construct Binary Tree from Parent Array ([GFG](https://practice.geeksforgeeks.org/batch/dsa-4/track/DSASP-Tree/problem/construct-binary-tree-from-parent-array))
Given an array of size **N** that can be used to represents a tree. The array indexes are values in tree nodes and array values give the parent node of that particular index (or node). The value of the root node index would always be **-1** as there is no parent for root. Construct the standard linked representation of Binary Tree from this array representation.
```
Input: N = 7
parent[] = {-1,0,0,1,1,3,5}
Output: 0 1 2 3 4 5 6 Explanation: the tree generated
will have a structure like 
          0
        /   \
       1     2
      / \
     3   4
    /
   5
 /
6
```



## Solution : O(N)

**We can solve this question by keeping a map or vector to store the node for every index. If parent of a index is -1 then make it the main root else check if parent[i] have any left or right node NULL if so then make the current index node its child.**

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

class Solution{
  public:
    //Function to construct binary tree from parent array.
    Node *createTree(int parent[], int N)
    {
        vector<Node*> vec;
        Node *root;
        
        for(int i=0;i<N;i++){ // initialize the vector with the index
            Node *temp = new Node(i);
            vec.push_back(temp);
        }
        
        for(int i=0;i<N;i++){
            if(parent[i]==-1){ // if parent[i]==-1 then it is the root node
                root = vec[i]; // set the current index to root node
            }
            else{
                if(vec[parent[i]]->left == NULL){ // set parent[i]->left to the current i 
                    vec[parent[i]]->left = vec[i]; // parent[i]->left = node with i value
                }
                else{
                    vec[parent[i]]->right = vec[i];
                }
            }
        }
        
        return root;
    }
};
```
