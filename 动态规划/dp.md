### 动态规划：  
通过把原问题分解为相对简单的子问题的方式求解复杂问题的方法。
## 特点：
1. 具有重叠的子问题   
    1. 子问题和原问题非常相似：具有递归性质
    2. 子问题可能出现多次：一个子问题只计算一次，然后进行记忆化存储（考虑使用空间压缩），便于下次之间查找，减少计算量
    3. 原问题的解可以由子问题推导：求状态转移方程
2. 最优子结构：如果一个问题的最优解包含其子问题的最优解，就称此问题具有最优子结构
    * 动态规划：求解全局最优要以局部最优的为基础，但是全局最优并不代表其每一个局部都是最优
    * 贪心算法：贪心算法局部最优直接导致全局最优
3. 子问题无后效性：即子问题的解一旦确定就不再改变，子问题影响后续大的问题的决策，但是不受在这之后、包含它的更大的问题的求解决策影响
4. 通常是自底向上的，由小问题到大问题。


### 一维线性 
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
2. [连续子数组的最大和](https://leetcode.cn/problems/lian-xu-zi-shu-zu-de-zui-da-he-lcof/)
* 首先确定dp的定义：为以nums[i]元素结尾的连续子数组的最大和
* max_sum寻找以nums[i]元素结尾的连续子数组最大和中的最大者
```C++
    int maxSubArray(vector<int>& nums) {
        int max_sum=nums[0];
        int dp=nums[0];
        for(int i=1;i<=nums.size()-1;i++)
        {
            dp=max(dp+nums[i],nums[i]);
            max_sum=max(dp,max_sum);
        }
        return max_sum;
    }
```
3. [连续子数组最大乘积](https://leetcode.cn/problems/maximum-product-subarray/)
```C++
    int maxProduct(vector<int>& nums) {
        if(nums.size()==1)
            return nums[0];
        int mx=nums[0],mn=nums[0],maxProduct=nums[0];
        for(int i=1;i<nums.size();i++)
        {
            int val=nums[i];
            if(val>0)
            {
                mx=max(mx*val,val);//求得包括val的连续数组最大乘积
                mn=min(mn*val,val);//求得包括val的连续数组最小乘积
                maxProduct=max(maxProduct,mx);
            }else{
                int a=mx,b=mn;
                mx=max(b*val,val);
                mn=min(a*val,val);
                maxProduct=max(maxProduct,mx);
            }
        }
        return maxProduct;
    }
```

5. [等差数列划分](https://leetcode.cn/problems/arithmetic-slices/description/)
```C++ 
    int numberOfArithmeticSlices(vector<int>& nums) {
        if(nums.size()<3)//size<3直接不存在等差数组
            return 0;
        int sum=0;//累积dp和，即输入数组nums总的等差数列个数
        int dp=0;//dp定义成以nums[i]元素结尾的连续子数组包含的等差数列个数
        for(int i=2;i<nums.size();i++)
        {
            if(nums[i]-nums[i-1]==nums[i-1]-nums[i-2])
            {
                dp+=1;
            }else
                dp=0;
            sum+=dp;
        }
        return sum;
    }
```

### 二维矩阵
1. [放苹果](https://www.nowcoder.com/practice/bfd8234bb5e84be0b493656e390bdebf?tpId=37&tqId=21284&rp=1&ru=/exam/oj/ta&qru=/exam/oj/ta&sourceUrl=%2Fexam%2Foj%2Fta%3Fdifficulty%3D2%26page%3D1%26pageSize%3D50%26search%3D%26tpId%3D37%26type%3D37&difficulty=2&judgeStatus=undefined&tags=&title=)
```C++
int main() {
    int m,n;
    while(cin>>m>>n)
    {
        vector<vector<int>> v(m+1,vector<int>(n+1,0));
        for(int i=1;i<=n;i++)
        {
            v[1][i]=1;//1个苹果放i个盘子方法为1种
            v[0][i]=1;//0个苹果放i个盘子方法为1种
        }    
        for(int j=1;j<=m;j++)
            v[j][1]=1;//j个苹果放1个盘子方法为1种
        for(int i=2;i<=m;i++)
        {
            for(int j=2;j<=n;j++)
            {
                if(j>i)//盘子比苹果多
                    v[i][j]=v[i][i];//盘子j大于苹果i的情况下，等同于i个盘子i个苹果
                else{
                    v[i][j]=v[i][j-1]+v[i-j][j];
                }
            }
        }
        cout<<v[m][n]<<endl;
    }
}
```
### 序列和子串问题
1. 回文子序列问题
* [最长回文子序列](https://leetcode.cn/problems/longest-palindromic-subsequence/description/)
```C++
    int longestPalindromeSubseq(string s) {
        int n=s.size();
        vector<vector<int>> dp(n,vector<int>(n,0));
        for(int i=0;i<n;i++)
            dp[i][i]=1;
        for(int len=1;len<n;len++)//长度为len+1的子串，串长越来越大
        {
            for(int pos=0;pos+len<n;pos++)
            {
                int i=pos;
                int j=pos+len;
                if(s[i]==s[j])
                {
                    dp[i][j]=dp[i+1][j-1]+2;
                }else{
                    dp[i][j]=max(dp[i+1][j],dp[i][j-1]);
                }
            }
        }
        return dp[0][n-1];
    }
```
* [最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/description/)
```C++
    string longestPalindrome(string s) {
        int n=s.size();
        vector<vector<int>> dp(n,vector<int>(n,0));
        string s0=s.substr(0,1); //最短长度为1，任意一个单字符串都可以
        for(int i=0;i<n;i++)
        {
            dp[i][i]=1;
        }
        for(int len=1;len<n;len++)// 长度不断增长
        {
            for(int pos=0;pos+len<n;pos++)
            {
                int i=pos;
                int j=pos+len;
                if(s[i]==s[j])
                {
                    if(dp[i+1][j-1]>=0) // 说明里面的子串是回文串
                    {
                        dp[i][j]=dp[i+1][j-1]+2;
                        int size=dp[i][j];
                        if(size>s0.size())
                        {
                            s0=s.substr(i,size);
                        }
                    }    
                    else
                        dp[i][j]=-1;// 因为dp[i+1][j-1]内串不是回文子串，所以即使相等也没有用
                }else{
                    dp[i][j]=-1;  // s[i]！=s[j] 不是回文串，标记为-1
                }
            }
        }
        return s0;
    }
```
2. 公共子序列问题
* [最长公共子序列](https://leetcode-cn.com/problems/longest-common-subsequence/)
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
* [最长公共子序列(二)](https://www.nowcoder.com/practice/6d29638c85bb4ffd80c020fe244baf11?tpId=295&tqId=991075&ru=/exam/oj&qru=/ta/format-top101/question-ranking&sourceUrl=%2Fexam%2Foj)
```C++
    string LCS(string s1, string s2) {
        int m = s1.size();
        int n = s2.size();
        if(m == 0 || n == 0)
            return "-1";
        vector<vector<int> > dp(m,vector<int> (n,0));
        vector<vector<int> > dr(m,vector<int> (n,0));
        for(int i = 0; i < m; ++i)
        {
            for(int j = 0; j < n; ++j)
            {
                if(s1[i] == s2[j])
                {
                    if(i == 0|| j == 0)
                    {
                        dp[i][j] = 1;
                        dr[i][j] = 0; // 没有地方后退了
                    }else{
                        dp[i][j] = dp[i-1][j-1] + 1;
                        dr[i][j] = 1; // 从左上角过来
                    }
                }else{
                    if(i == 0||j == 0)
                    {
                        if(i == 0 && j == 0)
                        {
                            dp[i][j] = 0;
                            dr[i][j] = 0; // 没有地方可退了
                        }else if(i > 0)
                        {
                            dp[i][j] = dp[i-1][j];
                            dr[i][j] = 2;
                        }else if(j > 0)
                        {
                            dp[i][j] = dp[i][j-1];
                            dr[i][j] = 3;
                        }
                    }else{
                        dp[i][j] = max(dp[i-1][j],dp[i][j-1]);
                        if(dp[i-1][j] > dp[i][j-1])
                        {
                            dr[i][j] = 2; // 从上边过来
                        }else{
                            dr[i][j] = 3; // 从左边过来
                        }
                    }
                }
            }
        }
        string com;
        if(dp[m-1][n-1] == 0)
            com = "-1";
        else{
            int i = m-1;
            int j = n-1;
            while(1)
            {
                if(s1[i] == s2[j])
                {
                    com = s1[i] + com;
                }
                if(dr[i][j] == 0)
                {
                    break;
                }else{
                    if(dr[i][j] == 1)
                    {
                        --i;
                        --j;
                    }else if(dr[i][j] == 2)
                    {
                        --i;
                    }else if(dr[i][j] == 3)
                    {
                        --j;
                    }
                }
            }
        }
        return com;
    }
```
3. 子串问题
* [最长公共子串](https://www.nowcoder.com/practice/f33f5adc55f444baa0e0ca87ad8a6aac?tpId=196&tqId=37132&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=%2Fexam%2Foj%3Fpage%3D1%26pageSize%3D50%26search%3D%25E6%259C%2580%25E9%2595%25BF%25E5%2585%25AC%25E5%2585%25B1%25E5%25AD%2590%26tab%3D%25E7%25AE%2597%25E6%25B3%2595%25E7%25AF%2587%26topicId%3D196&difficulty=undefined&judgeStatus=undefined&tags=&title=最长公共子)
```C++
    string LCS(string s1, string s2) {
        int m = s1.size();
        int n = s2.size();
        int mx = 0,pos=0;
        vector<vector<int> > dp(m,vector<int> (n,0));
        for(int i = 0; i < m; ++i)
        {
            for(int j = 0; j < n; ++j)
            {
                if(s1[i] == s2[j])
                {
                    if(i ==0 || j == 0)
                    {
                        dp[i][j] = 1;
                    }else{
                        dp[i][j] = dp[i-1][j-1] + 1;
                    }
                }else{
                    dp[i][j] = 0;
                }
                if(dp[i][j] > mx)
                {
                    mx = dp[i][j];
                    pos = i;
                }
            }
        }
        string str = s1.substr(pos-mx+1,mx); // 取得子串，从pos开始倒数mx个字符，包括pos位置的字符
        return str;
    }
```
4. [编辑距离](https://leetcode.cn/problems/edit-distance/)
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
5. [最小路径和](https://leetcode.cn/problems/minimum-path-sum/description/) 
```C++
    int minPathSum(vector<vector<int>>& grid) {
        int m=grid.size();
        int n=grid[0].size();
        for(int i=0;i<m;i++)
        {
            for(int j=0;j<n;j++)
            {
                if(i==0&&j==0)
                    continue;
                if(i==0&&j!=0)
                    grid[i][j]=grid[i][j]+grid[i][j-1];
                if(i!=0&&j==0)
                    grid[i][j]=grid[i][j]+grid[i-1][j];
                if(i!=0&&j!=0)
                    grid[i][j]=grid[i][j]+min(grid[i-1][j],grid[i][j-1]);
            }
        }
        return grid[m-1][n-1];
    }
```
6. [求路径数](https://www.nowcoder.com/practice/166eaff8439d4cd898e3ba933fbc6358?tpId=188&&tqId=38657&rp=1&ru=/activity/oj&qru=/ta/job-code-high-week/question-ranking)  
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
7. [最大正方形](https://leetcode-cn.com/problems/maximal-square/)
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
8.[正则式匹配](https://leetcode.cn/problems/regular-expression-matching/description/)
```C++
    // '*'的意思是前面的字符出现0次或者多次，例如出现0次代表消去前面的字符
    bool isMatch(string s, string p) {
    int n=s.size();
    int m=p.size();
    vector<vector<bool>> dp(n+1,vector<bool>(m+1,false));
    dp[0][0]=true;
    for(int j=2;j<=m;j++)//'*'不可能出现在串p开头第一的位置,所以从j=2开始
    {
        if(p[j-1]=='*')
            dp[0][j]=dp[0][j-2];//s为0，也是可能和正则式进行匹配的，如S=""和P="a*"
    }
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            if(p[j-1]=='.')
                dp[i][j]=dp[i-1][j-1];
            else{
                if(p[j-1]=='*')
                {
                    if(p[j-2]=='.')//p[j-2]为'.'的情况
                    {
                        dp[i][j]=dp[i][j-1]||dp[i][j-2]||dp[i-1][j];

                    }else{//p[j-2]为字母的情况
                        if(p[j-2]==s[i-1])
                        {
                            dp[i][j]=dp[i][j-1]||dp[i][j-2]||dp[i-1][j-1]||dp[i-1][j];
                        }else{
                            dp[i][j]=dp[i][j-2];//因为不相等，'*'前面那个字母出现0次
                            //直接匹配第i个字符和第j-2个字符，看是否匹配，如果匹配，那么
                            //dp[i][j]也匹配了，如果不匹配，那么dp[i][j]就不匹配
                        }
                    }
                }else{
                    if(p[j-1]==s[i-1]&&dp[i-1][j-1])//如果是字母，则判断二者是否相等
                        dp[i][j]=true;//并且前面是否匹配
                }
            }
        }
    }
    return dp[n][m];
    }
```
### 买股票问题
1. [买卖股票的最好时机](https://www.nowcoder.com/practice/64b4262d4e6d4f6181cd45446a5821ec?tpId=117&&tqId=37717) 
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
### 相邻性问题
1. [打家劫舍](https://leetcode-cn.com/problems/house-robber/) 
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
### 递增序列问题
1. [最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/description/)
```C++
    int lengthOfLIS(vector<int>& nums) {
        int n=nums.size();
        if(n==0||n==1)
            return n;
        vector<int> dp(n,1);//dp定义为以【第i(0~n-1)个元素nums[i]结尾的子序列】（不要求是连续的）的最大长度
        int max_len=1;
        for(int i=1;i<n;i++)//求dp[i]是建立在它前面元素的dp[j](不要求连续）的基础上，因此需要对nums[i]前面的每个元素nums[j]进行回溯（和nums[i]）进行比较
        {
            for(int j=0;j<i;j++)
            {
                if(nums[j]<nums[i])
                {
                    dp[i]=max(dp[i],dp[j]+1);
                }
            }
            max_len=max(max_len,dp[i]);//返回的max_len是整个数组的最大值
        }
        return max_len;
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
2.[0-1背包问题](https://www.nowcoder.com/practice/fd55637d3f24484e96dad9e992d3f62e?tpId=230&tqId=2032484&ru=/exam/oj&qru=/ta/dynamic-programming/question-ranking&sourceUrl=%2Fexam%2Foj%3Fpage%3D1%26tab%3D%25E7%25AE%2597%25E6%25B3%2595%25E7%25AF%2587%26topicId%3D230)
```C++
int main() {
    int n,V;
    cin>>n>>V;
    vector<long long> dp1(V+1,0);//求在背包容量允许下能过装下的物品最大价值
    vector<long long> dp2(V+1,-999999999999999999);//求背包刚刚好装满的情况下装下的最大的价值
    dp2[0]=0;
    for(int i=1;i<=n;i++)
    {
        int w,v;
        cin>>w>>v;
        for(int j=V;j>=w;--j)
        {
            dp1[j]=max(dp1[j],dp1[j-w]+v);
            dp2[j]=max(dp2[j],dp2[j-w]+v);
        }
    }
    cout<<dp1[V]<<endl;
    if(dp2[V]<0)
        cout<<0<<endl;
    else
        cout<<dp2[V]<<endl;
}
```
3.[完全背包问题](https://www.nowcoder.com/practice/237ae40ea1e84d8980c1d5666d1c53bc?tpId=230&tqId=2032575&ru=/exam/oj&qru=/ta/dynamic-programming/question-ranking&sourceUrl=%2Fexam%2Foj%3Fpage%3D1%26tab%3D%25E7%25AE%2597%25E6%25B3%2595%25E7%25AF%2587%26topicId%3D230)
```C++ 
int main() {
    int n,V;
    cin>>n>>V;
    vector<long long> dp1(V+1,0);////求在背包容量允许下能过装下的物品最大价值
    vector<long long> dp2(V+1,-999999999999999999);////求背包刚刚好装满的情况下装下的最大的价值
    dp2[0]=0;
    for(int i=1;i<=n;i++)
    {
        int w,v;
        cin>>w>>v;
        for(int j=w;j<=V;j++)
        {
            dp1[j]=max(dp1[j],dp1[j-w]+v);
            dp2[j]=max(dp2[j],dp2[j-w]+v);
        }
    }
    cout<<dp1[V]<<endl;
    if(dp2[V]<0)
        cout<<0<<endl;
    else
        cout<<dp2[V]<<endl;
}
```
4.[分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/description/)
```C++
    bool canPartition(vector<int>& nums) {
        int sum=accumulate(nums.begin(),nums.end(),0);//求数组和
        if(sum%2==1)//sum为奇数，肯定无法同等分割
            return false;
        int target=sum/2;//能否从数组中找到子集的和为target，如果找到，那么则可以分割
        vector<bool> dp(target+1,false);
        dp[0]=true;//如果能找到，则最好dp[target]为true
        for(int i=0;i<nums.size();i++)
        {
            for(int j=target;j>=nums[i];j--)
            {
                dp[j]=dp[j]||dp[j-nums[i]];
            }
        }
        return dp[target];
    }
```


### 分割类问题
1. [完全平方数个数](https://leetcode-cn.com/problems/perfect-squares/)
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
2. [单词拆分](https://leetcode-cn.com/problems/word-break/)
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
### 前缀和差分

1.[二维前缀](https://www.nowcoder.com/practice/99eb8040d116414ea3296467ce81cbbc?tpId=230&tqId=2023819&ru=/exam/oj&qru=/ta/dynamic-programming/question-ranking&sourceUrl=%2Fexam%2Foj%3Fpage%3D1%26tab%3D%25E7%25AE%2597%25E6%25B3%2595%25E7%25AF%2587%26topicId%3D230)
```C++
int main() {
    int n,m,q;
    cin>>n>>m>>q;
    vector<vector<long>> dp(n+1,vector<long>(m+1,0));
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            int val;
            cin>>val;
            dp[i][j]=dp[i][j-1]+dp[i-1][j]-dp[i-1][j-1]+val;//求前缀和
        }
    }
    for(int i=0;i<q;i++)
    {
        int x1,y1,x2,y2;
        cin>>x1>>y1>>x2>>y2;
        cout<<dp[x2][y2]-dp[x2][y1-1]-dp[x1-1][y2]+dp[x1-1][y1-1]<<endl;
    }
}
```
2.[二维差分](https://www.nowcoder.com/practice/50e1a93989df42efb0b1dec386fb4ccc?tpId=230&tqId=2024519&ru=/exam/oj&qru=/ta/dynamic-programming/question-ranking&sourceUrl=%2Fexam%2Foj%3Fpage%3D1%26tab%3D%25E7%25AE%2597%25E6%25B3%2595%25E7%25AF%2587%26topicId%3D230)
```C++
int main() {
    int n,m,q;
    cin>>n>>m>>q;
    vector<vector<long>> dp(n+1,vector<long>(m+1,0));
    vector<vector<long>> num(n+1,vector<long>(m+1,0));
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            cin>>num[i][j];
        }
    }
    for(int i=0;i<q;i++)
    {
        int x1,y1,x2,y2,k;
        cin>>x1>>y1>>x2>>y2>>k;
        dp[x1][y1]+=k;
        if(x2<n)
            dp[x2+1][y1]-=k;
        if(y2<m)
            dp[x1][y2+1]-=k;
        if(x2<n&&y2<m)
            dp[x2+1][y2+1]+=k;
    }
    //求前缀和
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            dp[i][j]+=dp[i][j-1]+dp[i-1][j]-dp[i-1][j-1];
        }
    }
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=m;j++)
        {
            cout<<(num[i][j]+dp[i][j])<<" ";
        }
        cout<<endl;
    }
}
```