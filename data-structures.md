# Data Structures

## Priority Queue

A data structure containing a set `S` of elements, each associated with a key, supporting the following operations:

* `insert(S,x)` - insert element x into set S
* `max(S)` - return element from S with largest Key
* `increase_key(S,x,k)` - increase the value of element x's key to new value k

### Heap

* Implementation of Priority Queue
* An array, visualized as a complete binary tree. 
* Max Heap property - `key of node is >= keys of children`
* **root** of the tree = element in array at index `i = 0`
* `left(i) = 2 * i + 1`
* `right(i) = left(i) + 1`
* `parent(i) = i - 1 / 2`

### Heap Operations

### Insert

To add an element to a heap we must perform an up-heap operation,

* Add the element to the bottom level of the heap.
* Compare the added element with its parent; if they are in the correct order, stop. \(upheap operation\)
* If not, swap the element with its parent and return to the previous step

### Extract Max / Min

To extract max / min element from an heap we must perform an down-heap operation.

* Replace the root of the heap with the last element on the last level
* Compare the new root with its children; if they are in the correct order, stop
* If not, swap the element with one of its children and return to the previous step.

### Build Min/Max Heap

Building a heap from an array of n input elements can be done by starting with an empty heap, then successively inserting each element. But takes `O(nlogn)` A better approach is to use down-heap operation on the input array to fix heap property violation from the bottom of the tree working its way towards the root. This approach takes `O(n)`

### Increase or decrease key

The operation of replacing/updating the key and iteratively doing down-heap/up-heap are not supported by many Priority Queue libraries such as C++ stl and Java. Doing an extract-min and insert function is less efficient.

```java
public class Main {
    private static int left(int i) {
        return 2 * i + 1;
    }

    private static int right(int i) {
        return left(i) + 1;
    }

    private static void swap(int[] A, int i, int j) {
        int temp = A[i];
        A[i] = A[j];
        A[j] = temp;
    }
// correct a single violation of the heap property in a subtree at its root.
// If A[root] is violating the max-Heap property correct the violation by 
// trickling element A[root] down the tree, making the subtree rooted at index 
// root a max-heap
    private static void maxHeapifyDown(int[] nums, int root) {
        if(nums == null || nums.length == 0 || root < 0) return;
        int l = left(root);
        int r = right(root);
        int max = root;
        if(l < nums.length && nums[l] > nums[max]) 
            max = l;
        if(r < nums.length && nums[r] > nums[max])
            max = r;
        if(max != root){
            swap(nums, max, root);
            maxHeapify(nums, max);
        }
    }
// produce a max-heap from an unordered array. Starting at n-1/2 because the 
// remaining are leaves. This may look like O(nlogn) 
// but if do careful analysis it will be O(n). 
    private static void buildMaxHeap(int[] nums) {
        if(nums == null || nums.length == 0) return;
        int start = (nums.length - 1) / 2;
        for(int i = start; i >=0; --i) {
            maxHeapifyDown(nums, i);
        }
    }

    public static void main(String[] args) {
        int[] nums = {1,2,3,4,5};
        buildMaxHeap(nums);
        for(int i : nums)
        System.out.println(i);
    }
}
```

| **Find-min** | **Delete-min** | **Insert** | **Decrease-key** |
| :--- | :--- | :--- | :--- |
| `O(1)` | `O(logn)` | `O(logn)` | `O(logn)` |

### Applications

* Heapsort: One of the best sorting methods being in-place and with no quadratic worst-case scenarios.
* Selection algorithms: A heap allows access to the min or max element in constant time, and other selections \(such as median or kth-element\) can be done in sub-linear time on data that is in a heap.
* Graph algorithms: By using heaps as internal traversal data structures, run time will be reduced by polynomial order. Examples of such problems are Prim's minimal-spanning-tree algorithm and Dijkstra's shortest-path algorithm.
* K-way merge: A heap data structure is useful to merge many already-sorted input streams into a single sorted output stream. Examples of the need for merging include external sorting and streaming results from distributed data such as a log structured merge tree. The inner loop is obtaining the min element, replacing with the next element for the corresponding input stream, then doing a sift-down heap operation. \(Alternatively the replace function.\) \(Using extract-max and insert functions of a priority queue are much less efficient.\)
* Order statistics: The Heap data structure can be used to efficiently find the kth smallest \(or largest\) element in an array.

