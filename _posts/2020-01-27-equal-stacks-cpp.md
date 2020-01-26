---
title: "[Stack] Equal Stacks, HackerRank"
excerpt: "Data Structures > Stacks > Equal Stacks, solution in cpp"

categoreis:
  - algorithm
tags:
  - algorithm
  - equal stacks
  - c++
  - hackerrank
  - stack
last_modified_at: 2020-01-27T04:30:00+09:00
---
![](https://eliotjang.github.io/assets/images/c++/equal-stacks.png){: .align-center}  

```cpp
#include <bits/stdc++.h>

using namespace std;

vector<string> split_string(string);

void swap(int& a, int& b)
{
    int temp = a;
    a = b;
    b = temp;
}

int equalStacks(vector<int> h1, vector<int> h2, vector<int> h3) {
    int t1 = accumulate(h1.begin(), h1.end(), 0);
    int t2 = accumulate(h2.begin(), h2.end(), 0);
    int i1=0, i2=0, i3=0;
    if (t1 < t2) {
        h1.swap(h2);
        swap(t1, t2);
    }
    int t3 = accumulate(h3.begin(), h3.end(), 0);
    if (t1 > t3 && t2 > t3) {
        // nothing works.
    }
    else if (t1 > t3 && t2 < t3) {
        h2.swap(h3);
        swap(t2,t3);
    }
    else if (t1 < t3 && t2 < t3) {
        h1.swap(h3);
        swap(t1,t3);
    }
    else if (t1 == t3) {
        h2.swap(h3);
        swap(t2,t3);
    }
    while (!(t1 == t2 && t2 == t3)) {
        while(t1 > t2) {
            t1 -= h1[i1];
            i1++;
        }
        while(t2 > t3) {
            t2 -= h2[i2];
            i2++;
        }
        if (t1 < t2) {
            t2 -= h2[i2];
            i2++;
        }
        if (t1 == t2) {
            while (t3 > t1) {
                t3 -= h3[i3];
                i3++;
            }
        }
    }
    return t1;
}

int main()
{
    ofstream fout(getenv("OUTPUT_PATH"));
    // For Mac Preferences
    //string name = "/Users/eliotjang/Desktop/Algorithm/HackerRank/Data_Structures/Equal_Stacks_archive/sample_output.txt";
    //ofstream fout(name);

    string n1N2N3_temp;
    getline(cin, n1N2N3_temp);

    vector<string> n1N2N3 = split_string(n1N2N3_temp);

    int n1 = stoi(n1N2N3[0]);

    int n2 = stoi(n1N2N3[1]);

    int n3 = stoi(n1N2N3[2]);

    string h1_temp_temp;
    getline(cin, h1_temp_temp);

    vector<string> h1_temp = split_string(h1_temp_temp);

    vector<int> h1(n1);

    for (int h1_itr = 0; h1_itr < n1; h1_itr++) {
        int h1_item = stoi(h1_temp[h1_itr]);

        h1[h1_itr] = h1_item;
    }

    string h2_temp_temp;
    getline(cin, h2_temp_temp);

    vector<string> h2_temp = split_string(h2_temp_temp);

    vector<int> h2(n2);

    for (int h2_itr = 0; h2_itr < n2; h2_itr++) {
        int h2_item = stoi(h2_temp[h2_itr]);

        h2[h2_itr] = h2_item;
    }

    string h3_temp_temp;
    getline(cin, h3_temp_temp);

    vector<string> h3_temp = split_string(h3_temp_temp);

    vector<int> h3(n3);

    for (int h3_itr = 0; h3_itr < n3; h3_itr++) {
        int h3_item = stoi(h3_temp[h3_itr]);

        h3[h3_itr] = h3_item;
    }

    int result = equalStacks(h1, h2, h3);

    fout << result << "\n";

    fout.close();

    return 0;
}

vector<string> split_string(string input_string) {
    string::iterator new_end = unique(input_string.begin(), input_string.end(), [] (const char &x, const char &y) {
        return x == y and x == ' ';
    });

    input_string.erase(new_end, input_string.end());

    while (input_string[input_string.length() - 1] == ' ') {
        input_string.pop_back();
    }

    vector<string> splits;
    char delimiter = ' ';

    size_t i = 0;
    size_t pos = input_string.find(delimiter);

    while (pos != string::npos) {
        splits.push_back(input_string.substr(i, pos - i));

        i = pos + 1;
        pos = input_string.find(delimiter, i);
    }

    splits.push_back(input_string.substr(i, min(pos, input_string.length()) - i + 1));

    return splits;
}

```  

### **IMP POINT**  
Keep in mind what **cylinder** will you **remove first** and when will stop the conditions?  

**Don't think** about **time execution**. Just think about solving problem.  

