Algorithm are set of sets to perform task efficiently. 
Data structure is a way to store data efficiently.
2 types of data structures
- premitive : int, float, string, boolean
- non-primitive
  - Linear: List, Tuples, linked list, array, queue, stack
  - Non-linear: Dictionary, Sets, Graph, Tree, 
Big -O notation is used describes the time eficiency of algorithms. It checks time and space complexity.
OMega greek notation is used for best case of algorithm
Theta greek notation is ued of avg case of algorithm
and Big O notation is used of worst case of algorithm

Run time complexity is measured by the number of opreation algorithm performs and illustrated as O(n) where n is operations.
O(nxn) Nsquare 

Rules of measuring time complexity of code
1. simple statement, if condition Big0(1)
2. simple for loop big(9n)
3. nested for loop big(n^2)
4. divide and conqure O(Logn)
5. when dealing with multiple statement just add them and remove constant and take biigest one.

Array: Its is an collection of same data type variable

```
import array

#define empty integer array
my_array = array.array('i')#--->O(1)
print(my_array)
my_array1 = array.array('i', [1,2,3,4]#--->O(N)
print(my_array)

import numpy as np
#declare empty np integer array 
np_array = np.array([], dtype=int)#--->O(1)
print(np_array)
np_array1 = np/array([1,2,3])#--->O(N)
print (np_array1)

# insert array
my_array1.inert(0,6) # insert at starting
my_array1.inert(2,6) # insert at middle
my_array1.inert(5,6) # insert at end

# Accessing an array by index
my_array1[2]
# removing element by value
arr1.remove(2)
# appending a value at the end
arr1.append(8)
# extending array
arr1.extend(my_new_array)
# Add items from list to array
arr1.fromlist(my_list)
# remove last array
arr.pop()
# get index of value
arr.index(22)
# reverse
arr.reverse()
# get memory buffer from arr
arr.buffer_info()
# get count occurance of value in array
arr.count(11)
# array to string
arr.tostring()
arr.fromstring()
# array to list
arr.tolist()
# slicing
arr.my_arr[1:3]
```






