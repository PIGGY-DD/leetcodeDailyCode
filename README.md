# leetcode每日一题相关题解记录
## 2023.1.6
![image](https://user-images.githubusercontent.com/61445378/210933277-d4b489b9-4080-4bdb-b987-db3749b16ed1.png)
### 思路和代码
因为数值范围在[1,1000]，所以可以使用暴力法。
遍历[1,num]范围内的每个数字，然后依次判断当前数字是否满足“各数字之和为偶数”。
```java
class Solution {
    public int countEven(int num) {
        int cnt = 0;
        
        for(int i=2;i<=num;i++)
        {
            cnt += help(i)?1:0;
        }

        return cnt;
    }

    public boolean help(int n)
    {
        int cnt = 0;
        while(n!=0)
        {
            cnt += n % 10;
            n /= 10;
        }

        return cnt%2==0;
    }
}
```

## 2023.1.7
![image](https://user-images.githubusercontent.com/61445378/211157225-ac2650e2-5551-42a3-8c6f-04bc4d289d08.png)

### 思路和代码
因为每次只能移除当前数组的最左侧和最右侧的元素作为 `组成x的成员`，如果分情况进行判断的话会非常麻烦，所以使用滑动窗口来做。
当滑动窗口的范围和为 `target=数组总和-x`时，记录当前的 `操作数=数组长度-滑动窗口长度`，最后返回最小的操作数即可。
```java
class Solution {
    public int minOperations(int[] nums, int x) {
        // 计算数组总和
        int cnt = 0;
        int len = nums.length;
        for (int i = 0; i < len; i++) {
            cnt += nums[i];
        }
        // 若数组总和都小于x，则返回-1 等于x则返回数组长度
        if(cnt<x)
            return -1;
        if(cnt == x)
            return len;
            
        // 求target    
        int target = cnt - x;

        // 使用滑动窗口获取最终结果
        int l = 0, r = l + 1;
        int curCnt = nums[l];
        int ans = Integer.MAX_VALUE;
        while (r < len) {
            while (r < len && curCnt < target) {
                curCnt += nums[r];
                r++;
            }
            
            // 当滑动窗口的总和等于x，记录当前的操作数
            if (curCnt == target) {
                int curAns = r - l;
                ans = Math.min(ans, len - curAns);
                
                // 为了找到所有的可能性，将左指针右移
                if(l<len)
                {
                    curCnt -= nums[l];
                    l++;
                }
            }

            while (l < r && curCnt > target) {
                curCnt -= nums[l];
                l++;
            }
            if (curCnt == target) {
                int curAns = r - l;
                ans = Math.min(ans, len - curAns);
                if(l<len)
                {
                    curCnt -= nums[l];
                    l++;
                }
            }

        }
        return ans == Integer.MAX_VALUE ? -1 : ans;
    }
}
```
