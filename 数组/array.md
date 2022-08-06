# 数组
*[验证回文字符串](https://leetcode.cn/problems/valid-palindrome-ii/)
```C++
class Solution {
public:
    bool Palindrome(string s,int l,int r)//判断一个字符串是不是回文串
    {
        while(l<r)
        {
            if(s[l]==s[r])
            {
                ++l;
                --r;
            }else
                return false;
        }
        return true;
    }
    bool validPalindrome(string s) {
        int i=0,j=s.size()-1;//同时从左边和右边进行逼近
        while(i<j)
        {
            if(s[i]==s[j])//当字符串相等，则++i,--j;
            {
                ++i;
                --j;
            }else{
            //如果出现字符不相等的情况
            //则整个字符串S是否是回文串取决于从i到j的字符串中S[i,j-1]或者S[i+1,j]二者之一是否是回文串,用这个过程模拟删除其中一个不等的字符串
                return Palindrome(s,i+1,j)||Palindrome(s,i,j-1);
            }
        }
        return true;
    }
};
```

