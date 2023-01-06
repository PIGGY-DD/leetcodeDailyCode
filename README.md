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
