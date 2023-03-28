### 分治法
* 分：把一个复杂的问题分成两个或更多的相同或相似的子问题，再把子问题分成更小的子问题……直到最后子问题可以简单的直接求解
* 治：原问题的解即子问题的解的合并 

* 归并排序
```C++
void MergeSort(vector<int> &v)
{
    MSort(v,0,v.size()-1);
}
void MSort(vector<int> &v,int i,int j)
{
    if(i<j){
        int k=(i+j)/2;
        MSort(v,i,k);
        MSort(v,k+1,j);
        Merge(v,i,k,j);
    }
}
void Merge(vector<int> &v,int start,int mid,int end)//归并算法
{
    int n=end-start+1;
    vector<int> t(n);
    int k=0;
    int j=mid+1;
    int i=start;
    for(;i<=mid&&j<=end;++k)
    {
        if(v[i]<=v[j])
        {
            t[k]=v[i++];
        }else{
            t[k]=v[j++];
        }
    }
    while(i<=mid)
    {
        t[k++]=v[i++];
    }
    while(j<=end)
    {
        t[k++]=v[j++];
    }
    for(int i=0;i<n;i++)//将归并后的有序序列重新赋值回去v
    {
        v[start+i]=t[i];
    }
}
```
* 快速排序
```C++
int partition(vector<int> &v,int low,int high)
{
    int pivotkey=v[low];
    while(low<high)
    {
        while(low<high&&v[high]>=pivotkey) --high;
        v[low]=v[high];
        while(low<high&&v[low]<=pivotkey) ++low;
        v[high]=v[low];
    }
    v[low]=pivotkey;
    return low;
}
void QSort(vector<int> &v,int low,int high)
{
    if(low<high)
    {
        int loc=partition(v,low,high);
        QSort(v,low,loc-1);
        QSort(v,loc+1,high);
    }
}
```
* [为运算表达式设计优先级](https://leetcode.cn/problems/different-ways-to-add-parentheses/description/)
```C++
    vector<int> diffWaysToCompute(string expression) {
        vector<int> res;
        for(int i=0;i<expression.size();i++)
        {
            char c=expression[i];
            if(c=='+'||c=='-'||c=='*')
            {
                // 分
                vector<int> left=diffWaysToCompute(expression.substr(0,i));
                vector<int> right=diffWaysToCompute(expression.substr(i+1));
                // 治
                for(auto v1:left)
                {
                    for(auto v2:right)
                    {
                        int sum=0;
                        switch(c){
                            case '+':
                                sum=v1+v2;
                                break;
                            case '-':
                                sum=v1-v2;
                                break;
                            case '*':
                                sum=v1*v2;
                                break;
                        }
                        res.push_back(sum);
                    }
                }
            }
        }
        if(res.empty())// 说明没有运算符，整个字符串是个数字字符串
        {
            res.push_back(stoi(expression));
        }
        return res;
    }
```

* [数组中的逆序对](https://www.nowcoder.com/practice/96bd6684e04a44eb80e6a68efc0ec6c5?tpId=295&tqId=23260&ru=/exam/oj&qru=/ta/format-top101/question-ranking&sourceUrl=%2Fexam%2Foj%3Fpage%3D1%26tab%3D%25E7%25AE%2597%25E6%25B3%2595%25E7%25AF%2587%26topicId%3D295)
```C++
    long long cnt = 0;
    int InversePairs(vector<int> data) {
        if(data.size() < 2)
            return 0;
        mergeSort(data,0,data.size() - 1);
        return cnt % 1000000007;
    }
    void mergeSort(vector<int> &data,int left,int right)
    {
        if(left < right)
        {
            int mid = left + (right - left) / 2;
            mergeSort(data,left,mid);
            mergeSort(data,mid + 1,right);
            Merge(data,left,mid,right);
        }
    }
    void Merge(vector<int> &data,int left,int mid,int right)
    {
        vector<int> temp(right - left +1,0); // O(n)的空间复杂度
        int k = 0;
        int j = left;
        int l = left;
        int r = mid + 1;
        while(l <= mid && r <= right)
        {
            if(data[l] <= data[r])
            {
                temp[k++] = data[l++];
            }else{
                temp[k++] = data[r++];
                // 从l到mid共有mid-l+1个元素大于data[r];
                cnt += mid - l + 1; // 关键点在这！层层交换
            }
        }
        while(l <= mid)
        {
            temp[k++] = data[l++];
        }
        while(r <= right)
        {
            temp[k++] = data[r++];
        }
        for(auto v : temp) // 需要把数据反填回去
        {
            data[j++] = v;
        }
    }
```