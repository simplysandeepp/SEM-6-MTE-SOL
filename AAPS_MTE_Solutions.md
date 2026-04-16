# AAPS MTE Solutions — Java (Brute Force + Optimal)

---

## 1. Prefix Sum Array

### Technique / Approach
Build a `prefix[]` array where `prefix[i] = prefix[i-1] + arr[i]`.  
Range sum `[L, R]` = `prefix[R] - prefix[L-1]`.  
Pre-process once → answer every query in O(1).

### Brute Force — O(N) per query
```java
public class PrefixSumBrute {
    static int rangeSum(int[] arr, int L, int R) {
        int sum = 0;
        for (int i = L; i <= R; i++) sum += arr[i];
        return sum;
    }

    public static void main(String[] args) {
        int[] arr = {2, 4, 6, 8, 10};
        System.out.println(rangeSum(arr, 1, 3)); // 18
    }
}
```

### Optimal — O(N) build, O(1) query
```java
public class PrefixSumOptimal {
    static int[] buildPrefix(int[] arr) {
        int n = arr.length;
        int[] prefix = new int[n];
        prefix[0] = arr[0];
        for (int i = 1; i < n; i++)
            prefix[i] = prefix[i - 1] + arr[i];
        return prefix;
    }

    static int rangeSum(int[] prefix, int L, int R) {
        return L == 0 ? prefix[R] : prefix[R] - prefix[L - 1];
    }

    public static void main(String[] args) {
        int[] arr = {2, 4, 6, 8, 10};
        int[] prefix = buildPrefix(arr);
        System.out.println(rangeSum(prefix, 1, 3)); // 18
    }
}
```

---

## 2. Equilibrium Index

### Technique / Approach
An index `i` is equilibrium if `sum(0..i-1) == sum(i+1..n-1)`.  
Use total sum → iterate left to right maintaining `leftSum`.  
At each index: `rightSum = total - leftSum - arr[i]`. If equal → found.

### Brute Force — O(N²)
```java
public class EquilibriumBrute {
    static int findEquilibrium(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n; i++) {
            int left = 0, right = 0;
            for (int j = 0; j < i; j++) left += arr[j];
            for (int j = i + 1; j < n; j++) right += arr[j];
            if (left == right) return i;
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {1, 3, 5, 2, 2};
        System.out.println(findEquilibrium(arr)); // 2
    }
}
```

### Optimal — O(N)
```java
public class EquilibriumOptimal {
    static int findEquilibrium(int[] arr) {
        int total = 0;
        for (int x : arr) total += x;
        int leftSum = 0;
        for (int i = 0; i < arr.length; i++) {
            int rightSum = total - leftSum - arr[i];
            if (leftSum == rightSum) return i;
            leftSum += arr[i];
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {1, 3, 5, 2, 2};
        System.out.println(findEquilibrium(arr)); // 2
    }
}
```

---

## 3. Two Sum in Sorted Array

### Technique / Approach
Array is sorted → use **Two Pointers** (left=0, right=n-1).  
- Sum > target → right--  
- Sum < target → left++  
- Sum == target → found!

### Brute Force — O(N²)
```java
public class TwoSumBrute {
    static int[] twoSum(int[] arr, int target) {
        for (int i = 0; i < arr.length; i++)
            for (int j = i + 1; j < arr.length; j++)
                if (arr[i] + arr[j] == target)
                    return new int[]{i, j};
        return new int[]{-1, -1};
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 6};
        int[] res = twoSum(arr, 6);
        System.out.println(res[0] + ", " + res[1]); // 1, 3
    }
}
```

### Optimal — O(N) Two Pointers
```java
public class TwoSumOptimal {
    static int[] twoSum(int[] arr, int target) {
        int left = 0, right = arr.length - 1;
        while (left < right) {
            int sum = arr[left] + arr[right];
            if (sum == target) return new int[]{left, right};
            else if (sum < target) left++;
            else right--;
        }
        return new int[]{-1, -1};
    }

    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 6};
        int[] res = twoSum(arr, 6);
        System.out.println(res[0] + ", " + res[1]); // 1, 3
    }
}
```

---

## 4. Majority Element

### Technique / Approach
Element appearing more than N/2 times → **Boyer-Moore Voting Algorithm**.  
Maintain a `candidate` and `count`. If count == 0, update candidate.  
Same element → count++, different → count--.  
The survivor is the majority element.

### Brute Force — O(N²)
```java
public class MajorityBrute {
    static int majority(int[] arr) {
        int n = arr.length;
        for (int i = 0; i < n; i++) {
            int count = 0;
            for (int j = 0; j < n; j++)
                if (arr[j] == arr[i]) count++;
            if (count > n / 2) return arr[i];
        }
        return -1;
    }

    public static void main(String[] args) {
        int[] arr = {2, 2, 1, 1, 2, 2, 2};
        System.out.println(majority(arr)); // 2
    }
}
```

### Optimal — O(N) Boyer-Moore
```java
public class MajorityOptimal {
    static int majority(int[] arr) {
        int candidate = arr[0], count = 1;
        for (int i = 1; i < arr.length; i++) {
            if (arr[i] == candidate) count++;
            else if (--count == 0) {
                candidate = arr[i];
                count = 1;
            }
        }
        return candidate;
    }

    public static void main(String[] args) {
        int[] arr = {2, 2, 1, 1, 2, 2, 2};
        System.out.println(majority(arr)); // 2
    }
}
```

---

## 5. Counting Bits

### Technique / Approach
For each number 0..n, count set bits.  
**DP trick**: `bits[i] = bits[i >> 1] + (i & 1)`  
`i >> 1` drops the last bit (already computed), `(i & 1)` adds current LSB.

### Brute Force — O(N log N)
```java
public class CountingBitsBrute {
    static int[] countBits(int n) {
        int[] res = new int[n + 1];
        for (int i = 0; i <= n; i++) {
            int x = i, cnt = 0;
            while (x > 0) { cnt += (x & 1); x >>= 1; }
            res[i] = cnt;
        }
        return res;
    }

    public static void main(String[] args) {
        int[] res = countBits(5);
        for (int x : res) System.out.print(x + " "); // 0 1 1 2 1 2
    }
}
```

### Optimal — O(N) DP
```java
public class CountingBitsOptimal {
    static int[] countBits(int n) {
        int[] bits = new int[n + 1];
        for (int i = 1; i <= n; i++)
            bits[i] = bits[i >> 1] + (i & 1);
        return bits;
    }

    public static void main(String[] args) {
        int[] res = countBits(5);
        for (int x : res) System.out.print(x + " "); // 0 1 1 2 1 2
    }
}
```

---

## 6. Power of Two

### Technique / Approach
A power of 2 has exactly one set bit.  
**Bit trick**: `n > 0 && (n & (n-1)) == 0`  
`n-1` flips all bits after the single set bit → AND gives 0.

### Brute Force — O(log N)
```java
public class PowerOfTwoBrute {
    static boolean isPowerOfTwo(int n) {
        if (n <= 0) return false;
        while (n > 1) {
            if (n % 2 != 0) return false;
            n /= 2;
        }
        return true;
    }

    public static void main(String[] args) {
        System.out.println(isPowerOfTwo(16)); // true
        System.out.println(isPowerOfTwo(18)); // false
    }
}
```

### Optimal — O(1) Bit Trick
```java
public class PowerOfTwoOptimal {
    static boolean isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1)) == 0;
    }

    public static void main(String[] args) {
        System.out.println(isPowerOfTwo(16)); // true
        System.out.println(isPowerOfTwo(18)); // false
    }
}
```

---

## 7. Trapping Rainwater

### Technique / Approach
Water at index `i` = `min(maxLeft[i], maxRight[i]) - height[i]`.  
**Two Pointer Optimal**: Use `left`, `right` pointers + `leftMax`, `rightMax`.  
Process the side with the smaller max — water there is determined.

### Brute Force — O(N²)
```java
public class TrappingRainwaterBrute {
    static int trap(int[] height) {
        int n = height.length, water = 0;
        for (int i = 1; i < n - 1; i++) {
            int lMax = 0, rMax = 0;
            for (int j = 0; j <= i; j++) lMax = Math.max(lMax, height[j]);
            for (int j = i; j < n; j++) rMax = Math.max(rMax, height[j]);
            water += Math.min(lMax, rMax) - height[i];
        }
        return water;
    }

    public static void main(String[] args) {
        int[] h = {0,1,0,2,1,0,1,3,2,1,2,1};
        System.out.println(trap(h)); // 6
    }
}
```

### Optimal — O(N) Two Pointers
```java
public class TrappingRainwaterOptimal {
    static int trap(int[] height) {
        int left = 0, right = height.length - 1;
        int leftMax = 0, rightMax = 0, water = 0;
        while (left < right) {
            if (height[left] < height[right]) {
                if (height[left] >= leftMax) leftMax = height[left];
                else water += leftMax - height[left];
                left++;
            } else {
                if (height[right] >= rightMax) rightMax = height[right];
                else water += rightMax - height[right];
                right--;
            }
        }
        return water;
    }

    public static void main(String[] args) {
        int[] h = {0,1,0,2,1,0,1,3,2,1,2,1};
        System.out.println(trap(h)); // 6
    }
}
```

---

## 8. Longest Palindromic Substring

### Technique / Approach
**Expand Around Center**: For each index (and gap between indices), expand outward while characters match.  
2N-1 centers total → O(N²) time, O(1) space.

### Brute Force — O(N³)
```java
public class LongestPalindromeBrute {
    static boolean isPalin(String s, int l, int r) {
        while (l < r) if (s.charAt(l++) != s.charAt(r--)) return false;
        return true;
    }

    static String longestPalindrome(String s) {
        int n = s.length(); String best = "";
        for (int i = 0; i < n; i++)
            for (int j = i; j < n; j++)
                if (isPalin(s, i, j) && j - i + 1 > best.length())
                    best = s.substring(i, j + 1);
        return best;
    }

    public static void main(String[] args) {
        System.out.println(longestPalindrome("babad")); // bab
    }
}
```

### Optimal — O(N²) Expand Around Center
```java
public class LongestPalindromeOptimal {
    static int[] expand(String s, int l, int r) {
        while (l >= 0 && r < s.length() && s.charAt(l) == s.charAt(r)) { l--; r++; }
        return new int[]{l + 1, r - 1};
    }

    static String longestPalindrome(String s) {
        int start = 0, end = 0;
        for (int i = 0; i < s.length(); i++) {
            int[] odd  = expand(s, i, i);
            int[] even = expand(s, i, i + 1);
            if (odd[1]  - odd[0]  > end - start) { start = odd[0];  end = odd[1]; }
            if (even[1] - even[0] > end - start) { start = even[0]; end = even[1]; }
        }
        return s.substring(start, end + 1);
    }

    public static void main(String[] args) {
        System.out.println(longestPalindrome("babad")); // bab
    }
}
```

---

## 9. Longest Common Prefix

### Technique / Approach
Take the first string as reference. For each character position, check if all other strings share that character. Stop when mismatch or any string is too short.

### Brute Force — O(N * M²) all pairs
```java
public class LongestCommonPrefixBrute {
    static String lcp(String[] strs) {
        if (strs.length == 0) return "";
        String prefix = strs[0];
        for (int i = 1; i < strs.length; i++)
            while (!strs[i].startsWith(prefix))
                prefix = prefix.substring(0, prefix.length() - 1);
        return prefix;
    }

    public static void main(String[] args) {
        String[] strs = {"flower","flow","flight"};
        System.out.println(lcp(strs)); // fl
    }
}
```

### Optimal — O(N * M) vertical scan
```java
public class LongestCommonPrefixOptimal {
    static String lcp(String[] strs) {
        if (strs == null || strs.length == 0) return "";
        for (int i = 0; i < strs[0].length(); i++) {
            char c = strs[0].charAt(i);
            for (int j = 1; j < strs.length; j++)
                if (i >= strs[j].length() || strs[j].charAt(i) != c)
                    return strs[0].substring(0, i);
        }
        return strs[0];
    }

    public static void main(String[] args) {
        String[] strs = {"flower","flow","flight"};
        System.out.println(lcp(strs)); // fl
    }
}
```

---

## 10. Merge Two Sorted Linked Lists

### Technique / Approach
Use a **dummy head** node. Compare current nodes of both lists, attach the smaller one, advance that pointer. Attach the remaining list at the end.

### Brute Force — collect + sort approach
```java
import java.util.*;
public class MergeSortedListsBrute {
    static class ListNode { int val; ListNode next; ListNode(int v){val=v;} }

    static ListNode merge(ListNode l1, ListNode l2) {
        List<Integer> vals = new ArrayList<>();
        while (l1 != null) { vals.add(l1.val); l1 = l1.next; }
        while (l2 != null) { vals.add(l2.val); l2 = l2.next; }
        Collections.sort(vals);
        ListNode dummy = new ListNode(0), cur = dummy;
        for (int v : vals) { cur.next = new ListNode(v); cur = cur.next; }
        return dummy.next;
    }
}
```

### Optimal — O(N+M) in-place merge
```java
public class MergeSortedListsOptimal {
    static class ListNode { int val; ListNode next; ListNode(int v){val=v;} }

    static ListNode merge(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0), cur = dummy;
        while (l1 != null && l2 != null) {
            if (l1.val <= l2.val) { cur.next = l1; l1 = l1.next; }
            else                  { cur.next = l2; l2 = l2.next; }
            cur = cur.next;
        }
        cur.next = (l1 != null) ? l1 : l2;
        return dummy.next;
    }
}
```

---

## 11. Intersection of Two Linked Lists

### Technique / Approach
**Two Pointer trick**: Pointer A traverses list A then list B; pointer B traverses list B then list A.  
They meet at the intersection node (or null). Both travel `lenA + lenB` total.

### Brute Force — O(N*M) nested check
```java
public class IntersectionBrute {
    static class ListNode { int val; ListNode next; ListNode(int v){val=v;} }

    static ListNode getIntersection(ListNode headA, ListNode headB) {
        ListNode a = headA;
        while (a != null) {
            ListNode b = headB;
            while (b != null) {
                if (a == b) return a;
                b = b.next;
            }
            a = a.next;
        }
        return null;
    }
}
```

### Optimal — O(N+M) Two Pointers
```java
public class IntersectionOptimal {
    static class ListNode { int val; ListNode next; ListNode(int v){val=v;} }

    static ListNode getIntersection(ListNode headA, ListNode headB) {
        ListNode a = headA, b = headB;
        while (a != b) {
            a = (a == null) ? headB : a.next;
            b = (b == null) ? headA : b.next;
        }
        return a;
    }
}
```

---

## 12. Two Stacks in a Single Array

### Technique / Approach
Stack 1 grows from the **left** (index 0 upward).  
Stack 2 grows from the **right** (index n-1 downward).  
Overflow only when they meet in the middle — maximally space-efficient.

### Brute Force — split array in half
```java
public class TwoStacksBrute {
    int[] arr;
    int top1, top2, mid;

    TwoStacksBrute(int n) {
        arr = new int[n]; mid = n / 2;
        top1 = -1; top2 = mid;
    }

    void push1(int x) { if (top1 < mid - 1) arr[++top1] = x; }
    void push2(int x) { if (top2 < arr.length) arr[top2++] = x; }
    int pop1() { return top1 >= 0 ? arr[top1--] : -1; }
    int pop2() { return top2 > mid ? arr[--top2] : -1; }
}
```

### Optimal — grow from both ends
```java
public class TwoStacksOptimal {
    int[] arr;
    int top1, top2;

    TwoStacksOptimal(int n) {
        arr = new int[n];
        top1 = -1;
        top2 = n;
    }

    void push1(int x) {
        if (top1 + 1 < top2) arr[++top1] = x;
        else System.out.println("Stack Overflow");
    }

    void push2(int x) {
        if (top1 + 1 < top2) arr[--top2] = x;
        else System.out.println("Stack Overflow");
    }

    int pop1() { return top1 >= 0 ? arr[top1--] : -1; }
    int pop2() { return top2 < arr.length ? arr[top2++] : -1; }

    public static void main(String[] args) {
        TwoStacksOptimal ts = new TwoStacksOptimal(10);
        ts.push1(1); ts.push1(2); ts.push2(9); ts.push2(8);
        System.out.println(ts.pop1()); // 2
        System.out.println(ts.pop2()); // 8
    }
}
```

---

## 13. Next Greater Element

### Technique / Approach
Use a **Monotonic Stack** (decreasing order).  
Traverse right to left. For each element, pop the stack while top ≤ current element.  
Stack top is the next greater; push current onto stack.

### Brute Force — O(N²)
```java
import java.util.Arrays;
public class NextGreaterBrute {
    static int[] nextGreater(int[] arr) {
        int n = arr.length;
        int[] res = new int[n];
        Arrays.fill(res, -1);
        for (int i = 0; i < n; i++)
            for (int j = i + 1; j < n; j++)
                if (arr[j] > arr[i]) { res[i] = arr[j]; break; }
        return res;
    }

    public static void main(String[] args) {
        int[] arr = {4, 5, 2, 10, 8};
        System.out.println(Arrays.toString(nextGreater(arr))); // [5,10,10,-1,-1]
    }
}
```

### Optimal — O(N) Monotonic Stack
```java
import java.util.*;
public class NextGreaterOptimal {
    static int[] nextGreater(int[] arr) {
        int n = arr.length;
        int[] res = new int[n];
        Arrays.fill(res, -1);
        Deque<Integer> stack = new ArrayDeque<>(); // stores indices
        for (int i = 0; i < n; i++) {
            while (!stack.isEmpty() && arr[stack.peek()] < arr[i])
                res[stack.pop()] = arr[i];
            stack.push(i);
        }
        return res;
    }

    public static void main(String[] args) {
        int[] arr = {4, 5, 2, 10, 8};
        System.out.println(Arrays.toString(nextGreater(arr))); // [5,10,10,-1,-1]
    }
}
```

---

## 14. Largest Rectangle in Histogram

### Technique / Approach
Use a **Monotonic Stack** (increasing order).  
For each bar, when a shorter bar is found, pop and calculate area using the popped bar as height.  
Width = current index − new stack top − 1.

### Brute Force — O(N²)
```java
public class LargestRectangleBrute {
    static int largestRect(int[] heights) {
        int n = heights.length, maxArea = 0;
        for (int i = 0; i < n; i++) {
            int minH = heights[i];
            for (int j = i; j < n; j++) {
                minH = Math.min(minH, heights[j]);
                maxArea = Math.max(maxArea, minH * (j - i + 1));
            }
        }
        return maxArea;
    }

    public static void main(String[] args) {
        int[] h = {2,1,5,6,2,3};
        System.out.println(largestRect(h)); // 10
    }
}
```

### Optimal — O(N) Monotonic Stack
```java
import java.util.*;
public class LargestRectangleOptimal {
    static int largestRect(int[] heights) {
        Deque<Integer> stack = new ArrayDeque<>();
        int maxArea = 0, n = heights.length;
        for (int i = 0; i <= n; i++) {
            int h = (i == n) ? 0 : heights[i];
            while (!stack.isEmpty() && heights[stack.peek()] > h) {
                int height = heights[stack.pop()];
                int width  = stack.isEmpty() ? i : i - stack.peek() - 1;
                maxArea = Math.max(maxArea, height * width);
            }
            stack.push(i);
        }
        return maxArea;
    }

    public static void main(String[] args) {
        int[] h = {2,1,5,6,2,3};
        System.out.println(largestRect(h)); // 10
    }
}
```

---

## 15. Sliding Window Maximum

### Technique / Approach
Use a **Deque** storing indices in decreasing order of values.  
- Remove indices outside the window from the front.  
- Remove smaller elements from the back (they can never be the max).  
- Front of deque is always the current window's maximum.

### Brute Force — O(N*K)
```java
import java.util.Arrays;
public class SlidingWindowMaxBrute {
    static int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] res = new int[n - k + 1];
        for (int i = 0; i <= n - k; i++) {
            int max = nums[i];
            for (int j = i + 1; j < i + k; j++) max = Math.max(max, nums[j]);
            res[i] = max;
        }
        return res;
    }

    public static void main(String[] args) {
        int[] nums = {1,3,-1,-3,5,3,6,7};
        System.out.println(Arrays.toString(maxSlidingWindow(nums, 3))); // [3,3,5,5,6,7]
    }
}
```

### Optimal — O(N) Deque
```java
import java.util.*;
public class SlidingWindowMaxOptimal {
    static int[] maxSlidingWindow(int[] nums, int k) {
        int n = nums.length;
        int[] res = new int[n - k + 1];
        Deque<Integer> dq = new ArrayDeque<>(); // stores indices

        for (int i = 0; i < n; i++) {
            // remove out-of-window indices
            while (!dq.isEmpty() && dq.peekFirst() < i - k + 1)
                dq.pollFirst();
            // remove smaller elements from back
            while (!dq.isEmpty() && nums[dq.peekLast()] < nums[i])
                dq.pollLast();
            dq.offerLast(i);
            if (i >= k - 1) res[i - k + 1] = nums[dq.peekFirst()];
        }
        return res;
    }

    public static void main(String[] args) {
        int[] nums = {1,3,-1,-3,5,3,6,7};
        System.out.println(Arrays.toString(maxSlidingWindow(nums, 3))); // [3,3,5,5,6,7]
    }
}
```

---

## 16. Subsets

### Technique / Approach
For N elements there are 2^N subsets.  
**Bit Masking**: iterate mask from 0 to 2^N - 1. For each mask, bit `j` set means include `arr[j]`.  
**Backtracking**: at each index, choose to include or exclude.

### Brute Force — Bitmask O(N * 2^N)
```java
import java.util.*;
public class SubsetsBrute {
    static List<List<Integer>> subsets(int[] nums) {
        int n = nums.length;
        List<List<Integer>> result = new ArrayList<>();
        for (int mask = 0; mask < (1 << n); mask++) {
            List<Integer> subset = new ArrayList<>();
            for (int j = 0; j < n; j++)
                if ((mask & (1 << j)) != 0) subset.add(nums[j]);
            result.add(subset);
        }
        return result;
    }

    public static void main(String[] args) {
        System.out.println(subsets(new int[]{1, 2, 3}));
    }
}
```

### Optimal — Backtracking O(2^N)
```java
import java.util.*;
public class SubsetsOptimal {
    static List<List<Integer>> result = new ArrayList<>();

    static void backtrack(int[] nums, int start, List<Integer> cur) {
        result.add(new ArrayList<>(cur));
        for (int i = start; i < nums.length; i++) {
            cur.add(nums[i]);
            backtrack(nums, i + 1, cur);
            cur.remove(cur.size() - 1);
        }
    }

    static List<List<Integer>> subsets(int[] nums) {
        result.clear();
        backtrack(nums, 0, new ArrayList<>());
        return result;
    }

    public static void main(String[] args) {
        System.out.println(subsets(new int[]{1, 2, 3}));
    }
}
```

---

## 17. Combination Sum

### Technique / Approach
**Backtracking**: try each candidate starting from current index (allow reuse → same index).  
If `remaining == 0` → valid combination found.  
If `remaining < 0` → prune (stop exploring).  
Sort candidates first to prune early.

### Brute Force — generate all + filter
```java
import java.util.*;
public class CombinationSumBrute {
    static List<List<Integer>> result = new ArrayList<>();

    static void generate(int[] cands, int target, int start, List<Integer> cur, int sum) {
        if (sum == target) { result.add(new ArrayList<>(cur)); return; }
        if (sum > target) return;
        for (int i = start; i < cands.length; i++) {
            cur.add(cands[i]);
            generate(cands, target, i, cur, sum + cands[i]);
            cur.remove(cur.size() - 1);
        }
    }

    static List<List<Integer>> combinationSum(int[] candidates, int target) {
        result.clear();
        Arrays.sort(candidates);
        generate(candidates, target, 0, new ArrayList<>(), 0);
        return result;
    }

    public static void main(String[] args) {
        System.out.println(combinationSum(new int[]{2,3,6,7}, 7));
        // [[2,2,3],[7]]
    }
}
```

### Optimal — Backtracking with early pruning O(2^(T/M))
```java
import java.util.*;
public class CombinationSumOptimal {
    static List<List<Integer>> result = new ArrayList<>();

    static void bt(int[] cands, int remaining, int start, List<Integer> cur) {
        if (remaining == 0) { result.add(new ArrayList<>(cur)); return; }
        for (int i = start; i < cands.length; i++) {
            if (cands[i] > remaining) break; // sorted → prune rest
            cur.add(cands[i]);
            bt(cands, remaining - cands[i], i, cur);
            cur.remove(cur.size() - 1);
        }
    }

    static List<List<Integer>> combinationSum(int[] candidates, int target) {
        result.clear();
        Arrays.sort(candidates);
        bt(candidates, target, 0, new ArrayList<>());
        return result;
    }

    public static void main(String[] args) {
        System.out.println(combinationSum(new int[]{2,3,6,7}, 7));
        // [[2,2,3],[7]]
    }
}
```

---

## 18. Maximum Frequency Element

### Technique / Approach
Use a **HashMap** to count frequencies, then find the key with the maximum count.  
Single pass for counting, single pass to find max.

### Brute Force — O(N²)
```java
public class MaxFrequencyBrute {
    static int maxFreqElement(int[] arr) {
        int maxCount = 0, result = arr[0];
        for (int i = 0; i < arr.length; i++) {
            int count = 0;
            for (int j = 0; j < arr.length; j++)
                if (arr[j] == arr[i]) count++;
            if (count > maxCount) { maxCount = count; result = arr[i]; }
        }
        return result;
    }

    public static void main(String[] args) {
        int[] arr = {1, 3, 2, 1, 4, 1, 3, 3, 3};
        System.out.println(maxFreqElement(arr)); // 3
    }
}
```

### Optimal — O(N) HashMap
```java
import java.util.*;
public class MaxFrequencyOptimal {
    static int maxFreqElement(int[] arr) {
        Map<Integer, Integer> freq = new HashMap<>();
        for (int x : arr) freq.merge(x, 1, Integer::sum);
        int maxCount = 0, result = arr[0];
        for (Map.Entry<Integer, Integer> e : freq.entrySet())
            if (e.getValue() > maxCount) { maxCount = e.getValue(); result = e.getKey(); }
        return result;
    }

    public static void main(String[] args) {
        int[] arr = {1, 3, 2, 1, 4, 1, 3, 3, 3};
        System.out.println(maxFreqElement(arr)); // 3
    }
}
```

---

## 19. Median of Two Sorted Arrays

### Technique / Approach
**Binary Search on the smaller array**.  
Partition both arrays such that left half has `(m+n+1)/2` elements.  
Ensure `maxLeft1 ≤ minRight2` and `maxLeft2 ≤ minRight1`.  
Binary search on partition of smaller array → O(log(min(m,n))).

### Brute Force — O((M+N) log(M+N)) merge & find
```java
import java.util.Arrays;
public class MedianTwoSortedBrute {
    static double findMedian(int[] A, int[] B) {
        int m = A.length, n = B.length;
        int[] merged = new int[m + n];
        System.arraycopy(A, 0, merged, 0, m);
        System.arraycopy(B, 0, merged, m, n);
        Arrays.sort(merged);
        int total = merged.length;
        return total % 2 == 0
            ? (merged[total/2 - 1] + merged[total/2]) / 2.0
            : merged[total/2];
    }

    public static void main(String[] args) {
        System.out.println(findMedian(new int[]{1,3}, new int[]{2}));     // 2.0
        System.out.println(findMedian(new int[]{1,2}, new int[]{3,4}));   // 2.5
    }
}
```

### Optimal — O(log(min(M,N))) Binary Search
```java
public class MedianTwoSortedOptimal {
    static double findMedian(int[] A, int[] B) {
        if (A.length > B.length) return findMedian(B, A); // ensure A is smaller
        int m = A.length, n = B.length;
        int lo = 0, hi = m;
        while (lo <= hi) {
            int partA = (lo + hi) / 2;
            int partB = (m + n + 1) / 2 - partA;

            int maxLeftA  = (partA == 0) ? Integer.MIN_VALUE : A[partA - 1];
            int minRightA = (partA == m) ? Integer.MAX_VALUE : A[partA];
            int maxLeftB  = (partB == 0) ? Integer.MIN_VALUE : B[partB - 1];
            int minRightB = (partB == n) ? Integer.MAX_VALUE : B[partB];

            if (maxLeftA <= minRightB && maxLeftB <= minRightA) {
                if ((m + n) % 2 == 0)
                    return (Math.max(maxLeftA, maxLeftB) + Math.min(minRightA, minRightB)) / 2.0;
                else
                    return Math.max(maxLeftA, maxLeftB);
            } else if (maxLeftA > minRightB) hi = partA - 1;
            else lo = partA + 1;
        }
        throw new IllegalArgumentException("Arrays are not sorted");
    }

    public static void main(String[] args) {
        System.out.println(findMedian(new int[]{1,3}, new int[]{2}));     // 2.0
        System.out.println(findMedian(new int[]{1,2}, new int[]{3,4}));   // 2.5
    }
}
```

---

## Quick Reference — Complexity Summary

| # | Problem | Brute Force | Optimal | Key Technique |
|---|---------|-------------|---------|---------------|
| 1 | Prefix Sum | O(N) / query | O(1) / query | Prefix Array |
| 2 | Equilibrium Index | O(N²) | O(N) | Running Sum |
| 3 | Two Sum (Sorted) | O(N²) | O(N) | Two Pointers |
| 4 | Majority Element | O(N²) | O(N) | Boyer-Moore Voting |
| 5 | Counting Bits | O(N log N) | O(N) | DP + Bit Shift |
| 6 | Power of Two | O(log N) | O(1) | n & (n-1) |
| 7 | Trapping Rainwater | O(N²) | O(N) | Two Pointers |
| 8 | Longest Palindrome | O(N³) | O(N²) | Expand Around Center |
| 9 | Longest Common Prefix | O(N·M²) | O(N·M) | Vertical Scan |
| 10 | Merge Sorted Lists | O((N+M) log) | O(N+M) | Dummy Head |
| 11 | Linked List Intersection | O(N·M) | O(N+M) | Two Pointers |
| 12 | Two Stacks in Array | O(N) split | O(N) | Grow from Ends |
| 13 | Next Greater Element | O(N²) | O(N) | Monotonic Stack |
| 14 | Histogram Rectangle | O(N²) | O(N) | Monotonic Stack |
| 15 | Sliding Window Max | O(N·K) | O(N) | Deque |
| 16 | Subsets | O(N·2^N) | O(2^N) | Backtracking |
| 17 | Combination Sum | O(2^(T/M)) | O(2^(T/M)) | Backtracking + Prune |
| 18 | Maximum Frequency | O(N²) | O(N) | HashMap |
| 19 | Median Two Sorted | O((M+N)log) | O(log min(M,N)) | Binary Search |
