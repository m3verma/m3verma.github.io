---
layout: default
---

# Cutting Recipes (RECIPE)

The chef has a recipe he wishes to use for his guests, but the recipe will make far more food than he can serve to the guests. The chef therefore would like to make a reduced version of the recipe which has the same ratios of ingredients, but makes less food. The chef, however, does not like fractions. The original recipe contains only whole numbers of ingredients, and the chef wants the reduced recipe to only contain whole numbers of ingredients as well. Help the chef determine how much of each ingredient to use in order to make as little food as possible.

### Input Format
Input will begin with an integer T, the number of test cases. Each test case consists of a single line. The line begins with a positive integer N, the number of ingredients. N integers follow, each indicating the quantity of a particular ingredient that is used.

### Output
For each test case, output exactly N space-separated integers on a line, giving the quantity of each ingredient that the chef should use in order to make as little food as possible.

### Constraints
T ≤ 100<br>
2 ≤ N ≤ 50<br>
All ingredient quantities are between 1 and 1000, inclusive.

### Example
```
3
2 4 4
3 2 3 4
4 3 15 9 6
```

```
1 1
2 3 4
1 5 3 2
```

## Solution

Its a very simple problem. Just store all numbers in an array. Find GCD of all the numbers using inbuilt C++ library. Then print whole array where each element is divided by GCD.

```c++
#include <iostream>
#include<algorithm>
using namespace std;

int main() {
	int t;
	cin>>t;
	while(t--)
	{
	    int n,ans,i;
	    cin>>n;
	    int a[n];
	    for(i=0;i<n;i++)
	        cin>>a[i];
	    ans=a[0];
	    for(i=1;i<n;i++)
	        ans=__gcd(ans,a[i]);
	    for(i=0;i<n;i++)
	        cout<<a[i]/ans<<" ";
	    cout<<endl;
	}
	return 0;
}
```

The above solution passes as expected.
