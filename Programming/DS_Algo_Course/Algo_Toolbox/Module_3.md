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

# Greedy Algorithms

A greedy algorithm builds a solution piece by piece and at each step, chooses the most profitable piece. This is best illustrated with examples. Our first example is the Largest Concatenate Problem: given a sequence of single-digit numbers, find the largest number that can be obtained by concatenating these numbers. For example, for the input sequence (2,3,9,1,2), the output is the number 93221. It is easy to come up with an algorithm for this problem. Clearly, the largest single-digit number should be selected as the first digit of the concatenate. Afterward, we face essentially the same problem: concatenate the remaining numbers to get as large number as possible.

```
LargestConcatenate(Numbers): 
result ← empty string
while Numbers is not empty:
	maxNumber ← largest among Numbers 
	append maxNumber to result
	remove maxNumber from Numbers
return result
```

## Queue Arrangement

n patients have come to the doctor's office at 9:00AM. They can be treated in any order. For ith patient, the time needed for treatment is t<sub>i</sub>. You need to arrange the patients in such a queue that the total waiting time is minimized.

```
t1 = 15, t2 = 20, t3 = 10
Arrangement (1,2,3):

> First patient doesn't wait
> Second patient waits for 15 minutes
> Third patient waits for 15+20=35 minutes
> Total waiting time is 15+35 = 50 minutes

Arrangement (3,1,2):

> First patient doesn't wait
> Second patient waits for 10 minutes
> Third patient waits for 10+15=25 minutes
> Total waiting time is 10+25 = 35 minutes
```

### Sub Problem

Subproblem is a similar problem of smaller size.

```
LargestNumber(3, 9, 5, 9, 7, 1) = ‘‘9’’+ LargestNumber(3, 5, 9, 7, 1)
 
Min number of refills from A to B = first refill at G + min number of refills from G to B
```

### Safe Move

A greedy choice is called safe move if there is an optimal solution consistent with this first move.

### General Strategy

1. Make a greedy choice
2. Prove that it is a safe Move
3. Reduce to a Subproblem
4. Solve the sub problem

# Celebration Party

Many children came to a celebration. Organize them into the minimum possible number of groups such that the age of any two children in the same group differ by at most one year.

### Naive algorithm

Compute all possible groups. It will take O(2<sup>n</sup>) time.

```
m ← len(C )
for each partition into groups 
C = G1 ∪ G2 ∪ · · · ∪ Gk :
	good ← true
	for i from 1 to k:
		if max(Gi ) − min(Gi ) > 1:
			good ← false
	if good:
		m ← min(m, k) 
return m
```

For n=50 it will take 1125899906842624 operations.

### Efficient algorithm

Let's think this problem from a different perspective. Assume a number line with children age on it represented by points. Our problem is to divide the number line into minimum segments such that each group contains only those points which have atmost difference of two.

Safe move: cover the leftmost point with a unit segment which starts in this point.

![Plot_1](https://m3verma.github.io/Programming/DS_Algo_Course/Algo_Toolbox/Images/Module_3/plot_1.png)

Assume the points (age of children) are given in ascending order. Algo :

```
R ← {}, i ← 1 
while i ≤ n:
	[ℓ, r] ← [xi , xi + 1] 
	R ← R ⋃︀{[ℓ, r]}
	i ← i + 1
	while i ≤ n and xi ≤ r:
		i ← i + 1
	return R
```

Total running time : 

Sort + PointsCoverSorted is O(nlogn).

This is faster even for n = 10000000

# Maximizing Loot

The maximum total value of fractions of items that fit into a bag of capacity W.

Safe move - There exists an optimal solution that uses as much as possible of an item with the maximal value per unit of weight.

Greedy Algo :

```
> While knapsack is not full 
> Choose item i with maximum vi/wi
> If item fits into knapsack, take all of it 
> Otherwise take so much as to fill the knapsack
> Return total value and amounts taken
```

```
A ← [0, 0, . . . , 0], V ← 0 
repeat n times:
	if W = 0: 
		return (V , A)
	select i with wi > 0 and max vi/wi
	a ← min(wi , W ) 
	V ← V + a(vi/wi)
	wi ← wi −a, A[i] ← A[i]+a, W ← W −a 
return (V , A)
```

Running Time - O(n<sup>2</sup>)

Improved Algo - Sort items by decreasing v/w, It will be O(nlogn)

```
A ← [0, 0, . . . , 0], V ← 0 
for i from 1 to n:
	if W = 0: 
		return (V , A)
	a ← min(wi , W ) 
	V ← V + a(vi/wi)
	wi ← wi −a, A[i] ← A[i]+a, W ← W −a 
return (V , A)
```

# Programming Challenge #3

## 1. Money Change

The goal in this problem is to find the minimum number of coins needed to change the input value (an integer) into coins with denominations 1, 5, and 10.

Input format - The input consists of a single integer 𝑚. <br>
Output format - Output the minimum number of coins with denominations 1, 5, 10 that changes 𝑚.<br>
Constraints - 1<=m<=10<sup>3</sup><br>

### Example 1

```
28
```

```
6
```

### Solution

```python
def change(money):
    coins=0
    if money >=10:
        coins+=money//10
        money%=10
    if money>=5:
        coins+=money//5
        money%=5
    if money>=1:
        coins+=money
    return coins


if __name__ == '__main__':
    m = int(input())
    print(change(m))
```

## 2. Maximum Value of the Loot

Find the maximal value of items that fit into the backpack.

Input format - The first line of the input contains the number n of compounds and the capacity W of a backpack. The next n lines define the costs and weights of the compounds. The i-th line contains the cost c<sub>i</sub> and the weight w<sub>i</sub> of the i-th compound. <br>
Output format - Output the maximum value of compounds that fit into the backpack.<br>
Constraints - 1 ≤ n ≤ 10<sup>3</sup>, 0 ≤ W ≤ 2 · 10<sup>6</sup>; 0 ≤ c<sub>i</sub> ≤ 2 · 10<sup>6</sup>, 0 < w<sub>i</sub> ≤ 2 · 10<sup>6</sup> for all 1 ≤ i ≤ n. All the numbers are integers.<br>

### Example 1

```
3 50 
60 20 
100 50
120 30
```

```
180.0000
```

### Solution

```python
from sys import stdin


def optimal_value(capacity, weights, values):
    value = 0.
    n=len(weights)
    lst=[]
    for _ in range(n):
        lst.append((values[_],weights[_]))

    lst.sort(key=lambda x: x[0]/x[1],reverse=True)
    for v,w in lst:
        if capacity==0:
            return value
        a=min(w,capacity)
        value+=a*(v/w)
        capacity-=a
    # write your code here

    return value


if __name__ == "__main__":
    data = list(map(int, stdin.read().split()))
    n, capacity = data[0:2]
    values = data[2:(2 * n + 2):2]
    weights = data[3:(2 * n + 2):2]
    opt_value = optimal_value(capacity, weights, values)
    print("{:.10f}".format(opt_value))
```

## 3. Car Fueling

Compute the minimum number of gas tank refills to get from one city to another.

Input format - The first line contains an integer d. The second line contains an integer m. The third line specifies an integer n. Finally, the last line contains integers stop1,stop2,...,stopn. <br>
Output format -  The minimum number of refills needed. If it is not possible to reach the destination, output −1.<br>
Constraints - 1 ≤ d ≤ 10<sup>5</sup>. 1 ≤ m ≤ 400. 1 ≤ n ≤ 300. 0 < stop1 < stop2 <· · · < stopn < d.<br>

### Example 1

```
10
3
4
1 2 5 9
```

```
-1
```

### Solution

```python
from sys import stdin


def min_refills(distance, tank, stops):
    num_refills = 0
    current_position = 0
    stops = [0] + stops + [distance]

    while current_position < len(stops) - 1:
        last_position = current_position

        while (current_position < len(stops) - 1 and
               stops[current_position + 1] - stops[last_position] <= tank):
            current_position += 1

        if current_position == last_position:
            return -1

        if current_position < len(stops) - 1:
            num_refills += 1

    return num_refills


if __name__ == '__main__':
    d, m, _, *stops = map(int, stdin.read().split())
    print(min_refills(d, m, stops))

```

## 4. Maximum Advertisement Revenue

You have 𝑛 ads to place on a popular Internet page. For each ad, you know how much is the advertiser willing to pay for one click on this ad. You have set up 𝑛 slots on your page and estimated the expected number of clicks per day for each slot. Now, your goal is to distribute the ads among the slots to maximize the total revenue.

Input format - The first line contains an integer 𝑛, the second one contains a sequence of integers 𝑎1, 𝑎2, . . . , 𝑎𝑛, the third one contains a sequence of integers 𝑏1, 𝑏2, . . . , 𝑏𝑛. <br>
Output format -  Output the maximum value of sum(𝑎𝑖𝑐𝑖), where 𝑐1, 𝑐2, . . . , 𝑐𝑛 is a permutation of 𝑏1, 𝑏2, . . . , 𝑏𝑛.<br>
Constraints - 1 ≤ 𝑛 ≤ 10<sup>3</sup>; −10<sup>5</sup> ≤ 𝑎𝑖, 𝑏𝑖 ≤ 10<sup>5</sup> for all 1 ≤ 𝑖 ≤ 𝑛.<br>

### Example 1

```
3
1 3 -5 
-2 4 1
```

```
23
```

### Solution

```python
from itertools import permutations

def max_dot_product(first_sequence, second_sequence):
    max_product = 0
    first_sequence.sort(reverse=True)
    second_sequence.sort(reverse=True)
    for i in range(len(first_sequence)):
        max_product += first_sequence[i] * second_sequence[i]

    return max_product


if __name__ == '__main__':
    n = int(input())
    prices = list(map(int, input().split()))
    clicks = list(map(int, input().split()))
    assert len(prices) == len(clicks) == n
    print(max_dot_product(prices, clicks))
```

## 5. Collecting Signatures

You are responsible for collecting signatures from all tenants of a certain building. For each tenant, you know a period of time when he or she is at home. You would like to collect all signatures by visiting the building as few times as possible. The mathematical model for this problem is the following. You are given a set of segments on a line and your goal is to mark as few points on a line as possible so that each segment contains at least one marked point.

Input format - The first line of the input contains the number 𝑛 of segments. Each of the following 𝑛 lines contains two integers 𝑎𝑖 and 𝑏𝑖 (separated by a space) defining the coordinates of endpoints of the 𝑖-th segment. <br>
Output format - Output the minimum number 𝑚 of points on the first line and the integer coordinates of 𝑚 points (separated by spaces) on the second line. You can output the points in any order. If there are many such sets of points, you can output any set. (It is not difficult to see that there always exist a set of points of the minimum size such that all the coordinates of the points are integers.)<br>
Constraints - 1 ≤ 𝑛 ≤ 100; 0 ≤ 𝑎𝑖 ≤ 𝑏𝑖 ≤ 10<sup>9</sup> for all 0 ≤ 𝑖 < 𝑛.<br>

### Example 1

```
4
4 7 
1 3 
2 5 
5 6
```

```
2
3 6
```

### Solution

```python
n = int(input())
seg = []
for i in range(n):
    ipt = input()
    seg.append(list(map(int, ipt.split())))
seg.sort()

def min_points(seg):
    n = len(seg)
    current = 0
    ends = []
    while current < n:
        last = current
        while current < n - 1 and seg[current + 1][0] <= seg[last][1]:
            current = current + 1
            if seg[current][1] < seg[last][1]:
                last = current
        ends.append(seg[last][1])
        current = current + 1
    return ends

ends = min_points(seg)
print(len(ends))
for end in ends:
    print(end, end = ' ')
```

## 6. Maximum Number of Prizes

You are organizing a funny competition for children. As a prize fund you have 𝑛 candies. You would like to use these candies for top 𝑘 places in a competition with a natural restriction that a higher place gets a larger number of candies. To make as many children happy as possible, you are going to find the largest value of 𝑘 for which it is possible.

Input format - The input consists of a single integer 𝑛. <br>
Output format -  In the first line, output the maximum number 𝑘 such that 𝑛 can be represented as a sum of 𝑘 pairwise distinct positive integers. In the second line, output 𝑘 pairwise distinct positive integers that sum up to 𝑛 (if there are many such representations, output any of them).<br>
Constraints - 1 ≤ 𝑛 ≤ 10<sup>9</sup> <br>

### Example 1

```
6
```

```
3
1 2 3
```

### Solution

```python
n = int(input())

prize = []
i = 0
while n > 0:
    i = i + 1
    if n - i <= i:
        i = n
    prize.append(i)
    n = n - i

print(len(prize))
for x in prize:
    print(x, end =' ')
```

## 7. Maximum Salary

As the last question of a successful interview, your boss gives you a few pieces of paper with numbers on it and asks you to compose a largest number from these numbers. The resulting number is going to be your salary, so you are very much interested in maximizing this number.

Input format - The first line of the input contains an integer 𝑛. The second line contains integers 𝑎1, 𝑎2, . . . , 𝑎𝑛. <br>
Output format -  Output the largest number that can be composed out of 𝑎1, 𝑎2, . . . , 𝑎𝑛.<br>
Constraints - 1 ≤ 𝑛 ≤ 100; 1 ≤ 𝑎𝑖 ≤ 10<sup>3</sup> for all 1 ≤ 𝑖 ≤ 𝑛. <br>

### Example 1

```
2
21 2
```

```
221
```

### Solution

```python
n = int(input())
digits = list(input().split())

def max_num(lst):
    n = len(lst)
    for i in range(n - 1):
        for i in range(n - 1 - i):
            if lst[i] + lst[i+1] < lst[i + 1] + lst[i]:
                 lst[i], lst[i + 1] = lst[i + 1], lst[i]
    return lst

digits = max_num(digits)
result = ''
for digit in digits:
    result = result + digit
print(result)
```