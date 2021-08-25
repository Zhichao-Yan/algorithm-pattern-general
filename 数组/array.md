# 数组
* [旋转数组](https://www.nowcoder.com/practice/e19927a8fd5d477794dac67096862042?tpId=117&&tqId=37834&rp=1&ru=/activity/oj&qru=/ta/job-code-high/question-ranking)  
### 思路： 
1. 旋转后面m个元素
2. 旋转前面n-m个元素
3. 旋转整个数组
```C++
class Solution {
public:
    /**
     * 旋转数组
     * @param n int整型 数组长度
     * @param m int整型 右移距离
     * @param a int整型vector 给定数组
     * @return int整型vector
     */
    vector<int> solve(int n, int m, vector<int>& a) {
        // write code here
        m=m%n;
        if(m==0)
            return a;
        reverse(a.begin(), a.end()-m);
        reverse(a.end()-m, a.end());
        reverse(a.begin(),a.end());
        return a;
    }
};
```
