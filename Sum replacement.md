# Binary Tree Sum Replacement Problem ([GfG](https://practice.geeksforgeeks.org/problems/check-for-balanced-tree/1))
Given a binary tree, find if it is height balanced or not.  
A tree is height balanced if difference between heights of left and right subtrees is **not more than one** for all nodes of tree.

```
Input:
     1
    /
   2
    \
     3 
Output:0 
Explanation: The max difference in height
of left subtree and right subtree is 2,
which is greater than 1. Hence unbalanced
```

We have 3 conditions for a tree to be height balanced:

 **1. Height of lst - Height of rst <= 1**
 **2. Lst must be balanced**
 **3. Rst must be balanced**

**Sub Problem:**
Min sub-problem can be a NULL root which has a height of 0 and is balanced.

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
 */

class Solution{
    int height(Node *root){
        if(root==NULL){
            return 0;
        }
        
        int l = height(root->left);
        int r = height(root->right);
        
        return max(l,r)+1;
    }
    
    bool func(Node* root){
        if(root==NULL){
            return 1;  // NULL root is balanced
        }
        int h1 = height(root->left);  // find lst height
        int h2 = height(root->right);  // find rst height
        bool b1 = func(root->left);  // check if lst is balanced
        bool b2 = func(root->right);  // check if rst is balanced
        if(abs(h1-h2)<=1 && b1 && b2){  // 3 conditions
            return 1;
        }
        return 0;
        
    }
    public:
    //Function to check whether a binary tree is balanced or not.
    bool isBalanced(Node *root)
    {
        return func(root);
    }
};

```
## Solution 2: O(N)
Instead of calculation height of subtrees separately we can make a pair class .
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
 */
class HBTree{
  public:
    int height;
    bool bal;
};

HBTree isBal(Node *root){
    HBTree hb;
    if(root==NULL){
        hb.height = 0;
        hb.bal = 1;
        return hb;
    }
    
    HBTree h1 = isBal(root->left);
    HBTree h2 = isBal(root->right);
    hb.height = max(h1.height,h2.height)+1;
    
    if(abs(h1.height-h2.height)<=1 && h1.bal && h2.bal){
        hb.bal = 1;
    }
    
    else{
        hb.bal = 0;
    }
    
    return hb;
}

class Solution{
    public:
    //Function to check whether a binary tree is balanced or not.
    bool isBalanced(Node *root)
    {
        HBTree ans = isBal(root);
        return ans.bal;
        
    }
};
```

#include <iostream>
#include <vector>
#include <string>
#include <set>

using namespace std;

string longestPalindrome(string s) {
    set<string> st;
    int n = s.length();
    vector<vector<bool>> dp(n, vector<bool>(n));
    int start = 0;
    int max_len = 1;

    // Initialize diagonal to true
    for (int i = 0; i < n; i++) {
        dp[i][i] = true;
    }

    // Check for palindromes of length 2 or more
    for (int length = 2; length <= n; length++) {
        for (int i = 0; i <= n-length; i++) {
            int j = i + length - 1;
            if (s[i] == s[j] && (length == 2 || dp[i+1][j-1])) {
                dp[i][j] = true;
                if (length > max_len || (length == max_len && i < start)) {
                    max_len = length;
                    start = i;
                    
                }

            }
        }
    }
    
    // string longest_palindrome = s.substr(start, max_len);
    // st.insert(longest_palindrome);
    for(auto x: dp){
        for(auto y : x){
            cout<< y<<" ";
        }
        cout<<"\n";
    }

    // string longest_palindrome = s.substr(start, max_len);

    // Check all substrings of length max_len starting from start to find lexicographically smallest one
    for (int i = start+1; i <= n-max_len-1; i++) {
        string substring = s.substr(i, max_len);
        int j = i+max_len-1;
        if(dp[i][j]==1){
            st.insert(substring);
        }
    }
    string res;
    auto x = st.begin();
    res = *x;

    return res;
}

int main() {
    string s1 = "babad";
    cout << longestPalindrome(s1) << endl; // expected output: "aba"

    string s2 = "cbbd";
    cout << longestPalindrome(s2) << endl; // expected output: "bb"

    return 0;
}

