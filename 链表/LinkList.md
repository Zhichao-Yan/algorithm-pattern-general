# 链表
## 要点
* 求链表最后一个节点
```
struct ListNode* LastNode(struct ListNode*L)
{
    if(L==NULL)
        return NULL;
    struct ListNode *p=L;
    while(p)
    {
        if(p->next==NULL)
            break;
    }
    return p;
}
```
* 求链表长度
```
int LengthOfList(struct ListNode*L)
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
* 快慢指针问题
  