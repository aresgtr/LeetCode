# Array

## Remove Duplicates from Sorted Array

### My solution

```java
class Solution {
    public int removeDuplicates(int[] nums) {
        
        if (nums.length == 0) {
            return 0;
        }
        
        int count = 1;
        
        int unique = nums[0];
        
        for (int current : nums) {
            
            if (current != unique) {
                nums[count] = current;
                unique = current;
                count++;
            }
        }
        
        return count;
        
    }
}
```

### Best solution

```java
public int removeDuplicates(int[] nums) {
    if (nums.length == 0) return 0;
    int i = 0;
    for (int j = 1; j < nums.length; j++) {
        if (nums[j] != nums[i]) {
            i++;
            nums[i] = nums[j];
        }
    }
    return i + 1;
}
```

### Notes

人家写的少定义了一个variable，更简洁，但我的方法readability更好

## Best Time to Buy and Sell Stock II

### My solution

```java
class Solution {
    public int maxProfit(int[] prices) {
        
        int sum = 0;
        
        for (int i = 0; i < prices.length - 1; i++) { // Avoid index out of bound
            
            if (prices[i] < prices[i + 1]) {
                sum += (prices[i + 1] - prices[i]);
            }
            
        }
        
        return sum;
    }
}
```

### Best solution

```java
class Solution {
    public int maxProfit(int[] prices) {
        int maxprofit = 0;
        for (int i = 1; i < prices.length; i++) {
            if (prices[i] > prices[i - 1])
                maxprofit += prices[i] - prices[i - 1];
        }
        return maxprofit;
    }
}
```

### Notes

基本没有区别

## Rotate Array

### My solution

```java
class Solution {
    public void rotate(int[] nums, int k) {
        for (int i = 0; i < k; i++) {
            int temp = nums[nums.length - 1];
            
            for (int j = nums.length - 1; j > 0; j--) {
                nums[j] = nums[j - 1];
            }
            
            nums[0] = temp;
        }
    }
}
```

### Best solution: Cyclic Replacements

```java
class Solution {
  public void rotate(int[] nums, int k) {
    k = k % nums.length;    //  为了保险起见，免得转好几圈
    int count = 0;
    for (int start = 0; count < nums.length; start++) {
      int current = start;
      int prev = nums[start];
      do {
        int next = (current + k) % nums.length;
        int temp = nums[next];
        nums[next] = prev;
        prev = temp;
        current = next;
        count++;
      } while (start != current);
    }
  }
}
```

```python
class Solution:
    def rotate(self, nums: List[int], k: int) -> None:
        n = len(nums)
        k %= n
        
        start = count = 0
        while count < n:
            current, prev = start, nums[start]
            while True:
                next_idx = (current + k) % n
                nums[next_idx], prev = prev, nums[next_idx]
                current = next_idx
                count += 1
                
                if start == current:
                    break
            start += 1
```

Time: O(n)

Space: O(1)

### Best solution: Reverse

```java
class Solution {
  public void rotate(int[] nums, int k) {
    k %= nums.length;
    reverse(nums, 0, nums.length - 1);
    reverse(nums, 0, k - 1);
    reverse(nums, k, nums.length - 1);
  }
  public void reverse(int[] nums, int start, int end) {
    while (start < end) {
      int temp = nums[start];
      nums[start] = nums[end];
      nums[end] = temp;
      start++;
      end--;
    }
  }
}
```

Time: O(n)

Space: O(1)

### Notes

Reverse的方法最好，最易于理解。Cyclic Replacement不易理解。自己的方法运算时间太长。

## 217.Contains Duplicate

### Best Solution: Sorting

```java
public boolean containsDuplicate(int[] nums) {
    Arrays.sort(nums);
    for (int i = 0; i < nums.length - 1; ++i) {
        if (nums[i] == nums[i + 1]) return true;
    }
    return false;
}
```

Time: O(nlogn)

Space: O(1)

### Best Solution: Hash Table

```java
public boolean containsDuplicate(int[] nums) {
    Set<Integer> set = new HashSet<>(nums.length);
    for (int x: nums) {
        if (set.contains(x)) return true;
        set.add(x);
    }
    return false;
}
```

Time: O(n)

Space: O(n)

## 136.Single Number

### My Solution

```java
class Solution {
    public int singleNumber(int[] nums) {
        Arrays.sort(nums);
        
        for (int i = 0; i < nums.length - 1; i += 2) {
            if (nums[i] != nums[i + 1]) {
                return nums[i];
            }
        }
        
        return nums[nums.length - 1];
    }
}
```

### Best Solution: Hash Table

```java
class Solution {
  public int singleNumber(int[] nums) {
    HashMap<Integer, Integer> hash_table = new HashMap<>();

    for (int i : nums) {
      hash_table.put(i, hash_table.getOrDefault(i, 0) + 1);
    }
    for (int i : nums) {
      if (hash_table.get(i) == 1) {
        return i;
      }
    }
    return 0;
  }
}
```

Time: O(n)
Space: O(n)

看到数字是几就在新做的第几位的value + 1（如果value是0那就0+1），最后看哪一位的value是1

### Notes

我认为我的解法更好一些

# LeetCode题解经典题

## 链表

### 7. 链表求和

445.

#### My Solution
```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {

        if (l1 == null) return l2;
        else if (l2 == null) return l1;

        ListNode digitNode = null;
        ListNode prev = null;
        int toNextDigit = 0;
        List<ListNode> nodeList1 = new ArrayList<>();
        List<ListNode> nodeList2 = new ArrayList<>();

        while (l1 != null) {
            nodeList1.add(l1);
            l1 = l1.next;
        }

        while (l2 != null) {
            nodeList2.add(l2);
            l2 = l2.next;
        }

        int i = nodeList1.size() - 1;
        int j = nodeList2.size() - 1;

        while (i >= 0 || j >= 0) {

            int first;
            int second;
            
            if (i >= 0) {
                first = nodeList1.get(i).val;
            } else {
                first = 0;
            }

            if (j >= 0) {
                second = nodeList2.get(j).val;
            } else {
                second = 0;
            }

            int digitInt = (first + second + toNextDigit) % 10;
            toNextDigit = (first + second + toNextDigit) / 10;

            digitNode = new ListNode(digitInt);
            digitNode.next = prev;
            prev = digitNode;

            i--;
            j--;
        }

        if (toNextDigit > 0) {
            digitNode = new ListNode(toNextDigit);
            digitNode.next = prev;
        }

        return digitNode;

    }
}
```

#### Best Solution

```java
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) { 
        Stack<Integer> stack1 = new Stack<>();
        Stack<Integer> stack2 = new Stack<>();
        while (l1 != null) {
            stack1.push(l1.val);
            l1 = l1.next;
        }
        while (l2 != null) {
            stack2.push(l2.val);
            l2 = l2.next;
        }
        
        int carry = 0;
        ListNode head = null;
        while (!stack1.isEmpty() || !stack2.isEmpty() || carry > 0) {
            int sum = carry;
            sum += stack1.isEmpty()? 0: stack1.pop();
            sum += stack2.isEmpty()? 0: stack2.pop();
            ListNode node = new ListNode(sum % 10);
            node.next = head;
            head = node;
            carry = sum / 10;
        }
        return head;
    }
}

作者：sweetiee
链接：https://leetcode-cn.com/problems/add-two-numbers-ii/solution/javakai-fa-by-sweetiee/
来源：力扣（LeetCode）
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。
```

### 8. 回文链表

234.

#### My Solution

```java
class Solution {
    public boolean isPalindrome(ListNode head) {
        if (head == null || head.next == null) return true;

        List<Integer> list = new ArrayList<>();

        while (head != null) {

            list.add(head.val);
            head = head.next;
        }

        for (int i = 0, j = list.size() - 1; i < j; i++, j--) {
            if (!list.get(i).equals(list.get(j))) {
                System.out.println(list.get(j));
                return false;
            }
        }

        return true;
    }
}
```

#### Best Solution

https://leetcode-cn.com/problems/palindrome-linked-list/solution/hui-wen-lian-biao-by-leetcode/

