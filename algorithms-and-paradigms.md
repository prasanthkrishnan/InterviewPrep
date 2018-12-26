# Algorithms and Paradigms

## Binary Search

Finding an element  in unsorted array is minimum `O(n)`. If the array is sorted one can do better via binary search. It takes `O(logn)` times to find the element. Conceptually, a binary search works as follows:

1. Pick a pivot
2. If the searching element is less than the pivot, then look at the sub array to the left of the pivot. 
3. else look at right.

Three different templates for binary search. \(From leetcode\).

{% tabs %}
{% tab title="Template 1" %}
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
{% endtab %}

{% tab title="Template 2" %}
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
{% endtab %}

{% tab title="Template 3" %}
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
{% endtab %}
{% endtabs %}

## Sorting

#### Applications

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

{% tab title="Quick Sort" %}

{% endtab %}

{% tab title="Counting Sort" %}

{% endtab %}

{% tab title="Radix Sort" %}

{% endtab %}
{% endtabs %}

### Comparison between Sorting algorithms

|  | Merge Sort | Heap Sort | Quick Sort |
| :--- | :--- | :--- | :--- |
| **Memory footprint** - when input is provided as array | O\(n\) - when merging need to copy | In place sort - `O(1)` | In place sort -  `O(1)` |
| **Stable Sort** ? - duplicate elements maintain their order after sorting | No | No | Yes |
| Can be converted to run in a **distributed** fashion easily ? | Yes | No  | No |
| Pros/Cons | Merge sort is often the best choice for sorting a linked list: in this situation it is relatively easy to implement a merge sort in such a way that it requires only Θ\(1\) extra space, and the slow random-access performance of a linked list makes some other algorithms \(such as quicksort\) perform poorly, and others \(such as heapsort\) completely impossible. Merge sort also has good locaclity of reference. | Cant be used to sort Linked Lists, since Heap sort requires random access to its children. | Worst Case`O(n*n)` Depends on the pivot picked at each time |

### External Sort

## Dynamic Programming

## Backtracking



