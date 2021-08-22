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

### 8.9~8.15

#### 寻找两个正序数组的中位数

```java
class Solution {
    public double findMedianSortedArrays(int[] nums1, int[] nums2) {
        int m = nums1.length;
        int n = nums2.length;
        return (helper(nums1, 0, nums1.length - 1, nums2, 0, nums2.length - 1, (n + m + 1) / 2)
                + helper(nums1, 0, nums1.length - 1, nums2, 0, nums2.length - 1, (n + m + 2) / 2)) / 2.0;
    }
    
    private int helper(int[] nums1, int st1, int ed1, int[] nums2, int st2, int ed2, int k) {
        int len1 = ed1 - st1 + 1;
        int len2 = ed2 - st2 + 1;
        if (len1 > len2) {
            return helper(nums2, st2, ed2, nums1, st1, ed1, k);
        }
        if (st1 > ed1) {
            return nums2[st2 + k - 1];
        }
        if (k == 1) 
            return Math.min(nums1[st1], nums2[st2]);
        len1 = Math.min(len1, k / 2);
        len2 = Math.min(len2, k / 2);
        if (nums1[st1 + len1 - 1] < nums2[st2 + len2 - 1]) {
            return helper(nums1, st1 + len1, ed1, nums2, st2, ed2, k - len1);
        }
        return helper(nums1, st1, ed1, nums2, st2 + len2, ed2, k - len2);
    }
}
```



#### 最长回文子串

```java
class Solution {
    public String longestPalindrome(String s) {
        if (s == null || s.length() == 0) return "";
        int len = 1;
        int n = s.length();
        boolean[][] f = new boolean[n][n];
        for (int i = 0; i < n; i++) {
            f[i][i] = true;
        }
        int l = 0, r = 0;
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < i; j++) {
                if (s.charAt(i) == s.charAt(j)) {
                    if (i - j == 1) {
                        f[j][i] = true;    
                    } else {
                        f[j][i] = f[j + 1][i - 1];
                    }
                }
                if (f[j][i] && i - j + 1 > len) {
                    l = j;
                    r = i;
                    len = i - j + 1;
                }
            }
        }
        return s.substring(l, r + 1);
    }
}
```



#### 正则表达式匹配

```java
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length(), n = p.length();
        boolean[][] f = new boolean[m + 1][n + 1];
        f[0][0] = true;
        for (int i = 2; i <= n; i += 2) {
            if (p.charAt(i - 1) == '*') {
                f[0][i] = f[0][i - 2];
            }
        }
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '.') {
                    f[i][j] = f[i - 1][j - 1];
                } else if (j > 1 && p.charAt(j - 1) == '*') {
                    if (f[i][j - 2]) {
                        f[i][j] = true;
                    } else {
                        if (p.charAt(j - 2) == '.' || p.charAt(j - 2) == s.charAt(i - 1)) {
                            f[i][j] = f[i - 1][j];
                        }
                    }
                }
            }
        }
        return f[m][n];
    }
}
```

### 8.16~8.22
#### 盛水最多的容器

```java
class Solution {
    public int maxArea(int[] height) {
        if (height == null || height.length == 0) {
            return 0;
        }
        int res = 0;
        int n = height.length;
        int l = 0, r = n - 1;
        int lmax = height[l], rmax = height[r];
        while (l < r) {
            if (lmax < rmax) {
                res = Math.max((r - l) * lmax, res);
                l++;
                lmax = Math.max(height[l], lmax);
            } else {
                res = Math.max((r - l) * rmax, res);
                r--;
                rmax = Math.max(height[r], rmax);
            }
        }
        return res;
    }
}
```



#### 三数之和

```java
class Solution {
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        if (nums == null || nums.length == 0) {
            return res;
        }
        int n = nums.length;
        Arrays.sort(nums);
        
        for (int i = 0; i < n - 2; i++) {
            if (nums[i] > 0) break;
            if (i > 0 && nums[i] == nums[i - 1]) continue;
            int l = i + 1, r = n - 1;
            while (l < r) {
                int sum = nums[i] + nums[l] + nums[r];
                if (sum == 0) {
                    List<Integer> path = new ArrayList<>();
                    path.add(nums[i]);
                    path.add(nums[l]);
                    path.add(nums[r]);
                    res.add(path);
                    while (l < r && nums[r] == nums[r - 1]) {
                        r--;
                    }                     
                    while (l < r && nums[l] == nums[l + 1]) {
                        l++;
                    }                    
                    l++;
                    r--;
                }
                if (sum > 0) r--;
                if (sum < 0) l++;
            }
        }
        return res;
    }
}
```



#### 电话号码的字母组合

```java
class Solution {
    List<String> res = new ArrayList<>();
    public List<String> letterCombinations(String digits) {
        if (digits == null || digits.length() == 0) return res;
        String[] strs = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        dfs(0, digits, strs, "");
        return res;
    }
    
    public void dfs(int level, String s, String[] strs, String path) {
        if (level == s.length()) {
            res.add(path);
            return;
        }
        int digit = s.charAt(level) - '0';
        String str = strs[digit];
        for (int i = 0; i < str.length(); i++) {
            dfs(level + 1, s, strs, path + str.charAt(i));
        }
    }
}
```






