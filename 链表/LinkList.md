# 链表
## 要点
* 求链表最后一个节点
```
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
```
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
```
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
```
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
  