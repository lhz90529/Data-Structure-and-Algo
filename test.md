## [1. Two Sum](https://leetcode.com/problems/two-sum/)
### Tags
Array, **HashTable**.  
#### Input: 
int target, an array
#### Output: 
Return the **_index_** of two numbers that can be added up to target;
#### Constrain: 
Each number can be used __ONLY once__
#### Basic idea:
* Fill the Hash Table with __<key, value>__ pair ---> __<what the value is, where it is>__
* For each number **‘x’** check if its counterpart **'target - x'** is existed in the table;
#### Pseudo code:
1. Create an empty Hash Table ---> **Spatial cost: O(N)**
2. Fill up the Hash Table:

		For each number x from input array ---> Temporal cost: O(N)
			table[x] = index of x; ---> Hash Table insertion: O(1)
		End For
**such that later:**
1. We can find if a value is existed in our table
2. If it's existed, we can find where it is
3. Search:
		
        For each number x from input array
		If we can find the counterpart 'v' = target - x from table ---> Hash Table Search: O(1) and If x and v are not the same number; i.e. they don't share same location;
            	return index of x and index of v;
        End For
       
