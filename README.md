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
		
### 210. Course Schedule II o
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
### 78. Subsets
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





