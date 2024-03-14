* [栈的使用](https://www.nowcoder.com/practice/95cb356556cf430f912e7bdf1bc2ec8f?tpId=196&tqId=37173&rp=1&ru=/exam/oj&qru=/exam/oj&sourceUrl=%2Fexam%2Foj%3Fdifficulty%3D2%26page%3D1%26pageSize%3D50%26search%3D%26tab%3D%25E7%25AE%2597%25E6%25B3%2595%25E7%25AF%2587%26topicId%3D196&difficulty=3&judgeStatus=undefined&tags=590&title=)
```C++
    vector<int> solve(vector<int>& a) {
        // write code here
        
        vector<int> mx(a.size(),0);
        for(int i = a.size() - 2; i >= 0; --i)
        {
            // mx[i] 表示数组a中从a[i + 1]～a[a.size() - 1]的最大值
            mx[i] = max(mx[i + 1],a[i + 1]); 
        }
       // 定义一个栈
        stack<int> sk;
        // 定义一个结果数组
        vector<int> res;
        // 遍历数组a
        for(int i = 0; i < a.size(); ++i)
        {
            // 将a中的元素压入栈中
            sk.push(a[i]);
            // 当栈不为空，且栈顶元素大于mx[i]时，将栈顶元素压入res中
            while(!sk.empty() && sk.top() > mx[i])
            {
                res.push_back(sk.top());
                sk.pop();
            }
        }
        // 当栈不为空时，将栈顶元素压入res中
        while(!sk.empty())
        {
            res.push_back(sk.top());
            sk.pop();
        }
        // 返回res
        return res;
    }
```
