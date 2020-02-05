---
title: "[Unsolved] Tree: Level Order Traversal, HackerRank"
excerpt: "Data Structures > Trees > Tree: Level Order Traversal"

categories:
  - algorithm
tags:
  - algorithm
  - unsolved
---

<https://www.hackerrank.com/challenges/tree-level-order-traversal/problem?isFullScreen=false>  

![](https://eliotjang.github.io/assets/images/unsolved/level-order-traversal.png){: .align-center}  

```cpp
#include <bits/stdc++.h>

using namespace std;
	
class Node {
    public:
        int data;
        Node *left;
        Node *right;
        Node(int d) {
            data = d;
            left = NULL;
            right = NULL;
        }
};

class Solution {
    public:
  		Node* insert(Node* root, int data) {
            if(root == NULL) {
                return new Node(data);
            } else {
                Node* cur;
                if(data <= root->data) {
                    cur = insert(root->left, data);
                    root->left = cur;
                } else {
                    cur = insert(root->right, data);
                    root->right = cur;
                }

               return root;
           }
        }
/*
class Node {
    public:
        int data;
        Node *left;
        Node *right;
        Node(int d) {
            data = d;
            left = NULL;
            right = NULL;
        }
};
*/

    void levelOrder(Node * root) {
        static queue<int> q;
        while (root->left || root->right)
        {
            if(!(root->left) && root->right) {
                q.push(root->data);
                root = root->right;
                continue;
            }
            if(!(root->right) && root->left) {
                q.push(root->data);
                root = root->left;
                continue;
            }

            if(root->right && root->left) {
                Node *tmp1 = root->left;
                Node *tmp2 = root->right;
                while (tmp1 || tmp2)
                {
                    if(tmp1) {
                        
                    }
                }
                
            }

        }
        cout << q.front() << " ";
        q.pop();
    }

}; //End of Solution

int main() {
    
    Solution myTree;
    Node* root = NULL;
    
    int t;
    int data;

    std::cin >> t;

    while(t-- > 0) {
        std::cin >> data;
        root = myTree.insert(root, data);
    }
  
    myTree.levelOrder(root);

    return 0;
}

```  

TODO: check level order traversal concept
