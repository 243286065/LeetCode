## 题目简述
有些数的素因子只有 3，5，7，请设计一个算法找出第 k 个数。注意，不是必须有这些素因子，而是必须不包含其他的素因子。例如，前几个数按顺序应该是 1，3，5，7，9，15，21。

示例 1:
```
输入: k = 5

输出: 9
```
来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/get-kth-magic-number-lcci


## 题解
这题除了暴力枚举外,比较好的办法是采用动态规划. 因为符合条件的数组里的数肯定都是3/5/7的倍数,且只含它们的话,那么后面的数其实也就是前面的数去乘以3/5/7得到.

```
class Solution {
public:
    int getKthMagicNumber(int k) {
        //动态规划
        vector<int> res = {1};
        int pos_3 = 0;
        int pos_5 = 0;
        int pos_7 = 0;

        int index = 1;
        while(index++ < k) {
            int next3 = 3 * res[pos_3];
            int next5 = 5 * res[pos_5];
            int next7 = 7 * res[pos_7];
            int min_next = min(min(next3, next5), next7);

            //注意不要使用if else,因为可能存在某几个值相同的情况,需要直接跳过去,否则会有重复的数
            if(next3 == min_next) {
                pos_3++;
            } 
            
            if(next5 == min_next) {
                pos_5++;
            }
            
            if(next7 == min_next){
                pos_7++;
            }

            res.push_back(min_next);
        }

        return res[k-1];
    }
};
```