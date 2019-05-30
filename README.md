## 动态规划算法————从斐波那契数列说起

> 斐波那契数列是这样一个数列，F(0)=0，F(1)=1,F(n)= F(n-1) + F(n-2)。

求斐波那契数列第n项的值，用递归很容易做到。
```java
public int fib(int n) {
    if(n <= 1) return n;
    return fib(n-1) + fib(n-2);
}
```
递归解法看似能得到正确的结果，其实效率很低。原因在于递归过程中有很多重复的计算。

如图，计算fib(8)时计算fib(7)，fib(9)时也计算了fib(7)，fib(9)计算fib(8)时又重复计算了fib(7).
从图中可以看出，每个节点都有多次重复，所以递归用来解斐波那契不是一个好的办法。
如何优化？
 * 可以记录`fib(n)`的值，遇到这个值时直接去查找。
 * 自底向上求解。

按照这个思路，新写一个解法：
```java
// 用数组记录每一个子问题的值
public static int fib(int n) {
    int[] arr = new int[n + 1];
    for (int i = 0; i <= n; i++) {
        if (i <= 1) {
            // 第0项为0，第1项为1，这里偷个懒
            arr[i] = i;
            continue;
        }
        arr[i] = arr[i - 1] + arr[i - 2];
    }
    return arr[n];
}

// 继续优化，计算fib(n)只需要知道fib(n-1)和fib(n-2)，所以无需用数组
public static int fib(int n) {
    if (n <= 1) return n;
    int prePre = 0;
    int pre = 1;
    int result = 0;
    for (int i = 2; i <= n; i++) {
        result = pre + prePre;
        prePre = pre;
        pre = result;
    }
    return result;
}

// 上面的算法用到了3个变量，还可以继续优化，只需2个变量即可计算
public static int fib(int n) {
    int pre = 0, next = 1;
    while (n-- > 0) {
        next += pre;
        pre = next - pre;
    }
    return pre;
}
```
恭喜你，已经入门了动态规划的精髓！
