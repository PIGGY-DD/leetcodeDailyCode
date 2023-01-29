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
## 2023.1.8
![image](https://user-images.githubusercontent.com/61445378/211178722-1691a3bc-953f-424d-8062-ed17ab346b3c.png)

### 思路和代码
今天的每日一题比较简单，直接使用`String.startWith(String pre)`函数即可解决。
```java
class Solution {
    public int prefixCount(String[] words, String pref) {
        int cnt = 0;
        for (String word : words) {
            if (word.startsWith(pref))
                cnt++;
        }
        return cnt;
    }
}
```


## 2023.1.9

![image](https://user-images.githubusercontent.com/61445378/211727721-8d2c986c-ab30-41b7-b3dc-f7dac77d6f1c.png)
![image](https://user-images.githubusercontent.com/61445378/211727749-c0bac15f-4494-4e0a-9ff3-8d62feef893c.png)
### 思路和代码
模拟运算过程，同时计数。
```java
class Solution {
    public int reinitializePermutation(int n) {
        int[] perm = new int[n];
        int[] arr = new int[n];
        for (int i = 0; i < n; i++) {
            perm[i] = i;
        }

        for (int i = 0; ; i++) {
            for (int j = 0; j < n; j++) {
                if (j % 2 == 0) {
                    arr[j] = perm[j / 2];
                } else {
                    arr[j] = perm[n / 2 + (j - 1) / 2];
                }
            }

            for (int k=0;k<n;k++)
            {
                perm[k] = arr[k];
            }

            int flag = 0;
            for (int k=0;k<n;k++)
            {
                if (perm[k] != k)
                {
                    flag = 1;
                    break;
                }
            }

            if (flag == 0)
                return i+1;
        }
    }
}
```

## 2023.1.10
### 思路和代码

## 2023.1.11
![image](https://user-images.githubusercontent.com/61445378/211724426-8f6b5762-5618-4527-ad6d-39c9a6fa803d.png)

### 思路和代码
先判断每个数字出现的次数，然后判断每个数字出现的次数是否等于对应位置字符串表示的数量。
```java
class Solution {
    public boolean digitCount(String num) {
        int[] cnt = new int[10];
        for(char ch : num.toCharArray())
        {
            cnt[ch-'0']++;
        }

        for(int i=0;i<num.length();i++)
        {
            if((num.charAt(i)-'0') != cnt[i])
                return false;
        }
        return true;
    }
}
```
## 2023.1.12
![image](https://user-images.githubusercontent.com/61445378/211966392-f89f9aa1-1e66-473f-a54a-79d4f38184fb.png)
![image](https://user-images.githubusercontent.com/61445378/211966418-696e7b71-26d0-40d6-8b4a-a47876ecdfe4.png)
### 思路和代码
简单的字符串匹配替换问题，唯一需要小心的就是数值范围比较大，所以需要提高查询替换词的效率，因此将List转存为Map来提升查询效率。
当遇到'('时开始存储括号内的字符串，当遇到')'时则停止，开始在Map中查询对应的字符串，找不到对应的则存为"?"，最后返回替换结果即可。
```java
class Solution {
    public String evaluate(String s, List<List<String>> knowledge) {
        StringBuffer sb = new StringBuffer();

        int len = s.length();
        
        HashMap<String,String> map = new HashMap<>();
        for (List<String> list : knowledge) {
            map.put(list.get(0),list.get(1));
        }
        
        
        for (int i=0;i<len;i++)
        {
            char ch = s.charAt(i);
            if (ch == '(')
            {
                StringBuffer ss = new StringBuffer();
                i++;
                while (s.charAt(i)!=')')
                {
                    ss.append(s.charAt(i));
                    i++;
                }
                
                if(map.containsKey(ss.toString()))
                {
                    sb.append(map.get(ss.toString()));
                }
                else
                    sb.append("?");
            }
            else
            {
                sb.append(ch);
            }
        }

        return sb.toString();
    }
}
```
## 2023.1.13
![image](https://user-images.githubusercontent.com/61445378/212288476-211d76c2-1630-420f-87fc-7ff893b3c046.png)
![image](https://user-images.githubusercontent.com/61445378/212288501-e702c7f6-dff8-4fbd-9003-2263cfbf688f.png)

### 思路和代码
因为不涉及到s中字母排列的先后顺序，所以只需要统计target和s中各个小写字母出现的次数，然后判断s中字母可以组成target的次数即可。
```java
class Solution {
    public int rearrangeCharacters(String s, String target) {
        int[] targetArray = new int[26];
        for(char ch : target.toCharArray())
        {
            targetArray[ch - 'a']++;
        }

        int[] cnt = new int[26];
        for(char ch : s.toCharArray())
        {
            cnt[ch - 'a']++;
        }
        int ans = Integer.MAX_VALUE;
        for(int i=0;i<26;i++)
        {
            if(targetArray[i] != 0)
            {
                ans = Math.min(ans, cnt[i] / targetArray[i]);
            }
        }
        return ans;

    }
}
```

## 2023.1.15
![image](https://user-images.githubusercontent.com/61445378/212596599-6c103822-38b6-4e52-a225-8a4e0e745840.png)
![image](https://user-images.githubusercontent.com/61445378/212596575-fe254f31-a77a-4b4d-a171-76d13cb63f25.png)
### 思路和代码
直接根据规律模拟整个的计算过程，然后递归即可，递归出口为数组长度为1.
```java
class Solution {
    public int minMaxGame(int[] nums) {
        int len = nums.length;
        if(len == 1)
            return nums[0];

        int[] newNums = new int[len/2];

        for(int i=0;i<len/2;i++)
        {
            if(i%2==0)
            {
                newNums[i] = Math.min(nums[2*i],nums[2*i+1]);
            }
            else
            {
                newNums[i] = Math.max(nums[2*i],nums[2*i+1]);
            }
        }

        return minMaxGame(newNums);
    }
}
```

## 2023.1.16
![image](https://user-images.githubusercontent.com/61445378/212596642-f09509f2-0c73-4692-a045-f348a609ea54.png)
![image](https://user-images.githubusercontent.com/61445378/212596670-7ec12129-1299-4a03-88c8-502adb111544.png)
### 思路和代码
```java
```

## 2023.1.20
![image](https://user-images.githubusercontent.com/61445378/214222772-52c3b880-83c1-4e68-9503-eb24320fa768.png)
![image](https://user-images.githubusercontent.com/61445378/214222807-7294c7fb-9a48-47a9-9d3a-102274acf770.png)

## 思路和代码
先统计每个用户的活跃时长，因为同一分钟的多次活跃次数记为一次，所以使用set。
然后根据统计各个时长的数量。
```java
class Solution {
    public int[] findingUsersActiveMinutes(int[][] logs, int k) {
        int[] ans = new int[k];

        // 先统计每个用户的活跃时间，然后统计活跃时间的数量

        HashMap<Integer, Set<Integer>> map = new HashMap<>();

        for (int[] log : logs) {
            int id = log[0];
            int time = log[1];
            if (map.containsKey(id)) {
                Set<Integer> set = map.get(id);
                set.add(time);
                map.put(id, set);
            } else {
                Set<Integer> set = new HashSet<>();
                set.add(time);
                map.put(id, set);
            }
        }
        for (int id : map.keySet()) {
            Set<Integer> set = map.get(id);
            int times = set.size();
            if (times > 0) {
                ans[times-1]++;
            }
        }

        return ans;
    }
}
```

## 2023.1.21
![image](https://user-images.githubusercontent.com/61445378/214220710-10981d32-6a47-4c8c-8794-a8bb93cb9faa.png)
![image](https://user-images.githubusercontent.com/61445378/214220739-9d44b474-44df-4ddf-9182-2d4cc80cd951.png)
![image](https://user-images.githubusercontent.com/61445378/214220764-daf468d0-e662-4d1b-bc0a-fa8fd28b9919.png)
![image](https://user-images.githubusercontent.com/61445378/214220805-1547322d-ee25-4a21-8fc8-82087b146a0c.png)
## 思路和代码
使用动态规划的思想，每次计算到当前位置需要的最少横跳的次数。计算过程中分为一下几种情况：
1. 当前位置有石头，则标记为最大值`dp[i][j] = 0x3f3f3f3f`
2. 当前位置没有石头，则分为两种情况
    1. 当前赛道的上一个位置没有石头，则无需横跳`dp[i][j]=dp[i-1][j]`；
    2. 当前赛道的上一个位置有石头，则只能从其他两个赛道横跳过来`dp[i][j] = min(dp[i-1][j], dp[i-1][(j+1)%3]+1, dp[i-1][(j+2)%3]+1)`
```java
public int minSideJumps(int[] obstacles) {
        int len = obstacles.length;

        int[][] dp = new int[len][3];

        dp[0][0] = 1;
        dp[0][1] = 0;
        dp[0][2] = 1;

        for (int i = 1; i < len; i++) {

            for (int j = 0; j < 3; j++) {
                // 当前位置没有石头，则可以由上个位置的任意一个跑道跳过来
                if (obstacles[i] != j + 1) {
                    dp[i][j] = dp[i - 1][j];
                    // 上个位置也有石头，则需要从其他跑道跳过来
                    if (obstacles[i - 1] != j + 1) {
                        dp[i][j] = Math.min(dp[i][j], Math.min(dp[i - 1][(j + 1) % 3] + 1, dp[i - 1][(j + 2) % 3] + 1));
                    }
                } else {
                    dp[i][j] = 0x3f3f3f3f;
                }
            }
        }

        return Math.min(dp[len - 1][0], Math.min(dp[len - 1][1], dp[len - 1][2]));
    }
```

## 2023.1.23
![image](https://user-images.githubusercontent.com/61445378/214214843-3416e5ea-40a3-4378-aa23-a227f4ef01b4.png)
![image](https://user-images.githubusercontent.com/61445378/214214872-4d03773e-a0a2-4a93-9b46-e4dc13c79ec2.png)
### 思路和代码
总结来说，最终结果为`ans = b[0][0] * b[0][1] + (b[1][0] - b[0][1]) * b[1][1] + ... + (b[i][0] - b[i-1][0]) * b[i][1] + ... + (income - b[t][0]) * b[t][1]`。但是需要注意几个特殊情况：
1. income小于等于b[0][0]时，答案为`income * brackets[0][1] * 0.01`;
2. income大于b[0][0]时，计算[0,b[0][0]]区间时的公式不一样；
3. 当income小于等于b[t][0]时，计算公式为'(income - brackets[t-1][0]) * b[t][1]'
```java
class Solution {
    public double calculateTax(int[][] brackets, int income) {
        double ans = 0;
        if (income <= brackets[0][0]) {
            return income * brackets[0][1] * 0.01;
        }
        for (int i = 0; i < brackets.length; i++) {
            int a = brackets[i][0];
            double b = brackets[i][1] * 0.01;
            if (a >= income) {
                ans += (income - brackets[i - 1][0]) * b;
                break;
            }
            if (i>0)
                ans += (a - brackets[i - 1][0]) * b;
            else
                ans = a * b;
        }
        return ans;
    }
}
```

## 2023.1.24
![image](https://user-images.githubusercontent.com/61445378/214212501-15e65f93-cdf6-41a0-9042-cb7d5f82cb73.png)
![image](https://user-images.githubusercontent.com/61445378/214212523-c52a27f6-4b26-4fd6-bfff-8131f37359a5.png)
![image](https://user-images.githubusercontent.com/61445378/214212561-fbcb3151-0ea2-493b-a9bb-12a828ee3e0c.png)
### 思路和代码
因为数据量并不大，所以直接暴力就可以。根据每个圆心坐标和半径信息，依次计算每个点和圆心之间的距离，与半径相对比即可。
```java
class Solution {
    public int[] countPoints(int[][] points, int[][] queries) {
        int len = queries.length;
        int[] ans = new int[len];

        for (int i = 0; i < len; i++) {
            int x = queries[i][0];
            int y = queries[i][1];
            int r = queries[i][2];
            for (int j = 0; j < points.length; j++) {
                int a = points[j][0];
                int b = points[j][1];
                int dis = Math.abs((x - a) * (x - a) + (y - b) * (y - b));
                // System.out.println(dis+"---"+r);
                if (dis<=r*r)
                {
                    ans[i]++;
                }
            }
        }


        return ans;
    }
}
```


## 2023.1.26
![image](https://user-images.githubusercontent.com/61445378/214756503-190b6356-a0d2-4169-9227-db3564d66e7d.png)
![image](https://user-images.githubusercontent.com/61445378/214756519-ee535364-3186-44ac-838b-621634a73786.png)
### 思路和代码
根据题目要求，最终答案是一个长度为n、由小写字母构成的字符串，且字典序要达到最小，因此最终的字符串只能为类似`aaa...`的格式。为了达到最小的字典序，所以使用倒序的形式求解。
1. 因为要保证答案的长度，所以每个位置至少为a，因此`k=k-n`；
2. 从最后的位置开始，每个位置均设置为可能的最大值即可得到最小字典序的答案。
```java
class Solution {
    public String getSmallestString(int n, int k) {
        StringBuffer sb = new StringBuffer();

         k -= n;

         for (int i=0;i<n;i++)
         {
             int cur = 0;

             if (k>=25)
             {
                 cur = 25;
                 k -= 25;
             }
             else
             {
                 cur = k;
                 k = 0;
             }

             char ch = (char) ('a' + cur);
             sb.append(ch);
         }

         return sb.reverse().toString();
    }
}
```
## 2023.1.27
![image](https://user-images.githubusercontent.com/61445378/215009691-9825117f-91d6-4fac-b7e9-145f7737fd50.png)
![image](https://user-images.githubusercontent.com/61445378/215009721-a1311452-bff3-4a74-b950-43f1b65b91f8.png)
### 思路和代码
分为两步，首先找到大写和小写均存在的字母，然后在这些字母中找到字典序最大的返回即可。
```java
class Solution {
    public String greatestLetter(String s) {
        int[] letter = new int[26];
        Arrays.fill(letter, -1);
        // 将大小写均出现的字母标记为2
        for (int i = 0; i < s.length(); i++) {
            char ch = s.charAt(i);
            if (Character.isLowerCase(ch)) {
                if (letter[ch - 'a'] == -1) {
                    letter[ch - 'a'] = 0;
                }
                if (letter[ch - 'a'] == 1) {
                    letter[ch - 'a'] = 2;
                }
            } else {
                if (letter[ch - 'A'] == -1) {
                    letter[ch - 'A'] = 1;
                }
                if (letter[ch - 'A'] == 0) {
                    letter[ch - 'A'] = 2;
                }
            }
        }
        String ans = "";
        
        for (int i = 25; i >= 0; i--) {
            if (letter[i] == 2)
                return String.valueOf((char) ('A' + i));
        }

        return ans;
    }
}
```
## 2023.1.28
![image](https://user-images.githubusercontent.com/61445378/215304089-b4320195-8cff-44ea-9e56-8f29bf2672c8.png)
![image](https://user-images.githubusercontent.com/61445378/215304094-78a6adbe-2e88-44ff-a7a1-ae2e9a1d53e5.png)

### 思路和代码
```java
class Solution {
    public int waysToMakeFair(int[] nums) {
        int len = nums.length;
        
        // 记录当前位置i的奇数和与偶数和 不包括当前位置i的值
        int[] odd = new int[len+1];
        int[] even = new  int[len+1];

        int ans = 0;
        int oddSum = 0, evenSum = 0;

        for (int i=0;i<len;i++)
        {
            if (i%2==0)
            {
                evenSum += nums[i];
            }
            else
            {
                oddSum += nums  [i];
            }
            even[i+1] = evenSum;
            odd[i+1] = oddSum;
        }

        for (int i=0;i<len;i++)
        {
            int curOddSum = 0, curEvenSum = 0;

            // 删除当前位置后，奇数和 偶数和
            // 奇数和 = 当前位置之前的奇数和 + 最大偶数和-当前位置+1的偶数和
            curOddSum = odd[i] + even[len] - even[i+1];
            curEvenSum = even[i] + odd[len] - odd[i+1];
            if (curEvenSum == curOddSum)
                ans++;
        }

        return ans;
    }
}
```

## 2023.1.29
![image](https://user-images.githubusercontent.com/61445378/215303151-5fd6e07c-545b-432c-b40f-0979fc937ba9.png)
![image](https://user-images.githubusercontent.com/61445378/215303188-e9f5489a-c401-4894-a40b-4c0719297a84.png)

### 思路和代码
使用标志位，在遇到第一个`|`前开始记录`*`的数量，每遇到一个`|`改变标志位的值，根据标志位的值决定是否记录`*`的数量。
```java
class Solution {
    public int countAsterisks(String s) {
        int cnt = 0;

        int flag = 1;
        int len = s.length();
        int index = 0;

        while (index<len)
        {
            if (flag == 1)
            {
                while (index<len)
                {
                    char ch = s.charAt(index);
                    if (ch == '*')
                        cnt++;
                    if (ch == '|')
                        break;
                    index++;
                }
            }
            else
            {
                while (index<len)
                {
                    char ch = s.charAt(index);
                    if (ch == '|')
                        break;
                    index++;
                }
            }
            flag *= -1;
            index++;
        }

        return cnt;
    }
}
```
