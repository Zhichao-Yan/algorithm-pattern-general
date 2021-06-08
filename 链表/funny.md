## 链表中一些巧妙有趣的题  
*** 
1.[剑指 Offer 35. 复杂链表的复制](https://leetcode-cn.com/problems/fu-za-lian-biao-de-fu-zhi-lcof/)  
**思路：**   
+ 指针p从前往后遍历原链表  
    1. 对原链表对p指向到结点复制一个新结点
    2. 插入到原链表被复制结点p和其下一个结点p->next之间
    3. p=p->next,p指向原链表被复制结点到下一个结点，即新链表复制结点的下一个结点
+ 指针p从前往后遍历新链表  
    1. p指针random指针指向的结点的next指向的结点即为P->next结点的random应该指向的结点
    2. P指向在原链表中的下一个结点，重复1操作  
+ 从新链表中拆分出新复制链表和原链表  
```C++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(head==NULL)
            return NULL;
        Node *p=head;
        while(p)
        {
            Node *q=new Node(p->val);
            q->next=p->next;
            p->next=q;
            p=q->next;
        }
        p=head;
        while(p)
        {   
            if(p->random)
                p->next->random=p->random->next;
            p=p->next->next;
        }
        p=head;
        Node *Cp=p->next;
        Node *q=Cp;
        while(q)
        {
            if(q->next)
            {
                p->next=q->next;
                p=p->next;
                q->next=p->next;
                q=q->next;
            }else{
                p->next=q->next;
                q=q->next;
            }  
        }
        return Cp;
    }
    
};
```
*** 
2. [有序链表转换二叉搜索树](https://leetcode-cn.com/problems/convert-sorted-list-to-binary-search-tree/)  
**思路：**
    + 构造出平衡的二叉树，想办法让根节点左子树中的节点个数与右子树中的节点个数尽可能接近
    + 链表中小于中位数的元素个数与大于中位数的元素个数要么相等，要么相差1 
    + 快慢指针法查找中位数分割链表，中位数构成根结点
    + 分治法对链表中位数之前的数构造根结点的左子树，中位数之后的数构造根结点的右子树

```C++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */

class Solution {
public:
    TreeNode* sortedListToBST(ListNode* head) {
        TreeNode* S=NULL;
        if(head==NULL)
            return S;
        if(head->next==NULL)
        {
            S=new TreeNode(head->val);
            return S;
        }
        if(head->next->next==NULL)
        {
           S=new TreeNode(head->next->val);
           S->left=new TreeNode(head->val);
        }else
        {
            ListNode *fast=head,*slow=head,*pre=NULL;
            while(fast->next&&fast->next->next)
            {
                fast=fast->next->next;
                pre=slow;
                slow=slow->next;
            }
            pre->next=NULL;
            S=new TreeNode(slow->val);
            S->left=sortedListToBST(head);
            S->right=sortedListToBST(slow->next);
        }
        return S;
    }
};
```