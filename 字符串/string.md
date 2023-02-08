### 字符串编程常用函数
1. 字符串输入
    > cin>>str;//遇到空白停止   
    > getline(cin,str);//遇到换行符停，读取空白
1. 字符串构造函数
    * string s(str, str_begin,str_len)
        > string str1="Hello World!";
        > string str3("12345", 0, 3);  //结果为"123"
    * string s(cstr,char_len)
        > char alls[] = "All's well that ends well";
	    > string str7(alls,10);//结果为 All's well
    * string s(str,str_index)
        > str1 = "Hello World!";
        > string str6(str1, 6);//结果为"World!"
2. 字符串增删操作函数
    * 增加操作
        > push_back()  
        > insert(p,t)
        > insert(p,n,t)
        > insert(p,b,e)
    * 删除操作
        > pop_back()  
        > c.erase(p)/c.erase(b,e)  
        > c.clear()  
        > string& erase(size_t pos, size_t len)
    * 拼接操作
        > string s1("abc");s1.append("def");
        > s1+=s2
3. 字符串遍历方法
    1. 迭代器
        * 正向迭代器 
            > str.begin()、str.end()、str.cbegin()、str.cend()
        * 反向迭代器 
            > str.rbegin()、str.rend()
    2. 范围for语句
        > for(auto &c:str)
    3. 下标范围
        > for(int i=0;i<str.size()-1;++i)
4. 字符大小写变化
    > tolower(char)
    > toupper(char)
5. 字符串替换
    * string& replace(size_t pos, size_t n, const char s)  
        > string s1("hello,world!");
        > s1.replace(5,6,"girl");// 结果：hello,girl.  
    * string& replace(iterator i1, iterator i2, const char s)    
        > s1.replace(s1.begin(), s1.begin() + 5, "boy");  // 结果：boy,girl.
6. 字符串查找
    * find(const string & str,size_type pos = 0) const;
        > 在当前字符串的pos索引位置开始，查找子串str，返回子字符串首次出现时其首字符的索引。
    * find (const char* s, size_t pos=0)
        > 在当前字符串的pos索引位置开始，查找子串s，返回找到的位置索引
    * find (char c, size_t pos=0)
        > 在当前字符串的pos索引位置开始，查找字符c，返回该字符首次出现的位置索引
    * rfind()
        > 查找字符串或者字符最后一次出现的位置，也就是从后往前查找,与find()相反。
8. 字符串提取字串
    * substr(begin_pos,sub_length);
10. 字符与数字转换 
    * 从字串到数值 stoi,stol,stod,stof
        > int stoi( const std::string& str, std::size_t* pos = 0, int base = 10 );
    * 从数值到字符 to_string
### 反转字符串
1. [仅仅反转字母](https://leetcode.cn/problems/reverse-only-letters/)
```C++
    string reverseOnlyLetters(string s) {
        string re;
        for(int i=0;i<s.size();i++)//先将字母提取出来，并且反转统一放入字符串re
        {
            if(isalpha(s[i]))
            {
                re=s[i]+re;
            }
        }
        for(int i=0,j=0;i<s.size();i++)//将字符串re字符按顺序依次填入s中英文字母的位置
        {
            if(isalpha(s[i]))
            {
                s[i]=re[j];
                ++j;
            }            
        }
        return s;
    }
```

