layout: post
title: Implementation of Hash Table
description: "哈希表的C++实现"
tags: [cpp]
image:
  feature: abstract-10.jpg
modified: 2023-06-09


---

```cpp

#include <iostream>
#include <vector>

using namespace std;

// 哈希表的节点类型
struct HashNode {
    int key;
    int value;
    HashNode(int k, int v): key(k), value(v) {}
};

// 哈希表的实现
class HashTable {
private:
    vector<vector<HashNode>> table;
    int size;
    int capacity;
    inline unsigned int hashFunc(int key) {
        return key % capacity;
    }

public:
    HashTable(int c): size(0), capacity(c) {
        table.resize(c);
    }

    bool insert(int key, int value) {
        unsigned int h = hashFunc(key);
        for (auto& node : table[h]) {
            if (node.key == key) {
                node.value = value;
                return false;
            }
        }
        table[h].emplace_back(key, value);
        size++;
        return true;
    }

    bool remove(int key) {
        unsigned int h = hashFunc(key);
        for (auto it = table[h].begin(); it != table[h].end(); it++) {
            if (it->key == key) {
                table[h].erase(it);
                size--;
                return true;
            }
        }
        return false;
    }

    int get(int key) {
        unsigned int h = hashFunc(key);
        for (auto& node : table[h]) {
            if (node.key == key) {
                return node.value;
            }
        }
        return -1;
    }

    bool empty() const {
        return size == 0;
    }

    int getSize() const {
        return size;
    }

    void print() const {
        for (const auto& bucket : table) {
            for (const auto& node : bucket) {
                cout << "key: " << node.key << ", value: " << node.value << endl;
            }
        }
    }
};

int main() {
    HashTable hashTable(10);
    hashTable.insert(1, 100);
    hashTable.insert(2, 200);
    hashTable.insert(3, 300);
    hashTable.insert(11, 1100);

    hashTable.print();

    cout << "哈希表大小：" << hashTable.getSize() << endl;

    cout << "查找键值对：key=1, value=" << hashTable.get(1) << endl;
    cout << "查找键值对：key=4, value=" << hashTable.get(4) << endl;

    cout << "删除键值对：key=11, removed=" << hashTable.remove(11) << endl;
    cout << "删除键值对：key=10, removed=" << hashTable.remove(10) << endl;

    hashTable.print();

    cout << "哈希表大小：" << hashTable.getSize() << endl;

    return 0;
}
```

在这个例子中，我们使用`vector<vector<HashNode>>`来实现哈希表，其中第一维表示桶（Bucket），每个桶中存储了哈希值对应的节点。每个节点包含一个key和value，用于存储键值对。哈希表还有一个成员变量size表示哈希表的大小、capacity表示哈希表可以存储的键值对数量。使用类的方式来封装哈希表的实现。变量h表示哈希函数计算得到的哈希值，我们使用除余法（余数为桶的数量）来实现哈希函数。查找、插入和删除键值对根据哈希值来完成操作。