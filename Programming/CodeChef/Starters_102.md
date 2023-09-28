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
Each test case consists of a single integer X, the cost of an insurance.

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
Working it out,<br>
$0.2X\cdot K \geq 100 $<br>
$X\cdot K \geq \frac{100}{0.2} = 500 $<br>
$K\geq \frac{500}{X}$<br>
Hence, we want $K\geq \frac{500}{X}$.<br>
$K$ must be an integer, so the smallest one we can choose is $K = \left\lceil \frac{500}{X} \right\rceil$.<br>
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

# 3. Chef and Stocks (STOCKMARKET)

Chef has started learning about the stock market and has already selected a favourite stock. He traded the stock for N consecutive days. Let A<sub>i</sub> denotes the profit earned by Chef on the i<sup>th</sup> day. Note that A<sub>i</sub><0 indicates that Chef had a loss on the i<sup>th</sup> day.

Chef wants to find the maximum amount of of profit he would have earned, if he skipped trading for exactly one day.

### Input Format
The first line of input will contain a single integer T, denoting the number of test cases.
Each test case consists of multiple lines of input. 
The first line of each test case contains N — the number of days.
The next line denotes N space-separated integers, denoting the profit earned by Chef on the i<sup>th</sup> day.

### Output
For each test case, output on a new line, the maximum amount of of profit he would have earned, if he skipped trading for exactly one day.

### Constraints
1≤T≤1000<br>
1≤N≤10<sup>5</sup><br>
−100≤A<sub>i</sub>≤100<br>
The sum of N over all test cases won't exceed 10<sup>6</sup>.

### Example
```
4
3
1 -2 3
4
4 1 5 1
4
10 -10 -10 10
5
-5 -4 -3 -2 -1
```

```
4
10
10
-10
```

### Explanation
Test case 1: To make maximum profit, Chef would have skipped day 2. The total profit for rest of the days is 1+3=4.

Test case 2: To make maximum profit, Chef would have skipped day 4. The total profit for rest of the days is 1+4+5=10.

Test case 3: To make maximum profit, Chef would have skipped day 3. The total profit for rest of the days is 10−10+10=10.

Test case 4: To make maximum profit, Chef would have skipped day 1. The total profit for rest of the days is −4−3−2−1=−10.

## Solution

Suppose Chef skipped day $i$.
Then, his profit would be the sum of everything other than $A_i$, which equals

$(A_1 + A_2 + \ldots + A_{i-1}) + (A_{i+1} + A_{i+2} + \ldots + A_N) $<br>
$= (A_1 + A_2 + \ldots + A_{i-1}) + A_i + (A_{i+1} + A_{i+2} + \ldots + A_N) - A_i $<br>
$= (A_1 + A_2 + \ldots + A_{i-1} + A_i + A_{i+1} + A_{i+2} + \ldots + A_N) - A_i $<br>

In other words, Chef’s profit equals the sum of all the elements of array $A$; minus $A_i$.
To maximize this, clearly we should subtract as small a value as possible, since the first term is a constant.
That is, the optimal choice is to subtract the minimum element.

So, the final answer is just $\text{sum}(A) - \min(A)$.

Since we have set a baseline lets try to code it :

```c++
#include <iostream>
using namespace std;

int main() {
	int t;
	cin>>t;
	while(t--)
    {
        int n;
        long long int sum=0, smallest=0;
        cin>>n;
        int a[n];
        for(int i=0;i<n;i++)
        {
            cin>>a[i];
            if(i==0)
                smallest=a[0];
            sum=sum+a[i];
            if(smallest>a[i])
                smallest=a[i];
        }
        cout<<sum-smallest<<endl;
    }
	return 0;
}
```

The above solution passes as expected.