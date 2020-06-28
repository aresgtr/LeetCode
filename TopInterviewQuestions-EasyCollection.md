# Array

## Remove Duplicates from Sorted Array

My solution

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

Best solution

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

Notes

人家写的少定义了一个variable，更简洁，但我的方法readability更好

## Best Time to Buy and Sell Stock II

My solution

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

Best solution

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

Notes

基本没有区别

