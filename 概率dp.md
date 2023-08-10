# 概率DP\期望DP

---

## 概念

### 概率运算

加法：`P(A∪B) = P(A) + P(B) - P(A ∩ B)`，若两事件**互斥**，则有 `P(A∪B) = P(A) + P(B)`

乘法：`P(A ∩ B) = P(A) * P(B)`

### **数学期望**

* **定义**：期望的意义就是**概率**的**加权平均数**——假设某随机试验 X 共有 n 种互斥的事件可能发生，其中第 i 个事件发生的概率为 `Pi`，价值为 `Xi`，则这个随机试验的期望是
  $$
  E(X) = \sum_i{p_i * x_i}
  $$
  期望也可以从**频率**的角度来理解，我们知道如果不断重复某个随机试验，某个事件发生的频率会趋近于其概率，而将发生过所有事件的价值取平均值，这个值就会趋近于这个随机试验的期望。

* **常见线性运算性质**：

  1. `E(X + Y) = E(X) + E(Y)`
  2. `E(aX) = aE(X)`
  3. `E(XY) = E(X) * E(Y)`
  4. 方差计算 `Var(X) = E(X^2) - [E(X)]^2`

### 概率DP\期望DP

内容上：实际上这类动态规划并不是一个新的类型，它都是在原有的动态规划的基础上，将所求的值改成了**概率**或**期望**的相关值。

解法上：大多数情况下只需要从题干中**选择可供转移的状态（有时会搭配状态压缩）**并**推导或化简出对应转移方程**。

* 概率DP：一般情况下，初始状态已知，故直接顺推即可。
* 期望DP：因为一般情况下期望值的初始状态未知，以**最终状态**为初始状态转移，即逆推。如`f[i]`表示期望还要走`f[i]`步到达终点。一般来说，初始状态确定时可用**顺推**（`dp[n]` 为答案），终止状态确定时可用**逆推**（`dp[0]` 为答案）。也有的时候已知每个样本对期望值的具体贡献量 Xi ，可以直接进行概率DP得到每个样本的贡献概率 Pi，最后直接线性统计 `∑XiPi` 即可。

难度上：代码量一般较少，但是考验思维深度和公式推演能力。概率DP可以和很多其他DP类型挂钩，比如背包，树形，状压等等。

### 循环转移处理方法

#### 代换系数法

设每个状态（假设每个状态都与 `dp[0]` 相关）为形如 
$$
dp[u]=a[u]*dp[fa]+b[u]*dp[0]+c[u]
$$
然后求出所有系数 `a[u]，b[u]，c[u]`



#### 高斯消元法

高斯消元是一种解线性方程组的方法，包括求解 n 元一次方程组。列出关于 n 个状态的 n 个方程往往可以直接解得目标状态的值，于是把 n 个**本质不同**（或者说线性无关）的状态转移方程列作方程组，用高斯消元求解即可解决一类**DP序混乱**的概率/期望DP。具体原理见**线性代数**。

理解性模板如下，原文链接https://blog.csdn.net/tianyuanzhidian/article/details/109015522：

```c++
int a[maxn][maxn];  //增广矩阵
int x[maxn];        //解集
bool freeX[maxn];   //标记解是否是自由变元
 
int gcd(int a, int b)
{
    return b ? gcd(b, a % b) : a;
}
 
int lcm(int a, int b)
{
    return a / gcd(a, b) * b;
}
 
//高斯消元解方程组
//返回值 -2 表示有浮点数解，无整数解
//返回值 -1 表示无解，0 表示有唯一解，大于 0 表示有无穷解，返回自由变元个数
//有equ个方程，var个变元
//增广矩阵行数[0, equ - 1]
//增广矩阵列数[0, var]
int gauss(int equ, int var)
{
    for (int i = 0; i <= var; i++)
    {
        x[i] = 0;
        freeX[i] = true;
    }
    //转换为阶梯矩阵
    //col表示当前正在处理的这一列
    int col = 0;
    int row = 0;
    //maxR表示当前这个列中元素绝对值最大的行
    int maxRow;
    for (; row < equ && col < var; row++, col++)
    {
        //枚举当前正在处理的行
        //找到该col列元素绝对值最大的那行与第k行交换
        maxRow = row;
        for (int i = row + 1; i < equ; i++)
        {
            if (abs(a[maxRow][col]) < abs(a[i][col]))
            {
                maxRow = i;
            }
        }
        if (maxRow != row)
        {
            //与第row行交换
            for (int j = row; j < var + 1; j++)
            {
                swap(a[row][j], a[maxRow][j]);
            }
        }
        if (a[row][col] == 0)
        {
            //说明该col列第row行以下全是0，处理当前行的下一列
            row--;
            continue;
        }
        for (int i = row + 1; i < equ; i++)
        {
            //枚举要删的行
            if (a[i][col] != 0)
            {
                int LCM = lcm(abs(a[i][col]), abs(a[row][col]));
                int ta = LCM / abs(a[i][col]);
                int tb = LCM / abs(a[row][col]);
                //异号
                if (a[i][col] * a[row][col] < 0)
                    tb = -tb;
                for (int j = col; j < var + 1; j++)
                {
                    a[i][j] = a[i][j] * ta - a[row][j] * tb;
                }
            }
        }
    }
 
    //1. 无解的情况: 化简的增广阵中存在(0, 0, ..., a)这样的行(a != 0).
    for (int i = row; i < equ; i++)
    {
        if (a[i][col] != 0)
        {
            return -1;
        }
    }
 
    // 2. 无穷解的情况: 在var * (var + 1)的增广阵中出现(0, 0, ..., 0)这样的行，即说明没有形成严格的上三角阵.
    //                  出现的行数即为自由变元的个数.
    if (row < var)
    {
        // 首先，自由变元有var - k个，即不确定的变元至少有var - k个.
        for (int i = row - 1; i >= 0; i--)
        {
            // 第i行一定不会是(0, 0, ..., 0)的情况，因为这样的行是在第k行到第equ行.
            // 同样，第i行一定不会是(0, 0, ..., a), a != 0的情况，这样的无解的.
            // freeNum用于判断该行中的不确定的变元的个数，如果超过1个，则无法求解，它们仍然为不确定的变元.
            int freeNum = 0;
            int freeIndex = 0;
            for (int j = 0; j < var; j++)
            {
                if (a[i][j] != 0 && freeX[j])
                {
                    freeNum++;
                    freeIndex = j;
                }
            }
            if (1 < freeNum)// 无法求解出确定的变元.
                continue;
            // 说明就只有一个不确定的变元freeIndex，那么可以求解出该变元，且该变元是确定的.
            int tmp = a[i][var];
            for (int j = 0; j < var; j++)
            {
                if (a[i][j] != 0 && j != freeIndex)
                {
                    tmp -= a[i][j] * x[j];
                }
            }
            x[freeIndex] = tmp / a[i][freeIndex];
            freeX[freeIndex] = false;
        }
        return var - row;
    }
    // 3. 唯一解的情况: 在var * (var + 1)的增广阵中形成严格的上三角阵.
    // 计算出Xn-1, Xn-2 ... X0.
    for (int i = var - 1; i >= 0; i--)
    {
        int tmp = a[i][var];
        for (int j = i + 1; j < var; j++)
        {
            if (a[i][j] != 0)
            {
                tmp -= a[i][j] * x[j];
            }
        }
        if (tmp % a[i][i] != 0)//浮点数
            return -2;
        x[i] = tmp / a[i][i];
    }
    return 0;
}
```



## 理解入门

以 [Kids and Prizes](https://codeforces.com/problemsets/acmsguru/problem/99999/495) 为例：

题意：n 个盒子里面装着奖品，m 个人随机选择盒子（对每个盒子的选择是等概率的 1 / n），选择后只取出奖品并放回空盒子（总盒子数不变），问得到的奖品数量的数学期望值。

* 定义 `dp[i]` 表示第 i 个人中奖的概率，思路是先通过概率DP求出每个人中奖的概率（即所得奖品数最多为 m 个）再统计数学期望，且每个人最多得到 1 个奖品，故 `E(X) = ∑(1 * dp[i])` 

*  第 i 个人中奖的状态来源：

  1. 第 i - 1 个人没中奖（概率为 `1 - dp[i - 1]`），也就是中奖概率不变，第 i 个人与第 i - 1 个人中奖概率都为 `dp[i - 1]`，故 `P1(第 i 个人中奖) = (1 - dp[i - 1]) * dp[i - 1]` 
  2. 第 i - 1 个人中奖了（概率为 `dp[i - 1]`），中奖概率线性减少 `1 / n`，则第 i 个人中奖概率为 `dp[i - 1] - 1.0 / n`，故 `P2(第 i 个人中奖) = dp[i - 1] * (dp[i - 1] - 1.0 / n)`

  最终得到概率DP转移方程： `dp[i] = (1 - dp[i - 1]) * dp[i - 1] + dp[i - 1] * (dp[i - 1] - 1.0 / n)`

```C++
#include <iostream>
#include <iomanip>
#include <algorithm>
#include <cstring>
using namespace std;
#define ll long long
const double eps = 1e-9;
const int N = 1e5 + 10;
double dp[N];

int main()
{
    int n, m;
    while(cin >> n >> m)
    {
        dp[1] = 1; // 此时每个盒子都有奖品，故初始必中奖
        for(int i = 2; i <= m; ++i)
        {
            dp[i] = (1 - dp[i - 1]) * dp[i - 1] + dp[i - 1] * (dp[i - 1] - 1.0 / n);
        }
        double ans = 0;
        for(int i = 1; i <= m; ++i) ans += 1 * dp[i]; // 累加期望值
        cout << fixed << setprecision(10) << ans << "\n";
    }
    return 0;
}
```





## 例题

| 序号 | 题号        | 标题                                                         | 题型                     | 难度评级 |
| ---- | ----------- | :----------------------------------------------------------- | ------------------------ | -------- |
| 1    | sgu(cf) 495 | [Kids and Prizes](https://codeforces.com/problemsets/acmsguru/problem/99999/495) | 概率DP + 求期望          | ⭐        |
| 2    | cf 148D     | [Bag of mice](https://codeforces.com/problemset/problem/148/D) | 概率DP                   | ⭐⭐       |
| 3    | hdu 4576    | [Robot](https://acm.hdu.edu.cn/showproblem.php?pid=4576)     | 概率DP                   | ⭐⭐⭐      |
| 4    | poj 3071    | [Football](http://poj.org/problem?id=3071)                   | 概率DP                   | ⭐⭐⭐      |
| 5    | hdu 3366    | [Passage](https://acm.hdu.edu.cn/showproblem.php?pid=3366)   | 概率DP                   | ⭐⭐⭐      |
| 6    | poj 2096    | [Collecting Bugs](http://poj.org/problem?id=2096)            | 期望DP                   | ⭐⭐       |
| 7    | hdu 3853    | [LOOPS](https://acm.hdu.edu.cn/showproblem.php?pid=3853)     | 期望DP                   | ⭐⭐       |
| 8    | zoj 3640    | [Help Me Escape](https://zoj.pintia.cn/problem-sets/91827364500/problems/91827369307) | 期望DP + 记忆化搜索      | ⭐⭐⭐      |
| 9    | zoj 3329    | [One Person Game](https://pintia.cn/problem-sets/91827364500/exam/problems/91827368253?type=7&page=23) | 期望公式推导(代换系数法) | ⭐⭐⭐⭐     |
| 10   | hdu 4870    | [Rating](http://acm.hdu.edu.cn/showproblem.php?pid=4870)     | 高斯消元法/递推法        | ⭐⭐⭐⭐⭐    |



---

## 题解

### 1. [Kids and Prizes](https://codeforces.com/problemsets/acmsguru/problem/99999/495)

**题意**

n 个盒子里面装着奖品，m 个人随机选择盒子（对每个盒子的选择是等概率的 1 / n），选择后只取出奖品并放回空盒子（总盒子数不变），问得到的奖品数量的数学期望值。

**解法**

定义 `dp[i]` 表示第 i 个人中奖的概率，思路是先通过概率 DP 求出每个人中奖的概率（即所得奖品数最多为 m 个）再统计数学期望，且每个人最多得到 1 个奖品，故 `E(X) = ∑(1 * dp[i])` 

第 i 个人中奖的状态来源：

1. 第 i - 1 个人没中奖（概率为 `1 - dp[i - 1]`），也就是中奖概率不变，第 i 个人与第 i - 1 个人中奖概率都为 `dp[i - 1]`，故 `P1(第 i 个人中奖) = (1 - dp[i - 1]) * dp[i - 1]` 
2. 第 i - 1 个人中奖了（概率为 `dp[i - 1]`），中奖概率线性减少 `1 / n`，则第 i 个人中奖概率为 `dp[i - 1] - 1.0 / n`，故 `P2(第 i 个人中奖) = dp[i - 1] * (dp[i - 1] - 1.0 / n)`

```c++
#include <iostream>
#include <iomanip>
#include <algorithm>
#include <cstring>
using namespace std;
#define ll long long
const double eps = 1e-9;
const int N = 1e5 + 10;
double dp[N];

int main()
{
    int n, m;
    while(cin >> n >> m)
    {
        dp[1] = 1; // 此时每个盒子都有奖品，故初始必中奖
        for(int i = 2; i <= m; ++i)
        {
            dp[i] = (1 - dp[i - 1]) * dp[i - 1] + dp[i - 1] * (dp[i - 1] - 1.0 / n);
        }
        double ans = 0;
        for(int i = 1; i <= m; ++i) ans += 1 * dp[i]; // 累加期望值
        cout << fixed << setprecision(10) << ans << "\n";
    }
    return 0;
}
```



### 2.  [Bag of mice](https://codeforces.com/problemset/problem/148/D)

**题意**

给定 n 个白鼠和 m 个黑鼠，公主和龙轮流抓老鼠，公主先抓。每一轮，公主会抓出一只老鼠（但不会有其他老鼠溜出），随后龙也会只抓一只老鼠但会不小心放走一只老鼠，显然每轮将选出 3 只老鼠。先抓到白鼠方赢，若两个人都没有抓到白色老鼠则龙赢。求有 n 个白鼠和 m 个黑鼠的情况下，公主胜利的概率。

**解法**

定义`dp[i][j]` 表示还剩下 i 只白鼠和 j 只黑鼠时公主胜利的概率（公主先手）。
结合题中要求，得出以下四种情况，

1. 先手白鼠，直接赢：`i / (i + j)`

2. 先手黑鼠、后手白鼠，直接输：`0` 

3. 先手黑鼠、后手黑鼠、放走白鼠，继承赢的情况：

    `dp[i - 1][j - 2] ：j / (i + j)  *  (j - 1) / (i + j - 1)  *  i / (i + j - 2)  *  dp[i - 1][j - 2]`

4. 先手黑鼠、后手黑鼠、放走黑鼠，继承赢的情况 ：

   `dp[i][j - 3] : j / (i + j)  *  (j - 1) / (i + j - 1)  *  (j - 2) / (i + j - 2)  *  dp[i][j - 3]`

```c++

#include <bits/stdc++.h>
using namespace std;
#define ll long long
const double eps = 1e-9;
const int N = 1e3 + 10;
double dp[N][N]; 

int main()
{
    int n, m;
    cin >> n >> m;
    for(int i = 1; i <= n; ++i) dp[i][0] = 1; // 初始化先手必胜的情况
    for(int i = 1; i <= n; ++i)
    {
        for(int j = 1; j <= m; ++j)
        {
            dp[i][j] = 1.0 * i / (i + j);
            if(j >= 2) dp[i][j] += 1.0  *  j / (i + j)  *  (j - 1) / (i + j - 1)  *  i / (i + j - 2)  *  dp[i - 1][j - 2];
            if(j >= 3) dp[i][j] += 1.0  *  j / (i + j)  *  (j - 1) / (i + j - 1)  *  (j - 2) / (i + j - 2)  *  dp[i][j - 3];
        }
    }
    cout << fixed << setprecision(9) << dp[n][m];
    return 0;
}
```



### 3. [Robot](https://acm.hdu.edu.cn/showproblem.php?pid=4576)

**题意**

有编号 1 ~ n 的格子围成一个圈，一开始机器人站在 1 号格子，有 m 个命令，每个命令机器人走 w 格，方向随机为顺时针或逆时针，概率均为 0.5。执行 m 个命令后，问机器人站在格子 `[l, r]` 上的总概率是多少。

**解法**

定义 `dp[i][j]` 为第 i 个命令后机器人站在位置 j 的概率，但是 m 非常大，所以用滚动数组优化即定义 `dp[2][j]` 只需要有 上一个命令`dp[now^1][j]` 和 当前命令`dp[now][j]` 的状态即可。

初始状态已知（初始必站在编号 1 上），故直接顺推下去（**刷表法**），分别转移两种方向的DP状态，最后暴力枚举 `[l, r]` 上所有格子并累和所有概率。

```c++
// 本题非常容易 TLE，需注意各种卡常细节。
#include <cstdio>
#include <iostream>
#include <iomanip>
using namespace std;
#define ll long long
const double eps = 1e-9;
const int N = 505;
double dp[2][N];

int main()
{
    int n, m, l, r, w;
    while(~scanf("%d%d%d%d", &n, &m, &l, &r))
    {
        if(n == 0 && m == 0 && l == 0 && r == 0) break;
        for(int i = 0; i < n; ++i) dp[0][i] = 0;                  // *编号设为 0 ~ (n - 1) 方便计算并避免 TLE
        dp[0][0] = 1;
        int now = 0;
        while(m--)
        {
            scanf("%d", &w);
            now ^= 1;
            for(int i = 0; i < n; ++i) dp[now][i] = 0;
            for(int i = 0; i < n; ++i)
            {
                if(dp[now ^ 1][i] == 0) continue;                 // *减少计算，否则TLE
                dp[now][(i + w) % n] += 0.5 * dp[now ^ 1][i];     // 顺时针
                dp[now][(i - w + n) % n] += 0.5 * dp[now ^ 1][i]; // 逆时针
            }
        }
        double ans = 0;
        --l, --r;
        while(l <= r) ans += dp[now][l++];						// 暴力求和即可
        printf("%.4lf\n", ans);
    }
    return 0;
}
```



### 4.   [Football](http://poj.org/problem?id=3071)

**题意**

2 ^ n 个队伍进行 n 场足球赛，给定每个队 i 打败其他队伍 j 的概率 `p[i][j]`，问 n 场比赛后胜利的概率最大的球队是哪支。

**解法**

定义 `dp[i][j]` 为进行第 i 场比赛时队伍 j 胜出的概率，记队伍 j 在第 i 轮比赛中赢过队伍 k，自然地，队伍 j、k 都将在上一轮获胜。转移方程：`dp[i][j] += dp[i - 1][j] * dp[i - 1][k] * p[j][k]`。

比赛进程图类似于一个倒二叉树（根为冠军队伍），如 8 支队伍：

1. 起初两两相邻配对分四组 `(1, 2) (3, 4) (5, 6) (7, 8)`，分别决出 4 支胜者队伍，
2. 设胜出队伍为 1、3、5、7，他们又两两分组，`(1, 3) (5, 7)` 决出两支队伍，最终决出冠军。

上述步骤 2 中，我们设第 1 组的胜者为队伍 1，而他将有概率与第二组的 3 或 4 比赛，那么组 1 和组 2 决出的决赛队伍 x 将可能与 5、6、7、8 其中一支队伍比赛。

本题难点，如何枚举 k：先得到 j 在该轮比赛中所在组数 `grpj = (j - 1) >> (i - 1)` （从 0 开始编号），然后得到相邻位置的对手在该层的组数 `grpk = ((j - 1) >> (i - 1)) ^ 1`，最后枚举所有可能进入组 `grpk` 的队伍 k 即可。

```c++
#include <iostream>
#include <cstring>
using namespace std;
#define ll long long
const double eps = 1e-9;
const int N = 200;
double dp[8][N], p[N][N];

int main()
{
    int n;
    while(cin >> n)
    {
        if(n == -1) break;
        memset(dp, 0, sizeof(dp));
        for(int i = 1; i <= (1 << n); ++i)
            for(int j = 1; j <= (1 << n); ++j)
                cin >> p[i][j];
        for(int i = 1; i <= (1 << n); ++i) dp[0][i] = 1; // 不进行比赛，默认为赢
        for(int i = 1; i <= n; ++i)
        {
            for(int j = 1; j <= (1 << n); ++j)
            {
                for(int k = 1; k <= (1 << n); ++k) // 枚举所有队伍，只取符合条件的队伍进行转移
                {
                    if((((j - 1) >> (i - 1)) ^ 1) == ((k - 1) >> (i - 1)))
                        dp[i][j] += dp[i - 1][j] * dp[i - 1][k] * p[j][k];
                    // cout << j << " " << k << " --- " << (((j - 1) >> (i - 1))) << " " << (((j - 1) >> (i - 1)) ^ 1) << " " << ((k - 1) >> (i - 1)) << "\n";
                }
            }
        }
        int ans = 1;
        for(int i = 2; i <= (1 << n); ++i)
            if(dp[n][ans] < dp[n][i]) ans = i;
        cout << ans << "\n";
    }
    return 0;
}
```



### 5. [Passage](https://acm.hdu.edu.cn/showproblem.php?pid=3366)

**题意**

给 n 条通道和 m 元钱，现要通过通道逃出此地，每条通道有三种情况：直接通过、碰上士兵则损失 1 元并返回原地（不够钱则死亡）、死路不通则返回原地。概率分别是 `Pi` `Qi` `1 - Pi - Qi`，返回后将重新选择从其他的通道逃走，问在选择最优方式的前提下成功逃生的概率。

**解法**

定义 `dp[i][j]` 为选择第 i 条通道时还剩下 j 元的概率。题目要求选择最优方式，即每次都选择逃生概率最大的通道走（贪心排序），很显然我们需要以逃生概率 `p / (p + q)` （因为不保证 `p + q = 1`，所以逃生概率不能直接用 p 衡量）来降序排序这些通道，以保证每层状态都选择的是当前逃生概率最大的通道，最后通过状态转移叠加出总的成功逃生概率。

每层状态有三种情况：

1. 从第 i 个通道直接通过：累加答案，`ans += dp[i][j] * p`
2. 在第 i 个通道碰到士兵，返回原地并选择其他道路，更新下一层状态：`dp[i + 1][j - 1] += dp[i][j] * q`
3. 在第 i 个通道遇上死路，返回原地并选择其他道路，更新下一层状态：`dp[i + 1][j] += dp[i][j] * (1 - p - q)`

```c++
#include <iostream>
#include <iomanip>
#include <cstring>
#include <algorithm>
using namespace std;
#define ll long long
const double eps = 1e-9;
const int N = 1e3 + 10;
struct nd{
    double p, q;
    bool operator <(const nd &ne) const{
        return p / (p + q) > ne.p / (ne.p + ne.q); // 或者 p / q > ne.p / ne.q
    };
}arr[N];

double dp[N][15]; 

int main()
{
    int T; 
    cin >> T;
    for(int _ = 1; _ <= T; ++_)
    {
        memset(dp, 0, sizeof dp);
        int n, m; 
        cin >> n >> m;
        for(int i = 1; i <= n; ++i) cin >> arr[i].p >> arr[i].q;
        sort(arr + 1, arr + 1 + n);
        double ans = 0;
        dp[1][m] = 1.0; // 初始时一定能选到第 1 条通道并剩下 m 元钱。
        for(int i = 1; i <= n; ++i)
        {
            for(int j = m; j >= 0; --j) // 类似01背包问题，逆序枚举，只取上一层状态而不取当前层状态
            {
                ans += dp[i][j] * arr[i].p;
                if(j > 0) dp[i + 1][j - 1] += dp[i][j] * arr[i].q;    // 刷表法
                dp[i + 1][j] += dp[i][j] * (1 - arr[i].p - arr[i].q);
            }
        }
        cout << fixed << setprecision(5) << "Case " << _ << ": " << ans << "\n";
    }
    return 0;
}
```



### 6. [Collecting Bugs](http://poj.org/problem?id=2096)

**题意**

一个软件有 s 个子组件，会产生 n 种bug。某人一天发现一个bug，这个bug属于一个子组件。每个bug属于某个子组件的概率是 1 / s，属于某种bug分类的概率是 1 / n。问发现 n 种bug，每个子组件都发现至少一个bug的天数的期望值（或者说所需天数的平均值）。

**解法**

定义 `dp[i][j]` 为已经在 j 个子组件中找到 i 种 bug 所需天数的期望（均值）
显然 `dp[n][s]` 表示任务已经完成，不再需要任何天数以达到目标状态（已经达到目标状态），故 `dp[n][s]` = 0。最终状态已知，而初始状态 `dp[0][0]` 即为答案，故采用逆推法。
`dp[i][j]` 的转移情况（当某天找到一个 bug 时）：

1. `dp[i][j]`       ：该 bug 已经属于已有 i 种bug分类，且属于这 j 个子组件，    `P1 = (i / n) * (j / s)`

2. `dp[i][j+1]`   ： 该 bug 已经属于已有 i 种bug分类，但不属于这 j 个子组件，`P2 = (i / n) * (1 - j / s)`

3. `dp[i+1][j]`   ：该 bug 不属于已有 i 种bug分类，但属于这 j 个子组件，        `P3 = (1 - i / n) * (j / s)`

4. `dp[i+1][j+1]`：该 bug 既不属于已有 i 种bug分类，也不属于这 j 个子组件，`P4 = (1 - i / n) * (1 - j / s)`

   由于发现 1 个bug需要 1 天，则每次转移时天数期望值加 1，可得到转移方程：

   `dp[i, j] = P1 * dp[i, j] + P2 * dp[i + 1, j] + P3 * dp[i, j + 1] + P4 * dp[i + 1, j + 1] + 1`;
   消去等号右侧的 `dp[i, j]`，化简为 ：

   `dp[i, j] = (P2 * dp[i + 1, j] + P3 * dp[i, j + 1] + P4 * dp[i + 1, j + 1] + 1) / (1 - P1)`。

```c++

#include <iostream>
#include <iomanip>
#include <cstring>
using namespace std;
#define ll long long
const double eps = 1e-9;
const int N = 1e3 + 10;
double dp[N][N]; 

int main()
{
    int n, s;
    while(cin >> n >> s)
    {
        dp[n][s] = 0;
        for(int i = n; i >= 0; --i)
        {
            for(int j = s; j >= 0; --j)
            {
                if(i == n && j == s) continue;
                double x = 1.0 * i / n, y = 1.0 * j / s;
                double p1 = x * y, p2 = x * (1 - y), p3 = (1 - x) * y, p4 = (1 - x) * (1 - y);
                dp[i][j] = (p2 * dp[i][j + 1] + p3 * dp[i + 1][j] + p4 * dp[i + 1][j + 1] + 1.0) / (1.0 - p1);
            }
        }
        cout << fixed << setprecision(4) << dp[0][0] << "\n";
    }
    return 0;
}
```



### 7. [LOOPS](https://acm.hdu.edu.cn/showproblem.php?pid=3853)

给 n * m 大小的迷宫，起点为`(1, 1)`，终点为`(n, m)`，且给定方格所走三个方向的概率，分别为原地，右边，下边，每走一格消耗 2 点能量，求到达终点的消耗能量的期望值。

```c++
// 本题与 Collecting Bugs 极为相似，都是使用逆推法得到初始状态的期望值，也同样需要化简转移方程，除了要注意 p1[i][j] == 1 即必然留在原地的情况外，基本与上题相同。
#include <iostream>
#include <iomanip>
#include <cstdio>
#include <cstring>
#include <cmath>
#include <algorithm>
using namespace std;
#define ll long long
const double eps = 1e-5;
const int N = 1e3 + 10;
double dp[N][N], p1[N][N], p2[N][N], p3[N][N]; 

int main()
{
    int n, m;
    while(~scanf("%d%d", &n, &m))
    {
        for(int i = 1; i <= n; ++i)
            for(int j = 1; j <= m; ++j)
                scanf("%lf%lf%lf", &p1[i][j], &p2[i][j], &p3[i][j]);
        dp[n][m] = 0;
        for(int i = n; i >= 1; --i)
        {
            for(int j = m; j >= 1; --j)
            {
                if(i == n && j == m) continue;
                if(fabs(1 - p1[i][j]) < eps) continue; // 若必然留在原地则无法转移状态
                dp[i][j] = (p2[i][j] * dp[i][j + 1] + p3[i][j] * dp[i + 1][j] + 2) / (1 - p1[i][j]);
            }
        }
        cout << fixed << setprecision(3) << dp[1][1] << "\n";
    }
    return 0;
}
```



### 8. [Help Me Escape](https://zoj.pintia.cn/problem-sets/91827364500/problems/91827369307)

有 n 条道路，每条道路有一个危险值 `c[i]`，你有一个初始战斗力 `f0`，如果你的战斗力大于某一条道路的战斗力，那么你需要花费 `(1 + sqrt(5)) / 2 * (c[i])^2` 天逃离，否则，你每天战斗力增加 `c[i]`，现在每天给你随机扔到一条道路上，直到你逃离这条路再继续下一条路。问你逃离所有道路的期望天数。

```c++
// 定义 dp[i] 表示战斗力为 i 时要逃离此地所需天数的期望值
#include <iostream>
#include <iomanip>
#include <cstdio>
#include <cstring>
#include <cmath>
#include <algorithm>
using namespace std;
#define ll long long
const double eps = 1e-9;
const int N = 2e5 + 10;
double dp[N];
int n, f0, c[110];

double dfs(int f) // 记忆化搜索降低复杂度
{
    if(dp[f]) return dp[f];
    dp[f] = 0;
    for(int i = 1; i <= n; ++i)
    {
        if(f > c[i])
        {
            int day = (1 + sqrt(5.0)) / 2 * c[i] * c[i];
            dp[f] += 1.0 * day / n; // day * (1.0 / n)
        }
        else
        {
            dp[f] += (1.0 + dfs(f + c[i])) / n;
        }
    }
    return dp[f];
}

int main()
{
    while(~scanf("%d%d", &n, &f0))
    {
        memset(dp, 0, sizeof dp);
        for(int i = 1; i <= n; ++i) scanf("%d", &c[i]);
        printf("%.3lf\n", dfs(f0));
    }
    return 0;
}
```



### 9. [One Person Game](https://pintia.cn/problem-sets/91827364500/exam/problems/91827368253?type=7&page=23)

给三个骰子，分别有 `k1, k2, k3` 个面，每次掷骰子，如果三个骰子的正面分别为 `a, b, c` 则分数置为 0；否则直接加上三个骰子的点数和。当分数大于 n 时游戏结束，游戏开始时初始步数为 0，求游戏结束时的期望步数。

设 `dp[i]` 为已经有 i 分时到达目标状态所需步数的期望，故答案为 `dp[0]`。设 `p[x]` 为投出 x 分的概率，则`p[0]` 为分数被置为 0 的概率，可得期望公式如下：
$$
dp[i] = \sum_{x=3}^{k_1 +k_2+k_3}{dp[i + x]p[x]} + dp[0]p[0] + 1
$$
可知，该期望公式中含有环（可以由高分数转移到低分数，所以转移存在环），且每一个 `dp[i]`都能转化到 `dp[0]`即此题所有的环都与 `dp[0]`有关，故所有的转移方程都能写成如下形式：
$$
dp[i] = a[i]dp[0] + b[i]
$$
后式代入前式，比较结果式和后式的系数得：
$$
a[i] = \sum_{x=3}^{k_1 +k_2+k_3}{a[i + x]} + p[0] \\
b[i] = \sum_{x=3}^{k_1 +k_2+k_3}{b[i + x]} + 1
$$
故问题转化为求解 `a[0]` 和 `b[0]` 的值，代入 `i = 0` 得答案为 `dp[0] = b[0] / (1 - a[0])`

```c++
#include <iostream>
#include <iomanip>
#include <cstdio>
#include <cstring>
#include <cmath>
#include <algorithm>
using namespace std;
#define ll long long
const double eps = 1e-5;
const int N = 600;
double A[N], B[N], p[100];

int main()
{
    int T;
    scanf("%d", &T);
    while(T--)
    {
        memset(p, 0, sizeof p);
        memset(A, 0, sizeof A);
        memset(B, 0, sizeof B);
        int n, k1, k2, k3, a, b, c;
        scanf("%d%d%d%d%d%d%d", &n, &k1, &k2, &k3, &a, &b, &c);
        p[0] = 1.0 / k1 / k2 / k3; 			 // 置为 0 的概率
        for(int i = 1; i <= k1; i++)
            for(int j = 1; j <= k2; j++)
                for(int k = 1; k <= k3; k++)
                    if(i != a || j != b || k != c)
                        p[i + j + k] += p[0]; // p[0] 同时也是投掷出一次某分数的概率
        for(int i = n; i >= 0; --i)
        {
            A[i] += p[0];
            B[i] += 1;
            for(int x = 3; x <= k1 + k2 + k3; ++x)
            {
                A[i] += A[i + x] * p[x];
                B[i] += B[i + x] * p[x];
            }
        }
        printf("%.15lf\n", B[0] / (1 - A[0]));
    }           
    return 0;
}
```



### 10. [Rating](http://acm.hdu.edu.cn/showproblem.php?pid=4870)

题意：一个人注册两个账号，初始rating都是 0，他每次拿低分的那个号去打比赛，赢了加 50 分，输了扣 100 分，胜率为 p，他会打到直到一个号有 1000 分为止，问此时关于比赛场次的期望值。

仅以**高斯消元法**为例：

分析：这里因为每次的操作是涨 50，或者掉 100 分。所以我们以 50 作为划分可以把 1000 分分为 20 段，会产生一共`(21 * 20) / 2 = 210`种状态。

思路：令`(x, y)`表示高分为 x，低分为 y 的状态（`x >= y`），`E(x, y)`表示从`(x, y)`到达`(1000, ?)`的比赛场数期望。容易得到`E(x, y) = P * E(x1, y1) + (1 - P) * E(x2, y2) + 1`，其中，`(x1, y1)`表示rating上升后的状态，`（x2, y2）`表示rating下降后的状态。把`E(1000, ?) = 0`带入可以得到包含 n 个未知数的 n 个方程中， n 大概 200 多，可以进行高斯消元求解。最终`E(0, 0)`即为答案。

```c++
#include <iostream>
#include <iomanip>
#include <cstdio>
#include <cstring>
#include <cmath>
#include <algorithm>
using namespace std;
#define ll long long
const double eps = 1e-10;
const int N = 250;
double a[N][N], x[N]; // 增广矩阵、解向量
int link[N][N];       // 给每种状态编号
int cnt = 0;          // 状态编号最大值
int equ = 0, var = 0; // equ 个方程、var 个变量

void init()
{
    equ = var = 210;
    cnt = 0;
    memset(link, -1,  sizeof(link));
    for(int i = 0; i < 20; i++)
        for(int j = 0; j <= i; j++)
            link[i][j] = cnt++;
}

double Gauss() // 默认有解
{
    int row, col, max_r;
    row = col = 0;
    while(row < equ && col < var)
    {
        max_r = row;
        for(int i = row + 1; i < equ; i++)
            if(fabs(a[i][col]) - fabs(a[max_r][col]) > eps)
                max_r = i;
        if(max_r != row)
        {
            for(int j = col; j <= var; j++)
                swap(a[row][j], a[max_r][j]);
        }
        if(fabs(a[row][col]) < eps)
        {
            col++;
            continue;
        }
        for(int i = row + 1; i < equ; i++)
        {
            if(fabs(a[i][col]) > eps)
            {
                double t = a[i][col] / a[row][col];
                a[i][col] = 0.0;
                for(int j = col + 1; j <= var; j++)
                    a[i][j] -= a[row][j] * t;
            }
        }
        row++;
        col++;
    }
    for(int i = equ - 1; i >= 0; i--)
    {
        if(fabs(a[i][i]) < eps) continue;
        double tmp = a[i][var];
        for(int j = i + 1; j < var; j++)
            tmp -= a[i][j] * x[j];
        x[i] = tmp / a[i][i];
    }
    return x[0];
}


int main()
{
    init();
    double p;
    while(~scanf("%lf", &p))
    {
        memset(a, 0, sizeof(a));
        // 构建增广矩阵
        int x; // 当前状态编号
        for(int i = 0; i < 20; ++i)
        {
            for(int j = 0; j < i; ++j)
            {
                x = link[i][j];
                a[x][x] = 1;
                a[x][210] = 1;
                a[x][link[i][j + 1]] -= p;
                a[x][link[i][max(0, j - 2)]] -= (1 - p);
            }
            x = link[i][i];
            a[x][x] = 1;
            a[x][210] = 1;
            a[x][link[i][max(0, i - 2)]] -= (1 - p);
            a[x][link[i + 1][i]] -= p;
        }
        printf("%.6lf\n", Gauss());
    }
    return 0;
}
```

