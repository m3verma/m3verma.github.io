---
layout: default
---

# 1. Stable Arrangement of Rooks

You have an n×n chessboard and k rooks. Rows of this chessboard are numbered by integers from 1 to n from top to bottom and columns of this chessboard are numbered by integers from 1 to n from left to right. The cell (x,y) is the cell on the intersection of row x and collumn y for 1≤x≤n and 1≤y≤n.

The arrangement of rooks on this board is called good, if no rook is beaten by another rook.

A rook beats all the rooks that shares the same row or collumn with it.

The good arrangement of rooks on this board is called not stable, if it is possible to move one rook to the adjacent cell so arrangement becomes not good. Otherwise, the good arrangement is stable. Here, adjacent cells are the cells that share a side.

![Rooks](https://m3verma.github.io/Programming/Codeforces/Images/Hello_2022_1.png)

Such arrangement of 3 rooks on the 4×4 chessboard is good, but it is not stable: the rook from (1,1) can be moved to the adjacent cell (2,1) and rooks on cells (2,1) and (2,4) will beat each other.
Please, find any stable arrangement of k rooks on the n×n chessboard or report that there is no such arrangement.

### Input
The first line contains a single integer t (1≤t≤100) — the number of test cases.

The first line of each test case contains two integers n, k (1≤k≤n≤40) — the size of the chessboard and the number of rooks.

### Output
If there is a stable arrangement of k rooks on the n×n chessboard, output n lines of symbols . and R. The j-th symbol of the i-th line should be equals R if and only if there is a rook on the cell (i,j) in your arrangement.

If there are multiple solutions, you may output any of them.

If there is no stable arrangement, output −1.

### Example
```
5
3 2
3 3
1 1
5 2
40 33
```

```
..R
...
R..
-1
R
.....
R....
.....
....R
.....
-1
```

### Note
In the first test case, you should find stable arrangement of 2 rooks on the 3×3 chessboard. Placing them in cells (3,1) and (1,3) gives stable arrangement.

In the second test case it can be shown that it is impossbile to place 3 rooks on the 3×3 chessboard to get stable arrangement.

## Solution

The problem is easier then it looks like. So basically what the setter is trying to say is that we have to place 'k' rooks on an nxn chessboard in such a way that :
1. No rook is beaten by another rook
2. Even if we move any rook to its adjacent sides then also it shouldn't beat any other rook on the chessboard.

So if in above image, we move Rook R1 (at 1,1) to it's adjacent side (2,1 or 1,2) than also it shouldn't beat any other rook. This condition should be satisfied by every rook on the chessboard. To understand it more clearly let's look at below figure.

![Rooks](https://m3verma.github.io/Programming/Codeforces/Images/Hello_2022_2.png)

Initially the rook is at (1,1) position. So no rook should be in any row/column crossed of by red colour. If we move rook to its adjacent side (denoted by green and yellow 'X' mark) then no rook should be in row/column denoted by yellow and green colour respectively. So effectively 1 rook is occupying 2 rows/columns.

Since it is an nxn chessboard and to make k rooks stable each rook requires 2 rows/columns so we can come up with a simple solution that if k > [(n+1)/2] then there is no stable arrangement. Moreover, we can always place k≤⌊(n+1)/2⌋ rooks into cells [(1,1), (3,3), …, (2k−1,2k−1)]. Such arrangement is stable because after moving any rook to the neighbouring row it will be in the same collumn and in the even numbered row where there are no other rooks. The same applies for moving rook to the neighbouring collumn.

Since we have set a baseline lets try to code it :

```c++
#include <iostream>
#include <bits/stdc++.h>
using namespace std;

int main() {
	int t,n,k,rooks_placed=0;
	cin>>t;
	while(t--)
	{
		cin>>n>>k;
		if(k>(n+1)/2)
		{
			cout<<"-1"<<endl;
		}
		else
		{
			rooks_placed=0;
			for(int i=0;i<n;i++)
			{
				for (int j=0;j<n;j++)
				{
					if(i==j && i%2==0 && rooks_placed<k)
					{
						rooks_placed++;
						cout<<"R";
					}
					else
					cout<<".";
					
				}
				cout<<endl;
			}
		}
	}
	return 0;
}
```

