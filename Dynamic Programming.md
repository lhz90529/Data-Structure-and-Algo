##[Dynamic Programming](https://people.eecs.berkeley.edu/~vazirani/algorithms/chap6.pdf)

### Shortest paths in dags, revisited
- The _special distinguishing feature of a dag_ is that its nodes can be ___linearized___; that is, they can be arranged on a line such that ___all edges go from left to right___
$\hspace{1mm}$
- __e.g.__
![example](https://raw.githubusercontent.com/lhz90529/Data-Structure-and-Algo/master/pictures/dag.PNG)
    - __Goal:__ find the $distance$ from node $S$ to all other nodes, where $distance$ stands for `the shortest path`
    - Suppose we focus our attention on node $D$. The only way to reach to it is through its $predecessors, B \hspace{1mm} or \hspace{1mm} C$
    - Thus we have: dist($D$) = $\min\{dist(B) + 1, dist(C) + 3\}$
    - The similar relation can be applied for every node
---
__Pseudo code__
$\hspace{1mm}$
Initialize $dist(v) = \infty$ for all $v \in V$
$dist(s) = 0 \longrightarrow$ can be obtained directedly
for each $v \in V$, in linearized order:
$\hspace{4mm}$ dist($v$) = $\min_{(u,v) \in E} \{dist(u) + l(u,v)\} $
>i.e. for all the incoming edge $(u,v)$ from $u$ to $v$
$\hspace{4mm}$ we compute dist($u$) + $l(u,v)$ $\rightarrow$ ___the shortest path from $s$ to $v$ via intermediate station $u$___
$\hspace{4mm}$ we want the minimum one to reach $v$ regardless what the inermediate station $u$ is

---
- This algorithm is solving ___a collection of subproblems___ dist($u$) : $u \in V$ 
    - i.e. ultimately we get all distance (shortest path) from $s$ to all other nodes
- ___Important conclusions:___
    - _Dynamic Programming_ is a very powerful algorithmic in which __a problem is solved by identifying a collection of subproblems and tackling them one by one, smallest first, using the answers to small problmes to help figure out larger ones, until the whole lot of them is solved.__
    - In dynamic programming we are not given a dag, the dag is _implicit_
        - __nodes:__ are the $subproblem$ we define
        - __edges:__ are the $dependencies$ between the subproblems
        - If to solve subproblem $B$ we need the answer to subproblem $A$, then there is a ___(conceptual)___ edge from $A$ to $B$, in this case, $A$ is thought of as a smaller subproblem $B$
        ![simple model](https://raw.githubusercontent.com/lhz90529/Data-Structure-and-Algo/master/pictures/simple%20model.png)

        - ***subproblems has topological ordering***
        - ***You can solve a subproblem when all subproblems that contribute to it has been solved already"***    
 ### [Longest increasing subsequences](https://leetcode.com/problems/longest-increasing-subsequence/description/)
 - **Problem definition:**
 > Given a sequence of numbers $a_{1},\dots,a_{n}$, find the length of the longest increasing sequence. For example, the longest incresing subsequence of 5, 2, 8, 6, 3, 6, 9, 7 is 2, 3, 6, 9
 - **Restrictions on increasing subsequence:**
    1. __Temporal:__ If you pick the $i-th$ element and include it into your solution set, you can only pick elements from $a_{i + 1},\dots,a_{n}$ in the future. _You can never pick elements backward_
    2. __Value:__ You can only pick elements strictly in increasing order
$\hspace{1mm}$
 - **Transfer Problem into DAG**
 ![LSI](https://raw.githubusercontent.com/lhz90529/Data-Structure-and-Algo/master/pictures/LSI.PNG)
 $\hspace{1mm}$
- __It is the restrctions mentioned (Temporal and Value) above define the edge of our DAG__
    - __Temporal:__ $(u,v)$ is always from left to right 
    - __Value:__ $(u,v)$ is exsited iff $v$ is strictly larger thant $u$
    - Basically, there is an edge between $u$ and $v$ if and only if
        -  $u$ is at the left of $v$
        - $value(v) > value(u)$ 
- __L($j$) is defined as the length of the longest increasing subsequence ending at j__
- Thus our problem turns into:
$$Problem = max\{L(j)\}, for \space all \space 0 \leq j < n$$
- We can compute L($j$) from **_left to right_** such that we already have the answer of all smaller subproblems contributing to L($j$) when we want to compute L($j$)
---
- **Pseudo code**
$\hspace{1mm}$
for $j = 0, 2,\dots,n - 1$
$\hspace{4mm}$$L(j) = 1 + \max\{L(i)\}: (i,j) \in E$
return $\max_{j}L(j)$
i.e. for all previous nodes that can contribute current node $j$, find the maximum contribution

---
### Related to Divide and Conquer
- Is a good idea to solve the problem above using recursion?
    - __NO__, since the subproblem is **highly overlapped** such that each subproblem may be computed for __multiple times__
- What problem is suitable for recursion?
    - when there is __no overlapping between subproblems__, i.e. they are **independent** to each other such that each subproblem will be computed __only once__. _That's why recursion work so well in Divide and Conquer_

|                         | Dependency between subproblems | Size of subproblems | Height of recursion tree |
|-------------------------|:------------------------------:|:-------------------:|:------------------------:|
| **Divide and Conquer**  |           Independent          |  drop substantially |           small          |
| **Dynamic Programming** |           Overlapped           |   drop very slowly  |           large          |

### [Edit distance](https://leetcode.com/problems/edit-distance/description/)
- Given two strings, find the minimum number of steps required to convert word1 to word2.
- 3 operations are allowed to perform on each word
    - *Insert a character*
    - *Delete a character*
    - *Replace a character*
- **Define subproblems**
> Try to *get rid of redundant information* to reduce the size of problem $\implies$ get subproblem
- Suppose we are given two string `S1[1 : m ]` and `S2[1 : n]`
    - __If the last character of S1 is exactly same as the last character of S2__
       - All we need to do is find the minimum operations needed to convert the prefix `S1[1 : m - 1]` to the prefix `S2[1 : n - 1]`
       ![same tail](https://raw.githubusercontent.com/lhz90529/Data-Structure-and-Algo/master/pictures/ED-eqtail.png)
        $\hspace {1mm}$
    - __Else $\implies$ the tail charcters are different__
        - We can do the following 3 operations:
            1. **delete the last character of S1,** and find the minimum operations needed to convert S1[1, m - 1] to S2[1, n];
            ![case1](https://raw.githubusercontent.com/lhz90529/Data-Structure-and-Algo/master/pictures/ED-difftail-case1.png)
            $\hspace {1mm}$
            2. **delete the last charcter of S2,** and find the minimum operations needed to convert S1[1, m] to S2[1, n - 1];
            ![case2](https://raw.githubusercontent.com/lhz90529/Data-Structure-and-Algo/master/pictures/ED-difftail-case2.png)
             $\hspace {1mm}$
            3. **replace the last character of S1 with the last character of S2,** and find the minimum operations needed to convert S1[1, m - 1] to S2[1, n - 1]
            ![case3](https://raw.githubusercontent.com/lhz90529/Data-Structure-and-Algo/master/pictures/ED-difftail-case3.png)

- **Relation of subproblem**
$$Prob(i, j) = Prob(i - 1, j - 1) \hspace{1mm} if \hspace{1mm} same \hspace{1mm} tail$$
$$=min\{Prob(i - 1, j), Prob(i, j - 1), Prob(i - 1, j - 1)\} + 1, different \space tail \space char$$
![DAG](https://raw.githubusercontent.com/lhz90529/Data-Structure-and-Algo/master/pictures/ED-DAG.png)
- __Base case:__
    - Prob(" "," ") = 0
    - Prob(" ", non-empty string) = size of non-empty string $\implies$ consecutive insertions
    - Prob(non-empty string, " ") = size of non-empty string $\implies$ consecutive deletions
- __Code__
```c++
class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size();
        int n = word2.size();
        vector<vector<int>> table(m + 1, vector<int>(n + 1));
        
        for (int i = 0; i < m + 1; ++i) {//non-empty to empty
            table[i][0] = i;
        }
        
        for (int j = 0; j < n + 1; ++j) {//empty to non-empty
            table[0][j] = j;
        }
        
        for (int i = 1; i < m + 1; ++i) {//fill table
            for (int j = 1; j < n + 1; ++j) {
                if (word1[i - 1] == word2[j - 1]) {//same tail char
                    table[i][j] = table[i - 1][j - 1];
                } else {//different tail
                    table[i][j] = min(min(table[i - 1][j - 1], table[i - 1][j]), table[i][j - 1])+ 1;
                }
            }
        }
  
        return table.back().back();
    }
};
```

### Common subproblems
> Finding the right subproblem takes _creativity and experimentation._ But there are a few standard choices that seem to arise __repeatedly__ in dynamic programming
1. The input is $x_{1},x_{2},\dots,x_{n}$, 
    - subproblem is $x_{1},x_{2},\dots,x_{i}$ $\implies$ **prefix**$[1\hspace{1mm}:\hspace{1mm}i]$
    - Number of subproblems: $O(n)$
2. The input is $x_{1},x_{2},\dots,x_{n}$ and $y_{1},y_{2},\dots,y_{m}$, 
    - subproblem is $x_{1},x_{2},\dots,x_{i}$ and $y_{1},y_{2},\dots,y_{j}$ $\implies$ prefix$[1\hspace{1mm}:\hspace{1mm}i]$, prefix$[1\hspace{1mm}:\hspace{1mm}j]$
    - Number of subproblems: $O(mn)$
3. The input is $x_{1},x_{2},\dots,x_{n}$, 
    - subproble is $x_{i},x_{i+1},\dots,x_{j}$ $\implies$ **substring**$[i\hspace{1mm}:\hspace{1mm}j]$
    - Number of subproblems: $O(N^2)$
4. The input is a rooted tree,
    - subproblem is a **rooted subtree**

### Knapsack $\rightarrow$ resource-constrained slections
- __Problem definition__
    - ___What is the maximum profit you can gain?___
    - Given a lists of items $\hspace{8mm}i_{1}, i_{2},\dots,i_{n}$
        - Each items has a value $\hspace{4mm}v_{1}, v_{2},\dots,v_{n}$
        - Each items have a weight $\hspace{1mm}w_{1}, w_{2},\dots,w_{n}$
    - Given a single container with __limit capacity__ $W$
    
- __Example__
    | Item | Weight | Value |
    |:----:|:------:|:-----:|
    |   1  |    6   |  $30  |
    |   2  |    3   |  $14  |
    |   3  |    4   |  $16  |
    |   4  |    2   |   $9  |
    The maximum profit you can gain: \$48 (two of items) if repetition is allowed
    The maximum profit you can gain: \$46 (item 1 and 3) if repetition is probhibit

- __Two versions__
    1. Pick items with repetition
    2. Pick items withouth repetition

#### Knapsack with repetition
- How to define subproblem,
    - Can we look at fewer items?
        - No, since repetition is allowed. You can always chose item from the whole list regardless what previous item you selected

    - Can we reduce the knapsack capacities?
        - Yes, when a item is selected and put into the knapsack, the capacity will decrease accordingly
- Guess:
    - If the item $i$ is in the optimal solution set,
        - $Prob(W) = Prob(W - w_{i}) + v_{i}, \space if \space w_{i} \leq W$
        - We don't know what the item $i$ is, *so we try them all*  
        > i.e. 
        If I pick item $1$, what the maximum profit I can gain?
        If I pick item $2$, what the maximum profit I can gain?
        $\vdots$
        If I pick item $n$, what the maximum profit i can gain?
        __Try them all__, and find the maximum one
        
    ![DAG](https://raw.githubusercontent.com/lhz90529/Data-Structure-and-Algo/master/pictures/knapsack%20repetition.png)

    > It can be easily see that there are total $W$ subproblems
    Each subproblem can be solved in $O(n)$, i.e. try all items
    The overall complexity: $O(Wn)$ 
- **Pseudo Code**
$\space$
$K(0) = 0$
for $w = 1 \hspace{2mm} to \hspace{2mm} W$
$\hspace{4mm} K(w) = \max\{K(w - w_{i}) + v_{i}:w_{i} \leq w\}$
$return \hspace{1mm} K(w)$

#### Knapsack without repetition
- __Try to decrease the size of your problem__
    - Can we look at fewer items?
        - Yes and Must, since repetition is not allowed. You cannot chose item that is already in your knapsack;

    - Can we reduce the knapsack capacities?
        - Yes, when a item is selected and put into the knapsack, the capacity will decrease
-  __Each time you pick an item and put it into your knapsack,__
    -  The capacity of your knapsack decreases $\downarrow$
    -  Your choice of items narrows down $\downarrow$
- __Guess:__
    - For each item $i$ you could have **ONLY** two choices, 
        1. Pick it up and put into your knapsack if you can ($w_{i} <= W$) , in this case
        $$Prob(W, n) = Prob(W - w_{i}, n - 1) + v_{i}$$ 
        2. Either you don't want the item at all, or the item is not affordable for your knapsack
        $$Prob(W, n) = Prob(W, n - 1)$$
    - Prob(W, n) is defined as: "the maximum profit you can gain if you can select items from $i_{1}, i_{2},\dots,i_{n}$, and your knapsack has $W$ available space
        > **Note that:** the item can been seen as `prefix[1:n]`

    ![DAG](https://raw.githubusercontent.com/lhz90529/Data-Structure-and-Algo/master/pictures/knapsack%20no%20repetition.png)
    
    > Number of subproblems: $\bold{W * n}$
    Each subproblem can be solved in: $\bold{O(1)}$
    The overall complclass: $\bold{O(Wn)}$


- **Pseudo Code**
Create a 2D table and initialize the first row and first column to 0
$for \space j = 1 \space to \space n:$
$\hspace{4mm} for \space w = 1 \space to \space W:$
$\hspace{8mm} if \space w_{j} > w$
$\hspace{12mm} K(w,j) = K(w,j - 1)$
$\hspace{8mm} else$
$\hspace{12mm} K(w,j) = \max\{K(w,j - 1), K(w - w_{j}, j - 1) + v_{j}\}$
$return K(W,n)$

#### [Practice](http://www.lintcode.com/en/problem/backpack-ii/)
```c++
class Solution {
public:
    int backPackII(int W, vector<int> &weight, vector<int> &value) {
        // write your code here
        int n = weight.size();
        vector<vector<int>> table(n + 1, vector<int>(W + 1));
        
        for (int i = 1; i < n + 1; ++i) {
            for (int j = 1; j < W + 1; ++j) {
                if (weight[i - 1] > j) {
                    table[i][j] = table[i - 1][j];
                } else {
                    table[i][j] = max(table[i - 1][j - weight[i - 1]] + value[i - 1], table[i - 1][j]);
                }
            }
        }
        
        return table.back().back();
    }
};
```

### Memoization
$\underline{getSolution}$(n) {
$\hspace{4mm}$if solution(n) is  in  hash-table
$\hspace{8mm}$ return solution(n) ***directly***
$\hspace{4mm}$else
$\hspace{8mm}$ calculate solution(n)
$\hspace{8mm}$ **save** solution(n) into hash-table before return it
$\hspace{8mm}$ return solution(n);
}
> such that each subproblem will be calculated **only once**, thus achieve overall complexity:
$$ Number of subproblems * time \space cost \space per \space subproblem$$
otherwise, this kind of recursive function without memoization could cause an exponential runtime