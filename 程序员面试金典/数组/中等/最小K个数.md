## 题目描述
设计一个算法，找出数组中最小的k个数。以任意顺序返回这k个数均可。

示例：
```
输入： arr = [1,3,5,7,2,4,6,8], k = 4
输出： [1,2,3,4]
```
提示：
```
0 <= len(arr) <= 100000
0 <= k <= min(100000, len(arr))
```
来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/smallest-k-lcci

## 题解
Top N问题有很多解法，常见的有：
1. 快排，注意在确定第k个元素时就可以停止排序了；
2. 最大堆/最小堆。

这里我使用一个multiset模拟一个最大堆：
```
class Solution {
public:
    vector<int> smallestK(vector<int>& arr, int k) {
        multiset<int, greater<int>> min_set;

        for(int i = 0; i < arr.size() ;i++) {
            if(min_set.size() < k)
                min_set.insert(arr[i]);
            else {
                if(arr[i] < *(min_set.begin())) {
                    min_set.erase(min_set.begin());
                    min_set.insert(arr[i]);
                }
            }
        }

        vector<int> res(min_set.begin(), min_set.end());
        return res;
    }
};
```

注意求最小的k个数应该是构建最大堆，堆顶是堆中的最大的数；求最大的k个数，应该构建最小堆。