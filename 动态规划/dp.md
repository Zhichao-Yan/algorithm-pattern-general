1. [爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)
```C++
class Solution {
public:
    int climbStairs(int n) {
        if(n<=2)
            return n;
        int a=1,b=2;
        for(int i=3;i<=n;i++)
        {
            int temp=a+b;
            a=b;
            b=temp;
        }
        return b;
    }
};
```
2. [打家劫舍](https://leetcode-cn.com/problems/house-robber/) 
``` C++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n=nums.size();
        int a=0,b=nums[0],sum;
        for(int i=1;i<n;i++)
        {
            sum=max(a+nums[i],b);
            a=b;
            b=sum;
        }
        return b;
    }
};
```
