### 动态规划
## 特点：通过把原问题分解为相对简单的子问题的方式求解复杂问题的方法。
## 常用于：
1. 具有重叠的子问题   
    1. 子问题和原问题非常相似：具有递归性质
    2. 子问题可能出现多次：一个子问题只计算一次，然后进行记忆化存储（考虑使用空间压缩），便于下次之间查找，减少计算量
    3. 原问题的解可以由子问题推导：求状态转移方程
    4. 无后效性：即子问题的解一旦确定，就不再改变，不受在这之后、包含它的更大的问题的求解决策影响
2. 最优子结构：如果一个问题的最优解包含其子问题的最优解，就称此问题具有最优子结构


### 简单入门  
1. [爬楼梯](https://leetcode-cn.com/problems/climbing-stairs/)
```C++
class Solution {
public:
    //到达第n阶梯的方法数=到达第n-1层方法数+到达第n-2层的方法数
    int climbStairs(int n) {
        if(n<=2)//如果阶梯只有一层，一步抵达
            return n;
        int pre1=1,pre2=2;
        for(int i=3;i<=n;++i)
        {
            int temp=pre1+pre2;
            pre1=pre2;
            pre2=temp;
        }
        return pre2;
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
        //a代表[0,i-2]的区间内不触发机关能抢到的最大金额（子问题最优解）
        //b代表[0,i-1]的区间内不触发机关能抢到的最大金额（子问题最优解）
        //求取[0,i]区间内抢劫的最大金额c受a和b的影响，c=max(a+nums[i],b))（全局最优解）
        for(int i=0;i<n;i++)
        {   
            int temp=max(a+nums[i],b);//状态转移方程
            a=b;
            b=temp;
        }
        return b;
    }
};
```
3. [矩阵的最小路径](https://www.nowcoder.com/practice/7d21b6be4c6b429bb92d219341c4f8bb?tpId=188&&tqId=38601&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking) 
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
4. [求路径数](https://www.nowcoder.com/practice/166eaff8439d4cd898e3ba933fbc6358?tpId=188&&tqId=38657&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking)  
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
                    //到达[i,j]位置的路径总数=到达[i-1,j]位置的路径总数+到达[i,j-1]位置的路径总数
                    dp[i][j]=dp[i-1][j]+dp[i][j-1];//状态转移方程
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
                //若成立，则《以nums[i]结尾的等差子数组》的个数比《以nums[i-1]结尾的等差子数组》个数多一个，多了那个是：(nums[i-2],nums[i-1],nums[i])
                dp=dp+1;//状态转移方程
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
9. [最大正方形](https://leetcode-cn.com/problems/maximal-square/)
```C++
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int n=matrix.size();
        int m=matrix[0].size();
        int max_side=0;//标记全局的最大边长
        vector<vector<int>> dp(n,vector<int>(m,0));//dp[i][j]代表以元素[i,j]为右下角的最大正方形的边长
        for(int i=0;i<n;i++)
        {
            for(int j=0;j<m;j++)
            {
                if(matrix[i][j]=='0')
                    dp[i][j]=0;
                else
                {
                    if(i!=0&&j!=0)
                    {
                        dp[i][j]=min(min(dp[i-1][j],dp[i][j-1]),dp[i-1][j-1])+1;//状态转移方程
                    }else
                    {
                        dp[i][j]=1;
                    }
                }
                if(dp[i][j]>max_side)
                    max_side=dp[i][j];
            }
        }
        return max_side*max_side;
    }
};
```
10. [最长公共子串](https://leetcode-cn.com/problems/longest-common-subsequence/)
```C++
class Solution {
public:
    int longestCommonSubsequence(string text1, string text2) {
        int m=text1.size();
        int n=text2.size();
        vector< vector<int>> dp(m,vector<int>(n,0));
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(text1[i]==text2[j])
                {
                    if(i==0||j==0)//如果i==0||j==0,所以text1[0~i-1]和text2[0~j-1]不存在公共子串
                        dp[i][j]=1;
                    else
                        dp[i][j]=dp[i-1][j-1]+1;//状态转移方程, text1[i]==text2[j]则公共子串长度在dp[i-1][j-1]+1
                }else{
                    if(i==0&&j==0)
                        dp[i][j]=0;//i==0&&j==0，那么text1[0~i-1]和text2[0~j-1]长度为0，不受前面的字符子数组影响
                    else
                    {   
                        if(j>0)
                            dp[i][j]=max(dp[i][j],dp[i][j-1]);//dp[i][j]受dp[i][j-1]或者dp[i-1][j]影响
                        if(i>0)
                            dp[i][j]=max(dp[i][j],dp[i-1][j]);
                    }
                }
            }
        }
        return dp[m-1][n-1];
    }
};
```
11. [编辑距离](https://leetcode.cn/problems/edit-distance/)
```C++
/*
dp[i][j] 表示str1的前i个字符和str2的前y个字符的编辑距离。首先初始化动态数组，dp[0][j]表示将一个空串转换成str2的前j个字符需要操作数，我们知道应该是j个插入操作;dp[i][0]表示将str1的前i个字符转换为空串的操作数，我们知道应该是i个删除操作。
接着用两层for循环遍历两个字符串str1、str2。
比较每一个字符str1[i-1]和str2[j-1]，即str1第i个字符和str2第j个字符
若两个字符相等，即str1[i-1] == str2[j-1]，则在这一个位置的编辑距离和上一个字符相同，因此对应的数组dp[i][j]=dp[i-1][j-1]；
若两个字符不相等：
可以通过可删除str1[i-1]这个字符，但是还需dp[i-1][j]个编辑操作才能使两个字符串相同
即dp[i][j] = 1 + dp[i-1][j]；
还可以通过删除str2[j-1]这个字符，但是还需dp[i][j-1]个编辑操作才能使两个字符串相同
即dp[i][j] = 1 + dp[i][j-1]；
也可以替换str1[i-1]为str2[i-1]使二者相等，此时dp[i][j] = 1 + dp[i-1][j-1]。
取三者之中最小为两个字符串的最小编辑距离
*/
    int minDistance(string word1, string word2) {
        int m=word1.size();
        int n=word2.size();
        vector< vector<int>> dp(m+1,vector<int>(n+1,0));
        dp[0][0]=0;
        for(int i=1;i<=m;i++)
            dp[i][0]=i;
        for(int j=1;j<=n;j++)
            dp[0][j]=j;
        for(int i=1;i<=m;i++)
        {
            for(int j=1;j<=n;j++)
            {
                if(word1[i-1]==word2[j-1])
                    dp[i][j]=dp[i-1][j-1];
                else
                    dp[i][j]=min(min(dp[i-1][j],dp[i][j-1]),dp[i-1][j-1])+1;
            }
        }
        return dp[m][n];
    }
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
        for(int i=1;i<=aim;i++)
        {
            for(int j=0;j<arr.size();j++)
            {   
                if(arr[j]<=i)//面值比i小才能进行兑换，否则没有意义
                    dp[i]=min(dp[i],dp[i-arr[j]]+1);
            }
        }
        return dp[aim]>aim?-1:dp[aim];////-1表示换钱失败
    }
};
```
2. [求整数由完全平方数组合的个数](https://leetcode-cn.com/problems/perfect-squares/)
```C++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n+1,INT_MAX);
        dp[0]=0;
        for(int i=1;i<=n;i++)
        {
            for(int j=1;j*j<=i;j++)
            {
                dp[i]=min(dp[i],dp[i-j*j]+1); //状态转移方程
            }
        }
        return dp[n];
    }
};
```
3. [单词拆分](https://leetcode-cn.com/problems/word-break/)
```C++
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {
        int n=s.length();
        vector<bool> dp(n+1,false);
        dp[0]=true;
        for(int i=1;i<=n;i++)
        {
            for( auto word:wordDict)
            {
                int len=word.length();
                if(i>=len&&s.substr(i-len,len)==word)
                {
                    dp[i]=dp[i]||dp[i-len];
                }
            }
        }
        return dp[n];
    }
};
```