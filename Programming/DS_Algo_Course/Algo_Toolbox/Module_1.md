---
layout: default
---

# Programming Challenge #1

This programming challenge is to get familiar with coursera submissions. In this very first programming challenge, your goal is to implement a program that reads two digits from the standard input and prints their sum to the standard output.

```python
def sum_of_two_digits(first_digit, second_digit):
    return first_digit + second_digit


if __name__ == '__main__':
    a, b = map(int, input().split())
    print(sum_of_two_digits(a, b))
```

# The maximum pairwise product challenge (Part 1)

Find the maximum product of two distinct numbers in a sequence of non-negative integers.

Input: An integer n and a sequence of n non-negative integers.
Output: The maximum value that can be obtained by multiplying two different elements from the sequence.

### Example 1
```
3
1 2 3
```

```
6
```

### Example 2
```
10
7 5 14 2 8 8 10 1 2 3
```

```
140
```

A naive way to solve the Maximum Pairwise Product Problem is to go through all possible pairs of the input elements A[1...n] = [a1,...,an] and to find a pair of distinct elements with the largest product:

```python
def max_pairwise_product(numbers):
	n = len(numbers)
	max_product = 0
	for first in range(n):
		for second in range(first + 1, n):
			max_product = max(max_product,
				numbers[first] * numbers[second])
	return max_product
	
if __name__ == '__main__':
	_ = int(input())
	input_numbers = list(map(int, input().split()))
	print(max_pairwise_product(input_numbers))
```

MaxPairwiseProductNaive performs of the order of n<sup>2</sup> steps on a sequence of length n. For the maximal possible value n = 2 · 105, the number of steps is of the order 4 · 10<sup>10</sup>. Since many modern computers perform roughly 10<sup>8</sup>–10<sup>9</sup> basic operations per second (this depends on a machine, of course), it may take tens of seconds to execute MaxPairwiseProductNaive, exceeding the time limit for the Maximum Pairwise Product Problem.

In search of a faster algorithm, you play with small examples like [5,6,2,7,4]. Eureka—it suffices to multiply the two largest elements of the array—7 and 6! But still the algorithm will fail for a testcase in the judge. To solve it we introduce a technique called stress test.

# Stress Test

We will now introduce stress testing—a technique for generating thousands of tests with the goal of finding a test case for which your solution fails. A stress test consists of four parts:
1. Your implementation of an algorithm.
2. An alternative, trivial and slow, but correct implementation of an algorithm for the same problem.
3. A random test generator.
4. An infinite loop in which a new test is generated and fed into both implementations to compare the results. If their results differ, the test and both answers are output, and the program stops, otherwise the loop repeats.

The idea behind stress testing is that two correct implementations should give the same answer for each test (provided the answer to the problem is unique). If, however, one of the implementations is incorrect, then there exists a test on which their answers differ. When we run stress test we get something like this :

```
OK
67232 68874 69499
OK
6132 56210 45236 95361 68380 16906 80495 95298
OK
62180 1856 89047 14251 8362 34171 93584 87362 83341 8784
OK
21468 16859 82178 70496 82939 44491
OK
2 9 3 1 9
Wrong answer: 81 27
```

Then we can debug this solution on this test case, find a bug, fix it, and repeat the stress test again. Now let’s examine index1 = 2 and index2 = 3. If we look at the code for determining the second maximum, we will notice a subtle bug. When we implemented a condition on i instead of comparing i and index1, we compared A[i] withA[index1]. This ensures that the second maximum differs from the first maximum by the value rather than by the index of the element that we select for solving the Maximum Pairwise Product Problem. So, our solution fails on any test case where the largest number is equal to the second largest number.

# The maximum pairwise product challenge (Part 2)

Now after finding bug and fixing it we can code it.

```python
def max_pairwise_product(numbers):
    n = len(numbers)
    numbers.sort()
    return numbers[n-1]*numbers[n-2]


if __name__ == '__main__':
    _ = int(input())
    input_numbers = list(map(int, input().split()))
    print(max_pairwise_product(input_numbers))
```

# What to do if your solution doesn't work?

The verdict is usually one the following:

1. Wrong answer
2. Time/memory limit exceeded
3. Failed (runtime error)

Although the strategies may differ depending on the verdict, generally you need to follow the same steps:

1. Check if you didn’t forget some corner case. For example if n = 1 or n is the maximum possible number satisfying the constraints. Check if your program behaves correctly given the input that hits constraints in all possible senses.
2. Design some general test(s) that you know the correct answer for. Don’t look at your code’s answer for this tests before you figure out what the answer should be with pen and paper. Otherwise it is easy to convince yourself that it is correct and fail to see some silly error.
3. If you got time limit exceeded error measure how long your program works for the larger inputs.
4. If you got a runtime error (usually the message is Unknown signal ...) then it is good news, that is the most informative verdict, that means your program crushes due to invalid memory access, arithmetic error or stack size.

### How to generate tests?

The simplest way to generate test is to write a program that prints a test to a text file. This needs to generate random tests or this technique would be useless.