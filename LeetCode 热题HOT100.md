## LeetCode 热题HOT100

### 8.2~8.8

#### 两数之和

```java
class Solution {
    public int[] twoSum(int[] nums, int target) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            if (map.containsKey(target - nums[i])) {
                return new int[]{map.get(target - nums[i]), i};
            }
            map.put(nums[i], i);
        }
        return null;
    }
}
```



#### 两数相加

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode dummy = new ListNode(0);
        ListNode l3 = dummy;
        int carry = 0;
        while (l1 != null || l2 != null) {
            int a = l1 == null? 0 : l1.val;
            int b = l2 == null? 0 : l2.val;
            int sum = a + b + carry;
            carry = sum / 10;
            l3.next = new ListNode(sum % 10);
            l3 = l3.next;
            l1 = l1 == null ? null : l1.next;
            l2 = l2 == null ? null : l2.next;
        }
        if (carry == 1) {
            l3.next = new ListNode(1);
        }
        return dummy.next;
    }
}
```



#### 无重复字符的最长字串

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        if (s == null) {
            return 0;
        }
        int[] map = new int[256];
        int len = s.length();
        int l = 0,  r = 0;
        int res = 0;
        while (r < len) {
            char rc = s.charAt(r);
            map[rc]++;
            while (l < r && map[rc] > 1) {
                char lc = s.charAt(l);
                map[lc]--;
                l++;
            }
            res = Math.max(res, r - l + 1);
            r++;
        }
        return res;
    }
}
```

