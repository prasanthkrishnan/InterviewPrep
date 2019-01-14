# Algorithms and Paradigms

## Binary Search

Finding an element in unsorted array is minimum `O(n)`. If the array is sorted one can do better via binary search. It takes `O(logn)` times to find the element. Conceptually, a binary search works as follows:

1. Pick a pivot
2. If the searching element is less than the pivot, then look at the sub array to the left of the pivot. 
3. else look at right.

Three different templates for binary search. \(From leetcode\).

```java
int binarySearch(int[] nums, int target){
  if(nums == null || nums.length == 0)
    return -1;

  int left = 0, right = nums.length - 1;
  while(left <= right){
    // Prevent (left + right) overflow
    int mid = left + (right - left) / 2;
    if(nums[mid] == target){ return mid; }
    else if(nums[mid] < target) { left = mid + 1; }
    else { right = mid - 1; }
  }
  // End Condition: left > right
  return -1;
}
```

* Most basic and elementary form of Binary Search
* Search Condition can be determined without comparing to the element's neighbors \(or use specific elements around it.
* No post-processing required because at each step, you are checking to see if the element has been found. If you reach the end, then you know the element is not found.

```java
int binarySearch(int[] nums, int target){
  if(nums == null || nums.length == 0)
    return -1;

  int left = 0, right = nums.length;
  while(left < right){
    // Prevent (left + right) overflow
    int mid = left + (right - left) / 2;
    if(nums[mid] == target){ return mid; }
    else if(nums[mid] < target) { left = mid + 1; }
    else { right = mid; }
  }
  // Post-processing:
  // End Condition: left == right
  if(left != nums.length && nums[left] == target) return left;
  return -1;
}
```

* An advanced way to implement Binary Search.
* Search Condition needs to access element’s immediate right neighbor
* Use element’s right neighbor to determine if condition is met and decide whether to go left or right
* Guarantees Search Space is at least 2 in size at each step
* Post-processing required. Loop/Recursion ends when you have 1 element left. Need to assess if the remaining element meets the condition.

```java
int binarySearch(int[] nums, int target) {
    if (nums == null || nums.length == 0)
        return -1;

    int left = 0, right = nums.length - 1;
    while (left + 1 < right){
        // Prevent (left + right) overflow
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid;
        } else {
            right = mid;
        }
    }
    // Post-processing:
    // End Condition: left + 1 == right
    if(nums[left] == target) return left;
    if(nums[right] == target) return right;
    return -1;
}
```

* An alternative way to implement Binary Search
* Search Condition needs to access element’s immediate left and right neighbors
* Use element’s neighbors to determine if condition is met and decide whether to go left or right
* Guarantees Search Space is at least 3 in size at each step
* Post-processing required. Loop/Recursion ends when you have 2 elements left. Need to assess if the remaining elements meet the condition.

## Sorting

### Applications

* Organize an Resource, i.e. Mp3 files
* Data compression: sorting finds duplicate
* Computer graphics: rendering scenes front to back
* Problems that become easy once items are in sorted order
  * Find a median, or find closest pairs
  * Binary search, identify statistical outliers

{% tabs %}
{% tab title="Insertion Sort" %}
```java
Sort(a[n]) {
    for (j <- 1 to n) {
        Insert a[j] into the right position in a already sorted
         sub array a[0..j-1] by pairwise swapping.
    }
}
```

While inserting `a[j]` in the correct position, the elements to the right has to be moved. But that might not be needed when the input is stored in a list.

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode insertionSortList(ListNode head) {
        if(head == null) return null;
        ListNode sorted = new ListNode(Integer.MIN_VALUE);
        ListNode unsorted = head;
        while(unsorted != null) {
            ListNode curr = unsorted;
            unsorted = unsorted.next;
            insert(sorted, curr);
        }
        return sorted.next;
    }

    public void insert(ListNode l, ListNode n) {
        ListNode iter = l;
        while(iter.next != null && iter.next.val < n.val) {
            iter = iter.next;
        }
        n.next = iter.next;
        iter.next = n;
    }
}
```

Running time `O(n*n)`

#### Binary Insertion Sort

```java
Sort(a[n]) {
    for (j <- 1 to n) {
        Insert a[j] into the right position in a already sorted
         sub array a[0..j-1]. Use binary search to find the right position.
    }
}
```

Binary search with take `Θ(log n)` time.

When the input is stored in an array. Shifting the elements after insertion will still take `Θ(n)` time. Thus even Binary insertion sort also takes `O(n*n).`To avoid having to make a series of swaps for each insertion, the input could be stored in a Linked List, which allows elements to be spliced into or out of the list in constant-time when the position in the list is known. However, searching a linked list requires sequentially following the links to the desired position: a linked list does not have random access, so it cannot use a faster method such as binary search. Therefore, the running time required for searching is `O(n)` and the time for sorting is `O(n*n)`.
{% endtab %}

{% tab title="Merge Sort" %}
Conceptually, a merge sort works as follows:

1. Divide the unsorted list into _n_ sublists, each containing 1 element \(a list of 1 element is considered sorted\).
2. Repeatedly merge sublists to produce new sorted sublists until there is only 1 sublist remaining. This will be the sorted list.

Running time `O(nlogn)`

Space complexit `O(n)` with arrays `O(1)` with linked lists

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode sortList(ListNode head) {
        return mergesort(head);
    }

    private ListNode findMid(ListNode head) {
        ListNode iter = head;
        ListNode fastIter = head;
        while(fastIter!= null && fastIter.next!= null && fastIter.next.next != null) {
            iter = iter.next;
            fastIter = fastIter.next.next;
        }
        return iter;
    }

    // start inclusive end exculsive
    private ListNode mergesort(ListNode head) {
        //single element
        if(head == null || head.next == null) return head;
        // create L and R list
        ListNode mid = findMid(head);
        ListNode R = mid.next;
        ListNode L = head;
        mid.next = null;

        ListNode Lres = mergesort(L);
        ListNode Rres = mergesort(R);
        return merge(Lres,Rres);
    }

    private ListNode merge(ListNode L, ListNode R) {
        ListNode res = new ListNode(-1);
        ListNode iter = res;
        while(L != null && R !=null) {
            if(L.val < R.val) {
                iter.next = L;
                L = L.next;
            }
            else {
                iter.next = R;
                R = R.next;
            }
            iter=iter.next;
        }

        while(L != null) {
            iter.next = L;
            L = L.next;
            iter=iter.next;
        }

        while(R != null) {
            iter.next = R;
            R = R.next;
            iter=iter.next;
        }
        return res.next;
    }
```

![](https://blobscdn.gitbook.com/v0/b/gitbook-28427.appspot.com/o/assets%2F-LD-xxGNrrOeNxoClrbP%2F-LDeTLZiN-ZY0P5jpWHO%2F-LDeTz4eUebP4CPBNEPJ%2FMerge_sort_algorithm_diagram.svg?alt=media&token=4142716d-6eba-4098-91b7-8431b12bea14)
{% endtab %}

{% tab title="Heap Sort" %}
Conceptually, a heap sort works as follows:

1. Build Max Heap from unordered array;
2. Find maximum element A\[1\];
3. Swap elements A\[n\] and A\[1\]: now max element is at the end of the array!
4. Discard node n from heap \(by decrementing heap-size variable\)
5. New root may violate max heap property, but its children are max heaps. Run max\_heapify to fix this.
6. Go to Step 2 unless heap is empty.

After `n` iterations the Heap is empty, every iteration involves a swap and a max\_heapify operation; hence it takes `O(logn)` time  
Thus overall `O(nlogn)` time.
{% endtab %}
{% endtabs %}

### Comparison between Sorting algorithms

|  | Merge Sort | Heap Sort | Quick Sort |
| :--- | :--- | :--- | :--- |
| **Memory footprint** - when input is provided as array | O\(n\) - when merging need to copy | In place sort - `O(1)` | In place sort -  `O(1)` |
| **Stable Sort** ? - duplicate elements maintain their order after sorting | No | No | Yes |
| Can be converted to run in a **distributed** fashion easily ? | Yes | No | No |
| Pros/Cons | Merge sort is often the best choice for sorting a linked list: in this situation it is relatively easy to implement a merge sort in such a way that it requires only Θ\(1\) extra space, and the slow random-access performance of a linked list makes some other algorithms \(such as quicksort\) perform poorly, and others \(such as heapsort\) completely impossible. Merge sort also has good locaclity of reference. | Cant be used to sort Linked Lists, since Heap sort requires random access to its children. | Worst Case`O(n*n)` Depends on the pivot picked at each time |

### External Sort

## Dynamic Programming

## Backtracking

## DFS

* follow path until you get stuck
* backtrack along breadcrumbs until reach unexplored neighbor
* recursively explore
* careful not to repeat a vertex

Implementation split into two parts,
* Given a input Graph G.
* dfsVisit(G, v) - search from start vertex v.
* dfs(G) - explore entire graph.


### Java Implementation
```java
class Solution {
    private static int N = 26;
    public String alienOrder(String[] words) {
        boolean[][] adj = new boolean[N][N];
        int[] status = new int[N];
        // 0 - non found
        // 1 - unvisited
        // 2 visiting in current
        // 3 visited
        
        buildGraph(words, adj, status);
        
        StringBuilder sb = new StringBuilder();
        for(int i = 0; i < N; ++i) {
            if(status[i] == 1) {
                if(!dfsVisit(i, status, adj, sb)) return "";
            }
        }
        return sb.reverse().toString();
    }
    
    private boolean dfsVisit(int v, int[] status, boolean[][] adj, StringBuilder sb) {
        status[v] = 2; // mark as visiting
        for(int i = 0; i < N; ++i) {
            if(adj[v][i]) {
                if(status[i] == 1) {
                    if(!dfsVisit(i, status, adj, sb)) return false;
                } else if(status[i] == 2) {
                    return false; // back edge, cycle;
                }
            }
        }
        status[v] = 3; // mark 
        sb.append((char)('a' + v));
        return true;
    }
}
```

Along with updating the status, one can also store parent pointers.

Application,
- Edge classification
- cycle detection
- Topological sort

### Running time
Linear time, O(V+E)
dfsVisit is traversing every edge reachable. dfs calls dfsVisit for all vertices. Therefore the total running time for dfsVisit is O(E).
Outer loop apart from dfsVisit adds O(V) time.


### Edge classification
- tree edge - Edges visited during DFS.
- Back edge - Edge connecting back to a ancestor.
- forward edge - Edge connecting to a descendant.
- cross edge - Edge connecting one tree to another tree

In undirected graphs, there will only be tree edge and back edge. Draw a undirected graphs to see the reason. 

In the algo above, in dfsVisit(G, v) when traversing an edge,
- If the destination vertex has status 2 i.e. being visiting currently, then it is an ancestor. Therefore its a back edge.
- If the destination vertex has status 3 i.e. already visited, then its part of another subtree. Therefore its a cross edge.

### Cycle Detection
- If you encounter a back edge then there exists a cycle.

### Topological sort
- Reverse of the dfsVisit completion.
- When dfsVisit completes for a vertex that means all the out going edges have been visited, there for in a dependency graph, it means, all the pre-requiste for this vertex has been completed.
- Store the vertex in a list when dfsVisit for that vertex completes.
- Reverse of that list gives topological sort.
- In the algo above StringBuilder stores the vertex as a string.

- Another algo for Topological sort without dfs, is to use indegree and our degree.
