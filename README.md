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
