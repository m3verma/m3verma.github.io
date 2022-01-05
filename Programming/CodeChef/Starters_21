---
layout: default
---

# 1. Devendra and Water Sports (DEVSPORTS)

Recently, Devendra went to Goa with his friends. Devendra is well known for not following a budget.

He had Rs. Z at the start of the trip and has already spent Rs. Y on the trip. There are three kinds of water sports one can enjoy, with prices Rs. A, B, and C. He wants to try each sport at least once.

If he can try all of them at least once output YES, otherwise output NO.

### Input Format
The first line of input contains a single integer T, denoting the number of test cases. The description of T test cases follows.

Each test case consists of a single line of input containing five space-separated integers Z,Y,A,B,C.

### Output
For each test case, print a single line containing the answer to that test case — YES if Devendra can try each ride at least once, and NO otherwise.

You may print each character of the string in uppercase or lowercase (for example, the strings "yEs", "yes", "Yes" and "YES" will all be treated as identical).

### Constraints
1≤T≤10
10<sup>4</sup>≤Z≤10<sup>5</sup>
0≤Y≤Z
100≤A,B,C≤5000

### Example
```
3
25000 22000 1000 1500 700
10000 7000 100 300 500
15000 5000 2000 5000 3000
```

```
NO
YES
YES
```

### Explanation
Test Case 1: After spending Rs. 22000 on the trip already, Devendra is left with Rs. 3000. The water sports cost a total of 1000+1500+700=3200. So, he does not have enough money left to try all the watersports.

Test Case 2: After spending Rs. 10000 on the trip, he is left with Rs. 3000. The water sports cost a total of 100+300+500=900. So, he has enough money to try all the watersports.

Test Case 3: After spending Rs. 5000 on the trip, he is left with Rs. 10000. The water sports cost a total of 2000+5000+3000=10000. So, he still has enough money left to try all the watersports.

## Solution

Initially Devandra has Z rupees. Out of that, Y rupees has already been spent. Now, the current amount he has will be Z−Y rupees.

The minimum cost to try all the sports will be A+B+C rupees.

Therefore, if Z−Y ≥ A+B+C, we output YES, else we output NO.

Since we have set a baseline lets try to code it :

```c++
#include <iostream>
using namespace std;

int main() {
	int t;
	cin>>t;
	while(t--)
	{
		int z,y,a,b,c;
		cin>>z>>y>>a>>b>>c;
		if(z-y-a-b-c>=0)
		cout<<"YES"<<endl;
		else
		cout<<"NO"<<endl;
	}
	// your code goes here
	return 0;
}
```

The above solution passes as expected.

* * *

# 2. Covid and Theatre Tickets (COVID_19)


