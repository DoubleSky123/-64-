# DAY 51｜200. Number of Islands 695. Max Area of Island
## 学习内容
[图论理论基础](https://programmercarl.com/kamacoder/%E5%9B%BE%E8%AE%BA%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%80.html#%E5%9B%BE%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%A6%82%E5%BF%B5)
## 200. Number of Islands
[Leetcode Link](https://leetcode.cn/problems/number-of-islands/description/)-Medium
### Description
>Given an m x n 2D binary grid grid which represents a map of '1's (land) and '0's (water), return the number of islands.
>An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.
>>**Example 1:**
>>**Input:**
>>Input: grid = [["1","1","1","1","0"],["1","1","0","1","0"],["1","1","0","0","0"],["0","0","0","0","0"]], Output: 1
>>**Output:**
>>1
### Code
#### Method 1 BFS
>
```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        r = len(grid)
        c = len(grid[0])
        visited = [[False]*c for _ in range(r)]
        count = 0
        def bfs(grid,visited,x,y):
            que = deque()
            que.append((x,y))
            direction = [(0,1),(1,0),(0,-1),(-1,0)]
            visited[x][y] = True
            while que:
                cur_x,cur_y = que.popleft()
                for dx,dy in direction:
                    next_x = cur_x+dx
                    next_y = cur_y+dy
                    if next_y < 0 or next_x < 0 or next_x >= len(grid) or next_y >= len(grid[0]):
                        continue
                    if not visited[next_x][next_y] and grid[next_x][next_y] == '1':
                        visited[next_x][next_y] = True
                        que.append((next_x,next_y))
        for i in range(r):
            for j in range(c):
                if grid[i][j] == '1' and not visited[i][j]:
                    count += 1
                    bfs(grid,visited,i,j)
        return count
```
> - Time: O(MN)
> - Space: O(MN)
#### Method 2 DFS
>
```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        n = len(grid)
        m = len(grid[0])
        visited = [[False] * m for _ in range(n)]
        result = 0
        for i in range(n):
            for j in range(m):
                if grid[i][j] == '1' and not visited[i][j]:
                    result += 1
                    visited[i][j] = True
                    self.dfs(grid,visited,i,j)
        return result
    def dfs(self,grid,visited,x,y):
        direction = [[0, 1], [1, 0], [0, -1], [-1, 0]]
        for i, j in direction:
            next_x = x+i
            next_y = y+j
            if next_x < 0 or next_x >= len(grid) or next_y < 0 or next_y >= len(grid[0]):
                continue
            if not visited[next_x][next_y] and grid[next_x][next_y] == '1':
                visited[next_x][next_y] = True
                self.dfs(grid,visited,next_x,next_y)
```
> - Time: O(MN)
> - Space: O(MN)
## 695. Max Area of Island
[Leetcode Link](https://leetcode.cn/problems/largest-rectangle-in-histogram/description/)-Hard
### Description
>Given an array of integers heights representing the histogram's bar height where the width of each bar is 1, return the area of the largest rectangle in the histogram.
>>**Example 1:**
>>**Input:**
>>heights = [2,1,5,6,2,3]
>>**Output:**
>>10
>>**Explanation:**
>>The above is a histogram where width of each bar is 1. The largest rectangle is shown in the red area, which has an area = 10 units.
### Code
>和上题一样还是求面积，用单调递减栈。
```python
class Solution:
    def largestRectangleArea(self, heights: List[int]) -> int:
        heights.insert(0,0)
        heights.append(0)
        stack = [0]
        res = 0
        for i in range(1,len(heights)):
            if heights[i] > heights[stack[-1]]:
                stack.append(i)
            elif heights[i] == heights[stack[-1]]:
                stack.pop()
                stack.append(i)
            else:
                while stack and heights[i] < heights[stack[-1]]:
                    mid = stack.pop()
                    if stack:
                        left = stack[-1]
                        right = i 
                        w = right-left -1
                        h = heights[mid]
                        res = max(res,w*h)
                        print(res,left,right,mid)
                stack.append(i)
        return res
```
> - Time: O(N)
> - Space: O(N)
## 今日心得
- 单调栈求面积，难点在于面积的计算，第一个弹出的值是中间值。以及碰到相同元素弹出。
