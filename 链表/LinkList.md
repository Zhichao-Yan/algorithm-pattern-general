# 链表
## 要点

### 基础属性
* 求链表最后一个节点
```C
struct ListNode* LastNode(struct ListNode* L)
{
    if(L==NULL)
        return NULL;
    struct ListNode *p=L;
    while(p)
    {
        if(p->next==NULL)
            break;
        else
            p=p->next;
    }
    return p;
}
```
* 求链表长度
```C
int ListLength(struct ListNode*L)
{
    int len=0;
    struct ListNode *p=L;
    while(p)
    {
        len++;
        p=p->next;
    }
    return len;
}
```
* 求链表head顺数第k个节点（链表 从 1 开始索引）
```C
struct ListNode* getForwardsNode(struct ListNode* head, int k){
    int i=1;
    struct ListNode*p=head;
    while(i!=k)
    {
        p=p->next;
        i++;
    }
    return p;
}
```
* 求链表head倒数第k个节点（链表 从 1 开始索引）
```C
struct ListNode* getBackwardsNode(struct ListNode* head, int k){
    int i=ListLength(head); //需要先求链表长度
    struct ListNode*p=head;
    while(i!=k)
    {
        p=p->next;
        i--;
    }
    return p;
}
```

### 技巧型
* 快慢指针问题  
    1. 判断链表中是否有环
    ```C++
    class Solution {
    public:
        bool hasCycle(ListNode *head) {
        ListNode *fast=head;
        ListNode *slow=head;
        int flag=0;
        while(1)
        {   
            if(fast&&fast->next)//fast指针每次走2步
                fast=fast->next->next;
            else
                break;
            //slow每次走一步，不用判断是否为空，因为在同一个链表上有fast趟雷
            slow=slow->next;
            if(fast==slow)
            {
                flag=1;
                break;
            }
        }
        if(flag==1)
            return true;
        return false;
        }
    }; 
    ```
    2. 判断链表中环的入口位置
    ```C++
    class Solution {
    public:
        ListNode *detectCycle(ListNode *head) {
            ListNode *fast=head;
            ListNode *slow=head;
            while(1)
            {   
                if(fast&&fast->next)//fast指针每次走2步
                    fast=fast->next->next;
                else
                    break;
                slow=slow->next;//slow每次走一步，不用判断是否为空，因为在同一个链表上有fast趟雷
                if(fast==slow)//fast和slow第一次相遇的位置
                {
                    fast=head;//让fast重新回到head,速度每次走一步
                    while(fast!=slow)
                    {
                        fast=fast->next;
                        slow=slow->next;
                    }
                    return fast;
                }
            }
            return NULL;
        }
    };
    ```
    3. 删除链表倒数第n个结点
    ```C++
    class Solution {
    public:
        ListNode* removeNthFromEnd(ListNode* head, int n) {
            ListNode* fast=head,*slow=head;
            int i=1;
            while(i<=n)
            {
                fast=fast->next;
                i++;
            }
            //因为n一定有效，但n等于链表长度时，说明删除倒数第n个结点，也即顺数第一个结点，
            if(fast==NULL)
            {
                head=head->next;
            }else{
                //当fast指向最后一个结点时，slow指向倒数第n个结点的前驱
                while(fast->next)
                {
                    fast=fast->next;
                    slow=slow->next;
                }
                slow->next=slow->next->next;
            }
        return head;
        }
    };
    ```
    4. 返回链表中间结点(奇数个返回中间结点，偶数个返回第n/2个结点)
    ```C++
    ListNode* MidListNode(ListNode* head)
    {
        ListNode *fast=head,*slow=head;
        while(fast->next&&fast->next->next)
        {
            fast=fast->next->next;
            slow=slow->next;
        }
        return slow;
    }
    ```
    
* 二叉树相关  
[例题：有序表转换成二叉树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)   
[例题：二叉树中的列表](https://leetcode-cn.com/problems/linked-list-in-binary-tree/) 

* 浅拷贝与深拷贝问题  
[例题：复制带随机指针的链表](https://leetcode-cn.com/problems/copy-list-with-random-pointer/)  

* 加头结点便于删除操作  
[例题：从链表中删去总和值为0的连续节点](https://leetcode-cn.com/problems/remove-zero-sum-consecutive-nodes-from-linked-list/)

在某些条件情况下，我们需要删除链表的首节点，并且返回链表指针。这与我们删除非首节点的操作不太相同。因为链表的首节点没有前驱节点。因此为了操作的统一和通用性，给链表加上一个pre节点，使原链表的每一个节点都有前驱节点
```C
struct ListNode* removeZeroSumSublists(struct ListNode* head){
    struct ListNode *pre=(struct ListNode*)malloc(sizeof(struct ListNode));
    pre->next=head;
    pre->val=0;//给链表构造一个头结点，便于删除节点后返回
    struct ListNode *p=pre;
    while(p)
    {
        struct ListNode *q=p->next;
        int sum=0;
        while(q)
        {
            sum+=q->val;
            if(sum==0)
                p->next=q->next;
            q=q->next;
        }
        p=p->next;
    }
    return pre->next;
}
```  

* 翻转一个链表 
```C
//第一种方法
struct ListNode* ReverseList(struct ListNode* pHead ) {
    // write code here
    struct ListNode*head;
    head=NULL;
    while(pHead)
    {
        struct ListNode*n=pHead;
        pHead=pHead->next;
        n->next=head;
        head=n;
    }
    return head;
}   
```
***
```C++
//第二种方法 (也算头插法一下子难明白,不推荐)
ListNode* ReverseList(ListNode*head)
{
    ListNode *pre=new ListNode(-1);
    pre->next=head;
    while(head->next)
    {
         ListNode *next=head->next;
         head->next=next->next;
         next->next=pre->next;
         pre->next=next;
    }
    return pre->next;
}
```
```C++
//第三种方法--头插法 简洁明了推荐++
ListNode* ReverseList(ListNode*head)
{
    ListNode *pre=new ListNode(-1);
    pre->next=NULL;
    while(head)
    {
        ListNode *temp=head;
        head=head->next;
        temp->next=pre->next;
        pre->next=temp;
    }
    return pre->next;
}
```

* 合并2个有序链表
```C++
class Solution {
public:
    /**
     * 
     * @param l1 ListNode类 
     * @param l2 ListNode类 
     * @return ListNode类
     */
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        if(l1==NULL)
            return l2;
        if(l2==NULL)
            return l1;
        ListNode *head=NULL,*tail=NULL,*S=NULL;
        while(l1&&l2)
        {   
            if(l1->val<l2->val)
            {
                S=l1;
                l1=l1->next;
            }else{
                S=l2;
                l2=l2->next;
            }
            S->next=NULL;
            if(head==NULL)
            {
                head=S;
                tail=S;
            }else{
                tail->next=S;
                tail=S;
            }
        }
        if(l1)
            tail->next=l1;
        if(l2)
            tail->next=l2;
        return head;
        
    }
};
```
* 单链表逆序遍历
```C++
void printListNode(ListNode *head)
{
    if(head==NULL)
        return NULL;
    printListNode(head->next);
    cout<< head->val;
    return;
}
```