---
layout: default
---

# Tax Slabs (SLAB)

In India, every individual is charged with income tax on the total income each year. This tax is applied to specific ranges of income, which are called income tax slabs. The slabs of income tax keep changing from year to year. This fiscal year (2020-21), the tax slabs and their respective tax rates are as follows:

| Total income (in rupees) | Tax rate |
|:-------------|:------------------|
| up to Rs. 250,000	| 0% |
| from Rs. 250,001 to Rs. 500,000	| 5% |
| from Rs. 500,001 to Rs. 750,000	| 10% |
| from Rs. 750,001 to Rs. 1,000,000	| 15% |
| from Rs. 1,000,001 to Rs. 1,250,000	| 20% |
| from Rs. 1,250,001 to Rs. 1,500,000	| 25% |
| above Rs. 1,500,000 |	30% |

See the sample explanation for details on how the income tax is calculated.

You are given Chef's total income: N rupees (Rs.). Find his net income. The net income is calculated by subtracting the total tax (also called tax reduction) from the total income. Note that you do not need to worry about any other kind of tax reductions, only the one described above.

### Input Format
The first line of the input contains a single integer T denoting the number of test cases. The description of T test cases follows.

The first and only line of each test case contains a single integer N.

### Output
For each test case, print a single line containing one integer — Chef's net income.

### Constraints
1≤T≤10<sup>3</sup><br> 
0≤N≤10<sup>7</sup><br>
N is a multiple of 100

### Example
```
2
600000
250000
```

```
577500
250000
```

### Explanation
Example case 1: We know that the total income is Rs. 6 lakh (1 lakh rupees = 105 rupees). The total tax for each slab is calculated as follows:
Up to 2.5 lakh, the tax is Rs. 0, since the tax rate is 0 percent.
From above Rs. 2.5 lakh to Rs. 5 lakh, the tax rate is 5 percent. Therefore, this tax is 0.05⋅(500,000−250,000), which is Rs. 12,500.
From above Rs. 5 lakh to Rs. 6 lakh, the tax rate is 10 percent. Therefore, this tax is 0.10⋅(600,000−500,000), which is Rs. 10,000.
Summing them up, we get that the total tax on Chef's whole income is Rs. 22,500. Since the net income is the total income minus the tax reduction, it is Rs. 600,000 minus Rs. 22,500, which is Rs. 577,500.
<br>
Example case 2: For income up to Rs. 2.5 lakh, we have no tax, so the net income is the same as the total income: Rs. 2.5 lakh.

## Solution

Net income = Total income - Total Tax
Using if-else ladder, find the person’s income bracket. Calculate the tax for this slab and add the taxes for all previous slabs. Subtract the total tax from the income to get the net income.

```c++
#include <iostream>
#include<bits/stdc++.h>
using namespace std;

int main() {
	int t;
	cin>>t;
	while(t--)
	{
	    int N,tax=0;
	    cin>>N;
	    if(N>250000)
	            tax+=(min(N,500000)-250000)*5/100;
	        if(N>500000)
	            tax+=(min(N,750000)-500000)*10/100;
	        if(N>750000)
	            tax+=(min(N,1000000)-750000)*15/100;
	        if(N>1000000)
	            tax+=(min(N,1250000)-1000000)*20/100;
	        if(N>1250000)
	            tax+=(min(N,1500000)-1250000)*25/100;
	        if(N>1500000)
	            tax+=(N-1500000)*30/100;

	        int net=N-tax;
	    cout<<net<<endl;
	}
	return 0;
}

```

The above solution passes as expected.
