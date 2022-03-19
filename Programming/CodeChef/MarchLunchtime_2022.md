---
layout: default
---

# 1. Minimum number of coins (MINCOINS)

Chef has infinite coins in denominations of rupees 5 and rupees 10.

Find the minimum number of coins Chef needs, to pay exactly X rupees. If it is impossible to pay X rupees in denominations of rupees 5 and 10 only, print −1.

### Input Format
First line will contain T, number of test cases. Then the test cases follow.

Each test case contains of a single integer X.

### Output
For each test case, print a single integer - the minimum number of coins Chef needs, to pay exactly X rupees. If it is impossible to pay X rupees in denominations of rupees 5 and 10 only, print −1.

### Constraints
1≤T≤1000<br>
1≤X≤1000

### Example
```
3
50
15
8
```

```
5
2
-1
```

### Explanation
Test Case 1: Chef would require at least 5 coins to pay 50 rupees. All these coins would be of rupees 10.

Test Case 2: Chef would require at least 2 coins to pay 15 rupees. Out of these, 1 coin would be of rupees 10 and 1 coin would be of rupees 5.

Test Case 3: Chef cannot pay exactly 8 rupees in denominations of rupees 5 and 10 only.

## Solution

Since we have only 2 types of denominations 5 and 10. So rupees like 18,13,4 etc which are not divisible by 5 cannot be paid in coins. 

So first case, if number is not divisible by 5 return -1.

Now remaining numbers will be either in format of 10,20,30 or 15,25,35 etc. In first case return (R%10) coins and in second return (R%10 + 1).

```c++
#include <iostream>
using namespace std;

int main() {
	int t;
	cin>>t;
	while(t--)
	{
	    int n;
	    cin>>n;
	    if(n%5!=0)
	        cout<<"-1"<<endl;
	    else
	    {
	        int rem=n%10;
	        if(rem==0)
	            cout<<n/10<<endl;
	        else
	            cout<<n/10 + 1<<endl;
	    }
	}
	return 0;
}
```

The above solution passes as expected.

* * *

# 2. Buy 2 Get 1 Free (SALE2)

There is a sale going on in Chefland. For every 2 items Chef pays for, he gets the third item for free (see sample explanations for more clarity).

It is given that the cost of 1 item is X rupees. Find the minimum money required by Chef to buy at least N items.

### Input Format
First line will contain T, number of test cases. Then the test cases follow.

Each test case contains of a single line of input, two integers N and X.

### Output
For each test case, output the minimum money required by Chef to buy at least N items.

### Constraints
1≤T≤1000 <br>
1≤N,X≤1000

### Example
```
4
3 4
4 2
5 3
6 1
```

```
8
6
12
4
```

### Explanation
Test Case 1: Chef wants 3 items where the cost of each item is 4 rupees. To minimise the expenditure, Chef can pay for 2 items and get the third item for free. This way, the total money required is 4X2=8 rupees.

Test Case 2: To minimise the expenditure, Chef can do the following:
Purchase 2 items for 2X2=4 rupees and get the third one free. Thus, the total number of items is 3.
Purchase 1 item for 2X1=2 rupees. Now Chef has total 4 items.
Thus, the minimum money required to buy 4 items is 4+2=6 rupees.

Test Case 3: To minimise the expenditure, Chef can do the following:
Purchase 2 items for 3X2=6 rupees and get the third one free. Thus, the total number of items is 3.
Purchase 2 items for 3X2=6 rupees. Chef gets a third item for free. Now Chef has total 6 items which is greater than his requirement of 5 items.
Chef wanted to buy at least 5 items. The minimum money required for that is 6+6=12 rupees.

Test Case 4: To minimise the expenditure, Chef can do the following:
Purchase 2 items for 1X2=2 rupees and get the third one free. Thus, the total number of items is 3.
Purchase 2 items for 1X2=2 rupees and get the third one free.. Now Chef has total 6 items.
Thus, the minimum money required to buy 6 items is 2+2=4 rupees.

## Solution

Here I have divided solution in 3 cases.

First : If items to buy is divisible by 3. Then whenever we buy 2 items 1 will be free. Hence we will always have to pay for 2 items multiplied by total sets of 3. So if we have to buy 12 items, 4 sets are created and hence out of these 4 sets we have to buy 8 items which will cost 8*X.

Second : If items to buy when divided by 3 gives remainder 1. So if we have to buy 13 items, 4 sets are created and 1 set with just 1 item. Hence out of these 4 sets we have to buy 8 items which will cost 8*X and remaining 1 item. So total cost will be 8*X + X.

Third : If items to buy when divided by 3 gives remainder 2. So if we have to buy 14 items, 4 sets are created and 1 set with just 2 items. Hence out of these 4 sets we have to buy 8 items which will cost 8*X and remaining 2 items. So total cost will be 8*X + 2*X.

Let's code it :

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
	    if(n%3==0)
	        cout<<k*(n/3)*2<<endl;
	    else if(n%3==1)
	         cout<<k*(n/3)*2+k<<endl;
	    else
	         cout<<k*(n/3)*2+k*2<<endl;
	}
	return 0;
}
```

The above solution passes as expected.

* * *

# 3. Sad Splits (SPLITPAIR)

You are given a positive integer N. You have to split each digit of N into either of two non-empty subsequences A or B.

For example, if N=104, some possible values of (A,B) can be (10,4),(14,0) and (1,4). Note that, after separating the digits into A and B, these subsequences are considered as positive integers and thus leading zeros can be omitted.

Let us define a function F(X,Y)=(X+Y)%2. Find out whether it is possible to find two non-empty subsequences A and B formed out of N such that F(A,B)=0.

### Input Format
First line will contain T, number of test cases. Then the test cases follow.

Each test case contains of a single line of input, one integer N.

### Output
For each test case, print YES if it is possible to find two non-empty subsequences A and B formed out of N such that F(A,B)=0. Otherwise, print NO.

You may print each character of the string in uppercase or lowercase (for example, the strings YeS, yEs, yes and YES will all be treated as identical).

### Constraints
1≤T≤1000 <br>
10≤N≤10<sup>9</sup>

### Example
```
2
10
73452
```

```
NO
YES
```

### Explanation
Test Case 1: The only two possibilities are:
A=0,B=1: Here F(A,B)=(A+B)%2=(0+1)%2=1%2=1.
A=1,B=0: Here F(A,B)=(A+B)%2=(1+0)%2=1%2=1. Hence it's not possible to achieve F(A,B)=0.

Test Case 2: One of the possible ways to split the digits of N such that F(A,B)=0 is:
A=74,B=352: Here F(A,B)=(A+B)%2=(74+352)%2=426%2=0.

## Solution

Let's remember the basic mathematics rule again :

Even+Even=Even
Odd+Odd=Even
Odd+Even=Odd

Now as every digit of the number needs to be part of exactly one subsequence A or B. Then observe that the last digit of the given number will be the last digit of any subsequence. Let’s put this digit in the subsequence A.

1. Last digit of the given number is Even.

It means that the subsequence A is going to be an even number. In order to get the remainder 0 we need to have (X+Y) as even. And for that the subsequence B needs to be also even. Now if there is any digit apart from the last digit in the given number as even we will be able to get the subsequence B also even.

2. Last digit of the given number is Odd.

It is opposite to that of the above case. Instead of Even we will be looking for odd as the last digit is odd hence the subsequence A will be odd. As odd added to odd only will produce the sum even which will produce the remainder 0.

Let's code it :

```c++
#include <iostream>
using namespace std;

int main() {
	int t;
	cin>>t;
	while(t--)
	{
	    long long int n;
	    bool flag=true;
	    cin>>n;
	    int last_digit=n%10;
	    int even_odd=last_digit%2;
	    n/=10;
	    while(n>0)
	    {
	        if((n%10)%2==even_odd)
	        {
	            cout<<"YES"<<endl;
	            flag=false;
	            break;
	        }
	        n=n/10;
	    }
	    if(flag)
	        cout<<"NO"<<endl;
	}
	return 0;
}
```

The above solution passes as expected.

* * *

# 4. Adjacency Love (ADJLOVE)

An array is called lovely if the sum of product of each adjacent pair of elements is odd.

More formally, the array S of size M is lovely if the value ∑M−1i=1 (Si.Si+1) is odd.

You are given an array A consisting of N positive integers. Find a permutation of array A which is lovely.

If multiple such permutations are possible, output any. If there is no lovely permutation, output -1.

### Input Format
The first line contains an integer T denoting the number of test cases. The T test cases then follow.

The first line of each test case contains an integer N.

The second line of each test case contains N space-separated integers A1,A2,…,AN.

### Output
For each test case, if a lovely permutation of A is possible, print a single line containing N integers denoting the elements of any lovely permutation of A. Otherwise, print -1.

### Constraints
1≤T≤1000<br>
2≤N≤500<br>
1≤Ai≤10<sup>6</sup>

### Example
```
2
5
1 2 3 4 10
3
7 11 145
```

```
3 1 10 2 4
-1
```

### Explanation
Test Case 1: The sum of products of adjacent pair of elements of the given array is 1*2+2*3+3*4+4*10=2+6+12+40=60. Since this value is even, this is not a lovely permutation of the array.
A lovely permutation of the given array is [3,1,10,2,4]. The sum of products for this array would be 3*1+1*10+10*2+2*4=41, which is odd.

Test Case 2: No permutation of the given array exists where the sum of products of adjacent pair of elements is odd. Thus, answer is −1.

## Solution

Let’s try to see what happens under various cases, before that remember the three basic properties:

Even+Even=Even
Odd+Odd=Even
Odd+Even=Odd
and
Even*Even=Even
Odd*Odd=Odd
Even*Odd=Even

Now let’s look the cases :

1. Odd numbers are less than or equal to 1.

Let’s see what happens when there is no odd number present. Hence in any permutation you arrange the array elements. The consecutive product is gonna be even for all the indexes look at the above property that even multiplied with even gives you an even number. And then the sum of all even numbers is going to be even.

Now when there is only one odd element in the given array. Wherever you place this odd number it’s left side element and right side element is going to be even. And as we know even multiplied with odd gives you even number. Again the sum of all even numbers is going to be even.

Hence we can conclude that in such cases it is never possible to achieve the odd sum in whatever permutation we arrange the given array.

2. All numbers of the given array are odd.

This is very interesting and simple case as all numbers are odd and hence when the odd number is multiplied with the odd number it produces an odd number. Hence every consecutive product is an odd number. Now the answer depends whether the sum of these consecutive product is even or odd.

- N is Even - There will be (N-1) consecutive products that exist for the the array of length N. Hence (N−1) is an odd number. If odd number is added odd times it produces an odd number.

- N is Odd - In this case (N-1) is going to be even. Hence when odd number is added even times it produces an even number.

Hence we can conclude that in such cases whether the sum is going to be odd depends on whether N is even.

3. All other cases - Ok, so for all other cases it is always possible to produce an odd number. Let’s say the number of odd numbers in the given array be X. Now let us try to prove how :

- X is Odd : Now we know that these numbers are the only chances for us to get the odd sum in the array. Now if we place all these numbers consecutively then there will be (X−1) consecutive odd products. Since (X−1) is even hence we will end up getting the sum as even.

Let us then do one thing place the (X-1)(X−1) odd numbers consecutively and the remaining odd number anywhere. Now there will be (X-2) odd consecutive products in the given array which leads us to get the odd sum.

Hence we can say that it is possible to get the odd sum when X is going to be odd.

- X is Even : Very simple if you read the Odd case above. Just place the all these X numbers consecutive and then you know what happens and you will be able to get the odd sum.

Let's code it :

```c++
#include<bits/stdc++.h>
using namespace std;

void solve()
{
    int n;
    cin>>n;
    
    vector<int> even,odd;
    
    for(int i=0;i<n;i++)
    {
        int x;
        cin>>x;
        
        if(x%2)
            odd.push_back(x);
        else 
            even.push_back(x);
    }
    
    if (odd.size()<=1 || (odd.size() == n && n%2))
    {
        cout<<-1<<endl;
        return;
    }
    
    if(odd.size()%2)
    {
        cout<<odd[0]<<" ";
        odd.erase(odd.begin());
    }
    
    for(auto itr: even)
        cout<<(itr)<<" ";
    
    for(auto itr: odd)
        cout<<(itr)<<" ";
        
    cout<<endl;
        
}

int main()
{
    int t;
    cin>>t;
    
    while(t--)
        solve();

return 0;
}
```

The above solution passes as expected.

* * *
