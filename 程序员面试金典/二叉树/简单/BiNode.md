##  题目描述
二叉树数据结构TreeNode可用来表示单向链表（其中left置空，right为下一个链表节点）。实现一个方法，把二叉搜索树转换为单向链表，要求值的顺序保持不变，转换操作应是原址的，也就是在原始的二叉搜索树上直接修改。

返回转换后的单向链表的头节点。

注意：本题相对原题稍作改动

 

示例：
```
输入： [4,2,5,1,3,null,6,0]
输出： [0,null,1,null,2,null,3,null,4,null,5,null,6]
```

提示：

节点数量不会超过 100000。

来源：力扣（LeetCode）
链接：https://leetcode-cn.com/problems/binode-lcci  

## 题解
二叉树中序遍历，注意要处理好left节点为NULL,否则会轻易导致超时，因为修改结构后形成了循环。
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* convertBiNode(TreeNode* root) {
        //中序遍历的结果就是链表的顺序
        // 非递归中序遍历
        // 使用栈
        if(root==NULL) return NULL;
        stack<TreeNode*> st;
        TreeNode* head = NULL;
        TreeNode* curr = head;
        TreeNode* node = root;
        
        while(!st.empty() || node != NULL) {
            //将左节点一直进栈
            while(node) {
                st.push(node);
                node = node->left;
            }

            if(!st.empty()) {
                node  = st.top();
                st.pop();
                node->left = NULL;
                if(head == NULL) {
                    head = node;
                    curr = head;
                }
                else {
                    curr->right = node;
                    curr= curr->right;
                }

                node = node->right;
            }
        }

        return head;
    }
};
```