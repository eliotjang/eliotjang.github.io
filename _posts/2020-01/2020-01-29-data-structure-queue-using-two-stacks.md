---
title: "[queue] Queue using Two Stacks, HackerRank"
excerpt: "Data Structures > Queues > Queue using Two Stacks, solution in cpp"

categories:
  - data-structure
tags:
  - cpp
  - hackerrank
  - queue

last_modified_at: 2020-01-29T22:15:00+09:00
---
![](https://eliotjang.github.io/assets/images/c++/queue-using-two-stacks.png){: .align-center}  

```cpp
#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;

int main() {
    vector<int> queue;
    int query, type, x;
    cin >> query;

    while(query) {
        cin >> type;
        switch (type)
        {
        case 1:
            cin >> x;
            queue.push_back(x);
            break;

        case 2:
            queue.erase(queue.begin());
            break;

        case 3:
            cout << queue.front() << endl; // or queue[0]
            break;

        default:
            return 1;
        }
        query--;
    }
    return 0;
}
```

**IMP POINT:**  

Think **devide type** and what will you gonna do!

Thank you for visiting my blog :D
