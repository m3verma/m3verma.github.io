---
layout: default
---

# Name Reduction (NAME1)

In an attempt to reduce the growing population, Archer was asked to come up with a plan. Archer being as intelligent as he is, came up with the following plan :

If N children, with names C1,C2,...,CN, are born to parents with names A and B, and you consider C to be the concatenation of all the names of the children, i.e. C=C1+C2+...+CN (where + is concatenation operator), then C should be a substring of one of the permutations of A+B.

You are given the task to verify whether the names parents propose to give their children are in fact permissible by Archer's plan or not.

### Input Format
The first line contains an integer T, the number of test cases. T test cases follow. Each test case stats with a line containing two space separated strings A and B, denoting the names of the parents. The next line contains a single integer N denoting the number of children A and B are planning to have. Following this are N lines, the ith line containing Ci, the proposed name for the ith child.

### Output
For each test case output a single line containing YES if the names are permissible by Archer's plan, otherwise print NO.

### Constraints
1≤T≤100 <br>
1≤N≤1000<br>
The lengths of all the strings including A, B, and all Ci will be in the range [1,40000], both inclusive. All these strings will contain only lowercase English letters.<br>
The combined lengths of all names of children will not exceed the combined length of the names of their parents.<br>

### Example
```
3
tom marvoloriddle
2
lord
voldemort
cheap up
1
heapcup
bruce wayne
2
bat
man
```

```
YES
YES
NO
```

### Explanation
Let Y denote the concatenation of names of all the children, and X denote the concatenation of the names of the parents.

Case 1: Here X = "tommarvoloriddle", and Y = "lordvoldemort". Consider Z = "iamlordvoldemort". It is not difficult to see that Z is a permutation of X and Y is a substring of Z. Hence Y is a substring of a permutation of X, so the answer is "YES".

Case 2: Here X = "cheapup", and Y = "heapcup". Since Y in itself is a permutation of X, and as every string is a substring of itself, Y is a substring of X and also a permutation of X. Hence "YES".

Case 3: Here X = "brucewayne", and Y = "batman". As "t" is not present in X, "t" wont be present in any permutation of X, hence the answer is "NO".

## Solution

Here we are using a counting array logic. Since we are making permutations of parents (Let's say a and b), if child contains more characters then parents or contains a character which is not in parents name then it should return no. So here I am counting all occurences of all characters in parents name. Then while accessing the childs name one by one I am subtracting it from the parents frequency. If at any time it becomes negative then return NO else return YES.

```c++
#include <iostream>
using namespace std;

int main() {
	int t;
	cin>>t;
	while(t--)
	{
	    string a,b;
	    bool flag=true;
	    cin>>a>>b;
	    int n;
	    cin>>n;
	    int count[26]={0};
	    for(int i=0;i<a.size();i++)
	    {
	        count[a[i]-'a']++;
	    }
	    for(int i=0;i<b.size();i++)
	    {
	        count[b[i]-'a']++;
	    }
	    while(n--)
	    {
	        string z;
	        cin>>z;
	        for(int i=0;i<z.size();i++)
	        {
	            if(count[z[i]-'a']==0)
	            {
	                flag=false;
	            }
	            count[z[i]-'a']--;
	        }
	    }
	    if(flag)
	        cout<<"YES"<<endl;
	    else
	        cout<<"NO"<<endl;
	}
	return 0;
}
```

The above solution passes as expected.
