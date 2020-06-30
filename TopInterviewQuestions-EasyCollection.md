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