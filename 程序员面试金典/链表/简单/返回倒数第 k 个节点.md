### 题目描述
实现一种算法，找出单向链表中倒数第 k 个节点。返回该节点的值。

注意：本题相对原题稍作改动

示例：
```
输入： 1->2->3->4->5 和 k = 2
输出： 4
```
说明：
给定的 k 保证是有效的。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/kth-node-from-end-of-list-lcci

### 题解
这题一开始的想法是两次遍历，第一次遍历先得到链表的长度len；第二次从头遍历，走len-k步即到达所需要取值的节点。  
其实更简单的做法是使用双指针，一个指针先走k步，再两个指针一起往后走，当先走的那个指针达到null节点，说明后出发的节点到达了需要取值的点。
```
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    int kthToLast(ListNode* head, int k) {
        ListNode* fast = head;
        //双指针，第一个指针先走k步
        while(k--) {
            fast = fast->next;
        }

        ListNode* slow = head;
        while(fast) {
            fast = fast->next;
            slow = slow->next;
        }
        return slow->val;
    }
};
```