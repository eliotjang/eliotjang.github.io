---
title: "[linked List] Cycle Detection, HackerRank"
excerpt: "Data Structures > Linked Lists > Cycle Detection, solution in cpp"

categories:
  - data-structure
tags:
  - cpp
  - hackerrank
  - linked-list

last_modified_at: 2020-01-30T17:10:00+09:00
---
<https://www.hackerrank.com/challenges/detect-whether-a-linked-list-contains-a-cycle/problem>  

![](https://eliotjang.github.io/assets/images/c++/cycle-detection.png){: .align-center}  

```cpp
#include <bits/stdc++.h>

using namespace std;

class SinglyLinkedListNode {
    public:
        int data;
        SinglyLinkedListNode *next;

        SinglyLinkedListNode(int node_data) {
            this->data = node_data;
            this->next = nullptr;
        }
};

class SinglyLinkedList {
    public:
        SinglyLinkedListNode *head;
        SinglyLinkedListNode *tail;

        SinglyLinkedList() {
            this->head = nullptr;
            this->tail = nullptr;
        }

        void insert_node(int node_data) {
            SinglyLinkedListNode* node = new SinglyLinkedListNode(node_data);

            if (!this->head) {
                this->head = node;
            } else {
                //If following code execute,
                this->tail->next = node;
                //this->head->next = node; also allocated.
                //I guess both node pointer points same node.
            }

            this->tail = node;
        }
};

void print_singly_linked_list(SinglyLinkedListNode* node, string sep, ofstream& fout) {
    while (node) {
        fout << node->data;

        node = node->next;

        if (node) {
            fout << sep;
        }
    }
}

void free_singly_linked_list(SinglyLinkedListNode* node) {
    while (node) {
        SinglyLinkedListNode* temp = node;
        node = node->next;

        free(temp);
    }
}

// Complete the has_cycle function below.
bool has_cycle(SinglyLinkedListNode* head) {
    int count = 1000;
    while(count) {
        if (!(head)) return false;
        head = head->next;
        count--;
    }
    return true;
}

int main()
{
    string name = "/Users/eliotjang/Desktop/Algorithm/HackerRank/Data_Structures/Linked Lists/Cycle_Detection_archive/sample_output.txt";
    ofstream fout(name);
    //ofstream fout(getenv("OUTPUT_PATH"));

    int tests;
    cin >> tests;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    for (int tests_itr = 0; tests_itr < tests; tests_itr++) {
        int index;
        cin >> index;
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        SinglyLinkedList* llist = new SinglyLinkedList();

        int llist_count;
        cin >> llist_count;
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        for (int i = 0; i < llist_count; i++) {
            int llist_item;
            cin >> llist_item;
            cin.ignore(numeric_limits<streamsize>::max(), '\n');

            llist->insert_node(llist_item);
        }
      
      	SinglyLinkedListNode* extra = new SinglyLinkedListNode(-1);
      	SinglyLinkedListNode* temp = llist->head;
      
      	for (int i = 0; i < llist_count; i++) {
            if (i == index) {
          		extra = temp;
            }
          	
          	if (i != llist_count-1) {
          		temp = temp->next;
            }
        }
      
      	temp->next = extra;

        bool result = has_cycle(llist->head);

        fout << result << "\n";
    }

    fout.close();

    return 0;
}
```

**KEY NOTE**  

If linked list have a cycle, it will ciculation till computer **shut down.**  
However, the problem have a **limitied size.**  
So you can think about that.  

- - -
Thank you for visiting my blog.  
