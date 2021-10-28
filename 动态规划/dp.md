### 简单  
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
                    matrix[i][j]=matrix[i][j]+min(matrix[i-1][j],matrix[i][j-1]);
            }
        }
        return matrix[m-1][n-1];
    }
};
```
4. [买卖股票的最好时机](https://www.nowcoder.com/practice/64b4262d4e6d4f6181cd45446a5821ec?tpId=117&&tqId=37717) 
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
                min=prices[i];
            }else{
                if(prices[i]-min>profit)
                    profit=prices[i]-min;
            }
        }
        return profit;
    }
};
```
5. [求路径](https://www.nowcoder.com/practice/166eaff8439d4cd898e3ba933fbc6358?tpId=188&&tqId=38657&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking)  
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
                    dp[i][j]=dp[i-1][j]+dp[i][j-1];
            }
        }
        return dp[m-1][n-1];
    }   
     
};
```
### 中等  
1. [子数组最大乘积](https://www.nowcoder.com/practice/9c158345c867466293fc413cff570356?tpId=188&&tqId=38656&rp=1&ru=/ta/job-code-high-week&qru=/ta/job-code-high-week/question-ranking)
```C++
class Solution {
public:
    double maxProduct(vector<double> arr) {
        double max_multiply=arr[0],mx=arr[0],mn=arr[0];
        for(int i=1;i<arr.size();i++)
        {   
            double a=mx,b=mn;
            if(arr[i]>0)
            {
                mx=max(arr[i], arr[i]*a);
                mn=min(arr[i], arr[i]*b);
            }else{
                mx=max(arr[i],arr[i]*b);
                mn=min(arr[i], arr[i]*a);
            }
            if(mx>max_multiply)
                max_multiply=mx;
        }
        return max_multiply;
    }
};
```
2. [连续子数组的最大和](https://www.nowcoder.com/practice/459bd355da1549fa8a49e350bf3df484?tpId=188&&tqId=38594&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking) 
```C++
class Solution {
public:
    int FindGreatestSumOfSubArray(vector<int> array) {
        int Max=array[0];
        int Maxi=array[0];//maxi定义为包含第i个元素在内的连续元素数组的最大值
        for(int i=1;i<array.size();i++)
        {
            Maxi=max(array[i],Maxi+array[i]);
            if(Maxi>Max)
                Max=Maxi;
        }   
        return Max;
    }
};
```

