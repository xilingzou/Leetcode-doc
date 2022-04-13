### 207. Course Schedule
Given a total of _numCourses_ courses and an array _prerequisites_ where _prerequisites[ai, bi]_ indicates you must <br>
take course _bi_ first if you want to take _ai_. Return whether you are able to take courses 0 to _numCourse - 1_.

#### 理解：
- Each course -> a vertex
- Determine if the prerequisites form a Directed Acyclic Graph (DAG)

#### 1. Backtracking
Check if there's a cycle
```
class Solution:
  define canFinish(self, numCourses, prerequisites):
      """
      :type numCourses: int
      :type prerequisites: List[List[int]]
      :rtype: bool
	  
      """
	  # defaultdict avoids keyError
	  from collections import defaultdict
	  # pass object type (create a default object when key doesn't exist, eg: int->0, list->[])
	  courseDict = defaultdict(list)
	  
	  for pair in prerequisites:
	      course, pre = pair[0], pair[1]
		  
		  # directly access the value of course
		  courseDict[course].append(pre)
		  
	   # set to record visited node
	   visited = set()
	   for c in range(numCourse):
	       if self.isCyclic(c, courseDict, visited):
			   return False
	   return True
		  
	  def isCyclic(node, courseDict, visited):
	  """
	  backtracing method: check that no cycle will be ditected starting from the node
	  """
		  if node in visited:
			  return True
		  # before backtracking, add the node to visited
		  visited.add(node)

		  ret = False
		  for c in courseDict[node]:
			  ret = self.isCyclic(c, courseDict, visited):
			  if ret:
				  break
					 
		  # !!!after backtracking, remove the node from visited
		  # because we are just examining this node
		  visited.remove(node)
		  return ret  
  
```
#### Sum:
- from collections import defaultdict: avoids KeyError, automatically create default objects
- 检查成环的基础算法：backtracking
- 明确函数的作用，isCyclic用于检查该node下去是否成环，检查完毕将该node移除visited，否则检查其他node成环会出错
- complexity:
	- time: O(|E|+|V|^2) (for each vertex, we could visited all other nodes if they are trained)
	- space: 
		- build an adjacency map of a graph: O(|V|+|E|)
		- visited set: |V|
		- recursion: all nodes chained up |V|

#### 2. Postorder DFS
- Postorder: visit a node's descendant nodes before the node itself
- If the descendant nodes don't form a cycle, then adding the node itself won't form a cycle
- We don't go through the chain that has been visited
```
class Solution:
  define canFinish(self, numCourses, prerequisites):
      """
      :type numCourses: int
      :type prerequisites: List[List[int]]
      :rtype: bool
	  
      """
	  # defaultdict avoids keyError
	  from collections import defaultdict
	  # pass object type (create a default object when key doesn't exist, eg: int->0, list->[])
	  courseDict = defaultdict(list)
	  
	  for pair in prerequisites:
	      course, pre = pair[0], pair[1]
		  
		  # directly access the value of course
		  courseDict[course].append(pre)
		  
	   # set to record visited node
	   visited = set()
	   for c in range(numCourse):
	       if self.isCyclic(c, courseDict, visited):
			   return False
	   return True
		  
	  def isCyclic(node, courseDict, visited):
	  
		  if node in checked:
		  	  # 1): this node and its subgraph has been checked
			  return False
			  
		  if node in visited:
		      # came across a visited node, cycle!
			  return True
		
		  visited.add(node)
		  
		  ret = False
		  for c in courseDict[node]:
			  ret = self.isCyclic(c, courseDict, visited):
			  if ret:
				  break 
					 
		  # !!!after backtracking, remove the node from visited
		  # because we are just examining this node
		  visited.remove(node)
		  
		  #2) Postorder: after recursion on its descendant nodes, process the node itself
		  checked.add(node)
		  
		  return ret  

```
#### Sum:
- Postorder DFS: after recursion process on its descendant nodes, process the node itself
- checked: keep record of checked subgraphs, avoid going through again
- Complexity:
	- time: O(|E|+|V|) 
		- build graph: traversal all edges O(|E|)
		- DFS traversal, visit each vertex and edge only once O(|V|+|E|)
	- space: O(|E|+|V|)
		- build an adjacency map of a graph: O(|V|+|E|)
		- visited and checked set: 2|V|
		- recursion: all nodes chained up |V|
		
### 210. Course Schedule II 
A total of _numCourses_ courses labeled from 0 to _numCourses_-1. Given _prerequisites_ where _prerequisites_[i] = [ai,bi], indicates you must take bi before taking ai. Return the ordering of courses
you should take to finish all courses. Return an empty array if not possible.
#### 理解：
- 有向图，应该以source为key建立dict: course c-> [all courses require c as a prerequisite]
- A stack to record topological order of a graph (from src -> dest in a directed graph), 先recurse添加node nei再添加node
- visited set不足以区分状态时，用dict记录: node->status(unprocessed, processing, processed)
```
from collections import defaultdict
class Solution:
    
    # class variable for all instances to access
    # 3 possible statuses of a node
    # unprocessed
    WHITE = 1
    # being processed
    GRAY = 2
    # processed
    BLACK = 3
    
    def findOrder(self, numCourses: int, prerequisites: List[List[int]]) -> List[int]:
        
        # create adjacency list representation of the graph 
        adj_list = defaultdict(list)
        
        # 有向图，应该souuce为key建立dict: course c -> [all courses require c as a prerequisite
        for dest, src in prerequisites:
            adj_list[src].append(dest)
        
        # a stack to record topological order of the graph (from src -> dest in a directed graph)
        topological_sorted_order = []
        is_possible = True
        
        # All vertices unprocessed
        # color dict to document the status of each node
        color = {k: Solution.WHITE for k in range(numCourses)}
        
        def dfs(node):
            nonlocal is_possible
            
            # don't recurse further if we found a cycle already
            if not is_possible:
                return
            
            # start processing
            color[node] = Solution.GRAY
            
            for nei in adj_list[node]:
                # unprocessed, recurse
                if color[nei] == Solution.WHITE:
                    dfs(nei)
                # an edge to a processing node, a cycle detected
                elif color[nei] == Solution.GRAY:
                    is_possible = False
                # for a processed nei, already in the stack, do nothing
            
            # Recursion ends. Finish processing, add to the stack
            color[node] = Solution.BLACK
            topological_sorted_order.append(node)
            
        for course in range(numCourses):
            # only need to deal with unprocessed nodes
            if color[course] == Solution.WHITE:
                dfs(course)
                    
        return topological_sorted_order[::-1] if is_possible else []
```
#### Complexity:
不确定取决于E还是V时，考虑游离unconnected vertices和两点c之间多条边(cycle)
- Time:  O(|V|+|E|)
	- build adjacency list: O(|E|)
	- DFS search: go through each vetex and edge only once: O(|V|+|E|)
不记录input和output的space
- Space: O(|V|+|E|)
	- build adjacency list: O(|E|)
	- status dict: O(|V|)
	- recursion: O(|E|)

### 78. Subsets (类似22）
Given an integer array _nums_ of unique elements, return all possible subsets.
#### 理解：
- 定义好search tree每一层的状态，index, current subset
- Top-down DFS，最后返回的答案是底层
- DFS第一步：base case check (可直接return的）
- res.append(list(cur)), 建立一份copy添加，该reference之后会被修改
- 结束每个子问题，进入下一个子问题之前，Restore State!
```
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        
        def dfs(res, nums, index, cur):
            """
            For the (index+1)(th) layer, nums[index] is added to cur subset or not
            index: Depth of the search tree: 0 to len(nums)-1
            cur: the current subset
            res: being passed and changed along the search tree
            """
            if index == len(nums):
                # Base case: reach the final layer, add to result and return
                # build a copy of cur!
                # the list referred by cur will be changed later, but we want to keep the current cur result
                res.append(list(cur))
                return
            
            # Enter next layer 
            # Subproblem 1:add num[index] and increment index
            cur.append(nums[index])     
            dfs(res, nums, index+1, cur)
            # for each of the subproblem, the last step is to RESTORE STATE
            cur.pop(len(cur)-1)
          
            # Subproblem 2: no addition of num[index] and increment index 
            dfs(res, nums, index+1, cur)
    
        res = []
        dfs(res, nums, 0, [])
        return res
```
### 139. Word Break
Given a string _s_ and a dictionary of strings _wordDict_, return True if _s_ cna be segemented into a sequence of one or more dictionary words.

#### 1. Recursion with memoization
- membership testing: set, dict O(1) much faster than list O(n)
  转化成set用 in 查找
- frozenset(list_name): immutable set, can be key in a dict
- store the result of subporblems with memo array

```
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
       
        # membership testing: set, dict O(1) much faster than list O(n)
        # 转化成set用 in 查找
        # frozenset: immutable set, can be key in a dict
        def wordBreakMemo(s: str, word_dict: FrozenSet[str], start: int, memo):
            """
            Recursion with memoization
            a memo array to store the result of subproblems, memo[i] represents the result of checking s[i:] by recursion
            return True if s[start:] can be segemented into a sequence of words in word_dict
            return False otherwise
            list->set->frozen set
            """
            #s[len(s):] is empty
            if start == len(s):
                return True
            if memo[start] != 0:
                return memo[start]
            for end in range(start+1, len(s)+1):
                if s[start:end] in word_dict and wordBreakMemo(s, word_dict, end, memo):
                    memo[start] = True
                    return True
            memo[start] = False
            return False
        
        # use memoization, avoid repeated recursion
        memo = [0 for _ in range(len(s)+1)]
        return wordBreakMemo(s, frozenset(wordDict), 0, memo)
	
```

### 22. Climb Stairs (类似 746)

#### Recursion with memoization
```
class Solution:
    def climbStairs(self, n: int) -> int:
        def dfs(left, ways):
            """
            left: the number of left cases
            ways: the number of ways by far
            return the number of ways in complete left cases
            """
            # Base case: the number of left cases<=2
            if left == 1:
                # can only take one step
                # number of ways doesn't change
                return ways
            elif left == 2:
                # can eithre take two one steps
                # or take one 2-stes
                return ways*2
            
            # check memo
            # dfs(left, 1)
            if memo[left-1] != 0:
                return ways*memo[left-1]
            
            # Two ways to take the next step
            # 1. take one step and continue
            memo[left-2] = dfs(left-1, 1)
            
            # 2. take a two step and continue
            memo[left-3] = dfs(left-2, 1)
            return ways*(memo[left-2]+memo[left-3])
        
        # number of ways is 1 when the number of left cases is n
        # use memo array to store the recursion results, avoid repeated recursion
        # memo[i] represents the dfs(i+1, 1)
        memo = [0 for _ in range(n)]
        return dfs(n, 1)
```
#### Complexity:
- Time: O(|n|) size of recursion tree can go up to n
- Space: O(|n|) the depth of recursion tree can go up tp n

#### 2. Dynamic Programming
Its optimal solution can be constructed efficiently from optimal solutions of its subproblems.
```
 #Dynamic programming

        if n<=2:
            return n
        # dp[i] represents ways to reach the (i+1)th step
        dp = [0 for _ in range(n)]

        # Base case: ways to reach to the 1st step 
        dp[0] = 1
        dp[1] = 2

        # two ways to reach the ith step:
        # take a step from the (i-1)th
        # take a two-step from the (i-2)th
        for i in range(2, n):
            dp[i] = dp[i-1]+dp[i-2]

        return dp[n-1]
```
#### Complexity:
- Time: O(|n|) single loop up to n
- Space: O(|n|) dp array size n

### Two approaches of Dynamic Programming: 
- 定义好状态
- 状态间的transition rule
#### Top-down (recursion + memoization) 
Hit the problem in a natural manner and hope that the solutions for the subproblem are already calculated and if they are not calculated, then we calculate them on the way.<br>
Start with F(n) = F(n-1) + F(n-2), recursively calculate F(n-1), F(n-2)
#### Bottom up (tabulation)
Filling up a table from the start. <br>
Start with F(0)=1, F(1)=1, F(2)=F(0)+F(1)

![alt text](https://user-images.githubusercontent.com/102558337/162212291-120272de-47f9-4934-b631-0e7bc2dfa436.png)
	
### 96. Unique Binary Search Trees

#### Recursion with memoization
```
class Solution:
    def numTrees(self, n: int) -> int:
        
        # Recursion with memoization
        # 选择了一个root后下分为两个子问题
        # 问子问题要答案，构建当前问题答案
        # the number of unique BST only depends on the number of nodes to be structured
          
        def dfs(k):
            if k<=1:
                return 1
            if memo[k] != "*":
                return memo[k]
            # choose different roots
            res = 0
            # i as root
            for i in range(1, k+1):
                res += dfs(i-1)*dfs(k-i)
            memo[k] = res
            return memo[k]
        
        # 0 to n, length n+1
        memo = ["*" for _ in range(n+1)] 
        return dfs(n)
```
##### Complexity:
- Time: for loop n, recursion depth log(n): O(nlog(n))
- Space: recursion stack goes up to O(logn)
#### Tabulation (Bottom up)
```
 # Tabulation (Bottom Up)
        memo = ["*" for _ in range(n+1)] 
        memo[0] = 1
        memo[1] = 1
        for i in range(2, n+1):
            res = 0
            for j in range(1, i+1):
                res += memo[j-1]*memo[i-j]
            memo[i] = res
            
        return memo[n]
```
##### Complexity:
- Time: Number of iterations sum(i) i=2,...,n, O(|n^2|)
- Space: O(n)

## 2D Dynamic Programming
两个变量定义状态
### 2D Array
### 63. Unique Path II
#### 1. Recursion with memoization
```
 # Recursion with memoization
        def dfs(grid, x, y):
            m = len(grid)
            n = len(grid[0])
            # return the number of unique paths from [0][0] to [x][y]
            if x == 0 and y == 0:
                return 1
            # cross the boundary
            if x>=m or y>=n or x<0 or y<0:
                return 0
            # meet obstacle
            if grid[x][y] == 1:
                return 0
            
            if memo[x][y] != "_":
                return memo[x][y]
            # ask from subproblems
            memo[x][y] = dfs(grid, x-1, y)+dfs(grid, x, y-1)
            return memo[x][y]
        
        # edge case, obstacle at the beginning
        if obstacleGrid[0][0] == 1:
            return 0
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        memo = [["_" for _ in range(n)] for _ in range(m)]
        return dfs(obstacleGrid, m-1, n-1)
```

#### Tabulation
```
# Tabulation
        m = len(obstacleGrid)
        n = len(obstacleGrid[0])
        memo = [[0 for _ in range(n)] for _ in range(m)]
        
        memo[0][0] = 1
        for i in range(m):
            for j in range(n):
                # covers edge case when obstacleGrid[0][0] == 1
                if obstacleGrid[i][j] == 1:
                    memo[i][j] = 0
                # exlcude [0][0]
                elif i>0 or j>0:
                    flagi = 0 if i-1<0 else 1
                    flagj = 0 if j-1<0 else 1
                    memo[i][j] = memo[i-1][j]*flagi + memo[i][j-1]*flagj
        return memo[m-1][n-1]
```
### Two 1D array
### 1143 Longest Common Subsequence
#### Recursion with memoization
```
def dfs(t1, t2, x, y):
            # return the length of longest common subsequence of t1[:x+1] and t2[:y+1]
            
            # out of range
            if x<0 or y<0 or x>=len(t1) or y>=len(t2):
                return 0
            if memo[x][y] != "_":
                return memo[x][y]
            # ask from subproblems
            if t1[x] == t2[y]:
                memo[x][y] = dfs(t1, t2, x-1, y-1) + 1
            else:
                memo[x][y] = max(dfs(t1, t2, x-1, y), dfs(t1, t2, x, y-1))
            return memo[x][y]
        
        # initialize 
        l1 = len(text1)
        l2 = len(text2)
        memo = [["_" for _ in range(l2)] for _ in range(l1)]
        return dfs(text1, text2, l1-1, l2-1)
```
#### Tabulation
```
 # initialize 
        l1 = len(text1)
        l2 = len(text2)
        # padding
        memo = [[0 for _ in range(l2+1)] for _ in range(l1+1)]
        # index for loop is 1 more than the corresponding index of t1 and t2, 0 for padding, loop starts from 1
        # memo[i][j] represents the result for t1[:i] and t2[:j]
        for i in range(1, l1+1):
            for j in range(1, l2+1):
                if text1[i-1] == text2[j-1]:
                    memo[i][j] = memo[i-1][j-1] + 1
                else:
                    memo[i][j] = max(memo[i-1][j], memo[i][j-1])
        return memo[l1][l2]
```
##### Sum
- Padding: add 1 more row and 1 more column for memo, index for loop starts from 1 (represents the results for index 0 of t1 and t2)

### 304. 
Calculate the sum of the elements of _matrix_ inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

#### Recursion with memoization
```
class NumMatrix:

    def __init__(self, matrix: List[List[int]]):
        self.mat = matrix
        self.m = len(matrix)
        self.n = len(matrix[0])
        self.memo = [["*" for _ in range(self.n)] for _ in range(self.m)]
    
    # recursion with memoization
    def dfs(self, r, c):
        # return the sum of submat from upper left corner [0][0] to lower right corner [r][c]
            # out of range
        if r<0 or c<0 or r>=self.m or c>=self.n:
            return 0
        if self.memo[r][c] != "*":
            return self.memo[r][c]
        if r == 0 and c == 0:
            return self.mat[0][0]
        self.memo[r][c] = self.mat[r][c]+self.dfs(r,c-1)+self.dfs(r-1,c)-self.dfs(r-1,c-1)
        return self.memo[r][c]

    def sumRegion(self, row1: int, col1: int, row2: int, col2: int) -> int:


    # Your NumMatrix object will be instantiated and called as such:
    # obj = NumMatrix(matrix)
    # param_1 = obj.sumRegion(row1,col1,row2,col2)

        
        return self.dfs(row2, col2) - self.dfs(row2, col1-1) - self.dfs(row1-1, col2) + self.dfs(row1-1, col1-1)
```

#### Tabulation (padding)
```
    def sumRegion(self, row1: int, col1: int, row2: int, col2: int) -> int:


    # Your NumMatrix object will be instantiated and called as such:
    # obj = NumMatrix(matrix)
    # param_1 = obj.sumRegion(row1,col1,row2,col2)

        # padding
        for i in range(1, self.m+1):
            for j in range(1, self.n+1):
                self.memo[i][j] = self.mat[i-1][j-1] + self.memo[i-1][j] + self.memo[i][j-1] - self.memo[i-1][j-1]
            
        
        return self.memo[row2+1][col2+1] - self.memo[row2+1][col1] - self.memo[row1][col2+1] + self.memo[row1][col1]
    
```
### Two pointers -> a subarray
### 877. Stone Game
Given a _piles_ of stones, two players take turns to take the first or last pile of the remaining stones, the one having the most stones wins.
#### Recursion with memoization
```
# Recursion with memoization
        def dfs(piles, i, j):
            # return the relative score of the player who takes the first move on the subarray piles[i:j+1]
            # relative score: the score of the first player - the score of the next player
            if i == j:
                return piles[i]
            # empty array
            if i>j:
                return 0
            if memo[i][j] != -1:
                return memo[i][j]
            # max: take the best move
            memo[i][j] = max(piles[i]-dfs(piles, i+1, j), piles[j]-dfs(piles, i, j-1))
            return memo[i][j]
        
        # Initialize
        p = len(piles)
        memo = [[-1 for _ in range(p)] for _ in range(p)]
        ans = dfs(piles, 0, p-1)
        return ans>0
```
#### Tabulation
```
 # Tabulation
        # 画出search tree, 从search tree最低端开始填
        # Initialize
        p = len(piles)
        # fill invalid (i>j) coordinates with 0
        memo = [[0 for _ in range(p)] for _ in range(p)]
        # subarray length == 1
        for i in range(p):
            memo[i][i] = piles[i]
        # subarray length
        for l in range(2, p+1):
            for i in range(p-l+1):
                memo[i][i+l-1] = max(piles[i]-memo[i+1][i+l-1], piles[i+l-1]-memo[i][i+l-2])
        return memo[0][p-1]>0
```

