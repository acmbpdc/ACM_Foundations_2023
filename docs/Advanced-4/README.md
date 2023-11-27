# Advanced Session: 4
## Recursion and Dynamic Programming

## Recursion
Recursion is like a funhouse mirror, creating a seemingly endless loop of self-referential code. It's a powerful technique that allows functions to call themselves, leading to a series of nested steps that solve complex problems in an elegant way.

**Eg 1:** counting down from a given number. Instead of writing a loop, you could use recursion to create a function that calls itself, decreasing the number by one each time until it reaches zero.

``` python
    def countdown(n):
        if n == 0:
            print("Blast off!")
        else:
            print(n)
            countdown(n - 1)
```

**Eg 2:** Factorial calculation (Factorial is the product of all positive integers from 1 to a given number)

``` python
    def factorial(n):
        if n == 1:
            return 1
        else:
            return n * factorial(n - 1)
```

Recursion has both it's pros and cons, as listed below.
**Advantages of recursion** 
* Elegant and Concise
* Modularity and Reusability
* Ease of Implementation

**Drawbacks of recursion**

Recursion uses memory space less efficiently. Repeated function calls create entries for all the variables and constants in the function stack. As the values are kept there until the function returns, there is always a limited amount of stack space in the system, thus making less efficient use of memory space. Additionally, a stack overflow error occurs if the recursive function requires more memory than is available in the stack.

Recursion is also relatively slow in comparison to iteration, which uses loops. When a function is called, there is an overhead of allocating space for the function and all its data in the function stack in recursion. This causes a slight delay in recursive functions.

## Dynamic Programming?

Dynamic programming is a computer programming technique where an algorithmic problem is first broken down into sub-problems, the results are saved, and then the sub-problems are optimized to find the overall solution — which usually has to do with finding the maximum and minimum range of the algorithmic query.

### Recursion vs. dynamic programming

In computer science, recursion is a crucial concept in which the solution to a problem depends on solutions to its smaller subproblems.

Meanwhile, dynamic programming is an optimization technique for recursive solutions. It is the preferred technique for solving recursive functions that make repeated calls to the same inputs. A function is known as recursive if it calls itself during execution. This process can repeat itself several times before the solution is computed and can repeat forever if it lacks a base case to enable it to fulfill its computation and stop the execution.

However, not all problems that use recursion can be solved by dynamic programming. Unless solutions to the subproblems overlap, a recursion solution can only be arrived at using a divide-and-conquer method.

For example, problems like merge, sort, and quick sort are not considered dynamic programming problems. This is because they involve putting together the best answers to subproblems that don’t overlap.

## Leetcode Question
[Problem Link](https://leetcode.com/problems/unique-paths/description/)

There is a robot on an  `m x n`  grid. The robot is initially located at the  **top-left corner**  (i.e.,  `grid[0][0]`). The robot tries to move to the  **bottom-right corner**  (i.e.,  `grid[m - 1][n - 1]`). The robot can only move either down or right at any point in time.

Given the two integers  `m`  and  `n`, return  _the number of possible unique paths that the robot can take to reach the bottom-right corner_.

The test cases are generated so that the answer will be less than or equal to  `2 * 109`.

**Example 1:**

![](https://assets.leetcode.com/uploads/2018/10/22/robot_maze.png)

**Input:** m = 3, n = 7
**Output:** 28

**Example 2:**

**Input:** m = 3, n = 2
**Output:** 3
**Explanation:** From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
1. Right -> Down -> Down
2. Down -> Down -> Right
3. Down -> Right -> Down

**Constraints:**

-   `1 <= m, n <= 100`


### Solution
### Question Understanding-

**Given are only two valid moves :**

-   **Move right**
-   **Move down**  
    Hence, it is clear number of paths from cell (i,j) = sum of paths from cell( i+1,j) and cell(i,j+1)

**It becomes a DP problem.**  
But implementing DP directly  **will lead to TLE**.  
**So memoization is useful -> Storing a result**  so that we donot need to calculate it again.

dp[i][j] = d[i+1][j] + dp[i][j+1]

**BASE CASES**

**If we are in last row**, i == m-1, we only have the choice to move RIGHT, Hence number of moves will be 1.  
**If we are in last column**, j == n-1, we only have the choice to move DOWN, Hence number of moves will be 1.

### **Top-Down Dynamic Programming:**

**Step 1: Initialize the Memoization Table**

-   **Create a memoization table**  (an auxiliary 2D array) of size m x n to store computed results. Initialize all entries to -1 to indicate that no results have been computed yet.

**Step 2: Recursive Function**

-   **Implement a recursive function**, say  **uniquePathsRecursive(x, y, m, n, memo)**, which calculates the number of unique paths to reach cell (x, y) from the top-left corner.
-   **In this function:**
    -   **Check if (x, y) is the destination cell**  (m - 1, n - 1). If yes, return 1 since there is one way to reach the destination.
        
    -   **Check if the result for (x, y) is already computed**  in the memoization table (memo[x][y] != -1).  **If yes, return the stored result.**
        
    -   **Otherwise, calculate the number of unique paths**  by recursively moving right and down (if valid) and adding the results.  **Use the following logic:**  
        ➡**If (x, y) can move right**  (i.e., x < m - 1), calculate rightPaths = uniquePathsRecursive(x + 1, y, m, n, memo).  
        ➡  **If (x, y) can move down**  (i.e., y < n - 1), calculate downPaths = uniquePathsRecursive(x, y + 1, m, n, memo).  
        ➡  **The total unique paths to (x, y) are rightPaths + downPaths.**  
        ➡** Store the result** in the memoization table (memo[x][y]) and return it.
        
### Code:
```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # Create a memoization table to store computed results
        memo = [[-1 for _ in range(n)] for _ in range(m)]
        
        # Call the recursive function to compute unique paths
        return self.uniquePathsRecursive(0, 0, m, n, memo)
    
    def uniquePathsRecursive(self, x: int, y: int, m: int, n: int, memo: List[List[int]]) -> int:
        # If we reach the destination (bottom-right corner), return 1
        if x == m - 1 and y == n - 1:
            return 1
        
        # If we have already computed the result for this cell, return it from the memo table
        if memo[x][y] != -1:
            return memo[x][y]
        
        # Calculate the number of unique paths by moving right and down
        rightPaths = 0
        downPaths = 0
        
        # Check if it's valid to move right
        if x < m - 1:
            rightPaths = self.uniquePathsRecursive(x + 1, y, m, n, memo)
        
        # Check if it's valid to move down
        if y < n - 1:
            downPaths = self.uniquePathsRecursive(x, y + 1, m, n, memo)
        
        # Store the result in the memo table and return it
        memo[x][y] = rightPaths + downPaths
        return memo[x][y]
```
**Step 3: Invoke the Recursive Function**

-   Call the recursive function with the initial arguments (0, 0, m, n, memo) to find the number of unique paths.

**Step 4: Return the Result**

-   The result obtained from the recursive function call represents the number of unique paths from the top-left corner to the bottom-right corner.


### **Bottom-Up Dynamic Programming:**

**Step 1: Initialize the DP Table**

-   Create a 2D DP (dynamic programming) table of  **size m x n**  to store the number of unique paths for each cell.
-   **Initialize the rightmost column and bottom row of the DP table to 1**  because there's only one way to reach each cell in those rows/columns (by moving all the way right or all the way down).

**Step 2: Fill in the DP Table**

-   **Start from the second-to-last**  row and second-to-last column (i.e., i = m - 2 and j = n - 2).
-   **For each cell (i, j) in the grid:**
    -   Calculate the  **number of unique paths to reach (i, j)**  as the sum of the unique paths from the cell below (i+1, j) and  **the cell to the right (i, j+1)**. Use this formula:  **dp[i][j] = dp[i+1][j] + dp[i][j+1].**
    -   **Continue filling**  in the DP table row by row and column by column until you reach the top-left corner (dp[0][0]).

**Step 3: Return the Result**

-   Return the value stored in the top-left corner of the DP table (dp[0][0]), which represents the number of unique paths from the top-left corner to the bottom-right corner.

### Code:

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        # Create a 2D DP table filled with zeros
        dp = [[0] * n for _ in range(m)]
        
        # Initialize the rightmost column and bottom row to 1
        for i in range(m):
            dp[i][n-1] = 1
        for j in range(n):
            dp[m-1][j] = 1
        
        # Fill in the DP table bottom-up
        for i in range(m - 2, -1, -1):
            for j in range(n - 2, -1, -1):
                dp[i][j] = dp[i+1][j] + dp[i][j+1]
        
        # Return the result stored in the top-left corner
        return dp[0][0]
```
## Complexity

----------

**Bottom-Up Dynamic Programming:**  
**Time Complexity (TC):**  The bottom-up approach fills in the DP table iteratively, visiting each cell once. There are m rows and n columns in the grid, so the TC is  **O(m * n)**.  
**Space Complexity (SC):**  The space complexity is determined by the DP table, which is of size m x n. Therefore,  **the SC is O(m * n)**  to store the DP table.

----------

**Top-Down Dynamic Programming (with Memoization):**  
**Time Complexity (TC):**  O(m * n).  
**Space Complexity (SC):**  O(m + n).


## Four step to solution
### 1. Recursive Approach-------->Top to Down

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        def solve(ind1,ind2):
            if ind1==0 and ind2==0:
                return 1
            if ind1<0  or ind2<0:
                return 0
            return solve(ind1-1,ind2)+solve(ind1,ind2-1)
        return solve(m-1,n-1)
```

### 2. Memorization Approach

```python
class Solution:
   def uniquePaths(self, m: int, n: int) -> int:
       dp=[[-1]*(n+1) for i in range(m)]
       def solve(ind1,ind2):
           if ind1==0 and ind2==0:
               return 1
           if ind1<0 or ind2<0:
               return 0
           if dp[ind1][ind2]!=-1:
               return dp[ind1][ind2]
           left=solve(ind1,ind2-1)
           up=solve(ind1-1,ind2)
           dp[ind1][ind2]=up+left
           return left+up
       return solve(m-1,n-1)
```

### 3. Tabulation------>Bottom to Up Approach

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        dp=[[0]*(n+1) for i in range(m+1)]
        for ind1 in range(1,m+1):
            for ind2 in range(1,n+1):
                if ind1==ind2==1:
                    dp[ind1][ind2]=1
                else:
                    left=up=0
                    if ind2-1>=0:
                        left=dp[ind1][ind2-1]
                    if ind1-1>=0:
                        up=dp[ind1-1][ind2]
                    dp[ind1][ind2]=up+left
        return dp[-1][-1]
```

### 4. Space Optimization

```python
class Solution:
    def uniquePaths(self, m: int, n: int) -> int:
        prev=[0]*(n+1)
        for ind1 in range(1,m+1):
            cur=[0]*(n+1)
            for ind2 in range(1,n+1):
                if ind1==ind2==1:
                    cur[ind2]=1
                else:
                    left=up=0
                    if ind2-1>=0:
                        left=cur[ind2-1]
                    if ind1-1>=0:
                        up=prev[ind2]
                    cur[ind2]=up+left
            prev=cur
        return cur[-1]
```