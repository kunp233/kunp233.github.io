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

