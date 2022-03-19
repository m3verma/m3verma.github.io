---
layout: default
---

# Fill the Bucket (FBC)

Chef has a bucket having a capacity of K liters. It is already filled with X liters of water.

Find the maximum amount of extra water in liters that Chef can fill in the bucket without overflowing.

### Input Format
The first line will contain T - the number of test cases. Then the test cases follow.

The first and only line of each test case contains two space separated integers K and X - as mentioned in the problem.

### Output
For each test case, output in a single line, the amount of extra water in liters that Chef can fill in the bucket without overflowing.

### Constraints
1≤T≤100<br>
1≤X<K≤1000

### Example
```
2
5 4
15 6
```

```
1
9
```

### Explanation
Test Case 1: The capacity of the bucket is 5 liters but it is already filled with 4 liters of water. Adding 1 more liter of water to the bucket fills it to (4+1)=5 liters. If we try to fill more water, it will overflow.

Test Case 2: The capacity of the bucket is 15 liters but it is already filled with 6 liters of water. Adding 9 more liters of water to the bucket fills it to (6+9)=15 liters. If we try to fill more water, it will overflow.

## Solution

A simple subtraction solves the problem. The bucket is filled with X liters. The capacity is K liters. Hence water needed will be equal to (K-X) liters.

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
	    cout<<n-k<<endl;
	}
	return 0;
}
```

The above solution passes as expected.
