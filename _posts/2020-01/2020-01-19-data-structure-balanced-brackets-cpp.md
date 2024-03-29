---
title: "[Stack] Balanced Brackets, HackerRank"
excerpt: "Data Structures > Stacks > Balanced Brackets, solution in cpp"

categories:
  - data-structure
tags:
  - cpp
  - hackerrank
  - stack
last_modified_at: 2020-01-19T21:00:00+09:00
---
![](https://eliotjang.github.io/assets/images/c++/balanced-brackets.png){: .align-center}  

```cpp
#include <iostream>
#include <fstream>
#include <stack>
#include <algorithm>

using namespace std;

string isBalanced(string s) {
    stack<char> bracketStack;
    char tmp = '?';
    for(char const &c: s) {
        switch (c)
        {
        case '(':
            bracketStack.push('(');
            break;
        case '{':
            bracketStack.push('{');
            break;
        case '[':
            bracketStack.push('[');
            break;
        case ')':
            if (bracketStack.empty())
                return "NO";
            tmp = bracketStack.top();
            if ('(' == tmp) {
                bracketStack.pop();
            }
            else return "NO";
            break;
        case '}':
            if (bracketStack.empty())
                return "NO";
            tmp = bracketStack.top();
            if ('{' == tmp) {
                bracketStack.pop();
            }
            else return "NO";
            break;
        case ']':
            if (bracketStack.empty())
                return "NO";
            tmp = bracketStack.top();
            if ('[' == tmp) {
                bracketStack.pop();
            }
            else return "NO";
            break;

        default:
            break;
        }
    }
    if (bracketStack.empty())
        return "YES";
    else
        return "NO";
}

int main()
{
    // for my Mac Preferences
    /*string name = "/Users/eliotjang/Desktop/Algorithm/HackerRank/Algorithms/Stacks/Balanced_Brackets_archive/sample_output.txt";
    ofstream fout(name);*/
    ofstream fout(getenv("OUTPUT_PATH"));

    int t;
    cin >> t;
    cin.ignore(numeric_limits<streamsize>::max(), '\n');

    for (int t_itr = 0; t_itr < t; t_itr++) {
        string s;
        getline(cin, s);

        string result = isBalanced(s);

        fout << result << "\n";
    }

    fout.close();

    return 0;
}
```  

**Key Point**  
Input(a string of brackets) doesn't have _regularity_.  

So code should tested  *All Exceptions*.  
Thank you for visiting my blog.  
:D
