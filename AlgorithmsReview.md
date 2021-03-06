# My Algorithms Review

## Reference Books
* Cracking the coding interview(outline and summary)
* An ultimate guide to coding interview, on read.amazon.com. (outline, summay and problems)
* Coding manual by Fangqin Dai(outline and problems)
* Robert Sedgewick's Algorithms(algorithms reference)
* Introduction to algorithms(algorithms reference)
* Head first Java(language reference)
* Leetcode C++ solutions by Fangqin Dai(solutions)
* Element of Programming Interviews(EPI)(solutions)
* Leetcode OJ(problems and solutions)

## Arrays and Hashtables
### Knowledge
* Hash Tables -- implementation, performance, collision.
  *  Search, insert and delete is average O(1), and we usually see them as O(1) in interview as we assume that the chance of collision is low.  
  https://discuss.leetcode.com/topic/53193/are-hash-tables-ok-here-they-re-not-really-o-1-are-they/2
  * Iterating each element in it usually requires iterating each bucket, so the iteration time is proportional to the "capacity" of the HashMap instance (the number of buckets) plus its size (the number of key-value mappings). Since the load factor is a constant, iterating n elements take O(n) time, and therefore the amortized time for iterate each element is O(1)(not strictly O(1) even if no collision. Iterator() method also needs to advance the next pointer to the next element, which could check multiple empty buckets before it). As for Java HashMap, each key-value pair is wrapped in a node, and once the number of items in a hash bucket grows beyond a certain threshold, that bucket will switch from using a linked list of entries to a balanced tree. In the case of high hash collisions, this will improve worst-case performance from O(n) to O(log n). http://openjdk.java.net/jeps/180
  * LinkedHashMap is faster at iteration and has the same time complexity for the basic operations(put, get and remove) as HashMap. Internally it uses a linked node which has before and after pointers in addition to `<key, val>` and next pointer used by HashMap.Node. The linked nodes are linked together in insertion order, and this doubly-linked list is used for iteration. Therefore, iteration over the collection-views of a LinkedHashMap requires time proportional to the size of the map, regardless of its capacity, and iterating each element takes O(1)(including getting the iterator and return the first element). However it doesn't support getting the last element in O(1) time. One workround could be caching the last element, like in "[Leetcode]Snake Game". We can also specifiy access order in the constructor so that merely querying the map with get can change the iteration order too. Note that TreeNode extends LinkedHashMap Node, so each bucket can also be turned into a BST, while keeping the insertion/access order through LinkedHashMap Node pointers.  
  Iteration on them often requires gettign a collection view first(like calling entrySet()/values()/keySet()), which just return internal references(they are all lazy-intialization, so initial value of entrySet, values and keySet fields are null) and do not copy any data. Then we can use for-each or iterator to do the iteration, which are essentially the same. https://stackoverflow.com/questions/2923856/is-the-order-guaranteed-for-the-return-of-keys-and-values-from-a-linkedhashmap-o/2924143#2924143
  * Chaining with linked lists, chaining with BST, Open Addressing with Linear Probing, Quadratic Probing and Double Hashing. (See CTCI)
* Resizeable array
  * Implementation: Vector(C++), ArrayList(Java)
  * Performance: get, set is O(1), amortized time of add is O(1) and why. Remove an element at some position is typically O(n), but could be O(1) by switching the last element with the one to be deleted and then remove the last element. In Java, ArrayList.remove(int id) does it, and if id is the last position, it simply decrease the size counter and doesn't copy any element.
* Basic language syntax for array(fixed and dynamic), hash map and hash set.
* In Java, it's better to use constructor to get the shallow copy of ArrayList than calling the clone method, since List interface does not have clone method.
* Implementation of equals and hashCode methods in Java(See *Effective Java*,
http://www.angelikalanger.com/Articles/JavaSolutions/SecretsOfEquals/Equals.html)
  * Note that if two objects are equal, their hash codes must be the same; but if two objects have the same hash codes, they don't have to be equal.
  * The hash code of Pair or unordered collection is usually implemented as sum of the hash codes of all elements.

### Problems
* [Leetcode] First Missing Positive(Algorithm and Implementation**). Think about the O(n) space solution first and then the O(1) space solution.
* K Sum problems. Can be solved using two approaches:
  * Sort the input array first. Then using two pointers starting from the beginning and the end of the array, moving one of them towards the other each time based on the comparison of the sum of the two elements and target, until they meet. The two pointers process takes O(n) time. No extra space needed, assuming sort is in place. For K > 2, always move the leftmost/rightmost pointer, and do it recursively, until the innermost two pointers which use the two pointer approach instead. Using this approach, it is easy to return unique combinations, easy but not effienct to return one solution, and hard to return complete combinations.
  * Using a hash set of combinations to record the results and remove duplicates. Sort the input array first if K > 2(this is optional and can make de-duple faster and easier to implement). Divide K into two parts first. Iterate the second part and solve the subproblem for the first part. Cache the sums and their corresponding elements at the end of each iteration. Using this approach, it is easy to return complete combinations, efficient to return one solution,  and also not very hard to return unique combinations, with additional space.  
  
  Time complexity of k Sum problems: omega(n^ceil(k/2)), O(n^(k-1)).
  http://www.sigmainfy.com/blogk-sum-problem-analysis-recursive-implementation-lower-bound.html
  - Two sum
    - Input array is random, find out one solution.
      + [Leetcode] Two sum(Algorithm).
      + Input array is random, find out all unique solutions(Algorithm*).  
      Unique: if a + b = sum, then (a, b) and (b, a) are duplicate pairs.  
      Remember the clever way of removing duplicate pairs!
      + [Leetcode] Two sum II -- Input array is sorted.
        + find out all unique solutions using no additional space.
        + find out all id pairs in which the first id is smaller than the second one? The hard part is when a + b = sum, we don't know how to move the two pointers. We can move both pointers until there are no duplicates, say ia --> ia', ib --> ib', and put all pairs formed by elements between num[ia] and num[ia'] and elments between num[ib] and num[ib'] into result list.
      + [Leetcode] Two Sum III -- Data structure design.
  - Three sum
    - Input array is random, find out all unique solutions.
      + [Leetcode] Three sum(Algorithm).
      + [Leetcode] Three sum closest(Algorithm).
    - Find out the count of all triplets whose sum is less than the target:
      + [Leetcode] Three Sum Smaller(Algorithm**) Using two pointers to find out the count efficiently in linear time. Finding number of distinct triplets -- e.g. (1, 2, 3) and (1, 3, 2) are not distinct, would be harder and the runtime would be O(n^3).  
      https://discuss.leetcode.com/topic/27075/what-if-the-question-is-to-find-the-number-of-triplets-instead-of-index
  - Four sum(Random input array, find out all unique solutions)
    + [Leetcode] Four Sum(Algorithms*).
    + [Leetcode] Four Sum II(Algorithm)
* Two pointers:  
  The two pointer schemes in LinkedList chapter may be used, if they run in the
  same direction. Need to think carefully about the end condition in order to know which one to use!
  * Pointers moving in the opposite direction.  
    This can often reduce runtime from O(n^2) to O(n). It basically tries to check the two elements pointed by the pointers against some condition and moves left pointer to the right or right pointer to the left accordingly, to skip some unnecessary comparisons and narrow down the search range. Optionally we can skip duplicate elements at the beginning of each iteration.
    - [Leetcode] Previous three sum problems.
    - [Leetcode] Container With Most Water(Algorithm)
    - [Leetcode] Trapping Rain Water(Multiple Algorithms** and implementation of best solution*)
  * Pointers moving in the same direction, on different lists.  
    Same as previous, this essentially skips unnecessary comparisons and narrow down the range of comparison. 
    - [Leetcode] Intersection of Two Arrays I and II(Algorithm when the arrays are sorted*). Remember the proof.
    - [Leetcode] Shortest Word Distance II(Algorithm*). O(1) get solution will TLE due to the lone pre-processing time. Remember the acceptable two pointer solution, and its proof.
  * Pointers moving in the same direction, on the same list.  
    Works as if the faster pointer is copying data to another list. Need to know why it is not affected by overriding.
    - Remove duplicates(Maximum allowed duplicates == K). Two ways of checking duplicates!
      * [Leetcode] Remove Duplicates from Sorted Array(Best Algorithm*).
      * [Leetcode] Remove Duplicates from Sorted Array2(Best Algorithm**). Remember the prooves for solutions to both problems.
    - [Leetcode] Move Zeros(Algorithms*). Remember both algorithms and how to prove them!
  * Pointers moving in the same direction on the same list, all elements in between are considered.  
    The main loop moves the faster pointer, and for each iteration, keep moving the slower pointer until some condition is met. Usually needs to maintain a max/min value and return it.
    This essentially skips some unnecessary checks of elements between certain pairs of pointers, which is mainly done by the inner loop -- elements starting with the slower pointer is skipped, and we only need to compare the elements between the slower pointer and the faster pointer, not all elements before the faster pointer. 
    - [Leetcode] Minimum Size Subarray Sum(Algorithms* and implementation of two pointer solution*). Sometimes the max/min value should be initialized failure value, not Integer.MAX_VALUE or Integer.MIN_VALUE, because it is likely those values are confused with some valid max/min values. The code is not that complicated for the update part -- one line is enough.  
    Note that the assuption of positive nums are important! Otherwise this two-pointer solution must be modified, and the runing time will has to be O(n^2) at best(proof?). And because of this assumption, the question makes sense asking the "minimum" size, not the "maximum" size. And similar questions like "finding maximum size subarray whose size is no larger than k" can also be solved similarly.
* [Leetcode] Shortest Word Distance(Algorithm*). It looks similar to another problem, but I forgot which one.
* [Leetcode] Shortest Word Distance III(Algorithm*). A follow-up of above. Easy to make mistake here!
* Subarray sum problems. Three approaches:
  * DP. If the problem asks for max/min sum and there is no more constraint on it.
  * Two pointers. 
    - [Leetcode] Minimum Size Subarray Sum(See previous).
  * Get accumulative sums first. This could be sorted in some cases. But usually we need to iterate on it while maintaining some kinds of maps. If order won't be changed, accumulative sums can be calculated on the fly instead of using additional array to store them.
    - [Leetcode] Maximum Size Subarray Sum Equals k(Algorithm*). Question could ask for minium size instead, which can also be handled similarly.
    - [Lintcode] Subarray Sum Closest(Algorithms**). Remember both solutions. Also note how useful a treemap is when getting the closest key instead of an exact key. If the questions asks for the minium length of subarray that has sum closest to k, it can still be solved with a few modifications.
    - [Lintcode] Submatrix Sum(Algorithms**). Remember how to enumerate all the sub rectangles in a 2D matrix. Remember the trick to add padding zeros. Hard to come up with the trick to reduce the 2D space usage to 1D!
    - [Leetcode] Max Sum of Rectangle No Larger Than K(Algorithm*). Extension of above.
* [Others] Task schedule with cool down(Algorithm and implementation**).
* [Leetcode] Product of Array Except Self(Algorithm**). Divide, compute one side and store it, then compute another side. Can be applied to many other problems.