## 题目描述
设计和构建一个“最近最少使用”缓存，该缓存会删除最近最少使用的项目。缓存应该从键映射到值(允许你插入和检索特定键对应的值)，并在初始化时指定最大容量。当缓存被填满时，它应该删除最近最少使用的项目。

它应该支持以下操作： 获取数据 `get` 和 写入数据 `put` 。

获取数据 `get(key)` - 如果密钥 `(key)` 存在于缓存中，则获取密钥的值（总是正数），否则返回 -1。
写入数据 `put(key, value)` - 如果密钥不存在，则写入其数据值。当缓存容量达到上限时，它应该在写入新数据之前删除最近最少使用的数据值，从而为新的数据值留出空间。

示例:
```
LRUCache cache = new LRUCache( 2 /* 缓存容量 */ );

cache.put(1, 1);
cache.put(2, 2);
cache.get(1);       // 返回  1
cache.put(3, 3);    // 该操作会使得密钥 2 作废
cache.get(2);       // 返回 -1 (未找到)
cache.put(4, 4);    // 该操作会使得密钥 1 作废
cache.get(1);       // 返回 -1 (未找到)
cache.get(3);       // 返回  3
cache.get(4);       // 返回  4
```
来源：力扣（LeetCode）  
链接：https://leetcode-cn.com/problems/lru-cache-lcci

## 题解
为了方便索引,使用hashmap+list
```
class LRUCache {
public:
    LRUCache(int capacity) {
        capacity_ = capacity;
    }
    
    int get(int key) {
        if(data_hash_.find(key) == data_hash_.end()) {
            return -1;
        } else {
            update(key);
            return data_hash_[key];
        }
    }
    
    void put(int key, int value) {
        if(data_hash_.find(key) != data_hash_.end()) {
            update(key);
            data_hash_[key] = value;
        } else {
            if(data_hash_.size() == capacity_) {
                remove(list_.front());
                put(key, value);
            } else {
                list_.push_back(key);
                data_hash_[key] = value;
            }
        }
    }

private:
    void update(int key) {
        auto it = find(list_.begin(), list_.end(), key);
        list_.erase(it);
        list_.push_back(key);
    }

    void remove(int key) {
        auto it = find(list_.begin(), list_.end(), key);
        list_.erase(it);
        data_hash_.erase(key);
    }

    list<int> list_;
    unordered_map<int, int> data_hash_;
    int capacity_;
};

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache* obj = new LRUCache(capacity);
 * int param_1 = obj->get(key);
 * obj->put(key,value);
 */
```