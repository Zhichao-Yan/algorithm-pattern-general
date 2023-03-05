### 贪心算法
> 一种在每一步选择中都采取在当前状态下最好或最优（即最有利）的选择，从而希望导致结果是最好或最优的算法。  

> 贪心算法的关键在于贪心策略的选择。必须证明：每一次最贪心的选择最终能得到问题的最优解，而次贪心的选择得到的结果不会优于它。这种情况下局部的最优解才会导致全局最优解。

适用：
* 最优子结构性质：当一个问题的最优解包含其子问题的最优解时。问题能够分解成子问题来解决，子问题的最优解能递推到最终问题的最优解。
* 最优化问题：求图中的最小生成树、求哈夫曼编码、课程表安排

优点：  
* 一旦一个问题可以通过贪心法来解决，那么贪心法一般是解决这个问题的最好办法
* 贪心法的高效性以及其所求得的答案比较接近最优结果  

缺点：
* 对于大部分的问题，贪心法通常都不能找出最佳解，因为它没有测试所有的可能的解。无法解决背包问题，只能近似最优
* 贪心法容易过早做决定，因而没法达到最佳解

区别于动态规划： 
* 贪心算法与动态规划的不同在于它对每个子问题的解决方案都做出选择，不能回退。动态规划则会保存以前的运算结果，并根据以前的结果对当前进行选择，有回退功能。
* 贪心算法通常是自顶向下的，而动态规划通常是自底向上的。
* 工作维度
    * 贪心算法通常工作在一维，因此一个全局最优解通常只有一个局部最优解，这个局部最优解将直接导致全局最优
    * 动态规划通常工作在二维三维，一个全局最优解来自多个局部最优解的推导。
* 一维的动态规划算法可以视为特殊的贪心算法，即贪心算法可以是动态规划算法的一种特例。

* [跳跃游戏](https://leetcode.com/problems/jump-game-ii/)
```C++
// 动态规划
    int jump(vector<int>& nums) {
        
        int n=nums.size();
        vector<int> dp(n,-1);
        dp[0]=0; // 第一格不需要跳跃，步数为0 dp[i]定义为到达i所需最小步数
        for(int i=0;i<n;i++)
        {
            if(dp[i]>-1)
            {
                for(int j=i+1;j<=i+nums[i]&&j<n;j++) // j表示i能跳跃到的位置
                {
                    if(dp[j]==-1)
                        dp[j]=dp[i]+1;
                    else
                        dp[j]=min(dp[j],dp[i]+1);
                }
            }
        }
        return dp[n-1];
    }
// 贪心算法
// 此处贪心策略：从前往后找，找到最前面的j----尽可能贪心地往前查找，使得能从j到达i,这样的步数会更少
int jump(vector<int>& nums) {
        int cnt=0;
        if(nums.size()==1)
            return cnt;
        for(int i=nums.size()-1;i>=0;i--)
        {
            // 到达j的最少步数+1就是到达i的最少步数，从i到j，范围缩小问题变小。有局部的j可以决定全局的i
            for(int j=0;j<i;j++)
            {
                int steps=nums[j];
                int len=i-j;
                if(steps>=len)
                {
                    i=j+1;// j+1的原因是后面还有减1
                    cnt++;
                    break;
                }
            }
        }
        return cnt;
}
```
* [无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)
```C++
// 贪心策略：每一次选择结尾最小的，以给后面留下更广阔的空间，[1,3]和[1,4]我们选择[1,3].
// 如果结尾相同，必然出现重叠，那么选择一个就好，并步影响贪心的选择，因为结尾相同，留给后面的空间是相同的。
// 例如[1,3],[2,3]我们随机选择一个，消除重叠部分
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        int n=intervals.size();
        if(n==0)
            return 0;
        //以结尾的关键字对每一个间隔进行排序，lambda
        sort(intervals.begin(),intervals.end(),[](vector<int> &a,vector<int>&b){ 
            return a[1]<b[1];
        });
        int pre=intervals[0][1];
        int cnt=0;
        for(int i=1;i<n;i++)
        {
            if(intervals[i][0]<pre)
                ++cnt;
            else
                pre=intervals[i][1];
        }
        return cnt;
    }
```