---
title: "[Tree] Inorder Traversal, HackerRank"
excerpt: "Data Structures > Trees > Tree: Inorder Traversal, solution in cpp"

categories:
  - algorithm
tags:
  - algorithm
  - hackerrank
  - c++
  - cpp
  - tree
  - inorder traversal
  - data structure
last_modified_at: 2020-01-21T00:50:00+09:00
---

![](https://eliotjang.github.io/assets/images/c++/inorder-traversal.png){: .align-center}  

```cpp
#include <iostream>
#include <cstddef>

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

    void inOrder(Node *root) {
        if (!(root->left==NULL)) {
            this->inOrder(root->left);
        }
	cout << root->data << ' ';
        if (!(root->right==NULL)) {
            this->inOrder(root->right);
        }
    }

};

int main() {
    
    Solution myTree;
    Node* root = NULL;

    string str_data;
    int data;
    int count = 0;

    while(cin >> str_data) {
        if(str_data.compare("\\")==0 || str_data.compare("/")==0) continue;
        count++;
        data = stoi(str_data);
        root = myTree.insert(root, data);
    }

    myTree.inOrder(root);

    return 0;
}

```

**Key Point**  
Think for **refer function ownself**.  
:D

**Before you know to coding**  
You don't need to touch except inOrder().  
I modified main() for debugging on VS Code.  

