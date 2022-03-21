---
layout: default
---

# Chef and Gift (PRGIFT)

Today is chef's friend's birthday. He wants to give a gift to his friend. So he was desperately searching for some gift here and there.
Fortunately, he found an array a of size n lying around. The array contains positive integers. Chef's friend likes even numbers very much. So for the gift, chef will choose a consecutive non-empty segment of the array. The segment should contain exactly k even integers. Though it can have any number of odd integers. He will then pick that segment and gift it to his friend.
But there is a problem. It might not be always possible for the chef to choose such a segment. Please tell whether it is possible for chef to select some gift or not?

### Input Format
First line of the input contains a single integer T denoting number of test cases.
For each test case, first line contains two space separated integers n, k.
Next line contains n space separated integers denoting content of array a.
It is also guaranteed that all the numbers in the array a are distinct.

### Output
For each test case, print a single line containing "YES" or "NO" (without quotes) corresponding to the situation.

### Constraints
1 ≤ T ≤ 10<br>
1 ≤ n ≤ 50<br>
0 ≤ k ≤ n<br>
1 ≤ a i ≤ 100

### Example
```
4
2 1
1 2
3 2
2 6 5
3 3
2 4 5
4 2
1 2 4 5
```

```
YES
YES
NO
YES
```

### Explanation
For  first  test case, we can select a[2, 2] = {2}. 

For second  test case, we can select a[1, 2] = {2, 6}. 

For third  test case, we can not select any consecutive segment having exactly 3 even numbers. 

For fourth  test case, we can select a[2, 3] = {2, 4}. 

## Solution

A simple counting of even numbers in the array solves the problem. So if amount of even numbers are less then 'k' then chef cant gift else he can gift. One tricky solution is when k is zero and array contains all even numbers. By above logic it will print 'YES' but it should print 'NO'. Since a gift cant be made with no even elements when array has only even elements.

```c++
#include <iostream>
using namespace std;

int main() {
	int t;
	cin>>t;
	while(t--)
	{
	    int n,k;
	    cin>>n>>k;
	    int count=0;
	    for(int i=0;i<n;i++)
	    {
	        int temp;
	        cin>>temp;
	        if(temp%2==0)
	            count++;
	    }
	    if(k==0&&n==count)
	        cout<<"NO"<<endl;
	    else if(count>=k)
	        cout<<"YES"<<endl;
	    else
	        cout<<"NO"<<endl;
	}
	return 0;
}
```

The above solution passes as expected.
