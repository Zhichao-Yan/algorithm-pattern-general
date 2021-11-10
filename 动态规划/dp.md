### 动态规划
## 特点
### 简单入门  
1. [爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)
```C++
class Solution {
public:
    //到达第n阶梯的方法数=到达第n-1层方法数+到达第n-2层的方法数
    int climbStairs(int n) {
        if(n<=1)//如果阶梯只有一层，一步抵达
            return 1;
        int a=1,b=1;//a代表能过到达第i-1阶的方法数，b代表能过到达第i-2阶的方法数
        for(int i=2;i<=n;i++)
        {
            int temp=a+b;
            b=a;
            a=temp;
        }
        return a;
    }
};
```
2. [打家劫舍](https://leetcode-cn.com/problems/house-robber/) 
``` C++
class Solution {
public:
    int rob(vector<int>& nums) {
        int n=nums.size();
        int a=0,b=0;//如何理解a和b？
        //a代表[0,i-2]的区间内不触发机关能抢到的最大金额
        //b代表[0,i-1]的区间内不触发机关能抢到的最大金额
        //求取[0,i]区间内抢劫的最大金额c受a和b的影响，c=max(a+nums[i],b))
        for(int i=0;i<n;i++)
        {   
            int temp=max(a+nums[i],b);
            a=b;
            b=temp;
        }
        return b;
    }
};
```
3. [矩阵路径的最小和](https://www.nowcoder.com/practice/7d21b6be4c6b429bb92d219341c4f8bb?tpId=188&&tqId=38601&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking) 
```C++
class Solution {
public:
    int minPathSum(vector<vector<int> >& matrix) {
        int m=matrix.size(),n=matrix[0].size();
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(i==0&&j==0)
                    continue;
                if(i==0&&j!=0)
                    matrix[i][j]=matrix[i][j]+matrix[i][j-1];
                if(i!=0&&j==0)
                    matrix[i][j]=matrix[i][j]+matrix[i-1][j];
                if(i!=0&&j!=0)
                    matrix[i][j]=matrix[i][j]+min(matrix[i-1][j],matrix[i][j-1]);//状态转移方程
            }//当前最小路径取决于：左边和上面位置路径的较小值
        }
        return matrix[m-1][n-1];
    }
};
```
4. [求路径](https://www.nowcoder.com/practice/166eaff8439d4cd898e3ba933fbc6358?tpId=188&&tqId=38657&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking)  
```C++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int> > dp(m,vector<int>(n,0));
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(i==0||j==0)
                    dp[i][j]=1;
                else
                    dp[i][j]=dp[i-1][j]+dp[i][j-1];//到达[i,j]位置的路径总数=到达[i-1,j]位置的路径总数+到达[i,j-1]位置的路径总数
            }
        }
        return dp[m-1][n-1];
    }   
};
``` 
5. [子数组最大乘积](https://www.nowcoder.com/practice/9c158345c867466293fc413cff570356?tpId=188&&tqId=38656&rp=1&ru=/ta/job-code-high-week&qru=/ta/job-code-high-week/question-ranking)
```C++
class Solution {
public:
    double maxProduct(vector<double> arr) {
        double max_multiply=arr[0],mx=arr[0],mn=arr[0];
        for(int i=1;i<arr.size();i++)
        {   
            //a代表以arr[i-1]结尾的连续数组元素累乘的最大值
            //b代表以arr[i-1]结尾的连续数组元素累乘的最小值
            double a=mx,b=mn;
            if(arr[i]>0)//值存在正负
            {
                mx=max(arr[i], arr[i]*a);
                mn=min(arr[i], arr[i]*b);
            }else{
                mx=max(arr[i],arr[i]*b);
                mn=min(arr[i], arr[i]*a);
            }
            if(mx>max_multiply)//max_multiply是其中子数组的累乘的最大积
                max_multiply=mx;
        }
        return max_multiply;
    }
};
```
6. [连续子数组的最大和](https://www.nowcoder.com/practice/459bd355da1549fa8a49e350bf3df484?tpId=188&&tqId=38594&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking) 
```C++
class Solution {
public:
    int FindGreatestSumOfSubArray(vector<int> array) {
        int Max=array[0];
        int sum=array[0];//sum表示以array[i-1]元素结尾的连续数组和的最大值
        //以array[i]元素结尾的数组的和的最大值：取决于以array[i-1]元素结尾的连续数组和的最大值和array[i]的值
        for(int i=1;i<array.size();i++)
        {
            sum=max(array[i],sum+array[i]);//状态方程
            if(sum>Max)//Max记录其中经历的最大的连续数组和
                Max=sum;
        }   
        return Max;
    }
};
```
7. [求等差数组数目的个数](https://leetcode-cn.com/problems/arithmetic-slices/)
```C++
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& nums) {
    
        int dp=0,sum=0;//sum表示总的等差数组个数
        if(nums.size()<3)
            return 0;//长度<0不存在等差数组
        for(int i=2;i<nums.size();i++)
        {
            if(nums[i]-nums[i-1]==nums[i-1]-nums[i-2])
            {   
                //若成立，则《以nums[i]结尾的等差子数组》的个数比《以nums[i]结尾的等差子数组》个数多一个，多了那个是：(nums[i-2],nums[i-1],nums[i])
                dp=dp+1;//状态方程
                sum+=dp;//将个数进行累加
            }else
                dp=0;
        }
        return sum;
    }
};
```
8. [买卖股票的最好时机](https://www.nowcoder.com/practice/64b4262d4e6d4f6181cd45446a5821ec?tpId=117&&tqId=37717) 
```C++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int min=prices[0];
        int profit=0;
        for(int i=1;i<prices.size();i++)
        {
            if(prices[i]<min)
            {
                min=prices[i];//profit不变
            }else{
                profit=max(prices[i]-min,profit);//状态方程
            }
        }
        return profit;
    }
};
```
### 背包问题
1. [换钱的最少货币数](https://www.nowcoder.com/practice/3911a20b3f8743058214ceaa099eeb45?tpId=188&&tqId=38635&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking)
```C++
//对于每一个需要找钱的额度，其换来的最少的纸币数量(不同面额）dp[money]=min(dp[money],dp[money-某一面额]+1)
class Solution {
public:
    int minMoney(vector<int>& arr, int aim) {
        vector<int> dp(aim+1,1e9);//初始化，假设需要很多张
        dp[0]=0;//找钱数为0时需要的货币数量为0
        for(int i=0;i<arr.size();i++)//对于每一种货币面值
        {
            for(int j=arr[i];j<=aim;j++)//初始值比arr[i]小没有意义，因为要用到j-arr[i]<0,对于dp[负数]没有意义
            {
                dp[j]=min(dp[j],dp[j-arr[i]]+1);
            }
        }
        return dp[aim]>aim?-1:dp[aim];//-1表示换钱失败
    }
};
```