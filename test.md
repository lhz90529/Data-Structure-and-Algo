## [Problem Name](https:link/to/the/problem)
### Tags
> Array, HashTable.  
#### Input: 
> int target, an array
#### Output: fv
> return the _index_ of two numbers that can be added up to target;
#### Constrain: 
> each number can be used __ONLY once__
#### Basic idea:
> 	Fill the Hash Table with __<key, value>__ pair ---> __<what the value is, where it is>__

> 	For each number ‘x’ check if its counterpart 'target - x' is existed in the table;
