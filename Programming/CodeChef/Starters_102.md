---
layout: default
---

 <script type="text/x-mathjax-config">
    MathJax.Hub.Config({
      tex2jax: {
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
        inlineMath: [['$','$']]
      }
    });
  </script>
  <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script> 

# 1. Sports Section (NEWSPAPER)

The newspaper in Chefland consists of 10 pages numbered 1 to 10. The last 3 pages of the newspaper are always dedicated to the sports section. Given a random page number X, determine whether that page is dedicated to the sports section.

### Input Format
The first and only line of input consists of a single integer X, denoting the page number.

### Output
Output on a new line, YES, if the given page number is dedicated to the sports section, and NO otherwise.

You may print each character of the string in uppercase or lowercase (for example, the strings YES, yEs, yes, and yeS will all be treated as identical).

### Constraints
1≤X≤10<br>

### Example
```
3
```

```
NO
```

### Explanation
Page number 3 is not dedicated to the sports section.

## Solution

The last three pages are page 8,9,10. So if X is one of these three, the answer is Yes; otherwise the answer is No.

This can be checked in various ways; for example you can check if X≥8 or not.

Since we have set a baseline lets try to code it :

```c++
#include <iostream>
using namespace std;

int main() {
	// your code goes here
	int a;
	cin>>a;
	if(a>=8)
	    cout<<"YES"<<endl;
	else
	    cout<<"NO"<<endl;
	return 0;
}

```

The above solution passes as expected.

* * *

# 2. Selling Insurance (AGENTCHEF)

Chef has started a new job as an insurance agent. Each insurance costs X dollars and Chef gets a 20% commission on selling each insurance.

Find the minimum number of insurances Chef needs to sell to earn at least 100 dollars.

### Input Format
The first line of input will contain a single integer T, denoting the number of test cases.
Each test case consists of a single integer X, the cost of an insurance..

### Output
For each test case, output on a new line, the minimum number of insurances Chef needs to sell to earn at least 100 dollars.

### Constraints
1≤T≤100<br>
1≤X≤100

### Example
```
4
20
80
1
5
```

```
25
7
500
100
```

### Explanation
Test case 1: On selling 1 insurance, Chef earns 20% of 20, which is, 4 dollars. This means that Chef needs to sell 25 insurances to earn 25X4=100 dollars.

Test case 2: On selling 1 insurance, Chef earns 20% of 80, which is, 16 dollars. On selling 6 insurances, Chef will earn  16X6=96 dollars. Thus, he would need to sell a minimum of 7 insurances to earn at least 100 dollars.

Test case 3: On selling 1 insurance, Chef earns 20% of 1, which is, 0.2 dollars. This means that Chef needs to sell 500 insurances to earn 500X0.2=100 dollars.

Test case 4: On selling 1 insurance, Chef earns 20% of 5, which is, 1 dollars. This means that Chef needs to sell 100 insurances to earn 100X1=100 dollars.

## Solution

20% of X can be written as $0.2X$, that is, each sale earns Chef $0.2X$ dollars.
So, if Chef makes $K$ sales, he’d earn $0.2X\cdot K$ dollars.
We want the smallest $K$ such that this is at least $100$.
Working it out,
$$
0.2X\cdot K \geq 100 \\
X\cdot K \geq \frac{100}{0.2} = 500 \\
K\geq \frac{500}{X}
$$
Hence, we want $K\geq \frac{500}{X}$.
$K$ must be an integer, so the smallest one we can choose is $K = \left\lceil \frac{500}{X} \right\rceil$.
Here, $\left\lceil \ \right\rceil$ denotes the ceiling function.

Since we have set a baseline lets try to code it :

```python
import math
t=input()
for i in range(0,int(t)):
    x=int(input())
    print(math.ceil(100/(x*0.2)))

```

The above solution passes as expected.

* * *