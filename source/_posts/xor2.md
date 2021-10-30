---
title: xor2
date: 2021-10-30 11:08:25
tags:
---

[只出现一次的数字 III - 只出现一次的数字 III - 力扣（LeetCode） (leetcode-cn.com)](https://leetcode-cn.com/problems/single-number-iii/solution/zhi-chu-xian-yi-ci-de-shu-zi-iii-by-leet-4i8e/)

↑原题链接附上

---

刚开始看到这道题我就知道肯定要用异或，但是却不知道对于得到两个答案的异或值如何处理（~~会，但不完全会~~）......

于是最后还是非常老实地用字典写了......

看了题解后恍然大悟（~~太弱小了我~~)

先附上代码：

```c#
public class Solution {
    public int[] SingleNumber(int[] nums) {
        int xorsum = 0;
        foreach (int num in nums) {
            xorsum ^= num;
        }
        // 防止溢出
        int lsb = (xorsum == int.MinValue ? xorsum : xorsum & (-xorsum));
        int type1 = 0, type2 = 0;
        foreach (int num in nums) {
            if ((num & lsb) != 0) {
                type1 ^= num;
            } else {
                type2 ^= num;
            }
        }
        return new int[]{type1, type2};
    }
}
```

---

我是%%%分割线

---

由于最佳方案要求你使用**常数空间复杂度**，所以用**异或**解题是必须的，问题是得到的双数异或结果怎么处理呢？

这时候，我们就要先回到**异或的定义**上来：同为0，不同为1；

* 同为0的话，似乎思路不甚明晰，答案中只要为0的部分都可以是两个数都为1或者都为0......看来从这个方面突破是行不通的
* 不同为1，这个我们就有想法了。假设答案为a,b,那么我们可以放心令a这一位为1，b这一位为0。这样一来，我们通过这一位是否为0可以恰好把nums分成两类，**而a，b刚好分别在各自的那一类里**，而各自那一类的其他数又都是成对的，这样分别对两组数异或就能分别求得a，b了
* 似乎思路已经很明确了，那么这为1的一位怎么找呢？用10%循环去找吗？如果学过树状数组的话一定会对一个操作不会陌生——x&(-x),它可以取得最低的那一位1+后面的零所表示的数，这样我们就可以以这一位为0,1把nums分成两类了，解决！
