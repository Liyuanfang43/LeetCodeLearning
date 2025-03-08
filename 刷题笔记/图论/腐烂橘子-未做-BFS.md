# 腐烂橘子

在给定的 m x n 网格 grid 中，每个单元格可以有以下三个值之一：

- 值 0 代表空单元格；
- 值 1 代表新鲜橘子；
- 值 2 代表腐烂的橘子。

每分钟，腐烂的橘子 周围 4 个方向上相邻 的新鲜橘子都会腐烂。

返回 直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 -1 。


思路：类似找轮数的问题都是BFS（思路参考树的层序遍历），需要使用队列。

````py
class Solution:
    def orangesRotting(self, grid: List[List[int]]) -> int:
        # BFS算法，需要使用队列
        m, n = len(grid), len(grid[0])

        queue = []
        count = 0
        for i in range(m):
            for j in range(n):
                if grid[i][j] == 1:
                    count += 1
                elif grid[i][j] == 2:
                    queue.append((i, j))
        
        mins = 0
        while count > 0 and queue:
            mins += 1
            for i in range(len(queue)):
                r, c = queue.pop(0)
                if r - 1 >= 0 and grid[r-1][c] == 1:
                    count -= 1
                    grid[r-1][c] = 2
                    queue.append((r-1, c))
                if r + 1 < m and grid[r+1][c] == 1:
                    count -= 1
                    grid[r+1][c] = 2
                    queue.append((r+1, c))
                if c - 1 >= 0 and grid[r][c-1] == 1:
                    count -= 1
                    grid[r][c-1] = 2
                    queue.append((r, c-1))
                if c + 1 < n and grid[r][c+1] == 1:
                    count -= 1
                    grid[r][c+1] = 2
                    queue.append((r, c+1))
        if count > 0:
            return -1
        else:
            return mins
````