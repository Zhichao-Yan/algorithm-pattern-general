# 链表
## 要点
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
* 快慢指针问题 
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
