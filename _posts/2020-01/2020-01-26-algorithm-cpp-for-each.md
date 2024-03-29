---
title: "[algorithm] C++ for_each"
excerpt: "hycszero.tistory.com/81"

categories:
  - algorithm
tags:
  - cpp

last_modified_at: 2020-01-26T21:20:00T09:00
---
### 1. Traditonal for()
```cpp
for (int i = 0; i < 10; i++)
{
  std::cout << i << std::endl;
}
```
Commonly used for function in C++.  
We declare integer and iterate this integer.  

### 2. for() additional in C++11  
```cpp
for (int i : {0, 1, 2, 3, 4, 5})
{
  std::cout << i << std::endl;
}
```
```cpp
std::vector<std::string> name_vector;
for (const auto& element : name_vector)
{
  std::cout << element << std::endl;
}
```  
We called **Range-based for loop**.  
Used Iterator internally, executing all elements in container like vector.  
Looking neat is the biggest advantage.  
Performance is same as traditinal for.  

### 3. std::for_each()  
```cpp
#include <algorithm>
std::vector<std::string> name_vector{ "test1", "test2", "test3" };
std::for_each(name_vector.begin(), name_vector.end(), [](auto& input) {std::cout << input << std::endl; });
```
```cpp
#include <algorithm>
void print(std::string& input)
{
  std::cout << input << std::endl;
}
std::vector<std::string> name_vector{ "test1", "test2", "test3" };
std::for_each(name_vector.begin(), name_vector.end(), print);
```  
for_each is defined at **algorithm** header.  
So you should wirte std::for_each form.  
received parameter is **start iterator, end iterator, function**.  
Last parameter can use lamda form.  

### 4. std::transform()  
```cpp  
#include <algorithm>
std::string change(std::string& input)
{
  return std::string("Changed!");
}
std::vector<std::string> name_vector{ "test1", "test2", "test3" };
std::vector<std::string> result{ "result1", "result2", "result3" };  
std::transform(name_vector.begin(), name_vector.end(), result.begin(), change);
//result = {"Changed!", "Changed!", "Changed!"}
```  
transform also used iterator.  
Stored result to another container.  
Of course, stored in original container.  
Last parameter can use lamda form.  

**TODO: std::for_each-n(C++17)**  


