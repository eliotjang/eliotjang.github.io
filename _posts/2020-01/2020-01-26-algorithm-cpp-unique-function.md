---
title: "[algorithm] C++ STL Algorithm std::unique"
excerpt: "thispointer.com, stl-algorithm-stdunique-tutorial"

categories:
  - algorithm
tags:
  - cpp

last_modified_at: 2020-01-26T21:20:00T09:00
---
std::unique is an STL Algorithms that provide a functionality to remove all consecutive duplicate elements from a given range.  

For example, we have an array of elements i.e.  
1,2,3,3,3,4,4,5,5  

As we can see above there are 4 consecutive duplicate element in above list i.e. two 3's, one 4 and on 5.  

Using std::unique algorithm we can remove these consecutive duplicate element from a given range.  

```cpp 
std::unique (<START OF RANGE>, <END OF RANGE>);
```

But an important point to rememer is,  

std::unique will not decrease the actual size of given range i.e. it will only overwrite the duplicate elements and returns the new virtual end of range.  
For example, after running std::unique on above range actual range will become,  
1,2,3,4,5,4,4,5,5  

As we can see that only first five elements of list are unique.  

std::unique returns the new virtual end of range after removing the consecutive duplicate elements.  

For example in above range std::unique will return 5. Therefore after executing std::unique we should erase the extra elements from range i.e.  

*Remove elements from position returned by std::unique till end.*  

**IMP POINT:**  

std::unique always removes the consecutive duplicate elements. Therefore before executing std::unique we should first the sort the given range, to make sure that duplicate elements are at consecutive positions.  

Let's see how to use std::unique  

### **Erase Duplicate Elements from a Vector using std::unique**  

```cpp
#include <iostream>
#include <algorithm>
#include <vector>

int main()
{
  std::vector<int> vecOfInts = {5,3,1,6,7,6,7,6,9,0,2,3,5};

  // First Sort the given range to bring duplicate
  // elements at consecutive positions
  std::sort(vecOfInts.begin(), vecOfInts.end());

  std::vector<int>::iterator newEnd;

  // Override duplicate elements
  newEnd = std::unique(vecOfInts.begin(), vecOfInts.end());

  vecofInts.erase(newEnd, vecOfInts.end());

  std:for_each(vecOfInts.begin(), vecOfInts.end(),
	      [](int a) {
		std::cout<<a<<" , ";
	      });
  return 0;
}
```

std::unique uses the == operator to compare the elements in given range. So to remove duplicate elements from vector of class objects we should override == operator in that class.  

