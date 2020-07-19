---
title: "[Stack] Maximum Element, HackerRank"
excerpt: "Data Structures > Stacks > Maximum Element, solution in cpp"

categories:
  - data-structure
tags:
  - cpp
  - hackerrank
  - stack

last_mdoified_at: 2020-01-18T20:20:00+09:00
---

![](https://eliotjang.github.io/assets/images/c++/maximum-element.png){: .align-center}  


```cpp  
#include <iostream>
#include <algorithm>
#include <array>

using namespace std;  

int main()
{
  array<int, 100000> stack = {0};  
  array<int, 100000> maxArray = {0};  
  
  int N = 1;  
  int type;  
  int tmp;  
  int index = 0;  
  int maxIndex = -1;  
  int max;  

  cin >> N;  

  for (int i=0; i<N; i+) {  
    cin >> type;  
    switch (type) {  
      case 1:  
	cin >> tmp;  
	if (maxArray[maxIndex] <= tmp) {  
	maxIndex++;  
	maxArray[maxIndex] = tmp;  
	}  
	stack[index] = tmp;  
	index++;  
	break;  
      case 2:  
	index--;  
	if (maxArray[maxIndex] == stack[index]) {  
	maxArray[maxIndex] = 0;  
	maxIndex--;  
	}  
	stack[index] = 0;  
	break;  
      case 3:  
	cout << maxArray[maxIndex] << endl;  
	break;  
      default:  
	break;  
    }  
  }  
}  
```

### **Key Point**  
N can be up to ten million.  
So it must write a code that is time efficient.  

I wrote two arrays for time efficient.  

And It could not only array but also stack, original array data structure.  

Thanks for watch my code review.  
**:D**



