---
date: 2025-08-27
tags:
  - 哈希表
  - 前缀和
  - 数学
  - 数组
  - 枚举
  - 模运算
  - 中等
---
[和可被 K 整除的子数组](https://leetcode.cn/problems/subarray-sums-divisible-by-k/)
## 解题思路

- [[枚举右，维护左]]：结合[[前缀和]]计算公式：`prefix[r + 1] - prefix[l]`，想要得到一个能被 k 整除的子数组和，有两种情况：`prefix[r + 1]`本身能被 k 整除；`prefix[r + 1]`和`prefix[l]`**模 k 的余数相同**（这样它们相减就能把余数去掉），所以我们需要枚举新的[[前缀和]]，然后用[[哈希表]]存储余数相等的子数组的数量

```cpp
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        int ans = 0, sum = 0;
        unordered_map<int, int> cnt;
        for (int& n : nums) {
            sum += n;
            ans += sum % k == 0;
            ans += cnt[sum % k]++;
        }
        return ans;
    }
};
```

然后就错了，因为余数可能是负数，但是只要将余数加上 k 再模 k 就一定是整数了

```cpp
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        int ans = 0, sum = 0;
        unordered_map<int, int> cnt;
        for (int& n : nums) {
            sum += n;
            // 特判当前子数组和本身就能整除 k
            ans += sum % k == 0;
            // 累加并更新哈希表
            ans += cnt[(sum % k + k) % k]++;
        }
        return ans;
    }
};
```

但是超~级~慢，而且空间开销也不小，优化一下：

```cpp
class Solution {
public:
    int subarraysDivByK(vector<int>& nums, int k) {
        int ans = 0, sum = 0;
	    // 用数组代替哈希表，速度更快
        vector<int> cnt(k, 0);
        // 预处理余数为零的情况，就不用每次循环都特判了
        cnt[0] = 1;
        for (int& n : nums) {
            sum += n;
            // 模运算的开销不小，单独声明一个变量
            int remainder = (sum % k + k) % k;
            ans += cnt[remainder]++;
        }
        return ans;
    }
};
```
## 复杂度分析

### 时间复杂度：**O(n)**

其中 n 为数组大小

### 空间复杂度：**O(k)**

k 为给定常数

## 新知识点

[[模运算]]有不小的开销；数组比哈希表快

## 总结

虽然上述代码的复杂度在量级上没有区别，但是细微的优化就能提高很多速度
虽然数组要快很多，但是当 k 很大时，数组的空间开销将会很大，哈希表泛用性更强
