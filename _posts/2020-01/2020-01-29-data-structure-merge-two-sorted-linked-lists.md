---
title: "[linked List] Merge two sorted linked lists, HackerRank"
excerpt: "Data Structures > Linked Lists > Merge two sorted linked lists, solution in cpp"

categories:
  - data-structure
tags:
  - cpp
  - hackerrank
  - linked-list

last_modified_at: 2020-01-29T20:35:00+09:00
---
![](https://eliotjang.github.io/assets/images/c++/merge-two-sorted-linked-lists.png){: .align-center}  

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
                this->tail->next = node;
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

// Complete the mergeLists function below.
SinglyLinkedListNode* mergeLists(SinglyLinkedListNode* head1, SinglyLinkedListNode* head2) {
    SinglyLinkedList* result = new SinglyLinkedList();
    while (head1 || head2) {
        if (!(head1)) {
            while (head2) {
                result->insert_node(head2->data);
                head2 = head2->next;
            }
            break;
        }
        else if (!(head2)) {
            while (head1) {
                result->insert_node(head1->data);
                head1 = head1->next;
            }
            break;
        }
        else if (head1->data <= head2->data) {
            result->insert_node(head1->data);
            head1 = head1->next;
        }
        else { //head1->data > head2->data
            result->insert_node(head2->data);
            head2 = head2->next;
        }
    }
    return result->head;
}

int main()
{
    // For myMac Preferences
    //string name = "/Users/eliotjang/Desktop/Algorithm/HackerRank/Data_Structures/Linked Lists/Merge_two_sorted_linked_lists_archive/sample_output.txt";
    //ofstream fout(name);
    ofstream fout(getenv("OUTPUT_PATH"));

    int tests;
    cin >> tests;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    for (int tests_itr = 0; tests_itr < tests; tests_itr++) {
        SinglyLinkedList* llist1 = new SinglyLinkedList();

        int llist1_count;
        cin >> llist1_count;
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        for (int i = 0; i < llist1_count; i++) {
            int llist1_item;
            cin >> llist1_item;
            cin.ignore(numeric_limits<streamsize>::max(), '\n');

            llist1->insert_node(llist1_item);
        }
      
      	SinglyLinkedList* llist2 = new SinglyLinkedList();

        int llist2_count;
        cin >> llist2_count;
        cin.ignore(numeric_limits<streamsize>::max(), '\n');

        for (int i = 0; i < llist2_count; i++) {
            int llist2_item;
            cin >> llist2_item;
            cin.ignore(numeric_limits<streamsize>::max(), '\n');

            llist2->insert_node(llist2_item);
        }

        SinglyLinkedListNode* llist3 = mergeLists(llist1->head, llist2->head);

        print_singly_linked_list(llist3, " ", fout);
        fout << "\n";

        free_singly_linked_list(llist3);
    }

    fout.close();

    return 0;
}
```

**IMP POINT:**  

Remind that you have to use pointer well.  
**Don't lose** where the pointer **points**.  

Thank you for visiting my blog :D
