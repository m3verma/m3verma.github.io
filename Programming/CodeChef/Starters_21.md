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
1≤T≤10<br>
10<sup>4</sup>≤Z≤10<sup>5</sup><br>
0≤Y≤Z<br>
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
	return 0;
}
```

The above solution passes as expected.

* * *

# 2. Covid and Theatre Tickets (COVID_19)

Mr. Chef is the manager of the Code cinemas and after a long break, the theatres are now open to the public again. To compensate for the loss in revenue due to Covid-19, Mr. Chef wants to maximize the profits for every show from now on and at the same time follow the guidelines set the by government. The guidelines are:

1. If two people are seated in the same row, there must be at least one empty seat between them.
2. If two people are seated in different rows, there must be at least one completely empty row between them. That is, if there are people seated in rows i and j where i<j, there must be some row k such that i<k<j and nobody is seated in row k.

Given the information about the number of rows and the number of seats in each row, find the maximum number of tickets Mr. Chef can sell.

### Input Format
The first line of input will contain a single integer T, denoting the number of test cases. The description of T test cases follows.

Each test case consists of a single line of input containing two space-separated integers N,M — the number of rows and the number of seats in each row, respectively.

### Output
For each test case, output a single line containing one integer – the maximum number of tickets Mr. Chef can sell.

### Constraints
1≤T≤100<br>
1≤N,M≤100

### Example
```
3
1 5
3 3
4 4
```

```
3
4
4
```

### Explanation
Test Case 1: There is only one row with five seats. Mr. Chef can sell a maximum of 3 tickets for seat numbers 1, 3 and 5.

Test Case 2: There are three rows with three seats each. Mr. Chef can sell a maximum of 4 tickets, for seats at the start and end of row numbers 1 and 3.

Test Case 3: There are four rows with four seats each. Mr. Chef can sell a maximum of 4 tickets, for example by choosing the seats at the start and end of row numbers 1 and 4.

## Solution

Clearly, the optimal way to allocate the seats would be to select the rows as row 1, row 3, ... and in each row we select the seats as seat 1, seat 3, ...

Then, total number of rows selected will be (N + 1)/2 and the number of seats in each row would be (M + 1)/2.

Therefore, the total number of seats selected will be [(N + 1)/2]X[(M + 1)/2].

Since we have set a baseline lets try to code it :

```c++
#include <iostream>
using namespace std;

int main() {
	int t;
	cin>>t;
	while(t--)
	{
		int row,seat,total=0;
		cin>>row>>seat;
		if(seat%2==0)
		{
			total=total+seat/2;
		}
		else
		{
			total=total+((seat+1)/2);
		}
		if(row==1)
			cout<<total<<endl;
		else
			cout<<total*((row+1)/2)<<endl;
	}
	return 0;
}
```

The above solution passes as expected.

* * *

# 3. Akash and Grid (CHEFGRD)

Akash is stuck in a N×N grid, where N is odd. The rows of the grid are numbered 1 to N from top to bottom, and the columns are numbered 1 to N from left to right. The cell at the intersection of the i-th row and j-th column will be denoted (i,j).

The grid has a unique center cell — ((N+1)/2,(N+1)/2). For example, when N=5 the center is cell (3,3).

Akash is currently at cell (x<sub>s</sub>,y<sub>s</sub>). He would like to reach the exit of the grid, which is located at the center. It is guaranteed that (x<sub>s</sub>,y<sub>s</sub>) is not the center.

Suppose Akash is at cell (x,y). He can make the following movements:

1. He can freely move along diagonals, i.e, to cells (x−1,y−1),(x−1,y+1),(x+1,y−1),(x+1,y+1)
2. He can move one step horizontally or vertically with the cost of 1 gold coin, i.e, to cells (x,y−1),(x,y+1),(x−1,y),(x+1,y)

Note that Akash is not allowed to make a move that will take him out of bounds of the grid.

Akash would like to minimize the number of coins required to reach the center. Please help him find this number.

### Input Format
The first line of input contains a single integer T, denoting the number of test cases. The description of T test cases follows.
	
Each test case consists of a single line of input, containing three space-separated integers N,x<sub>s</sub>,y<sub>s</sub> — the size of the grid and the coordinates of Akash's starting cell.

### Output
For each test case, output in a single line the minimum number of gold coins Akash needs to reach the center.

### Constraints
1≤T≤10<sup>4</sup><br>
3≤N<2⋅10<sup>4</sup><br>
N is always odd.
1≤x<sub>s</sub>,y<sub>s</sub>≤N

### Example
```
2
3 2 1
5 3 1
```

```
1
0
```

### Explanation
Test case 1: For a 3×3 grid, the center is at (2,2). It is not possible to reach (2,2) from (2,1) using only diagonal moves. So, Akash will directly go to the center using 1 gold coin.

Test case 2: N=5, so the center is (3,3). Akash can go from (3,1) to (2,2) with a diagonal move, then from (2,2) to (3,3) with another diagonal move. So, he needs zero coins.

## Solution

I solved it with slightly different way. To understand it lets draw 2 grids each of size 5 and 7 respectively as shown in figure.

![Grid](https://m3verma.github.io/Programming/CodeChef/Images/Starters_21_1.PNG)

The black filled circle is the center where Akash wants to move.

The empty circles are the places where if Akash started will always end up at circle with just moving diagonal.

The empty grid are the places where its impossible to reach center by just moving diagonally. Akash will always be short by 1 box and hence he will always have to use 1 coin.
	
So clearly we got a pattern.

As seen in figure if Akash starts from box whose indexes are both even or both odd, he will reach center with 0 coins.
If Akash starts from any other box, he will reach center by paying 1 coin.

And since size of grid is always odd, there is no other way possible.

Since we have set a baseline lets try to code it :

```c++
#include <iostream>
using namespace std;

int main() {
	
	int t;
	cin>>t;
	while(t--)
	{
		int n,x,y;
		cin>>n>>x>>y;
		if(x%2==0 && y%2==0)
			cout<<"0"<<endl;
		else if(x%2!=0 && y%2!=0)
			cout<<"0"<<endl;
		else
			cout<<"1"<<endl;
	}
	
	
	return 0;
}
```

The above solution passes as expected.
	
* * *

# 4. And Or Union (ANDORUNI)

You are given an array A consisting of N integers.

From array A, we create a new array B containing all pairwise bitwise ANDs of elements from A. That is, B consists of N⋅(N−1)/2 elements, each of the form A<sub>i</sub>&A<sub>j</sub> for some 1≤i<j≤N.

In array B, we repeat the following process till there is only one element remaining:

Let the current maximum and minimum element of array B be X and Y respectively.
Remove X and Y from array B and insert X|Y into it.

Determine the last remaining element in array B.

### Input Format
The first line of input contains a single integer T, denoting the number of test cases. The description of T test cases follows.

The first line of each test case contains a single integer N, denoting the number of elements in A.

The second line of each test case contains N space-separated integers A<sub>1</sub>,A<sub>2</sub>,…,A<sub>N</sub>.

### Output
For each test case, print a single line containing one integer — the last remaining element in array B.

### Constraints
1≤T≤10<sup>4</sup> <br>
2≤N≤10<sup>5</sup> <br>
0≤A<sub>i</sub>≤10<sup>9</sup> <br>
The sum of N over all test cases does not exceed 2⋅10<sup>5</sup>.

### Example
```
3
2
1 3
3
2 7 1
4
4 6 7 2
```

```
1
3
6
```

### Explanation
Test Case 1: Array B will be [A<sub>1</sub>&A<sub>2</sub>]=[1&3]=[1]. There is only one element in B so the answer is 1.

Test Case 2: Array B will be [A<sub>1</sub>&A<sub>2</sub>,A<sub>1</sub>&A<sub>3</sub>,A<sub>2</sub>&A<sub>3</sub>]=[2&7,2&1,7&1]=[2,0,1].

Then, we do the following operations on B:

Remove 2 and 0 from B and insert 2|0=2 into it. Now, B=[1,2].
Remove 2 and 1 from B and insert 2|1=3 into it. Now, B=[3].
	
The last remaining element is thus 3.

## Solution

I solved it with slightly different way. To understand it we need to remember the concepts of bitwise operators.

First lets see the second part - OR part. The property of OR operators tells us that ((A|B)|C)|D = A|B|C|D.
So technically we dont have to get maximum or minimum numbers first to OR them.
We can simply do OR of all numbers from start till end for second part and answer will still remain the same.
So no need for sorting.

Now let's see how to get array B. Let's start by example array A with 4 elements :  arr[0] , arr[1] , arr[2] , arr[3]
For easier calculations lets assume :<br>
OR denoted by +<br>
AND denoted by .<br>
arr[0] , arr[1] , arr[2] , arr[3] denoted by 0,1,2,3 respectively.<br>

So our problem wants a solution of the equation :

```
(arr[0] & arr[1]) | (arr[0] & arr[2]) | (arr[0] & arr[3]) | (arr[1] & arr[2]) | (arr[1] & arr[3]) | (arr[2] & arr[3])
= 01 + 02 + 03 + 12 + 13 + 23
Now using some basic arithmetic operator logic
= 01 + 02 + 12 + 03 + 13 + 23
= 10 + 2(0+1) + 3(0+1+2)
```

Similarly if you do it for 5 numbers in A you will get :
```
= 10 + 2(0+1) + 3(0+1+2) + 4(0+1+2+3)
```

Can you see the pattern now?

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
		cin>>n;
		int a[n];
		for(int i=0;i<n;i++)
		{
			cin>>a[i];
		}
		int final=0;
		int AND=a[0]&a[1];
		int OR=a[0]|a[1];
		for(int i=2;i<n;i++)
		{
			final=final|(a[i]&(OR));
			OR=OR|a[i];
		}
		final=AND|final;
		cout<<final<<endl;
	}
	return 0;
}
```

The above solution passes as expected.


