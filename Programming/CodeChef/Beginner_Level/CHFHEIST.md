---
layout: default
---

# Bella ciao (CHFHEIST)

Chef is planning a heist in the reserve bank of Chefland. They are planning to hijack the bank for D days and print the money. The initial rate of printing the currency is P dollars per day and they increase the production by Q dollars after every interval of d days. For example, after d days the rate is P+Q dollars per day, and after 2d days the rate is P+2Q dollars per day, and so on. Output the amount of money they will be able to print in the given period.

### Input Format
The first line contains an integer T, the number of test cases. Then the test cases follow.

Each test case contains a single line of input, four integers D,d,P,Q.

### Output
For each test case, output in a single line the answer to the problem.

### Constraints
1≤T≤10<sup>5</sup><br>
1≤d≤D≤10<sup>6</sup><br>
1≤P,Q≤10<sup>6</sup>

### Example
```
3
2 1 1 1
3 2 1 1
5 2 1 2
```

```
3
4
13
```

### Explanation
Test Case 1:
On the first day, the rate of production is 1 dollar per day so 1 dollar is printed on the first day.
On the second day, the rate of production is 1+1=2 dollars per day so 2 dollars are printed on the second day.
The total amount of money printed in 2 days is 1+2=3 dollars.

Test Case 2:
For the first two days, the rate of production is 1 dollar per day so 1x2=2 dollars are printed on the first two days.
On the third day, the rate of production is 1+1=2 dollars per day so 2 dollars are printed on the third day.
The total amount of money printed in 3 days is 2+2=4 dollars.

Test Case 3:
For the first two days, the rate of production is 1 dollar per day so 1x2=2 dollars are printed on the first two days.
On the next two days, the rate of production is 1+2=3 dollars per day so 3x2=6 dollars are printed on the next two days.
On the last day, the rate of production is 3+2=5 dollars per day so 5 dollars are printed on the last day.
The total amount of money printed in 5 days is 2+6+5=13 dollars.

## Solution

For subtask 1, we can iterate up to D, maintaining track of the number of dollars earned and current earning per day, so I’ll focus on complete solution.

Let us see what the sequence looks like with D = 11, d = 4 for some P and Q

| Day        | Dollars          |
|:-------------|:------------------|
| 1 | P |
| 2 | P |
| 3 | P |
| 4 | P |
| 5 | P+Q |
| 6 | P+Q |
| 7 | P+Q |
| 8 | P+Q |
| 9 | P+2xQ |
| 10 | P+2xQ |
| 11 | P+2xQ |

We can see that the same value appears in groups of d days, except the last group which may have less than d elements. Let’s ignore those now.

Let’s consider these groups, we can see that the terms P, P+Q, P+2xQ form an arithmetic progression.

We need to compute dxP+dx(P+Q)+dx(P+2xQ) ... = dx(P+(P+Q)+(P+2xQ)+ ... )

What is the number of groups? In the above example, we had two groups, one with value P and one with value P+Q(Remember that we are ignoring the last group, which has less than d elements).

In the general case, we can see by basic maths that there are exactly N = D/d complete groups, and additionally, if D is not a multiple of d, there would be an incomplete group with D%d elements.

Hence, ignoring last D%d days, we can write sum of dollars earned as dx APsum(P, Q, D/d) where APsum(a, d, n) denotes the sum of first n terms of arithmetic progression with first term a and common difference d, given by formula nx(2xa+(n-1)xd)/2 

For the last D%d days, the amount earned would be the same value, which is P+NxQ, since that is a part of (N+1)-th group. Hence, we can add (D%d) x (P+NxQ) to account for dollars earned in last D%d days.

```c++
#include <iostream>
using namespace std;

int main() {
	long long int t;
	cin>>t;
	while(t--)
	{
	    long long int D,d,p,q;
	    cin>>D>>d>>p>>q;
	    long long int count=0,group=0,rem=0;
	    group=D/d;
	    rem=D%d;
	    count=d*(group*(2*p+(group-1)*q))/2;
	    count+=rem*(p+(group*q));
	    cout<<count<<endl;
	    
	}
	return 0;
}
```

The above solution passes as expected.
