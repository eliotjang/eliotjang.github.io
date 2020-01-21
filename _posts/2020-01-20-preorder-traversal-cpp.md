---
title: "[Tree] Preorder Traversal, HackerRank"
excerpt: "Data Structures > Trees > Tree: Preorder Traversal, solution in cpp"

categories:
  - algorithm
tags:
  - algorithm
  - hackerrank
  - c++
  - cpp
  - tree
  - preorder traversal
  - data structure
last_modified_at: 2020-01-20T22:25:00+09:00
---

![](https://eliotjang.github.io/assets/images/c++/preorder-traversal.png){: .align-center}  

```cpp
#include <iostream>
#include <cstddef>

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

    void preOrder(Node *root) {
	std::cout << root->data << ' ';
        if (!(root->left==NULL)) {
            this->preOrder(root->left);
        }
        if (!(root->right==NULL)) {
            this->preOrder(root->right);
        }
    }

};

int main() {
    
    Solution myTree;
    Node* root = NULL;

    std::string str_data;
    int data;
    int count = 0;

    while(std::cin >> str_data) {
        if(str_data.compare("\\")==0 || str_data.compare("/")==0) continue;
        count++;
        data = stoi(str_data);
        root = myTree.insert(root, data);
    }

    myTree.preOrder(root);

    return 0;
}

```

**Key Point**  
Thinks that Class can refer function **ownself**.  
:D

**Have to know before you coding**  
You don't need to touch except preOrder().  
I modified main() for debugging on VS Code.  

