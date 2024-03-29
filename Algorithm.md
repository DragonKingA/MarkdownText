# Algorithm

---

[TOC]

---



# 零. 待补坑

---

1. 原始字符串 cout << R"(???)" << '\n';
1. 

---













# 一. 数据结构

---



## 哈希表（Hash table）

---

### 应用场景

现有一个长度为 N 的整数序列 A，要求统计其中每个数出现的次数。

* 当 A 中的值都较小时，通常直接用一个数组计数（建立一个大小大于等于值域的数组进行映射和统计，即最朴素最简单的Hash思想应用）；

* 当 A 中的值都**非常大**时，我们定义Hash函数 `H(x) = (x mod P) + 1`，其中 P 是一个比较大的**质数**但不超过N。

  此时就把数列 A 的数**划分成 P 个类**



---

### 基本概念

> **哈希表，又称散列表，是一种根据关键码值（Key value）将数据映射到表中某个位置进行快速访问的数据结构，常用于在数值和量值较大的分散序列中查找数据。**

哈希表中元素是由哈希函数 H(x) 确定的。将原数据元素的关键字 key 作为自变量，通过函数关系 H(x) 计算出的哈希值即为该元素的存储地址。

* 优点：运算速度快
* 缺点：基于数组，难以扩展，不可遍历

建立哈希表主要面临两个问题：

* 设法构造**均匀**的哈希函数。使得 `H(key)` 均匀分布在哈希表中，提高地址的计算速度。

  常见构造方法：

  1. **直接寻址法（常用）**：

     * 内容：取关键字或关键字的某个线性函数值作为散列地址。即`H(key) = key`或`H(key) = a * key + b`，其中a和b为常数(这种散列函数叫做自身函数)。

     * 特点：简单而均匀，但需要提前知道关键字的分布情况。

     * 应用：适合查找数据小且连续的情况。

  2. **除留余数法(常用)**：

     * 内容：取关键字被一个**大于数据总数 N 但不大于散列表长度 M** 的数 P 求余得到的余数作为散列地址。即`H(key) = key mod P, P > N && P <= M`，不仅可以对关键字直接取模，也可在平方取中、折叠等运算之后再取模。

     * 注意：一般选择余数 P 是一个**小于 M，但最接近或等于 M 的质数**，以尽量减少碰撞。一般 P 的取法为：

       先初始化 P 为一个奇数（因为大于 2 的偶数必是非质数），尽量靠近 N 但大于 N

       ```c++
       int P = (N % 2) ? (N + 2) : (N + 1);
       ```

       然后取小于 M，但尽量接近 M 的质数（质数判断时最大值设为 `sqrt(N)` 即可）

       ```c++
       int i;
       while (P < M)
       {
           for (i = (int)sqrt(N); i >= 2; --i)
               if(P % i == 0) break;
           if(i < 2) break;//说明for循环退出时 i == 1，即找到了质数
           else P += 2;    //非质数则继续寻找，并跨越偶数值
       }
       ```

       

  3. **平方取中法**：

     * 内容：取关键字**平方后的中间几位**作为散列地址。如数 5241 平方后为 27468081 取中间三位数 468 或 680 作为散列地址。
     * 应用：不知道关键字的分布情况，且关键字的位数较多时。

  4. **折叠法**：

     * 内容：将关键字**分割成位数相同的几部分**（最后一部分位数可不同），然后计算所有部分的**叠加和**值（去除进位）来作为散列地址。
     * 应用：原始值为数字，不知道关键字的分布情况，且关键字的位数较多时。

  5. **随机数法**：

     * 内容：构造随机函数 random(key)，取关键字作为随机函数的种子生成随机值作为散列地址。即 `H(key) = random(key)`
     * 应用：关键字长度差异较大时。

  6. **数字分析法**：

     * 内容：找出数字的普遍分布规律（不用太具体），观察每个数据中哪些部分与其它数据差异较大，利用这些数据来构造散列地址可以减少冲突的发生几率。如管理一些人的出生日期（年月日），可以知道年数、月数和日数的前部分的重复率较高，那么我们只取各自后部分经过一些整合处理，得到的地址值不容易冲突。

     * 应用：任何时候。至少用以上五种方法时可以先用数学分析法**确定哈希处理的对象**或**简化哈希处理后的地址值**。

       

* 处理**冲突（碰撞）**问题。指在哈希表中，不同 key 值却指向同一个存储位置的现象，即**存在 `x1 ≠ x2` 有 `H(x1) = H(x2)`**。

  再怎么均与的哈希函数都只能尽量减少冲突，冲突是**无法完全避免**的，而发生冲突后必须**寻找下一个可用地址**。

  解决冲突方法：**开放定址法（包括线性探测，二次探测，随机探测），再哈希法，链地址法，建立公共溢出区**等。

  常见解决方法：

  1. **拉链法（开散列）**：

     * 内容：一种基于**链表**的 Hash，当产生冲突时就将**多个**原始数据以链表的形式放在**同一个**Hash存储单元中。实现过程：首先对关键码集合用哈希函数计算出对应哈希地址，然后将具有相同的哈希地址的关键码用一个小链表存起来，一般使用**头插**的方式将链表的第一个节点存入哈希表中。

     * 注意：当哈希表中同一个存储单元的链表中有多个数据时，数据的插入、删除和查找的时间复杂度会上升(大于 O(1))，所以必须要构造**具有优良性能的哈希函数**来尽量缩短各链表长度，否则最差劲的情况就是哈希表几乎退化成一个链表。

     * 优点：

       ​	①处理冲突简单，且无堆积现象，即**非同义词决不会发生冲突**，因此平均查找长度较短；

       ​	②很适用在构造哈希表前无法确定表长的情况，因为它的链表空间是**动态申请**的；

       ​	③当**结点规模较大**时，拉链法增设的头插指针占用的空间可忽略不计，比开放定址法更**节省空间**；

       ​	④易于实现**删除结点**的操作，它只需简单地删去链表上对应结点即可。而**开放地址法**的散列表不能简单将结点地址对应的		空间置为空，否则将截断后面同义词的查找路径（如数a和b哈希值相同，都为 H，但数b加上位移量后存储在后面，若		你直接置a对应散列表值为空，导致原哈希值为 H 的值都会找不到，因为它是以探测到空闲地址作为查找失败条件），		因此**开放地址法**执行删除操作时，只能在被删结点处**做删除标记**，而没办法真正删除结点而实现空间减负。

  2. **开放定址法（闭散列）**:

     * 内容：当产生冲突时就寻找**下一个空闲地址**，即给当前重复地址加上**位移值 d**（后移，d > 0），并将数据存入该空闲地址中。只要散列表足够大，就一定能找到空闲地址。常用寻空址公式：`H(key) = (H(key) + d) mod P , d = 1,2,3,···,P-1`

       而位移值 d 如何确定？需要当冲突发生时，使用某种探测技术在散列表中形成一个**探测序列**并沿此序列逐个单元地查找，直到找到给定的关键字，或者遇到一个空闲地址为止。

     * 注意：**插入**元素时，探查到空闲地址，即可将数据存入该地址单元中；**查找**元素时，只需判断当**探测到空闲地址**时则该关键字尚不存在，即查找失败。

     * 优点：适用于**结点规模较小**时，除去了拉链法的指针占用空间，这些空间可以用来扩展散列表的规模，使得该方法下的冲突产生几率降低，从而提高平均查找效率。



Hash表主要包括两个基本操作：

* 计算Hash函数的值
* 定位到对应链表中依次遍历、比较



---

### 基础哈希

**例题**

> **PTA - 航空公司VIP客户查询**
>
> **题目描述**
> 不少航空公司都会提供优惠的会员服务，当某顾客飞行里程累积达到一定数量后，可以使用里程积分直接兑换奖励机票或奖励升舱等服务。现给定某航空公司全体会员的飞行记录，要求实现根据身份证号码快速查询会员里程积分的功能。
>
> **输入格式**
> 首先给出两个正整数 N（N <= 1e5）和 K（K <= 500）。其中 K 是最低里程，为照顾乘坐短程航班的会员，航空公司还会将航程低于K公里的航班也按K公里累积。
>
> 随后N行，每行给出一条飞行记录。飞行记录的输入格式为：18位身份证号码 飞行里程(如`330106199010080419 499`)。其中身份证号码由17位数字加最后一位校验码组成，校验码的取值范围为 0 ~ 9 和 x 共 11 个符号；飞行里程单位为公里，是(0, 15 000]区间内的整数。然后给出一个正整数 M（M <= 1e5），随后给出M行查询人的身份证号码。
>
> **输出格式**
> 对每个查询人，给出其当前的里程累积值。如果该人不是会员，则输出`No Info`。每个查询结果占一行。
>
> **输入样例**
>
> ```
> 4 500
> 330106199010080419 499
> 110108198403100012 15000
> 120104195510156021 800
> 330106199010080419 1
> 4
> 120104195510156021
> 110108198403100012
> 330106199010080419
> 33010619901008041x
> ```
>
> **输出样例**
>
> ```
> 800
> 15000
> 1000
> No Info
> ```
>

- 本题可以用哈希映射存储结点信息，并且明确是**存在冲突可能**的哈希，故需要处理冲突 -- 结点规模为 1e5，故这里倾向使用 **拉链法** 来解决冲突 。

拉链法构建哈希表，需要结构体存储，包含指针next使链表连接起来

```c++
struct node{
    char arr[20];  //身份证号 [0, 17]
    int pts;       //里程积分
    node *next;    //指向下一个单元，没有则为 NULL
};
```

此时需要拟出较好的哈希函数，对于身份证号，首先第17位必取，然后就是尽量靠右取

```c++
int cal(const char *s)//哈希处理身份证号，得到下标地址
{
    //依据待定散列表大小，处理得到的 id 为 1e5 级别，这里共选5位，必选最后一位，前四位集中在后段会快很多
    int id = num(s[5]) * 10000 + num(s[9]) * 1000 + num(s[13]) * 100 + num(s[15]) * 10 + num(s[16]);
    if(s[17] == 'x') id = id * 10 + 10;//特殊处理17位字符位
    else id = id * 10 + num(s[17]);
    return id;
}
```

然后依据数据元素量 N 和散列表长度 M 求得模数 P  （要求`N < P <= M`，且为质数）

```c++
const int M = 2e5;//使用开放定址法，散列表必须足够大
//实际上这道题由于给的空间仅64MB，否则要取五位数字拟哈希值的话，M的大小应至少定义为1e6（这里2e5测试数据已经63MB左右了）
int getprime(int N)
{
    int p = (N % 2) ? (N + 2) : (N + 1);
    int i;
    while(p < M)
    {
        for(i = (int)sqrt(N); i >= 2; --i)
            if(p % i == 0) break;
        if(i < 2) break;
        else p += 2;
    }
    return p;
}
```

结点的插入：头插法，使得存储在数组 head[ind] 中的就是链表的起始结点（重点）

```c++
//注意：链表的连接方向 与 散列表方向 相反！！！
//所以每次加入的新结点都会变成该链表的头，即原来的head[ind]链表一整个接入新结点下
Hash insert(const Hash h, char *s, int d) //哈希链表的插入
{
    Hash p = h;
    while(p) 				   // 即 p != NULL
    {
        if(!strcmp(p -> arr, s))//已存在，则叠加里程积分
        {
            p -> pts += d;
            return h;           //不改变链表，则返回原来的头
        }
        p = p -> next;
    }
    //该结点尚不存在，则新建结点后插入
    p = (Hash)malloc(sizeof(node));
    strcpy(p -> arr, s);
    p -> pts = d;
    p -> next = h;
    return p;			       //新建的结点成为原链表的头，且该结点连向原来的头
}
```

完整代码：

```c++
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cmath>
#include <cstdlib>
#include <cstring>
using namespace std;
#define ll long long
#define ull unsigned long long
#define num(x) ((x) - '0')
const int M = 2e5;

int n, m, k, P;

struct node{
    char arr[20];
    int pts;
    node *next;
};
typedef node* Hash;

int cal(const char *s)//哈希处理身份证号，得到下标地址
{
    int id = num(s[5]) * 10000 + num(s[9]) * 1000 + num(s[13]) * 100 + num(s[15]) * 10 + num(s[16]);
    if(s[17] == 'x') id = id * 10 + 10;
    else id = id * 10 + num(s[17]);
    return id;
}
int getprime(int N)
{
    int p = (N % 2) ? (N + 2) : (N + 1);
    int i;
    while(p < M)
    {
        for(i = (int)sqrt(N); i >= 2; --i)
            if(p % i == 0) break;
        if(i < 2) break;
        else p += 2;
    }
    return p;
}
Hash insert(const Hash h, char *s, int d) //哈希链表的插入
{
    Hash p = h;
    while(p)
    {
        if(!strcmp(p -> arr, s))
        {
            p -> pts += d;
            return h;
        }
        p = p -> next;
    }
    //尚不存在，则新建结点后插入
    p = (Hash)malloc(sizeof(node));
    strcpy(p -> arr, s);
    p -> pts = d;
    p -> next = h;
    return p;
}
void check(const Hash h, char *s) //判断该人是否为会员
{
    Hash p = h;
    while(p) 
    {
        if(!strcmp(p -> arr, s)) //相等时, strcmp() 返回 0
        {
            printf("%d\n", p -> pts);
            return;
        }
        p = p -> next;
    }
    printf("No Info\n");
}
int main()
{
    scanf("%d%d", &n, &k);

    P = getprime(n);
    Hash *head = (Hash *)malloc(P * sizeof(Hash));//头插法的指针数组，存储链表起点
    for(int i = 0; i < P; i++) head[i] = NULL;    //初始化指针数组

    int ind, d;  //下标，里程
    char arr[20];//身份证号

    while(n--)
    {
        scanf("%s%d", arr, &d);
        if(d < k) d = k;//题意：航程低于k公里的航班也按k公里累积
        ind = cal(arr) % P;
        head[ind] = insert(head[ind], arr, d);
    }

    scanf("%d", &m);
    while(m--)
    {
        scanf("%s", arr);
        ind = cal(arr) % P;
        check(head[ind], arr);
    }

    return 0;
}
```

非指针数组写法：

```c++
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cmath>
#include <cstdlib>
#include <cstring>
using namespace std;
#define ll long long
#define ull unsigned long long
#define num(x) ((x) - '0')
const int M = 2e5;//使用开放定址法，散列表必须足够大
int n, m, k, P;
int head[M];
int cnt = 0;      //vec[]的尾标
struct node{
    char arr[20]; //身份证号 [0, 17]
    int pts;      //里程积分
    int next;     //指向下一个单元
}vec[M];
typedef node* Hash;
int cal(const char *s)//哈希处理身份证号，得到下标地址
{
    int id = num(s[5]) * 10000 + num(s[9]) * 1000 + num(s[13]) * 100 + num(s[15]) * 10 + num(s[16]);
    if(s[17] == 'x') id = id * 10 + 10;
    else id = id * 10 + num(s[17]);
    return id;
}
int getprime(int N)
{
    int p = (N % 2) ? (N + 2) : (N + 1);
    int i;
    while(p < M)
    {
        for(i = (int)sqrt(N); i >= 2; --i)
            if(p % i == 0) break;
        if(i < 2) break;
        else p += 2;
    }
    return p;
}
void insert(int id, char *s, int d) //哈希链表的插入
{
    for(int i = head[id]; ~i; i = vec[i].next)
    {
        if(!strcmp(vec[i].arr, s))//已存在，则叠加里程积分
        {
            vec[i].pts += d;
            return;
        }
    }
    //尚不存在，则新建结点后插入
    ++cnt;
    strcpy(vec[cnt].arr, s);
    vec[cnt].pts = d;
    vec[cnt].next = head[id];//新结点（链表新头）指向原链表头结点
    head[id] = cnt;          //新结点作为链表新的头结点
}
void check(int id, char *s) //判断该人是否为会员
{
    for(int i = head[id]; ~i; i = vec[i].next)
    {
        if(!strcmp(vec[i].arr, s)) //相等时, strcmp() 返回 0
        {
            printf("%d\n", vec[i].pts);
            return;
        }
    }
    printf("No Info\n");
}
int main()
{
    int ind, d;  //下标，里程
    char arr[20];//身份证号
    memset(head, -1, sizeof(head));
    scanf("%d%d", &n, &k);
    P = getprime(n);
    while(n--)
    {
        scanf("%s%d", arr, &d);
        if(d < k) d = k;//题意：航程低于k公里的航班也按k公里累积
        ind = cal(arr) % P;
        insert(ind, arr, d);
    }
    scanf("%d", &m);
    while(m--)
    {
        scanf("%s", arr);
        ind = cal(arr) % P;
        check(ind, arr);
    }
    return 0;
}
```







### 字符串哈希

与数值哈希类似，这里则根据字符串的特征来获得对应哈希值。在字符串操作中，常见的是判断字符串之间是否相同，那么就要求我们不同的字符串得到的哈希结果**尽量不同**。最简单的处理就是根据字符串的长度和总ASCII码值来映射，但如`ab`和 `ba`两者不同但这样处理得到的哈希值却相同，说明该处理方式碰撞率太高了，这就需要我们用**低冲突率**的哈希函数。

**进制哈希**

* 核心：给出固定进制 `base`，将一个串中的每个元素都当作某个进制位上的数字，此时该串就能看做是一个**base进制的数**，以该数作为哈希值。

* 防止冲突的方法：

  1. **自然溢出法**（隐性取余）：仅需底数 `base`，然后定义哈希值数据类型为 `unsigned long long` ，相当于此时模数 P = 2^64^ ，当哈希值 H > P 时会自动溢出，等价于自动对 P 取余，这样比起具体取余运算更为高效。
2. **单Hash法**（手动取余）：定义底数 `base` 和余数 `P` ，一般数据类型为 `long long` 即可。
  3. **双Hash法**（手动取余 + 二元组映射）：定义两组底数和余数，即 `base1, base2` 和 `P1, P2` ，定义两个哈希表数组或一个二元结构体数组，每次哈希处理相当于计算**两次单Hash**，将两次哈希结果作为二元组来单一映射一个字符串，当且仅当对应上 `<Hash1[i], Hash2[i]>`才能得到该字符串，大大降低冲突率（用时空复杂度换取安全性）。

* 小优化，一般会将 **base^i^** 的值先预处理算出来，存到`p[i]`里，分为自然溢出、存在模数`P`的情况：

  ```c++
  #define ll long long
  #define ull unsigned long long
  //自然溢出
  ull base, n, p[N] = {1};
  for(int i = 1; i < n; i++)
      p[i] = p[i - 1] * base;
  //存在模数 P
  ll base, n, P, p[N] = {1};
  for(int i = 1; i < n; i++)
      p[i] = p[i - 1] * base % P;
  ```

* 应用：

  1. 给若干个不定长字符串 `S`，判断里面有多少种不同的字符串。一般直接 `Hash[i]` 存储字符串`i`对应的哈希值。

     ```c++
     ull cal(const char *s)
     {
         int len = strlen(s);
         ull h = 0;//哈希值
         for(int i = 0; i < len; i++)
             ans = (ans * base + num(s[i])) % P;
         return h;
     }
     ```

     存储公式如下：`Hash[i] = cal(str)`

  2. 给定一个最长字符串 `S`，对其**连续子串**进行操作（求字符串循环子串数、回文字符串的最大长度、回文字符串的子回文串数等）。一般定义 `Hash[i]` 来表示字符串 `S` 前 `i` 个字符的对应哈希值（类似于**前缀和性质**），以及函数` #define num(x) ((x) - '0')`字符转整数，现假设 `base = 13, P = 101, S = "abc"` ，填充 `Hash[i]`：

     ```c++
     //构建前缀哈希值 Hash[i]
     Hash[0] = 0
     Hash[1] = (Hash[0] * 13 + num('a')) % 101 = 1
     Hash[2] = (Hash[1] * 13 + num('b')) % 101 = 15
     Hash[3] = (Hash[2] * 13 + num('c')) % 101 = 97//即 97 就是 "abc" 的哈希值，15 就是 "ab" 的哈希值
     
     //取[L, R]子串哈希值
     //如 S = "abcd"，记 H[i] 为第i个字符的哈希值
     Hash[0] = 0
     Hash[1] = H[1]
     Hash[2] = H[1] * base + H[2]
     Hash[3] = H[1] * base^2 + H[2] * base + H[3]
     Hash[4] = H[1] * base^3 + H[2] * base^2 + H[3] * base + H[4]
     要得到 s_sub = "cd" 的哈希值，由于该哈希值是基于进制数的叠加计算，试着 Hash[4] - Hash[2] = H[1] * (base^3 - base) + H[2] * (base^2 - 1) ≠ H[3] * base + H[4]，显然需要 Hash[4] - base^2 * Hash[2] 才得所求。
     类比十进制数 1234，已知 1234 和 12，要求 34，即 1234 - 10^2 * 12 = 34，故 base^i 的 i 与长度差有关。
     ```

     存储公式如下：

     * `Hash[i] = (Hash[i - 1] * 10 + num(S[i])) % P`

     求`[L, R]`子串哈希值公式：

     * `H = Hash[R] - Hash[L - 1] * p[R - L + 1]`

     * 一般需要对 P 取余，此时由于取余后可能有 `Hash[R] < Hash[L - 1]`，又哈希值 H 不能为负数，则如下处理（计算结果求余后再加上模数 P，最后又求余）：

       `H = (Hash[R] - Hash[L - 1] * p[R - L + 1] % P + P) % P`

     判断`S`的某子串是否为**回文子串**：

     * 求 `S`的前缀哈希 `hash1[]` 和 后缀哈希 `hash2[]`，，，

       








**例题**

[洛谷 P3370 【模板】字符串哈希]([P3370 【模板】字符串哈希 - 洛谷 | 计算机科学教育新生态 (luogu.com.cn)](https://www.luogu.com.cn/problem/P3370))

```c++
//写法1：自然溢出（158ms 得益于 uo_set 的加速）
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cmath>
#include <cstdlib>
#include <cstring>
#include <unordered_set>
using namespace std;
#define ll long long
#define ull unsigned long long
#define num(x) ((x) - '0')
const int N = 1e4 + 5, M = 1505;
ull base = 131;
char s[M];
unordered_set<ull> cnt;//或者存起哈希值并排序，两两前后对比哈希值是否不同
ull cal(char *s)
{
    int len = strlen(s);
    ull h = 0;
    for(int i = 0; i < len; i++)
        h = h * base + num(s[i]);
    return h;
}
int main()
{
    int n;
    scanf("%d", &n);
    for(int i = 0; i < n; i++)
    {
        scanf("%s", s);
        cnt.insert(cal(s));
    }
    printf("%d\n", cnt.size());
    return 0;
}

//写法2：单Hash法（609ms）
//由于数据量较小，底数取 31，余数 P 取到 1e9 ~ 1e10，使其尽量不要到达取模点即可“避免”冲突。
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cmath>
#include <cstdlib>
#include <cstring>
using namespace std;
#define ll long long
#define ull unsigned long long
#define num(x) ((x) - '0')
const int N = 1e4 + 5, M = 1505;
ll base = 31, P = 9999999929;
char s[N][M];
ll Hash[N];
void print_prime(ll MIN, ll MAX)
{
    for(; MIN <= MAX; MIN++)
    {
        bool ok = 1;
        for(ll i = (ll)sqrt(MIN); i >= 2; i--)
        {
            if(MIN % i == 0) 
            {
                ok = 0;
                break;
            }
        }
        if(ok) printf("%lld\n", MIN);
    }
}
ll cal(char *s)
{
    ll len = strlen(s);
    ll h = 0;
    for(int i = 0; i < len; i++)
        h = (h * base + num(s[i])) % P;
    return h;
}
int main()
{
    // print_prime(9999999900LL, (ll)1e10);
    ll ans = 1;
    int n;
    scanf("%d", &n);
    for(int i = 0; i < n; i++)
    {
        scanf("%s", s[i]);
        Hash[i] = cal(s[i]);
    }
    sort(Hash, Hash + n);
    for(int i = 1; i < n; i++)
    {
        if(Hash[i] != Hash[i - 1])
            ans++;
    }
    printf("%d\n", ans);
    return 0;
}

//写法3：双Hash法（1070ms）
//二元哈希值对应一个字符串，基本不会冲突
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cmath>
#include <cstdlib>
#include <cstring>
using namespace std;
#define ll long long
#define ull unsigned long long
#define num(x) ((x) - '0')
const int N = 1e4 + 5, M = 1505;
ll base1 = 31, base2 = 131, P1 = 9999999929, P2 = 9999999943;
char s[M];
struct nd{
    ll h1, h2;
    bool operator <(const nd &x) const{ return h1 < x.h1;}
}Hash[N];
ll cal(ll base, ll P, char *s)
{
    ll len = strlen(s);
    ll h = 0;
    for(int i = 0; i < len; i++)
        h = (h * base + num(s[i])) % P;
    return h;
}
int main()
{
    ll ans = 1;
    int n;
    scanf("%d", &n);
    for(int i = 0; i < n; i++)
    {
        scanf("%s", s);
        Hash[i].h1 = cal(base1, P1, s);
        Hash[i].h2 = cal(base2, P2, s);
    }
    sort(Hash, Hash + n);
    for(int i = 1; i < n; i++)
        if(Hash[i].h1 != Hash[i - 1].h1 || Hash[i].h2 != Hash[i - 1].h2)
            ++ans;
    printf("%d\n", ans);
    return 0;
}
```

[不同的循环子字符串（求循环子串数目）](https://leetcode.cn/problems/distinct-echo-substrings/)

循环子串（必为偶数长度子串）：该子串的左半串与右半串相同，如下符合循环串："abab"、"leelee"、"ee"（注意与回文串区分）

```c++
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cmath>
#include <cstdlib>
#include <cstring>
#include <unordered_set>
using namespace std;
#define ll long long
#define ull unsigned long long
#define num(x) ((x) - 'a' + 1)//注意这里不能是 x - 'a'，则对于"aaaaaa"这类串所有哈希值都为0，导致出错
const int M = 2005;
ull base = 31;
char s[M];
ull Hash[M], p[M];
ull gethash(int L, int R)
{
    return Hash[R] - Hash[L - 1] * p[R - L + 1];
}
void init()
{
    Hash[0] = 0, p[0] = 1;
    for(int i = 1; i < M; ++i) p[i] = p[i - 1] * base;//预处理计算 base^i
}
int main()
{
    init();    
    scanf("%s", s + 1);
    int n = strlen(s + 1);
    //处理前缀哈希值
    for(int i = 1; i <= n; i++) Hash[i] = Hash[i - 1] * base + num(s[i]);

    //枚举子串长度，判断循环串
    unordered_set<ull> ans;
    ull temp = 0;
    for(int len = 1; 2 * len <= n; len++)
        for(int L = 1; L + 2 * len - 1 <= n; L++)
            if((temp = gethash(L, L + len - 1)) == gethash(L + len, L + 2 * len - 1))
                ans.insert(temp);
    printf("%d\n", ans.size());
    return 0;
}
```

**其他**

```c++
1.Crazy Search（滚动哈希）
//注意如果用 set 去重会 TLE，常数太大
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cmath>
#include <cstdlib>
#include <cstring>
#include <vector>
using namespace std;
#define ll long long
#define ull unsigned long long
#define num(x) ((x) - 'a' + 1)
const int M = 1e6 + 5;
ull h = 0, p = 1, base = 31, P = 800000821;
int len, n, m;
char s[M];
vector<ull> cnt;
int main()
{
    scanf("%d%d", &len, &m);
    scanf("%s", s + 1);
    int n = strlen(s + 1);
    //初始化第一个窗口作为基底，维护窗口[l, r]内哈希值 h 
    for(int i = 1; i <= len && i <= n; i++)//防止 len > n
    {
        h = h * base + num(s[i]);
        p *= base;
    }
    if(len <= n) cnt.push_back(h);
    //窗口移动，整体移向高位空出低位，去除最高位，最后低位加上右边的字符
    //如len = 3， s1*base^2 + s2*base + s3,  p = base^3
    for(int i = len + 1; i <= n; i++)
    {
        h = h * base + num(s[i]) - num(s[i - len]) * p;
        cnt.push_back(h);
    }
    sort(cnt.begin(), cnt.end());
    int num = unique(cnt.begin(), cnt.end()) - cnt.begin();
    printf("%d\n", num);
    return 0;
}



2.Barn Echoes G（简单前缀哈希应用）
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cmath>
#include <cstdlib>
#include <cstring>
#include <vector>
using namespace std;
#define ll long long
#define ull unsigned long long
#define num(x) ((x) - 'a' + 1)
const int N = 1e2 + 5, M = 1e6;
ull base = 31, P = 800000821;
ull p[N] = {1}, h1[N], h2[N];
int len1, len2, ans = 0;
char s1[N], s2[N];
inline ull gethash(const ull *h, int L, int R){ return h[R] - h[L - 1] * p[R - L + 1];}
int main()
{
    for(int i = 1; i < N; i++) p[i] = p[i - 1] * base;
    scanf("%s %s", s1 + 1, s2 + 1);
    len1 = strlen(s1 + 1), len2 = strlen(s2 + 1);
    for(int i = 1; i <= len1; i++) h1[i] = h1[i - 1] * base + num(s1[i]);
    for(int i = 1; i <= len2; i++) h2[i] = h2[i - 1] * base + num(s2[i]);
    for(int len = 1; len <= min(len1, len2); len++)
        if(h1[len] == gethash(h2, len2 - len + 1, len2) || gethash(h1, len1 - len + 1, len1) == h2[len])
            ans = max(ans, len);
    printf("%d\n", ans);
    return 0;
}



3.*Three Friends（求[L,R]子串删去其中一个字符后的哈希值）
//枚举要删除的字符，分在前半、恰好在中间、在后半的情况
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cmath>
#include <cstdlib>
#include <cstring>
#include <string>
#include <map>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
#define ll long long
#define ull unsigned long long
#define num(x) ((x) - ' ' + 1)
const ll N = 2e6 + 10;
ull base = 131;
ll n;
string s;
ull h[N], p[N];
map<ull, bool> vis;//防止找到重复串
ull gethash(int L, int R)
{
    return h[R] - h[L - 1] * p[R - L + 1];
}
int main()
{
    untie();
    cin >> n >> s;
    s = ' ' + s;
    if(!(n & 1))
    {
        cout << "NOT POSSIBLE\n";
        return 0;
    }
    ll mid = n + 1 >> 1, pos = 0, cnt = 0;
    p[0] = 1;
    for(int i = 1; i <= n; i++) h[i] = h[i - 1] * base + num(s[i]), p[i] = p[i - 1] * base;
    
    ull tl, tr;
    //前半
    for(int i = 1; i <= mid - 1; i++)
    {
        //此处tl求的就是删去第i个字符后[1, mid]的哈希值
        tl = gethash(1, i - 1) * p[mid - i] + gethash(i + 1, mid), tr = gethash(mid + 1, n);
        if(tl == tr && !vis[tl])
        {
            vis[tl] = 1;
            ++cnt;
            pos = i;
        }
    }
    //令 i = mid
    tl = gethash(1, mid - 1), tr = gethash(mid + 1, n);
    if(tl == tr && !vis[tl])
    {
        vis[tl] = 1;
        ++cnt;
        pos = mid;
    }
    //后半
    for(int i = mid + 1; i <= n; i++)
    {
        tl = gethash(1, mid - 1), tr = gethash(mid, i - 1) * p[n - i] + gethash(i + 1, n);
        if(tl == tr && !vis[tl])
        {
            vis[tl] = 1;
            ++cnt;
            pos = i;
        }
    }
    if(cnt == 1) cout << (pos <= mid ? s.substr(mid + 1) : s.substr(1, mid - 1));
    else cout << (cnt > 1 ? "NOT UNIQUE" : "NOT POSSIBLE");
    return 0;
}



4.Long Long Message（二分 + 滚动哈希 + 排序后binary_search()判重）
//二分最大重复长度mid，每次用滚动窗口取出所有长为mid的子串的哈希值，有相同就扩大左界找更长
//其中一般用map、set判重会因常数过大而TLE，用求余减小常数又会导致Hash冲突，只能退而求其次，用 排序 + 二分 查重
//可规定 len1 <= len2 便于操作
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cstring>
#include <string>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
#define ll long long
#define ull unsigned long long
#define num(x) ((x) - 'a' + 1)
const ll N = 1e5 + 10;
ull base = 131, P = 9999999943;
int n, len1, len2;
ull h1[N], h2[N], p[N];
string s1 = "", s2 = "";
void init(int n) //初始化前缀哈希
{
    p[0] = 1;
    for(int i = 1; i <= n; i++) p[i] = p[i - 1] * base;
    for(int i = 1; i <= len1; i++) h1[i] = h1[i - 1] * base + num(s1[i]);
    for(int i = 1; i <= len2; i++) h2[i] = h2[i - 1] * base + num(s2[i]);
}
ull gethash(const ull *h, int L, int R)
{
    return h[R] - h[L - 1] * p[R - L + 1];
}
bool check(int len)
{
    vector<ull> v;
    for(int l = 1, r = len; r <= len1; l++, r++) v.push_back(gethash(h1, l, r));
    sort(v.begin(), v.end());
    for(int l = 1, r = len; r <= len2; l++, r++)
        if(binary_search(v.begin(), v.end(), gethash(h2, l, r)))//二分查找函数binary_search()会返回是否查到具体值
            return 1;
    return 0;
}
int main()
{
    untie();

    cin >> s1 >> s2;
    len1 = s1.size(), len2 = s2.size();
    if(len1 > len2) swap(s1, s2), swap(len1, len2);
    s1 = "{" + s1, s2 = "{" + s2;

    init(len2);

    int l = 0, r = min(len1, len2), ans = 0;
    while(l <= r)
    {
        int mid = l + r >> 1;
        if(check(mid)) l = mid + 1, ans = max(ans, mid);
        else r = mid - 1;
    }
    cout << ans;
    return 0;
}
```





 















---

## 树状数组（Fenwick）

---

### 应用场景

现有一个长度为 `n` 的数组 `a[]`，要求完成 `q` 次操作，操作类型有：

* `i c`		给 `a[i]` 加上数 `c`
* `x y c`	 给区间 `[x, y]` 上的每个数都加上数 `c`
* `x y`        查询区间 `[x, y] `上所有数的和

暴力解法：直接维护原数组 `a[]`

* 单点修改：`O(1)`
* 区间修改：`O(n)`
* 求区间和：`O(n)`


树状数组解法：

* 单点修改、区间修改、求区间和：O(log~2~n)

总体来说树状数组运作起来是非常快的。

---

### 基本概念

![树状数组](D:\MarkdownText\image\数据结构\树状数组\树状数组.png)

通过观察树状数组图可以探求规律（以结点 `t[1 ... 8]` 为例）：

| 树状数组结点 t[i] | t[i] 包含的 结点 和 值a[i]    | t[i] 表示的类后缀和                                       |
| ----------------- | ----------------------------- | --------------------------------------------------------- |
| `t[1]`            | = `a[1]`                      | = `a[1]`                                                  |
| `t[2]`            | = `a[1] + a[2]`               | = `a[1] + a[2]`                                           |
| `t[3]`            | = `a[3]`                      | = `a[3]`                                                  |
| `t[4]`            | = `t[2] + t[3] + a[4]`        | = `a[1] + a[2] + a[3] + a[4]`                             |
| `t[5]`            | = `a[5]`                      | = `a[5]`                                                  |
| `t[6]`            | = `t[5] + a[6]`               | = `a[5] + a[6]`                                           |
| `t[7]`            | = `a[7]`                      | = `a[7]`                                                  |
| `t[8]`            | = `t[4] + t[6] + t[7] + a[8]` | = `a[1] + a[2] + a[3] + a[4] + a[5] + a[6] + a[7] + a[8]` |

故对于树状数组 `t[i]` ，有如下性质：

* 当 `i` 为奇数时，`t[i]` 只代表一个原值即 `a[i]`；

* 当 `i` 为偶数时，`t[i]` 则代表一段长为 **2^k** `(k ∈ N*)` 的区间和（从 `a[i]` 开始**往前**加到 `a[i - 2^k + 1]`），如 `t[6]` 代表长为 `2` 的区间和、`t[12]` 代表长为 `4` 的区间和；

  仅当 **i = 2^m^** (m ∈ N*) 时，`t[i]` 则是一段**严格的后缀和**，如 `t[8] = Σa[8 ... 1]` 、`t[4] = Σa[4 ... 1]`  等。

那么要得出 `t[i]` 所管辖的区间长度，由于上述信息处处与 2 的次幂相关，通过观察 `i` 的二进制性质，`t[6] = t[ 0110 ]` 有 `2` 项、`t[4] = t[ 0100 ]` 有 `3` 项、 `t[8] = t[ 1000 ]` 有 `8` 项，不难发现：

  * **k** 就是 `i` 的二进制数的**后缀0个数**，则 **2^k** 就代表 `i` 的二进制数的**末尾1的二进制数**。

例如：

* 当 i = 6，  二进制数为 011**0**，则 k = 1，项数为 2^k^ = 2 即 00**10** = 2；
* 当 i = 12，二进制数为 11**00**，则 k = 2，项数为 2^k^ = 4 即 0**100** = 4。

核心性质：每个结点存储信息的方式类似后缀和思想（并非完整的后缀和）

> **`t[i]` 表示原序列 `a[]` 区间 `[i - 2^k + 1, i]` 的区间和**



---

### 巧妙的 lowbit(x)

此时我们定义函数 **lowbit(x)** 来获取 `x` 的二进制数的 **末尾1** 的二进制数

假设 `x = 44` 即 `0010 1100` ，现要依据 `x` 得到 末尾1的二进制数 `0000 0100`  ：

* 首先要使得 末尾1 的左侧全变成 **0**，即存在运算 **⊗** 使二进制数 **n** 的每位数都变成 **0** 即 **永假**，

  不难想到：逻辑运算里 `p ∧ ┐p` 永假 -------> 二进制运算里 `n & ~n` 结果永假，

  得到 `~x = ~(0010 1100) =  1101 0011` ，而 `x & ~x = 0000 0000` 显然不是所求，故还需要更改运算 **⊗**。

* 现需 **保留末尾1及其后缀0**，由于 **末尾1** 的右侧必为 **0** 即取反后必为 **1**，此时 `~x` 右侧的 **0011** 与 `x` 右侧的 **0100** 只差 **+1**，此时 `~x + 1 = 1101 0100`，故 `x & (~x + 1)`  即所求。

其中，取反然后加1的操作莫名熟悉——即负数的**补码**，所以我们可以直接获得 `x` 的负数的补码 即 `-x`，

最终函数内容被简化成 `x & -x`，即

```c++
#define lowbit(x) ((x) & -(x))
```

此时核心性质可表达为：

> **`t[i]` 表示原序列 `a[]` 区间 `[i - lowbit(i) + 1, i]` 的区间和**





### 基本操作

* 区间查询

即求 `[1, x]`区间前缀和 `S(x)`；

根据核心性质，一段 区间和 一般包含**若干个结点**存储的类后缀和；

如设输入数列 `a[i] = i`，求区间和`[1, 14]`即 `x = 14` 时，显然该区间上包括 三个主结点 `14 12 8`，

即 `S(14) = t[14] + t[12] + t[8] = (13 + 14) + (9 + 10 + 11 + 12) + (1 + 2 + 3 + 4 + 5 + 6 + 7 + 8) = 27 + 42 + 36 = 105`

![树状数组2](D:\MarkdownText\image\数据结构\树状数组\树状数组2.png)

如何实现遍历 `14 -> 12 -> 8` 来求区间和 `Σa[1 ... 14]` ？

由核心性质可以推出 `t[x]` 的前一个结点是 `t[x - lowbit(x)]`，以此类推可以遍历到所有前面的结点，直到囊括区间 `a[1 ... x]`

则实现 `14 -> 12 -> 8` 即每次 `x -= lowbit(x)` 进行结点跳跃。

故查询 `[1, x]` 区间和代码实现如下：

```c++
int sum(int x)
{
    int res = 0;
    for(; x > 0; x -= lowbit(x))
		res += tree[x];
    return res;
}
```

根据前缀和性质，可求得`[x, y]` 的区间和：

```c++
S(x, y) = sum(y) - sum(x - 1)
```



* 单点修改

如图，下级结点的修改会影响**所有**上级结点的类**后缀和**值，即需要处理至树状数组的顶端 `n`。

它是相对于查询区间和的逆操作，假设现在要给 `t[9]` 加上值 `d` ，不难发现，`9 -> 10 -> 12 -> 16`是末尾1不断**向高位推进**的过程，即操作 `x += lowbit(x)`

![树状数组3](D:\MarkdownText\image\数据结构\树状数组\树状数组3.png)

单点修改代码实现如下：

```c++
void update(int x, int d)//更新结点后缀和，d 即变化值
{
    for(; x <= n; x += lowbit(x))//n 为树状数组大小（数列长度）
    {
        tree[x] += d;
    }
}
```

以上就是基本树状数组（维护原数组前缀和，仅可单点修改 + 区间查询）。

#### 单点修改 + 区间查询

实现模板：

[洛谷 P3374 树状数组 1](https://www.luogu.com.cn/problem/P3374) 

```c++
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
#define ll long long
#define lowbit(x) ((x) & -(x))
const int N = 5e5 + 10;
int n, m;
int tree[N];
void update(int x, int d)
{
    for(; x <= n; x += lowbit(x))
        tree[x] += d;
}
int sum(int x)
{
    int res = 0;
    for(; x > 0; x -= lowbit(x))
        res += tree[x];
    return res;
}
int main()
{
    scanf("%d%d", &n, &m);
    for(int i = 1; i <= n; i++)
    {
        int x;
        scanf("%d", &x);
        update(i, x);
    }
    while(m--)
    {
        int op, x, y;
        scanf("%d%d%d", &op, &x, &y);
        if(op == 1) update(x, y);
        else printf("%d\n", sum(y) - sum(x - 1));
    }
    return 0;
}
```



#### 区间修改 + 单点查询

由于树状数组缺乏优良的递归搜索性质，它并不能像线段树那样直接修改一段区间，只能采用 **差分\前缀和思想** 将单点修改操作能作用到区间上，故现在只需要换成维护原数组 `a[i]` 的**差分数组**`b[i]`，操作代码不变。

```c++
//初始化时插入差分
update(i, a[i] - a[i - 1]);
//查询单值 a[x] 即查询前缀和 ∑b[1 ... x]
a[x] = sum(x);
```

显然，它的弊端是最终只能查询单点值 `a[x]`。

实现模板：

[洛谷 P3368 树状数组 2](https://www.luogu.com.cn/problem/P3368)

```c++
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
#define ll long long
#define lowbit(x) ((x) & -(x))
const int N = 5e5 + 10;
int a[N], tree[N];
int n, m;
void update(int x, int d)
{
    for(; x <= n; x += lowbit(x))
        tree[x] += d;
}
int sum(int x)//查询前缀和 ∑b[1 ... x]
{
    int res = 0;
    for(; x > 0; x -= lowbit(x))
        res += tree[x];
    return res;
}
int main()
{
    scanf("%d%d", &n, &m);
    for(int i = 1; i <= n; ++i) 
    {
        scanf("%d", &a[i]);
        update(i, a[i] - a[i - 1]);//插入差分 b[i]
    }
    while(m--)
    {
        int op, x, y;
        ll k;
        scanf("%d", &op);
        if(op == 1)
        {
            scanf("%d%d%d", &x, &y, &k);
            update(x, k);
            update(y + 1, -k);
        }
        else//此时前缀和 ∑b[1 ... x] 即原单值 a[x]
        {
            scanf("%d", &x);
            printf("%d\n", sum(x));
        }
    }
    return 0;
}
```



#### 区间修改 + 区间查询

维护差分数组的树状数组如何进行区间查询？现对公式推导简化，寻找便于维护的信息来实现计算区间和：

$$
\begin{aligned}

原数组为& \ a[i] \\  构造差分数组为&\ b[i] = a[i] - a[i - 1]
\\则有&\ a[i] = \sum_{j = 1}^{i}{b[j]}
\\ 故区间[1, n]前缀和为&\ \sum_{i = 1}^{n}{a[i]} = \sum_{i = 1}^{n}{\sum_{j = 1}^{i}{b[j]}}
\\ 展开为
\\ &\ \ \ \ \ a[1] + a[2] + a[3] + ··· + a[n]\\
\\ &= b[1] + (b[1] + b[2]) + (b[1] + b[2] + b[3]) + ··· + (b[1] + b[2] + ··· + b[n])\\
\\ &= n * b[1] + (n - 1) * b[2] + (n - 2) * b[3] + ··· + 1 * b[n]\\
\\ &= \sum_{i = 1}^{n}{(n - i + 1) * b[i]}
\\ &= (n + 1) * \sum_{i = 1}^{n}{b[i]} - \sum_{i = 1}^{n}{i * b[i]}

\end{aligned}
$$

故区间 `[1, x]` 前缀和即 `∑a[1 ... x] = (x + 1) * ∑b[x] - ∑(x * b[x])` ，

其中乘项 `(x + 1)`在存储中可视为**常项**（可以等实际求和的时候再乘上），那么需维护的信息是两个前缀和：`∑b[i]` 和 `∑(i * b[i])`，

故可维护两个基于差分数组 `b[k]` 的树状数组 `t1[N]` 和 `t2[N]`，并**同时更新**它们。

当要给区间 `a[l ... r]` 每个数加上值 `d` 时：

* 对 `t1[i]`，即直接更新 `b[l] += d` 和 `b[r + 1] += -d` ，

  即函数内 `t1[i] += d`。

* 对 `t2[i]`，其真正的变量只有 `b[x]`，由于此时有 `b[l] += d` 和 `b[r + 1] += -d`，

  亦有 `l * (b[l] + d) = l * b[l] + l * d` 和 `(r + 1) * (b[r + 1] - d) = (r + 1) * b[r + 1] - (r + 1) * d` ，

  故变化量分别为 `l * d` 和 `(r + 1) * (-d)`，

  即函数内 `t2[i] += x * d `。

```c++
//更新 update(x, d) 其中 d 为变化值。
//一般化写法：
void update(int *t, int x, int d)//传递 b[x] 和 x * b[x] 的变化量
{
    while(x <= n)
    {
        t[x] += d;
        x += lowbit(x);
    }
}
update(t1, x, d);
update(t1, y + 1, -d);
update(t2, x, x * d);
update(t2, y + 1, -(y + 1) * d);


//简化写法：
void update(int x, int d)
{
    for(int i = x; i <= n; i += lowbit(i))
    {
        t1[i] += d;
        t2[i] += x * d;
    }
}
update(x, k);
update(y + 1, -k);
```

实现模板：

[洛谷 P3372 线段树 1](https://www.luogu.com.cn/problem/P3372)

```c++
//注意开 long long 防数据溢出
#include <cstdio>
#include <algorithm>
#include <iostream>
using namespace std;
#define ll long long
#define lowbit(x) ((x) & -(x))
const int N = 1e5 + 10;
ll t1[N], t2[N];
int n, m;

void update(ll x, ll d)
{
    for(int i = x; i <= n; i += lowbit(i))
    {
        t1[i] += d;    
        t2[i] += x * d;
    }
}
ll sum(ll x)//查询前缀和 ∑a[1 .. x]
{
    ll res = 0;
    for(int i = x; i > 0; i -= lowbit(i))
    {
        res += (x + 1) * t1[i] - t2[i];//记得给 t1[i] 补上乘项 (x + 1)
    }
    return res;
}

int main()
{
    scanf("%d%d", &n, &m);
    ll now, last = 0;
    for(int i = 1; i <= n; ++i)
    {
        scanf("%lld", &now);
        update(i, now - last);//插入差分b[i]
        last = now;
    }
    while(m--)
    {
        ll op, x, y, k;
        scanf("%lld%lld%lld", &op, &x, &y);
        if(op == 1) 
        {
            scanf("%lld", &k);
            update(x, k);
            update(y + 1, -k);
        }
        else
        {
            printf("%lld\n", sum(y) - sum(x - 1));
        }
    }
    return 0;
}
```

---

### 偏序问题 与 权值树状数组

**偏序问题**

定义：

​	设 R 是集合 A 上的一个**二元**关系，若 R 满足：

* **自反性**：对任意 x ∈ A，有 xRx；
* **反对称性**（即 R 对称上不成立）：对任意 x，y ∈ A，若已有 xRy，且 yRx，则必 x = y，即只有自反时满足，否则一旦存在 xRy 就不可能存在对称的 yRx 成立；
* **传递性**：对任意 x，y，z ∈ A，若有 xRy 且 yRz ，则必 xRz 成立。

​	则称 R 为 A 上的偏序关系。

那么**偏序问题**在题目中一般是“求解给定序列中**满足给定偏序关系的关系组组数**”。

拿逆序关系（属于偏序关系的一种）举例：

* 逆序对：指在序列 A 中满足 `ai > aj` 且 `i < j` 的二元对 `<i, j>`。

* 有一序列 `A = {5 4 2 6 3 1}`，诸如 `<1, 2>、<3, 6>、<5, 6>` 等都是逆序对，逆序对的偏序性主要表现在反对称性（如 `<1, 2>` 成立，而 `<2, 1>` 不成立）和传递性（如 `<1, 3>` 和 `<3, 6>` 同时成立，那么 `<1, 6>` 也必成立）。



**二维偏序问题**

此时 R 是**一对互相限制的、共同成立的二元关系**（或者说有 R1 和 R2 两个偏序关系），即此时所寻求的关系组需满足给定**一对偏序关系**，当且仅当两种关系同时成立时才计数。

以此类推，有 **n维偏序关系**。



**权值树状数组**

除了像普通树状数组维护的就是原序列的数值，还有在原序列的**权值数组**上构建的树状数组 —— 权值树状数组。

设序列 `A = {1 4 3 2 3 2}` ，其权值数组为 `B = {1 2 2 1}`。对 `A[i] = x`，其权值数组 `B[x] = x的出现次数`，则 `B` 的大小和 `A`的值域有关。若 `A` 的值域过大，一般像逆序对这类**偏序问题**，需要处理的是数之间的**相对大小关系**，故都会进行**离散化处理**后再建立权值数组，并通过维护权值树状数组来担任偏序问题中**统计数的个数**的角色。

具体题中使用细节见[逆序对](https://www.luogu.com.cn/problem/P1908)等题。


---

### 总结

* 结构：完全二叉树（除了最后一层可能会缺结点，并且最后一层剩余的结点都靠左且连续，其余层数一定组成一个满二叉树）

  ![完全二叉树](D:\MarkdownText\image\数据结构\树状数组\完全二叉树.png)

  这就区别于线段树的满二叉树（完美二叉树）结构。

* 注意：树状数组无根（或者说可向上无限延伸），且树状数组首个叶子结点编号**必须从 1 开始**。

* 特点：普通树状数组仅支持  **单点\区间修改** 和 **单点\区间查询** ，码量少，时空复杂度低。

* 缺点：拓展性比线段树差，线段树可以解决树状数组能解决的问题（除非卡空间），但反过来就不一定。

* 应用：树状数组维护的结果是**前缀和**，“**动态更新并求解前缀和**”是树状数组在做的事。通常考虑能否将题目问题转化为前缀和问题，用树状数组快速解决。多应用于 **偏序问题** 和 维护公式里的求和式。其中偏序问题一般用的是**权值树状数组**（以原序列的值 x 作为下标，表达数 x 在区间内的出现次数），也常常搭配**离散化处理**技巧。



### 模板

基于差分：

单点/区间修改、单点/区间查询

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
#define endl '\n'
#define gcd(a, b) __gcd(a, b)
#define mk(a, b) make_pair((a), (b))

const int N = 2e5 + 10, inf = 1e9;
const ll linf = 1e18;

class Fenwick
{
public:
    #define lowbit(x) ((x) & -(x))
    #define T long long

    int n;
    vector<T> t1, t2;

    Fenwick() : n(0){}
    Fenwick(int nn)
    {
        n = nn;
        t1.assign(n + 5, 0);
        t2.assign(n + 5, 0);
    }

    void setval(vector<T> arr) // 传入原数组 arr
    {
        T last = 0;
        for(int i = 1; i <= n; ++i)
        {
            UPDATE(i, arr[i] - last); // 基于差分
            last = arr[i];
        }
    }

    void UPDATE(int x, T d) // 差分数组上更新
    {
        for(int i = x; i <= n; i += lowbit(i))
        {
            t1[i] += d;
            t2[i] += x * d;
        }
    }
    void update(int x, T d) // 单点修改
    {
        UPDATE(x, d);
        UPDATE(x + 1, -d);
    }
    void update(int l, int r, T d) // 区间修改
    {
        UPDATE(l, d);
        UPDATE(r + 1, -d);
    }

    T QUERY(int x) // 差分数组上查询[1, x]前缀和
    {
        T res = 0;
        for(int i = x; i > 0; i -= lowbit(i))
        {
            res += (x + 1) * t1[i] - t2[i];
        }
        return res;
    }
    T query(int x) // 单点查询
    {
        return QUERY(x) - QUERY(x - 1);
    }
    T query(int l, int r) // 区间查询
    {
        return QUERY(r) - QUERY(l - 1);
    }

    #undef lowbit
    #undef T
};

void Solve()
{
    int n, q;
    cin >> n >> q;

    vector<ll> arr(n + 1);
    for(int i = 1; i <= n; ++i)
        cin >> arr[i];

    Fenwick tree(n);
    tree.setval(arr);

    while(q--)
    {
        ll op, l, r, d;
        cin >> op >> l >> r;
        if(op == 1)
        {
            cin >> d;
            tree.update(l, r, d);
        }
        else
        {
            cout << tree.query(l, r) << endl;
        }
    }
}

int main()
{
    untie();
    int T = 1;
    // cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```



基于原数组：







### 题目

| 序号 | 题号       | 标题                                                         | 题型                                                         | 难度评级 |
| ---- | ---------- | :----------------------------------------------------------- | ------------------------------------------------------------ | -------- |
| 1    | hdu 1166   | [敌兵布阵](https://vjudge.csgrandeur.cn/problem/HDU-1166)    | 单点修改 + 区间查询                                          | ⭐        |
| 2    | poj 1195   | [Mobile phones](http://poj.org/problem?id=1195)              | 二维基础树状数组                                             | ⭐        |
| 3    | 洛谷 P3368 | [【模板】树状数组 2](https://www.luogu.com.cn/problem/P3368) | 区间修改 + 单点查询                                          | ⭐        |
| 4    | 洛谷 P2357 | [守墓人](https://www.luogu.com.cn/problem/P2357)             | 区间修改 + 区间查询                                          | ⭐⭐       |
| 5    | 洛谷 P2184 | [贪婪大陆](https://www.luogu.com.cn/problem/P2184)           | 区间覆盖问题                                                 | ⭐⭐⭐      |
| 6    | 洛谷 P5094 | [[USACO04OPEN\] MooFest G 加强版](https://www.luogu.com.cn/problem/P5094) | 前缀和 + 排序                                                | ⭐⭐⭐⭐     |
| 7    | 洛谷 P5142 | [区间方差](https://www.luogu.com.cn/problem/P5142)           | 前缀和 + 数论（[逆元\|分数取模](https://blog.csdn.net/godleaf/article/details/79844074)） | ⭐⭐⭐⭐     |
| 8    | 洛谷 P1908 | [逆序对](https://www.luogu.com.cn/problem/P1908)             | 偏序问题（权值树状数组） + 离散化                            | ⭐⭐       |
| 9    | 洛谷 P3531 | [[POI2012\]LIT-Letters](https://www.luogu.com.cn/problem/P3531) | 偏序问题（权值树状数组）                                     | ⭐⭐⭐      |
| 10   | 牛客       | [牛客 数星星 Stars](https://ac.nowcoder.com/acm/problem/50428) | 二维偏序模板（权值树状数组）                                 | ⭐⭐       |
| 11   | 洛谷 P1637 | [三元上升子序列](https://www.luogu.com.cn/problem/P1637)     | 二维偏序问题（权值树状数组）                                 | ⭐⭐⭐      |
| 12   | 洛谷 P1972 | [[SDOI2009] HH的项链](https://www.luogu.com.cn/problem/P1972) | 二维偏序问题 + 离线排序                                      | ⭐⭐⭐⭐     |
| 13   | 洛谷 P4113 | [[HEOI2012\]采花](https://www.luogu.com.cn/problem/P4113)    | 三维偏序问题 + 离线排序                                      | ⭐⭐⭐⭐⭐    |



---

## 线段树

---

* 线段树是一种基于 分治思想 的 二叉树(二分)，用于在 区间 上进行信息的统计(对于 区间操作 的通用解法)。

* 它把一些对于区间（或者线段）的修改、维护，从 O(N) 的时间复杂度变成 O(log~2~N)

* 线段树 可以理解为 分治思想 + 二叉树结构 + Lazy-Tag技术 

​		1.线段树的每一个节点都代表一个区间

​		2.线段树具有的唯一根节点，代表的区间是整个统计范围，[1，n]

​		3.线段树的每个叶节点都代表长度为 1 的元区间 [x，x]

​		4.对于每个内部节点 [l，r]，它的左节点是 [l, mid]，右节点是 [mid, r]，其中 mid = (l + r) / 2(向下取整)

* 一个线段树基本结构有：信息和节点的存储、信息合并、信息更新、建树、单点修改、区间查询等

​		线段树的每个区间维护自己的 左右边界 和 区间总和\最大值\最小值等，假设当前节点维护的区间为[L, R]，

​		则 mid = (L + R) / 2，他的左子节点维护[L, mid]，右子节点维护[mid + 1, R]。

* 操作分析 O(logn)：

  	线段树构造 build() 分治构造 ，pushup() (从下往上传递区间信息)。
  	
  	单点修改：直接修改叶子节点上元素的值，然后从下往上更新线段树；

​	  区间修改update()：用标记数组 tag[i] 统计记录对 区间i 的修改(延迟修改)，仅当修改操作间产生重叠或冲突时不得不向子节点传递 pushdown() (从上往下传递)；

  	区间查询 query()：查询[L, R]信息(最值或区间和等)，从根节点区间[1, n]开始递归，设为[pl, pr]，每次递归操作分两种情况：

​      		[L, R] 完全覆盖 [pl, pr]，直接返回 编号p 所代表的值；

​      		[L, R] 与 [pl, pr] 部分重叠，那么二分搜索有重叠的部分。若 L < pr，左子节点与区间有重叠，则继续递归左子。若 R > pl，继续递归右子。

  	

### 基础区间操作问题

```c++
1.A Simple Problem with Integers
区间加值 + 查询区间和
#include <cstdio>
#include <algorithm>
#include <iostream>
using namespace std;
#define ls (p << 1)
#define rs (p << 1 | 1)
#define ll long long
#define lc ls, pl, mid
#define rc rs, mid + 1, pr
const int N = 1e5 + 10;
struct node{
    ll sum;
    ll tag;
}tree[N << 2];
void addtag(ll p, ll pl, ll pr, ll d)
{
    tree[p].tag += d;
    tree[p].sum += d * (pr - pl + 1);
}
void push_up(ll p)
{
    tree[p].sum = tree[ls].sum + tree[rs].sum;
}
void push_down(ll p, ll pl, ll pr)
{
    if(tree[p].tag != 0)
    {
        ll mid = (pl + pr) >> 1;
        addtag(lc, tree[p].tag);
        addtag(rc, tree[p].tag);
        tree[p].tag = 0;
    }
}
void build(ll p, ll pl, ll pr)
{
    tree[p].tag = 0;
    if(pl == pr)
    {
        scanf("%lld", &tree[p].sum);
        return;
    }
    ll mid = (pl + pr) >> 1;
    build(lc);
    build(rc);
    push_up(p);
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && R >= pr)
    {
        addtag(p, pl, pr, d);
        return;
    }
    push_down(p, pl, pr);
    ll mid = (pl + pr) >> 1;
    if(L <= mid) update(L, R, lc, d);
    if(R > mid) update(L, R, rc, d);
    push_up(p);
}
ll query(ll L, ll R, ll p, ll pl, ll pr)
{
    if(L <= pl && R >= pr)
        return tree[p].sum;
    push_down(p, pl, pr);
    ll mid = (pl + pr) >> 1, res = 0;
    if(L <= mid) res += query(L, R, lc);
    if(R > mid) res += query(L, R, rc);
    return res;
}
int main()
{
    int n, m;
    scanf("%d%d", &n, &m);
    build(1, 1, n);
    while(m--)
    {
        char op;
        ll a, b, c;
        getchar();
        scanf("%c%lld%lld", &op, &a, &b);
        if(op == 'Q') printf("%lld\n", query(a, b, 1, 1, n));
        else
        {
            scanf("%lld", &c);
            update(a, b, 1, 1, n, c);
        }
    }
    return 0;
}



2.Just a Hook
区间置值 + 查询区间和
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
#define ll int
#define ls (p << 1)
#define rs (p << 1 | 1)
#define lc ls, pl, mid
#define rc rs, mid + 1, pr
const ll N = 1e5 + 10;
long long tree[N << 2];
ll tag[N << 2];
void addtag(ll p, ll pl, ll pr, long long d)
{
    tag[p] = d;
    tree[p] = d * (pr - pl + 1);
}
void push_up(ll p)
{
    tree[p] = tree[ls] + tree[rs];
}
void push_down(ll p, ll pl, ll pr)
{
    if(tag[p])
    {
        ll mid = (pl + pr) >> 1;
        addtag(lc, tag[p]);
        addtag(rc, tag[p]);
        tag[p] = 0;
    }
}
void build(ll p, ll pl, ll pr)
{
    tag[p] = 0, tree[p] = 0;
    if(pl == pr)
    {
        tree[p] = 1;
        return ;
    }
    ll mid = (pl + pr) >> 1;
    build(lc);
    build(rc);
    push_up(p);
}
void update(ll L, ll R, ll p, ll pl, ll pr, long long d)
{
    if(L <= pl && R >= pr)
    {
        addtag(p, pl, pr, d);
        return;
    }
    push_down(p, pl, pr);
    ll mid = (pl + pr) >> 1;
    if(L <= mid) update(L, R, lc, d);
    if(R > mid) update(L, R, rc, d);
    push_up(p);
}
int main()
{
    int T;
    scanf("%d", &T);
    for(int i = 1; i <= T; ++i)
    {
        int n, q;
        scanf("%d%d", &n, &q);
        build(1, 1, n);
        while(q--)
        {
            ll x, y;
            long long z;
            scanf("%d%d%lld", &x, &y, &z);
            update(x, y, 1, 1, n, z);
        }
        printf("Case %d: The total value of the hook is %lld.\n", i, tree[1]);
    }
    return 0;
}



3.Balanced Lineup
查询区间当前最值
#include <cstdio>
#include <algorithm>
#include <iostream>
using namespace std;
#define ll int
#define ls (p << 1)
#define rs (p << 1 | 1)
#define lc ls, pl, mid
#define rc rs, mid + 1, pr
const ll N = 5e4 + 5, INF = 1e6;
struct node{
    ll min_, max_;
    node(ll mmin = 0, ll mmax = 0) {min_ = mmin, max_ = mmax;}
}tree[N << 2];
ll min_res = INF, max_res = 0;
void push_up(ll p)
{
    tree[p].max_ = max(tree[ls].max_, tree[rs].max_);
    tree[p].min_ = min(tree[ls].min_, tree[rs].min_);
}
void build(ll p, ll pl, ll pr)
{
    tree[p] = node(INF, 0);
    if(pl == pr)
    {
        ll t;
        scanf("%d", &t);
        tree[p] = node(t, t);
        return;
    }
    ll mid = (pl + pr) >> 1;
    build(lc);
    build(rc);
    push_up(p);
}
void query(ll L, ll R, ll p, ll pl, ll pr)
{
    if(L <= pl && R >= pr)
    {
        max_res = max(max_res, tree[p].max_);
        min_res = min(min_res, tree[p].min_);
        return;
    }
    ll mid = (pl + pr) >> 1;
    if(L <= mid) query(L, R, lc);
    if(R > mid) query(L, R, rc);
    return;
}
//同时查询最大最小值的写法
// PII query(int now, int L, int R) {
//     // 找到了要的区间返回这个区间的最大值和最小值，用来和后面的区间进行对比 
//     if (L <= root[now].l && root[now].r <= R) return PII(root[now].hei, root[now].low);
//     // 规定first为最大值，second为最小值，那么没找到就返回一个极端值就行了 
//     if (L > root[now].r || R < root[now].l) return PII(-1e9, 1e9);
//     // 不用pushdown
//     // 找到左右子树的最大值和最小值对，然后对比 
//     PII nhei = query(now << 1, L, R);
//     PII nlow = query(now << 1 | 1, L ,R); 
//     // 对比左区间的最大值和右区间的最大值，左区间的最小值和右区间的最小值... 
//     return PII(max(nhei.first, nlow.first), min(nhei.second, nlow.second));
// }
int main()
{
    int n, q;
    scanf("%d%d", &n, &q);
    build(1, 1, n);
    while(q--)
    {
        min_res = INF, max_res = 0;
        ll l, r;
        scanf("%d%d", &l, &r);
        query(l, r, 1, 1, n);
        printf("%d\n", max_res - min_res);
    }
    return 0;
}



4.开关
初始数列为 0串，无需单独建树
异或1 实现lazy标记状态切换 ，区间长度 - 原本亮灯的数量 = 现在亮灯的数量
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
#define ll long long
#define il inline
const int N = 1e5 + 10;
ll a[N], tree[N << 2], tag[N << 2];
il ll ls(ll p){ return p << 1;} 
il ll rs(ll p){ return p << 1 | 1;} 
il void addtag(ll p, ll pl, ll pr, ll d){ tag[p] ^= 1, tree[p] = pr - pl + 1 - tree[p];}
il void push_up(ll p){ tree[p] = tree[ls(p)] + tree[rs(p)];}
il void push_down(ll p, ll pl, ll pr)
{
    if(tag[p])
    {
        ll mid = (pl + pr) >> 1;
        addtag(ls(p), pl, mid, tag[p]);
        addtag(rs(p), mid + 1, pr, tag[p]);
        tag[p] = 0;
    }
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && pr <= R){ addtag(p, pl, pr, d); return;}
    push_down(p, pl, pr);
    ll mid = (pl + pr) >> 1;
    if(L <= mid) update(L, R, ls(p), pl, mid, d);
    if(R > mid) update(L, R, rs(p), mid + 1, pr, d);
    push_up(p);
}
ll query(ll L, ll R, ll p, ll pl, ll pr)
{
    if(pl >= L && R >= pr) return tree[p];
    push_down(p, pl, pr);
    ll res = 0;
    ll mid = (pl + pr) >> 1;
    if(L <= mid) res += query(L, R, ls(p), pl, mid);
    if(R > mid) res += query(L, R, rs(p), mid + 1, pr);
    return res;
}
int main()
{
    ll n, m, q, L, R;
    scanf("%lld%lld", &n, &m);
    while(m--)
    {
        scanf("%lld%lld%lld", &q, &L, &R);
        if(q) printf("%lld\n", query(L, R, 1, 1, n));
        else update(L, R, 1, 1, n, 1);
    }
    return 0;
}
   

    
5.线段树 2
区间加值 + 区间乘值 + 查询区间和
重点在于如何维护lazy标记实现两种赋值操作，自然需要引入第二个标记实现区间乘
对[L, R]每个数乘以 k，实际上就是 tree[p] = tree[p] * k % p，p 即区间[L, R]的编号
/*多操作问题解读（以加法和乘法操作为例）：
首先要记住，lazy标记是用于 当前编号 的 儿子区间 的 修改，当前区间的修改需引用 父区间 的标记
优先级关系：乘法（加法），因此添加标记时需要注意 积标记 对 和标记 的影响
和标记 与 积标记 的关系推导：从需求出发(查询的是什么) -- 查询和
此前区间值(可能已改变，并非特指初始化时的数列) X = al + ··· + ar，和操作 与 乘操作 都是对原数列各值作用。
不同情况下：
    添加标记时的更新 update()：
        1.此前无标记 + 添加 和\积标记: X' = al + ··· + ar + (r - l + 1) * d_sum 或 X' = (al + ··· + ar) * d_mul

        2.原和标记 + 和标记: X' = (al + ··· + ar) + (r - l + 1) * d_sum + (r - l + 1) * d_sum' = (al + ··· + ar) + (r - l + 1) * (d_sum + d_sum') 
                            = X + (r - l + 1) * (d_sum + d_sum') -- 和标记间累和
          原积标记 + 积标记：X' = X * d_mul * d_mul'

        3.原和标记 + 积标记：X' = (al + d_sum) * d_mul + (al+1 + d_sum) * d_mul ··· (ar + d_sum) * d_mul
                            = (al + ··· + ar) * d_mul + (r - l + 1) * d_sum * d_mul
                            = X * d_mul + (r - l + 1) * (d_sum * d_mul) -- 此处 新和标记 = 原和标记 * 新积标记
                            所以进行乘法操作时，除了 原乘法标记 的更新，还需要更新 原和标记
          原积标记 + 和标记：X' = (al * d_mul + d_sum) + (al+1 * d_mul + d_sum) ··· (ar * d_mul + d_sum) = (al + ··· + ar) * d_mul + (r - l + 1) * d_sum
                            = X * d_mul + (r - l + 1) * d_sum
                            结合 和+和，任何情况下，进行加和操作时，只需要更新 原和标记
    区间值更新，标记的传递与积累 push_down()：
        （此前区间值(含 原和标记，原积标记) =  X * d_mul + (r - l + 1) * d_sum * d_mul = X * mul_tag + (r-l+1) * sum_tag，其中 X 为 此前数列值和）
        由于这些标记的传递是统一的，同时的，互相影响的，需将两种标记的传递合并：
            可知，乘法标记向下传递时影响儿子的所有标记，因为 和标记 实际上是 原来要加的值d * 当前积标记自顶向下的累乘值
            所以，各标记积累：
                sum_tag = sum_tag' + sum_tag * mul_tag'，
                mul_tag = mul_tag * mul_tag'。
            区间值更新(基于新标记)：
                X' = X * mul_tag' + (r - l + 1) * sum_tag'。
*/
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
#define ll long long
#define il inline
const int N = 1e5 + 10;
ll n, m, mod;
ll a[N], tree[N << 2], tag[N << 2], tag2[N << 2];
il ll ls(ll p){ return p << 1;} 
il ll rs(ll p){ return p << 1 | 1;} 
il void addtag(ll p, ll pl, ll pr, ll d)
{
    tag[p] = (tag[p] + d) % mod, tree[p] = (tree[p] + d * (pr - pl + 1) % mod) % mod;
}
il void addtag2(ll p, ll pl, ll pr, ll d)//乘法标记影响加法标记
{
    tag[p] = tag[p] * d % mod, tag2[p] = tag2[p] * d % mod, tree[p] = tree[p] * d % mod;
}
il void push(ll p, ll pl, ll pr, ll u) //传值时同时计算加法和乘法，并且传递加法标记时，加法标记受原乘法标记影响
{
    tree[p] = ((pr - pl + 1) * tag[u] % mod + (tree[p] * tag2[u]) % mod) % mod;
    tag[p] = (tag[p] * tag2[u] % mod + tag[u]) % mod;
    tag2[p] = tag2[p] * tag2[u] % mod;
}
il void push_up(ll p){ tree[p] = (tree[ls(p)] + tree[rs(p)]) % mod;}
il void push_down(ll p, ll pl, ll pr)
{
    if(tag[p] || tag2[p] != 1)
    {
        ll mid = (pl + pr) >> 1;
        push(ls(p), pl, mid, p);
        push(rs(p), mid + 1, pr, p);
        tag[p] = 0, tag2[p] = 1;
    }
}
void build(ll p, ll pl, ll pr)
{
    tag[p] = 0, tag2[p] = 1;//乘法标记初始化为 1
    if(pl == pr){ tree[p] = a[pl] % mod; return;}
    ll mid = (pl + pr) >> 1;
    build(ls(p), pl, mid);
    build(rs(p), mid + 1, pr);
    push_up(p);
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d, ll op)
{
    if(L <= pl && pr <= R)
    {
        if(op == 2) addtag(p, pl, pr, d);
        else addtag2(p, pl, pr, d);
        return;
    }
    push_down(p, pl, pr);
    ll mid = (pl + pr) >> 1;
    if(L <= mid) update(L, R, ls(p), pl, mid, d, op);
    if(R > mid) update(L, R, rs(p), mid + 1, pr, d, op);
    push_up(p);
}
ll query(ll L, ll R, ll p, ll pl, ll pr)
{
    if(pl >= L && R >= pr) return tree[p] % mod;
    push_down(p, pl, pr);
    ll res = 0, mid = (pl + pr) >> 1;
    if(L <= mid) res += query(L, R, ls(p), pl, mid);
    if(R > mid) res += query(L, R, rs(p), mid + 1, pr);
    return res % mod;
}
int main()
{
    scanf("%lld%lld%lld", &n, &m, &mod);
    for(ll i = 1; i <= n; i++) scanf("%lld", &a[i]);
    build(1, 1, n);
    while(m--)
    {
        ll q, L, R, d;
        scanf("%lld%lld%lld", &q, &L, &R);
        if(q == 3) printf("%lld\n", query(L, R, 1, 1, n));
        else scanf("%lld", &d), update(L, R, 1, 1, n, d, q);
    }
    return 0;
}
```



### 染色问题

一维、二维染色问题参考：https://blog.csdn.net/qq_45748404/article/details/119489831?ops_request_misc=&request_id=&biz_id=102&utm_term=Mayor%27s%20posters&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-3-119489831.142^v73^pc_search_v2,201^v4^add_ask,239^v2^insert_chatgpt&spm=1018.2226.3001.4187

#### · 一维染色问题

```c++
1.Mayor's posters （一维染色问题 + 特殊离散化处理）
给一个无限长广告牌，给你n个广告和其放置的位置，按照输入数据的顺序放置前后，问能看见几个广告（注意，看见一部分也算）。
题意转换：设每个广告都是独立的一种颜色，放置广告相当于给一个区间染色，问某个区间有多少种颜色。
由于 l, r <= 1e7，需要离散化降低复杂度，并且l和r两两离散，为保证不交叉区间的非连续性，需要离散时 间隔一个数
如输入区间[1, 100],[1, 30],[50, 100]，其答案为3，若连续离散则得到[1, 4],[1, 2],[3, 4]导致答案为2，因此应该将离散数字两两间隔一个数
即[1, 7],[1, 3],[5, 7]，才能得到答案为3，
unique() 的返回值是一个地址指向去重后序列（这个序列不含有重复数值）的 末尾 的 下一个元素
v.erase(unique(v.begin(), v.end()), v.end()) 可以去掉容器中后面重复的元素
    
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <cstring>
#include <vector>
#include <set>
using namespace std;
#define ll int
#define ls (p << 1)
#define rs (p << 1 | 1)
#define lc ls, pl, mid
#define rc rs, mid + 1, pr
const ll N = 1e6 + 5;
vector<ll> v;
set<ll> st;
pair<ll, ll> que[N];
ll tree[N << 2], n ,T;
void pushdown(ll p)
{
    if(tree[p])
    {
        tree[ls] = tree[rs] = tree[p];
        tree[p] = 0;
    }
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && R >= pr)
    {
        tree[p] = d;
        return;
    }
    pushdown(p);
    ll mid = (pl + pr) >> 1;
    if(L <= mid) update(L, R, lc, d);
    if(R > mid) update(L, R, rc, d);
}
void query(ll L, ll R, ll p, ll pl, ll pr)//重点
{
    if(tree[p]) //区间p有颜色就记录
    {
        st.insert(tree[p]);
        return;//区间p有颜色，说明该区间下都为这个颜色，故不用再搜索
    }
    if(pl == pr) return;
    ll mid = (pl + pr) >> 1;
    if(L <= mid) query(L, R, lc);
    if(R > mid) query(L, R, rc);
}
int main()
{
    scanf("%d", &T);
    while(T--)
    {
        v.clear();
        st.clear();
        memset(tree, 0, sizeof(tree));
        scanf("%d", &n);
        for(int i = 1; i <= n; i++)
        {
            ll l, r;
            scanf("%d%d", &l, &r);
            que[i] = make_pair(l, r);
            v.push_back(l);
            v.push_back(r);
        }
        sort(v.begin(), v.end());
        v.erase(unique(v.begin(), v.end()), v.end());
        ll len = v.size();
        for(int i = 1; i < len; i++)
            if(v[i] - v[i - 1] > 1) // 即两区间不连续
                v.push_back(v[i - 1] + 1);
        sort(v.begin(), v.end());
        len = v.size();
        for(int i = 1; i <= n; i++)
        {
            ll L = lower_bound(v.begin(), v.end(), que[i].first) - v.begin() + 1;//记得要使其从1开始，都得加1。
            ll R = lower_bound(v.begin(), v.end(), que[i].second) - v.begin() + 1;
            update(L, R, 1, 1, len, i);
        }
        query(1, len, 1, 1, len);
        printf("%d\n", st.size());
    }
    return 0;
}



2.Count the Colors
题意：在长度为n的线段上，每次操作给其中一个区间染上颜色c，可以覆盖前面的颜色，求最后整条线段上能看到多少种颜色。
l, r <= 8e3，无需离散化
本题重点在于如何联系连续区间(注意区间[1,3][3,5]连续可看作[1,5]，而[1,2][3,4]不连续，因为被[2,3]分开了)。
搞明白涂色的是区间而不是点，染色区间[a,b]并不是把a～b的所有点都染成c，故区间不能取闭区间。
防止不连续区间判断为连续，需要将一侧的端点收束（左端点+1 或 右端点-1）或直接两端点值乘2，来使得连续数字间有中间值，因为连续数字的两个区间其实不连续，
这里可以选择用 左开右闭 区间，设染色区间[l, r]即染色[l + 1, r]上的每个点，对于一个单位区间[0, 1]，将点1染色等同于将[0, 1]染色，
即染色某个点，即染色该点的前一段单位区间。
    
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <cstring>
#include <set>
#include <map>
using namespace std;
#define ll int
#define ls (p << 1)
#define rs (p << 1 | 1)
const ll N = 8e3 + 5;
ll tree[N << 2], ans[N], last = 0;//重点：记录上次碰到的颜色或者说上次碰到非颜色区间，方便排除连续区间
void pushdown(ll p)
{
    if(tree[p])
    {
        tree[ls] = tree[rs] = tree[p];
        tree[p] = 0;
    }
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(tree[p] == d) return;//优化，若已经染成d色，则退出
    if(L <= pl && R >= pr)
    {
        tree[p] = d;
        return;
    }
    pushdown(p);
    ll mid = (pl + pr) >> 1;
    if(L <= mid) update(L, R, ls, pl, mid, d);
    if(R > mid) update(L, R, rs, mid + 1, pr, d);
}
void query(ll p, ll pl, ll pr)//搜索每个线段树区间
{
    if(tree[p])//有颜色
    {
        if(tree[p] != last) 
        {
            ans[tree[p]]++;//非连续区间才计数
            last = tree[p];//更新last
        }
        return;
    }
    if(pl == pr)//到了叶子结点都还没颜色，说明颜色断片，已经不连续了，那就初始化last
    {
        last = 0;
        return;
    }
    ll mid = (pl + pr) >> 1;
    query(ls, pl, mid);
    query(rs, mid + 1, pr);
}
int main()
{
    int n;
    while(~scanf("%d", &n))
    {
        last = 0;
        memset(ans, 0, sizeof(ans));
        memset(tree, 0, sizeof(tree));
        for(int i = 1; i <= n; i++)
        {
            ll l, r, c;
            scanf("%d%d%d", &l, &r, &c);
            update(l + 1, r, 1, 1, N, c + 1);
        }
        query(1, 1, N);
        for(int i = 1; i < N; i++)
            if(ans[i]) printf("%d %d\n", i - 1, ans[i]);
        printf("\n");
    }
    return 0;
}
```



#### · 二维染色问题



```c++







```



### 区间合并问题



```c++
1.Tunnel Warfare 
三种操作：单点修改、求最长连续1的个数 和 撤销上一次单点修改
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
using namespace std;
#define ls (p << 1)
#define rs (p << 1 | 1)
#define lc ls, pl, mid
#define rc rs, mid + 1, pr
#define ll int
const ll N = 5e4 + 5;

//线段树维护子节点，pre保存区间前缀最长1的个数，suf保存区间后缀最长1的个数
ll tree[N << 2], pre[N << 2], suf[N << 2];

//存在撤回操作，记录村庄被毁的历史
ll history[N];

void push_up(ll p, ll len)//更新区间p的前缀1和后缀1个数
{
    //先传承，再判断左右子区间合并情况
    pre[p] = pre[ls];
    suf[p] = suf[rs];
    //如果左子区间均为1，则可以将值合并到右子区间的前缀1和。右子同理。
    if(pre[ls] == len - (len >> 1)) pre[p] += pre[rs];
    if(suf[rs] == (len >> 1)) suf[p] += suf[ls];
}
void build(ll p, ll pl, ll pr)
{
    if(pl == pr)
    {
        tree[p] = pre[p] = suf[p] = 1;
        return;
    }
    ll mid = (pl + pr) >> 1;
    build(lc);
    build(rc);
    push_up(p, pr - pl + 1);
}
void update(ll x, ll p, ll pl, ll pr, ll d)
{
    if(pl == pr) 
    {
        tree[p] = pre[p] = suf[p] = d;
        return;
    }
    ll mid = (pl + pr) >> 1;
    if(x <= mid) update(x, lc, d);
    else update(x, rc, d);
    push_up(p, pr - pl + 1);
}
ll query(ll x, ll p, ll pl, ll pr)
{
    if(pl == pr) return tree[p];
    ll mid = (pl + pr) >> 1;
    if(x <= mid)//查询点落在左子
    {
        //若查询点落在左子的后缀最长1区间内，直接返回最长连续1；若不是则继续探查左子
        if(x + suf[ls] > mid) return suf[ls] + pre[rs];
        else return query(x, lc);
    }
    else
    {
        if(pre[rs] >= x - mid) return suf[ls] + pre[rs];
        else return query(x, rc);
    }
}
int main()
{
    ll n, q;
    while(~scanf("%d%d", &n, &q))
    {
        ll ind = 0;
        build(1, 1, n);
        while(q--)
        {
            char op[5];
            ll x;
            scanf("%s", &op);
            if(op[0] == 'D')
            {
                scanf("%d", &x);
                update(x, 1, 1, n, 0);
                history[++ind] = x;//依次记录破坏操作
            }
            else if(op[0] == 'Q')
            {
                scanf("%d", &x);
                printf("%d\n", query(x, 1, 1, n));
            }
            else
            {
                x = history[ind--];
                update(x, 1, 1, n, 1);
            }
        }
    }
    return 0;
}



2.Hotel
区间修改 + 统计连续个数
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
using namespace std;
#define ls (p << 1)
#define rs (p << 1 | 1)
#define lc ls, pl, mid
#define rc rs, mid + 1, pr
#define ll int
const ll N = 5e4 + 4;
struct nd{
    ll sum, pre, suf;//统计对象为1，即空位置个数
    ll tag;          //为0\1时区间置为0\1
    nd(ll s = 0, ll p = 0, ll sf = 0, ll t = 0) { sum = s, pre = p, suf = sf, tag = t;}
}tree[N << 2];
void addtag(ll p, ll len, ll d)
{
    ll x = d * len;
    tree[p] = nd(x, x, x, d);
}
void push_up(ll p, ll len)
{
    tree[p].pre = tree[ls].pre;
    tree[p].suf = tree[rs].suf;
    
    if(tree[ls].pre == len - (len >> 1)) tree[p].pre += tree[rs].pre;
    if(tree[rs].suf == (len >> 1)) tree[p].suf += tree[ls].suf;
    tree[p].sum = max(max(tree[ls].sum, tree[rs].sum), tree[ls].suf + tree[rs].pre);
}
void push_down(ll p, ll pl, ll pr)
{
    if(tree[p].tag != -1)
    {
        ll mid = pl + pr >> 1;
        addtag(ls, mid - pl + 1, tree[p].tag);
        addtag(rs, pr - mid, tree[p].tag);
        tree[p].tag = -1;
    }
}
void build(ll p, ll pl, ll pr)
{
    ll len = pr - pl + 1;
    tree[p] = nd(len, len, len, -1);
    if(pl == pr) return;
    ll mid = pl + pr >> 1;
    build(lc);
    build(rc);
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && R >= pr)
    {
        addtag(p, pr - pl + 1, d);
        return;
    }
    push_down(p, pl, pr);
    ll mid = pl + pr >> 1;
    if(L <= mid) update(L, R, lc, d);
    if(R > mid) update(L, R, rc, d);
    push_up(p, pr - pl + 1);
}
ll query(ll p, ll pl, ll pr, ll k)
{
    if(pl == pr && k == 1) return pl;//特判
    push_down(p, pl, pr);
    //按左端点编号由小到大搜，左->中->右
    ll mid = pl + pr >> 1;
    if(tree[p].sum < k) return 0;
    if(tree[ls].sum >= k)
        return query(lc, k);
    else if(tree[ls].suf + tree[rs].pre >= k)
        return mid - tree[ls].suf + 1;
    else
        return query(rc, k);
}
int main()
{
    ll n, m;
    scanf("%d%d", &n, &m);
    build(1, 1, n);
    while(m--)
    {
        ll op, x, d;
        scanf("%d", &op);
        if(op == 1)
        {
            scanf("%d", &d);
            ll res = query(1, 1, n, d);
            printf("%d\n", res);
            if(res) update(res, res + d - 1, 1, 1, n, 0);
        }
        else
        {
            scanf("%d%d", &x, &d);
            update(x, x + d - 1, 1, 1, n, 1);
        }
    }
    return 0;
}



3.约会安排
需要同时维护两个区间，两区间之间具有单向覆盖关系 + 维护最长连续序列长度。
维护两个时间区间 DS、NS，需要记录连续0个数(最长前缀连续、最长后缀连续 和 最长总连续)
DS T 和 NS T 都要求找一段编号最靠前的长为T的连续空闲空间，而 NS 如果没找到则可以占据 DS 的空间； STUDY!! L R 表示清空[L, R]区间
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
#define ls (p << 1)
#define rs (p << 1 | 1)
#define lc ls, pl, mid
#define rc rs, mid + 1, pr
#define ll int
const int N = 1e5 + 5;
struct nd{
    ll pre, suf, cnt;//1代表有空间
    ll tag;
    nd(ll a = 0, ll b = 0, ll c = 0, ll d = 0) {pre = a, suf = b, cnt = c, tag = d;}
}tree[2][N << 2];
ll n, m, _;
void addtag(ll p, ll pl, ll pr, ll ind, ll d)
{
    ll len = pr - pl + 1, x = d * len;
    tree[ind][p] = nd(x, x, x, d);
}
void push_up(ll p, ll pl, ll pr, ll i)
{
    ll len = pr - pl + 1;
    tree[i][p].pre = tree[i][ls].pre;
    tree[i][p].suf = tree[i][rs].suf;
    if(tree[i][p].pre == len - (len >> 1)) tree[i][p].pre += tree[i][rs].pre;
    if(tree[i][p].suf == (len >> 1)) tree[i][p].suf += tree[i][ls].suf;
    tree[i][p].cnt = max(max(tree[i][ls].cnt, tree[i][rs].cnt), tree[i][ls].suf + tree[i][rs].pre);
}
void push_down(ll p, ll pl, ll pr, ll i)
{
    ll mid = pl + pr >> 1;
    if(tree[i][p].tag != -1)
    {
        addtag(lc, i, tree[i][p].tag);
        addtag(rc, i, tree[i][p].tag);
        tree[i][p].tag = -1;
    }
}
void build(ll p, ll pl, ll pr)
{
    ll len = pr - pl + 1;
    for(int i = 0; i < 2; i++) tree[i][p] = nd(len, len, len, -1);
    if(pl == pr) return;
    ll mid = pl + pr >> 1;
    build(lc);
    build(rc);
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll ind, ll d)
{
    if(L <= pl && R >= pr)
    {
        addtag(p, pl, pr, ind, d);
        return;
    }
    push_down(p, pl, pr, ind);
    ll mid = pl + pr >> 1;
    if(L <= mid) update(L, R, lc, ind, d);
    if(R > mid) update(L, R, rc, ind, d);
    push_up(p, pl, pr, ind);
}
ll query(ll p, ll pl, ll pr, ll ind, ll d)
{
    if(pl == pr) return pl;
    push_down(p, pl, pr, ind);
    ll mid = pl + pr >> 1;
    if(tree[ind][ls].cnt >= d) return query(lc, ind, d);
    else if(tree[ind][ls].suf + tree[ind][rs].pre >= d) return mid - tree[ind][ls].suf + 1;
    else return query(rc, ind, d);
}
int main()
{
    scanf("%d", &_);
    for(int __ = 1; __ <= _; __++)
    {
        printf("Case %d:\n", __);
        scanf("%d%d", &n, &m);
        build(1, 1, n);
        while(m--)
        {
            char op[5];
            ll T, L, R, pos;
            scanf("%s", &op);
            if(op[0] == 'D')
            {
                scanf("%d", &T);
                if(tree[0][1].cnt < T)//tree[0] 同时受NS、DS区间占用情况的影响，即 DS 只能找tree[0]
                {
                    printf("fly with yourself\n");
                    continue;   
                }
                //DS区间只影响tree[0]
                pos = query(1, 1, n, 0, T);
                update(pos, pos + T - 1, 1, 1, n, 0, 0);
                printf("%d,let's fly\n", pos);
            }
            else if(op[0] == 'N')
            {
                ll res;
                scanf("%d", &T);
                if(tree[1][1].cnt < T)
                {
                    printf("wait for me\n");
                    continue;
                }
                if(tree[0][1].cnt >= T)        //依据题意看DS区间是否有足够的连续空闲区间
                    res = query(1, 1, n, 0, T);
                else                           //再“无视DS区间占用”找
                    res = query(1, 1, n, 1, T);

                update(res, res + T - 1, 1, 1, n, 0, 0);
                update(res, res + T - 1, 1, 1, n, 1, 0);
                printf("%d,don't put my gezi\n", res);
            }
            else
            {
                scanf("%d%d", &L, &R);
                update(L, R, 1, 1, n, 0, 1);
                update(L, R, 1, 1, n, 1, 1);
                printf("I am the hope of chinese chengxuyuan!!\n");
            }
        }
    }
    return 0;
}
```



### 扫描线

#### 矩形面积并

##### · 一次覆盖问题（基础）

```c++
1.Atlantis
题意：x-y坐标系上有若干个矩形，它们的边分别平行两个坐标轴，求它们的面积并（面积并集），要求重复部分面积只计算一次。
注意：由于x, y是实数，需要离散化处理。由于线段树维护的是x的区间长度，都是区间即不含点，如区间[2, 2]等点不具有意义，所以线段树的叶子结点为区间[pl, pl + 1]。
基本原理：遍历所有扫描线，只要还没遇到其对应的出边时，该入边长度一直有效（指属于当前新矩形的底边长度），每次会得到当前的总有效长度tree[1]，就是新矩形的底边长。
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
#define ls (p << 1)
#define rs (p << 1 | 1)
#define ll int
#define db double
const int N = 1e3 + 5;
ll n, cnt[N << 2];//cnt[p] 记录当前区间p被覆盖的次数，跟其它节点无关。可判断该区间长度是否有效
//一个节点代表的区间被覆盖的次数不需要继承其父亲的信息，每条入边一一对应一条等长的出边，由该入边产生的覆盖次数cnt应由其对应出边抵消，因此去掉pushdown
db tree[N << 2], X[N << 2];
struct Line{
    db lx, rx, y;
    ll d;//1为入边，-1为出边
    Line(db a = 0, db b = 0, db c = 0, ll dd = 0) {lx = a, rx = b, y = c, d = dd;}
    bool operator <(const Line &x) const{ return y < x.y;}
}line[N << 2];
void push_up(ll p, ll pl, ll pr)
{
    if(cnt[p]) 
        tree[p] = X[pr] - X[pl];
    //区间p长度无效时
    else if(pl + 1 == pr)//叶子结点没办法向下索取有效长度，即后面没有有效长度排队占用，置为0即可
        tree[p] = 0;
    else
        tree[p] = tree[ls] + tree[rs];//向下索取有效长度（或者说到这段有效长度占据区间p了）
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && R >= pr)
    {
        cnt[p] += d;
        push_up(p, pl, pr);
        return;
    }
    if(pl + 1 == pr) return;
    ll mid = pl + pr >> 1;
    if(L <= mid) update(L, R, ls, pl, mid, d);
    if(R > mid) update(L, R, rs, mid, pr, d); //注意这里不是 mid + 1

    if(!cnt[p]) tree[p] = tree[ls] + tree[rs];
    //仅区间p没有有效长度时才传值，因为若区间p已经有效，说明它已经是最长的覆盖长度pr-pl。若直接传值则会错误地覆盖这个仍有效的长度。
    //应等区间p被出边抵消时，才把子区间值传上来，即它是有顺序的。
    push_up(p, pl, pr);
}
int main()
{
    int _ = 1;
    while(~scanf("%d", &n))
    {
        if(!n) break;
        memset(tree, 0, sizeof(tree));
        memset(cnt, 0, sizeof(cnt));
        db ans = 0;
        ll ind = 0;
        for(int i = 0; i < n; i++)
        {
            db x1, y1, x2, y2;
            scanf("%lf%lf%lf%lf", &x1, &y1, &x2, &y2);
            line[++ind] = Line(x1, x2, y1, 1);
            X[ind] = x1;
            line[++ind] = Line(x1, x2, y2, -1);
            X[ind] = x2;
        }
        sort(X + 1, X + 1 + ind);
        sort(line + 1, line + 1 + ind);
        ll max_x = unique(X + 1, X + 1 + ind) - X - 1;
        for(int i = 1; i <= ind; i++)
        {
            ans += tree[1] * (line[i].y - line[i - 1].y);//第一条边不计算面积
            ll L = lower_bound(X + 1, X + 1 + max_x, line[i].lx) - X;
            ll R = lower_bound(X + 1, X + 1 + max_x, line[i].rx) - X;
            update(L, R, 1, 1, max_x, line[i].d);
        }
        printf("Test case #%d\nTotal explored area: %.2lf\n\n", _++, ans);
    }
    return 0;
}
```



##### · 多次覆盖问题（进阶）

```c++
1.覆盖的面积
    
写法1：区间处理为[L, R - 1]，线段树叶子结点为一个点即如[1, 1], [3, 3] -- 实际上分别代表区间[1, 2], [3, 4]
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cstring>
using namespace std;
#define ls (p << 1)
#define rs (p << 1 | 1)
#define ll int
#define db double
const ll N = 1e5 + 5;
ll n, T;
ll cnt[N];
db tree[N], tree2[N], xx[N];//tree[]维护独立有效长度，tree2[]维护被覆盖的有效长度
struct Line{
    db lx, rx, h;
    ll d;
    Line(){}
    Line(db a, db b, db c, ll dd) {lx = a, rx = b, h = c, d = dd;}
    bool operator <(const Line &m) const{ return h < m.h;}
}line[N];
void push_up(ll p, ll pl, ll pr)
{
    if(cnt[p])
        tree[p] = xx[pr + 1] - xx[pl];//注意要线段树区间如[1, 2]代表实际区间[1, 3]，长度按实际区间长度算
    else if(pl == pr)
        tree[p] = 0;
    else
        tree[p] = tree[ls] + tree[rs];

    if(cnt[p] > 1)
        tree2[p] = xx[pr + 1] - xx[pl];
    else if(pl == pr)
        tree2[p] = 0;
    else if(cnt[p] == 1)
        tree2[p] = tree[ls] + tree[rs];
    else
        tree2[p] = tree2[ls] + tree2[rs];
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && R >= pr)
    {
        cnt[p] += d;
        push_up(p, pl, pr);
        return;
    }
    if(pl == pr) return;
    ll mid = pl + pr >> 1;
    if(L <= mid) update(L, R, ls, pl, mid, d);
    if(R > mid) update(L, R, rs, mid + 1, pr, d);
    push_up(p, pl, pr);
}
int main()
{
    scanf("%d", &T);
    while(T--)
    {
        memset(cnt, 0, sizeof(cnt));
        memset(tree, 0, sizeof(tree));
        memset(tree2, 0, sizeof(tree2));
        db ans = 0;
        ll ind = 0;
        scanf("%d", &n);
        for(int i = 1; i <= n; i++)
        {
            db x1, y1, x2, y2;
            scanf("%lf%lf%lf%lf", &x1, &y1, &x2, &y2);
            line[++ind] = Line(x1, x2, y1, 1);
            xx[ind] = x1;
            line[++ind] = Line(x1, x2, y2, -1);
            xx[ind] = x2;
        }
        sort(line + 1, line + 1 + ind);
        sort(xx + 1, xx + 1 + ind);
        ll num = unique(xx + 1, xx + 1 + ind) - xx - 1;
        for(int i = 1; i <= ind; i++) 
        {
            ans += tree2[1] * (line[i].h - line[i - 1].h);
            ll L = lower_bound(xx + 1, xx + 1 + num, line[i].lx) - xx;
            ll R = lower_bound(xx + 1, xx + 1 + num, line[i].rx) - xx;
            update(L, R - 1, 1, 1, num, line[i].d);
        }
        printf("%.2lf\n", ans);
    }
    return 0;
}

写法2：将线段树叶子结点设为[pl, pl + 1]，即设定最小结点是一个长度为1的小区间
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cstring>
using namespace std;
#define ls (p << 1)
#define rs (p << 1 | 1)
#define ll int
#define db double
const ll N = 1e5 + 5;
ll n, T;
ll cnt[N];
db tree[N], tree2[N], xx[N];//tree[]维护有效长度len1(至少覆盖过一次)，tree2[]维护至少被覆盖两次的有效长度len2
struct Line{
    db lx, rx, h;
    ll d;
    Line(){}
    Line(db a, db b, db c, ll dd) {lx = a, rx = b, h = c, d = dd;}
    bool operator <(const Line &m) const{ return h < m.h;}
}line[N];
void push_up(ll p, ll pl, ll pr)
{
    //len1
    if(cnt[p])
        tree[p] = xx[pr] - xx[pl];
    else if(pl + 1 == pr)
        tree[p] = 0;
    else
        tree[p] = tree[ls] + tree[rs];
    
    //对于len2的更新情况进行讨论：
    //经过一次 覆盖 或 撤除 后 (cnt[p] += d)：
    //1. 当区间p至少共被一次性完全覆盖过两次时，此时 cnt[p] >= 2，说明整段p都是有效覆盖长度len2，即 tree2[p] = xx[pr] - xx[pl]
    //2. 当区间p没达到有效覆盖要求，且为叶子结点(这里是长度为1的最小区间)时，没办法再向下索取长度，只能 tree2[p] = 0
    //3. 当区间p(非叶子)被一次性完全覆盖过一次时，此时cnt[p] == 1，说明它有资格向下从左右子索取同样是曾被覆盖过一次的长度len1，这部分加起来就覆盖了两次
    //   即 tree2[p] = tree[ls] + tree[rs] 
    //4. 当区间p(非叶子)从没被一次性完全覆盖过时，此时cnt[p] == 0，注意只是没有一次性覆盖完，它还可以从左右子len2索取有效覆盖长度
    //   即 tree2[p] = tree2[ls] + tree2[rs]
    if(cnt[p] > 1)
        tree2[p] = xx[pr] - xx[pl];//对len2，至少覆盖两次时 长度有效
    else if(pl + 1 == pr)
        tree2[p] = 0;
    else if(cnt[p] == 1)//重点
        tree2[p] = tree[ls] + tree[rs];
    else
        tree2[p] = tree2[ls] + tree2[rs];
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && R >= pr)
    {
        cnt[p] += d;
        push_up(p, pl, pr);
        return;
    }
    if(pl + 1 == pr) return;
    ll mid = pl + pr >> 1;
    if(L <= mid) update(L, R, ls, pl, mid, d);
    if(R > mid) update(L, R, rs, mid, pr, d);
    push_up(p, pl, pr);
}
int main()
{
    scanf("%d", &T);
    while(T--)
    {
        memset(cnt, 0, sizeof(cnt));
        memset(tree, 0, sizeof(tree));
        memset(tree2, 0, sizeof(tree2));
        db ans = 0;
        ll ind = 0;
        scanf("%d", &n);
        for(int i = 1; i <= n; i++)
        {
            db x1, y1, x2, y2;
            scanf("%lf%lf%lf%lf", &x1, &y1, &x2, &y2);
            line[++ind] = Line(x1, x2, y1, 1);
            xx[ind] = x1;
            line[++ind] = Line(x1, x2, y2, -1);
            xx[ind] = x2;
        }
        sort(line + 1, line + 1 + ind);
        sort(xx + 1, xx + 1 + ind);
        ll num = unique(xx + 1, xx + 1 + ind) - xx - 1;
        for(int i = 1; i <= ind; i++) 
        {
            ans += tree2[1] * (line[i].h - line[i - 1].h);
            ll L = lower_bound(xx + 1, xx + 1 + num, line[i].lx) - xx;
            ll R = lower_bound(xx + 1, xx + 1 + num, line[i].rx) - xx;
            update(L, R, 1, 1, num, line[i].d);
        }
        printf("%.2f\n", ans);
    }
    return 0;
}
```





#### 矩形周长并

```c++
1.Picture
给n个矩形，求n个矩形合并后的周长是多少。(数据都是整数)
重点：判断当前扫描线是否被已有长度所覆盖，若是则不能计算该扫描线。
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <cstring>
#include <cmath>
using namespace std;
#define ls (p << 1)
#define rs (p << 1 | 1)
#define ll int
const ll N = 1e4 + 5;//这里一定要开够大
struct Line{
    ll lx, rx, h, d;
    Line(ll a = 0, ll b = 0, ll c = 0, ll dd = 0) {lx = a, rx = b, h = c, d = dd;}
    bool operator <(const Line &m) const{ return h < m.h;}
}line[N << 1];
struct nd{
    ll len;               //总有效长度
    bool l_cover, r_cover;//标记线段左、右端点是否被覆盖
    nd(ll s = 0, bool a = 0, bool b = 0) {len = s, l_cover = a, r_cover = b;}
}tree[N << 2];
ll n, ind = 0, ans = 0, last = 0;
ll cnt[N << 2], num[N << 2], xx[N];//cnt记录区间入边出边情况，而num记录区间内有多少条独立的线段以计算出有多少对竖边
void push_up(ll p, ll pl, ll pr)
{
    if(cnt[p])            //区间p有效，且整个区间p已被完全覆盖，则此时独立线段个数仅1条
    {
        tree[p] = nd(xx[pr] - xx[pl], 1, 1);
        num[p] = 1;
    }
    else if(pl + 1 == pr)
    {
        tree[p] = nd(0, 0, 0);
        num[p] = 0;       //这里记得清空为0，因为出边后叶子节点已经无法从下面索取到有效长度，拥有的线段数量自然也必须为0
    }
    else                  //向上传递覆盖性、有效长度 和 独立线段个数
    {
        tree[p] = nd(tree[ls].len + tree[rs].len, tree[ls].l_cover, tree[rs].r_cover);
        num[p] = num[ls] + num[rs];
        if(tree[ls].r_cover && tree[rs].l_cover) num[p]--;//左右子线段合并
    }
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && R >= pr)
    {
        cnt[p] += d;
        push_up(p, pl, pr);
        return;
    }
    if(pl + 1 == pr) return;
    ll mid = pl + pr >> 1;
    if(L <= mid) update(L, R, ls, pl, mid, d);
    if(R > mid) update(L, R, rs, mid, pr, d);

    push_up(p, pl, pr);
    // 也可以分出来写    
    // if(!cnt[p])
    // {
    //     tree[p] = nd(tree[ls].len + tree[rs].len, tree[ls].l_cover, tree[rs].r_cover);
    //     num[p] = num[ls] + num[rs];
    //     if(tree[ls].r_cover && tree[rs].l_cover) num[p]--;
    // }
}
int main()
{
    scanf("%d", &n);
    for(int i = 1; i <= n; i++)
    {
        ll x1, y1, x2, y2;
        scanf("%d%d%d%d", &x1, &y1, &x2, &y2);
        line[++ind] = Line(x1, x2, y1, 1);
        xx[ind] = x1;
        line[++ind] = Line(x1, x2, y2, -1);
        xx[ind] = x2;
    }
    sort(line + 1, line + 1 + ind);
    sort(xx + 1, xx + 1 + ind);
    ll max_x = unique(xx + 1, xx + 1 + ind) - xx - 1;
    for(int i = 1; i <= ind; i++)
    {
        ll L = lower_bound(xx + 1, xx + 1 + max_x, line[i].lx) - xx;
        ll R = lower_bound(xx + 1, xx + 1 + max_x, line[i].rx) - xx;
        update(L, R, 1, 1, max_x, line[i].d);
        ans += 2 * num[1] * (line[i + 1].h - line[i].h);//总竖边长(2 * 竖边对数num * 长度)
        ans += abs(tree[1].len - last);                 //底边长(除去了覆盖部分)
        last = tree[1].len;
    }
    printf("%d\n", ans);
    return 0;
}
```



#### 矩体体积并（三维扫描线）

```c++
1.Get The Treasury （枚举 + 三维扫描线）
参考：https://blog.csdn.net/qq_41280600/article/details/104094400
题意：给若干个矩体（长方体），求至少重合三次的体积之和。
思路：维护x, y的面积并，枚举 z (|z| < 500 且为整数)，每次枚举都相当于寻找高度为1的矩体三次重叠部分的体积 -- 体积元。
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
#define ls (p << 1)
#define rs (p << 1 | 1)
#define lc ls, pl, mid
#define rc rs, mid + 1, pr
#define ll long long
const ll N = 2e4 + 5;
ll n, T, cnt[N << 2];
ll xx[N], yy[N], zz[N];
struct Line{
    ll lx, rx, y, d;
    ll z1, z2;//所属矩体高度范围
    Line(ll a = 0, ll b = 0, ll c = 0, ll dd = 0, ll e = 0, ll f = 0){ lx = a, rx = b, y = c, d = dd, z1 = e, z2 = f;}
    bool operator <(const Line &m) const{ return y < m.y;}
}line[N];
struct nd{
    ll len1, len2, len3;
    nd(ll l1 = 0, ll l2 = 0, ll l3 = 0) {len1 = l1, len2 = l2, len3 = l3;}
}tree[N << 2];
void build(ll p, ll pl, ll pr)
{
    tree[p] = nd(0, 0, 0);
    cnt[p] = 0;
    if(pl == pr) return;
    ll mid = pl + pr >> 1;
    build(lc);
    build(rc);
}
void push_up(ll p, ll pl, ll pr)                                      //记得 xx[pr + 1] - xx[pl]
{
    if(cnt[p]) tree[p].len1 = xx[pr + 1] - xx[pl];
    else if(pl == pr) tree[p].len1 = 0;
    else tree[p].len1 = tree[ls].len1 + tree[rs].len1;

    if(cnt[p] >= 2) tree[p].len2 = xx[pr + 1] - xx[pl];
    else if(pl == pr) tree[p].len2 = 0;
    else if(cnt[p] == 1) tree[p].len2 = tree[ls].len1 + tree[rs].len1;//只差一次覆盖，从len1拿
    else tree[p].len2 = tree[ls].len2 + tree[rs].len2;

    if(cnt[p] >= 3) tree[p].len3 = xx[pr + 1] - xx[pl];
    else if(pl == pr) tree[p].len3 = 0;
    else if(cnt[p] == 2) tree[p].len3 = tree[ls].len1 + tree[rs].len1;//只差一次覆盖，从len1拿
    else if(cnt[p] == 1) tree[p].len3 = tree[ls].len2 + tree[rs].len2;//还差两次覆盖，从len2拿
    else tree[p].len3 = tree[ls].len3 + tree[rs].len3;
    
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && R >= pr)
    {
        cnt[p] += d;
        push_up(p, pl, pr);
        return;
    }
    if(pl == pr) return;
    ll mid = pl + pr >> 1;
    if(L <= mid) update(L, R, lc, d);
    if(R > mid) update(L, R, rc, d);
    push_up(p, pl, pr);
}
int main()
{
    scanf("%lld", &T);
    for(int _ = 1; _ <= T; _++)
    {
        ll ind = 0, ans = 0;
        scanf("%lld", &n);

        for(int i = 1; i <= n; i++)
        {
            ll x1, y1, z1, x2, y2, z2;
            scanf("%lld%lld%lld%lld%lld%lld", &x1, &y1, &z1, &x2, &y2, &z2);
            line[++ind] = Line(x1, x2, y1, 1, z1, z2);
            xx[ind] = x1, zz[ind] = z1;
            line[++ind] = Line(x1, x2, y2, -1, z1, z2);
            xx[ind] = x2, zz[ind] = z2;
        }

        sort(xx + 1, xx + 1 + ind);
        sort(line + 1, line + 1 + ind);
        ll num_x = unique(xx + 1, xx + 1 + ind) - xx - 1;

        for(int z = -500; z <= 500; z++)             //遍历所有体积元
        {
            build(1, 1, num_x);
            ll area = 0, last = 0;                   //扫描面符合条件的面积
            for(int i = 1; i <= ind; i++)            //遍历所有属于体积元所在的矩体连结体的"扫描面"
            {
                if(line[i].z1 <= z && z < line[i].z2)//一个矩体的体积元个数为 z2 - z1 个，即区间 [z1, z2)，合起来恰好是该矩体的体积
                {
                    //这里不能直接 line[i].y - line[i-1].y，因为最开始一次扫描line[i-1].y不一定就是line[0].y 即不一定为0，所以很可能减多了
                    //这也是没有对 Z坐标 离散化处理的弊端
                    area += tree[1].len3 * (line[i].y - last);
                    last = line[i].y;
                    ll L = lower_bound(xx + 1, xx + 1 + num_x, line[i].lx) - xx;
                    ll R = lower_bound(xx + 1, xx + 1 + num_x, line[i].rx) - xx;
                    update(L, R - 1, 1, 1, num_x, line[i].d);
                }
            }
            ans += area;                             //相当于加一个高为1的矩体体积 ans += 1 * area
        }
        printf("Case %d: %lld\n", _, ans);
    }
    return 0;
}
```







### 其他问题

#### dfs序

* dfs序是指：每个节点在dfs深度优先遍历中的进出栈的时间序列。

```c++
1.Assign the task
题意：给你一棵树，共n个结点，每个结点具有一个颜色，可以对结点所处的子树（包括其自身）的染色 以及 查询某结点的颜色。
先建图存树，用 dfs序 将 树 区间化，可以求出每个节点的管辖区间（即 所得该节点区间 表示 以该节点为根的子树），以此维护任意一个子树或单个结点的变化。
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;
#define ls (p << 1)
#define rs (p << 1 | 1)
#define lc ls, pl, mid
#define rc rs, mid + 1, pr
#define ll int
const int N = 5e4 + 5;
ll tree[N << 2], tag[N << 2];
ll in[N], out[N];//记录结点在dfs序上进出栈的时间
ll n, q, time = 0;//时间
ll indegree[N];//记录入度，方便找树根
vector<ll> G[N];
void init()
{
    memset(indegree, 0, sizeof(indegree));
    memset(tree, -1, sizeof(tree));
    memset(tag, -1, sizeof(tag));
    for(int i = 1; i <= n; i++) G[i].clear();
    time = 0;
}
void dfs(int u)
{
    in[u] = ++time;
    for(int i = 0; i < G[u].size(); i++) dfs(G[u][i]);
    out[u] = time;
}
void push_down(ll p)
{
    if(tag[p] != -1)
    {
        tree[ls] = tree[rs] = tag[p];
        tag[ls] = tag[rs] = tag[p];
        tag[p] = -1;
    }
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && R >= pr)
    {
        tree[p] = tag[p] = d;
        return;
    }
    push_down(p);
    ll mid = pl + pr >> 1;
    if(L <= mid) update(L, R, lc, d);
    if(R > mid) update(L, R, rc, d);
}
ll query(ll x, ll p, ll pl, ll pr)
{
    if(pl == pr) return tree[p];
    push_down(p);
    ll mid = pl + pr >> 1;
    if(x <= mid) return query(x, lc);
    else return query(x, rc);
}
int main()
{
    int T;
    scanf("%d", &T);
    for(int _ = 1; _ <= T; _++)
    {
        printf("Case #%d:\n", _);
        scanf("%d", &n);
        init();
        for(int i = 1; i < n; i++)//n个结点建一颗单根树，需要 n-1 条边
        {
            ll u, v;
            scanf("%d%d", &v, &u);
            G[u].push_back(v);
            ++indegree[v];
        }
        //存储dfs时间序列
        for(int i = 1; i <= n; i++)
        {
            if(!indegree[i]) //从树根出发一次即可
            {
                dfs(i);
                break;
            }
        }
        scanf("%d", &q);
        while(q--)
        {
            char op[5];
            ll x, d;
            scanf("%s", &op);
            if(op[0] == 'T')
            {
                scanf("%d%d", &x, &d);
                update(in[x], out[x], 1, 1, n, d);
            }
            else
            {
                scanf("%d", &x);
                printf("%d\n", query(in[x], 1, 1, n));
            }
        }
    }
    return 0;
}
```



#### 混合多种区间操作

* 区间操作有：单点\区间修改（加值、乘值、置值、取反等）、单点\区间查询（区间和、区间平方和、区间立方和等）
* 重点：标记间的优先级（覆盖性）



```c++
1.Transformation
一个长为n的序列，对区间[l,r]有4种操作：每个数加c，每个数乘c，每个数置为c，查询每个数p次方后的区间和
思路1：对查询操作的p次方和，用s[3]分别存起1次到3次方的值，传递时结合完全平方式和完全立方式
对于(x + d)^3 = x^3 + 3 * x^2 * d + 3 * x * d^2 + d^3，拓展到长度为len的区间得到：Σ(x + d)^3 = Σx^3 + 3 * d * Σ(x^2) + 3 * d^2 * Σx + Σd^3
即tree[p].s[2] = tree[p].s[2] + 3 * d * tree[p].s[1] + 3 * d * d * tree[p].s[0] + len * d * d * d 
对 (x + d)^2 = x^2 + 2*d*x + d^2 同理。
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <vector>
#include <cstring>
#include <cmath>
using namespace std;
#define ls (p << 1)
#define rs (p << 1 | 1)
#define lc ls, pl, mid
#define rc rs, mid + 1, pr
#define ll int
#define mod(x) ((x) % 10007)
const int N = 1e5 + 5;
ll n, m;
struct nd{
    ll s[3];
    ll sum, mul, setx;
}tree[N << 2];
void addtag_sum(ll p, ll pl, ll pr, ll d)//注意三种求和的更新先后关系
{
    ll len = pr - pl + 1;
    tree[p].s[2] = mod(tree[p].s[2] + mod(3 * d * tree[p].s[1]) + mod(mod(3 * d * d) * tree[p].s[0]) + mod(mod(mod(len * d) * d) * d));
    tree[p].s[1] = mod(tree[p].s[1] + mod(2 * d * tree[p].s[0]) + mod(mod(len * d) * d));
    tree[p].s[0] = mod(tree[p].s[0] + mod(len * d));
    tree[p].sum  = mod(tree[p].sum + d);
}
void addtag_mul(ll p, ll pl, ll pr, ll d)
{
    tree[p].s[2] = mod(mod(mod(tree[p].s[2] * d) * d) * d);
    tree[p].s[1] = mod(mod(tree[p].s[1] * d) * d);
    tree[p].s[0] = mod(tree[p].s[0] * d);
    tree[p].sum  = mod(tree[p].sum * d);
    tree[p].mul  = mod(tree[p].mul * d);
}
void addtag_set(ll p, ll pl, ll pr, ll d)
{
    ll len = pr - pl + 1;
    tree[p].s[2] = mod(mod(mod(len * d) * d) * d);
    tree[p].s[1] = mod(mod(len * d) * d);
    tree[p].s[0] = mod(len * d);
    tree[p].setx = d;
    tree[p].mul = 1;//向下覆盖
    tree[p].sum = 0;
}
void push_up(ll p)
{
    for(int i = 0; i < 3; i++) tree[p].s[i] = mod(tree[ls].s[i] + tree[rs].s[i]);
}
void push_down(ll p, ll pl, ll pr)//注意标记更新的先后关系和覆盖性
{
    ll mid = pl + pr >> 1;
    if(tree[p].setx != -1)//记得它会覆盖子区间的其他标记
    {
        addtag_set(lc, tree[p].setx);
        addtag_set(rc, tree[p].setx);
        tree[p].setx = -1;
    }

    if(tree[p].mul != 1)
    {
        addtag_mul(lc, tree[p].mul);
        addtag_mul(rc, tree[p].mul);
        tree[p].mul = 1;
    }
    if(tree[p].sum)
    {
        addtag_sum(lc, tree[p].sum);
        addtag_sum(rc, tree[p].sum);
        tree[p].sum = 0;
    }
}
void build(ll p, ll pl, ll pr)
{
    tree[p].s[0] = tree[p].s[1] = tree[p].s[2] = tree[p].sum = 0;
    tree[p].mul = 1, tree[p].setx = -1;
    if(pl == pr) return;
    ll mid = pl + pr >> 1;
    build(lc);
    build(rc);
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll op, ll d)
{
    if(L <= pl && R >= pr)
    {
        if(op == 1) addtag_sum(p, pl, pr, d);
        else if(op == 2) addtag_mul(p, pl, pr, d);
        else addtag_set(p, pl, pr, d);
        return;
    }
    push_down(p, pl, pr);
    ll mid = pl + pr >> 1;
    if(L <= mid) update(L, R, lc, op, d);
    if(R > mid) update(L, R, rc, op, d);
    push_up(p);
}
ll query(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && R >= pr) return mod(tree[p].s[d]);
    push_down(p, pl, pr);
    ll mid = pl + pr >> 1, ans = 0;
    if(L <= mid) ans = mod(ans + query(L, R, lc, d));
    if(R > mid) ans = mod(ans + query(L, R, rc, d));
    return mod(ans);
}
int main()
{
    while(~scanf("%d%d", &n, &m))//用 while(~scanf("%d%d", &n, &m), n) 会WA
    {
        if(n + m == 0) break;
        build(1, 1, n);
        while(m--)
        {
            ll op, x, y, k;
            scanf("%d%d%d%d", &op, &x, &y, &k);
            if(op == 4) printf("%d\n", query(x, y, 1, 1, n, k - 1));
            else update(x, y, 1, 1, n, op, k);
        }
    }
    return 0;
}



2.序列操作
两种区间修改：
    区间置为0\1 、 区间取反
两种查询操作：
    区间和、最长连续1串长度
/*
如序列 01101011
            01101011
      0110           1011
   01      10     10      11
 0   1   1   0   1   0   1   1
push_up():
    1. 1的个数sum，即两子区间的sum之和
    2. 连续1的最大数量cnt，
        若 左儿子的cntr && 右儿子的cntl，则 cnt = 左儿子区间从右往左的连续1数量cntr + 右儿子区间从左往右的连续1数量cntl
        若有一个为0则无法连接，取max(左儿子cnt，右儿子cnt)
    3. 由于翻转操作的存在，连续0串的最大数量也需同上存储，最终视 是否进行了翻转 来决定是取 连续0 还是 连续1 的个数
操作优先级：取反（单点修改置值1或0），则当 (tag0 || tag1) == 1 时取反操作应用到两个标记上，对两标记取反，否则对 nega_tag取反
该题实际操作过程中极容易出错。
*/
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
#define ll int
#define il inline
#define ls (p << 1)
#define rs (p << 1 | 1)
const int N = 2e5 + 10;
struct node{
    ll sum, cnt[2], cntl[2], cntr[2];//要查询的值，1的个数(亦可得出0的个数)、连续0\1的最大数量，从左往右最长0\1个数cntl，从右往左最长0\1个数cntr
    ll set_tag, nega_tag;//lazy标记，set_tag初始为-1，若为0或1则说明置为0或1，nega_tag 为1说明进行取反操作
    ll len;//特别地，存一下该区间长度
}tree[N << 2];
ll n, m;
il void push_up(ll p)
{
    tree[p].sum = tree[ls].sum + tree[rs].sum;
    for(int i = 0; i < 2; i++)
    {
        tree[p].cntl[i] = tree[ls].cntl[i];//先传递左子的cntl，作为当前区间的cntl
        if(tree[ls].cntl[i] == tree[ls].len) tree[p].cntl[i] += tree[rs].cntl[i]; //如果左子是全1串，那么当前区间的cntl可以继续接上右子的cntl
        tree[p].cntr[i] = tree[rs].cntr[i];//同上
        if(tree[rs].cntr[i] == tree[rs].len) tree[p].cntr[i] += tree[ls].cntr[i];
        //上述更新的是cntl 和 cntr，现在更新cnt
        tree[p].cnt[i] = max(tree[ls].cntr[i] + tree[rs].cntl[i], max(tree[ls].cnt[i], tree[rs].cnt[i])); //合并后中间连续串 与 左、右连续子串 求三者的最大
    }
}
il void push_set(ll p, ll d)
{
    tree[p].set_tag = d, tree[p].nega_tag = 0, tree[p].sum = d * tree[p].len;
    tree[p].cnt[d] = tree[p].cntl[d] = tree[p].cntr[d] = tree[p].len;
    tree[p].cnt[!d] = tree[p].cntl[!d] = tree[p].cntr[!d] = 0;
}
il void push_rev(ll p)
{
    tree[p].sum = tree[p].len - tree[p].sum;
    swap(tree[p].cnt[0], tree[p].cnt[1]);
    swap(tree[p].cntl[0], tree[p].cntl[1]);
    swap(tree[p].cntr[0], tree[p].cntr[1]);
    if(tree[p].set_tag != -1) tree[p].set_tag ^= 1;//影响置值
    else tree[p].nega_tag ^= 1;
}
il void push_down(ll p, ll pl, ll pr)
{
    ll mid = (pl + pr) >> 1;
    if(tree[p].set_tag != -1)
    {
        push_set(ls, tree[p].set_tag);
        push_set(rs, tree[p].set_tag);
        tree[p].set_tag = -1;
    }
    if(tree[p].nega_tag)
    {
        push_rev(ls);
        push_rev(rs);
        tree[p].nega_tag = 0;
    }
}
void build(ll p, ll pl, ll pr)
{
    tree[p].set_tag = -1, tree[p].len = pr - pl + 1;
    if(pl == pr)
    {
        ll d; scanf("%d", &d);
        tree[p].sum = d, tree[p].cnt[d] = tree[p].cntl[d] = tree[p].cntr[d] = 1;
        return;
    }
    ll mid = (pl + pr) >> 1;
    build(ls, pl, mid);
    build(rs, mid + 1, pr);
    push_up(p);
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && R >= pr){ d == 2 ? push_rev(p) : push_set(p, d); return;}
    push_down(p, pl, pr);
    ll mid = (pl + pr) >> 1;
    if(L <= mid) update(L, R, ls, pl, mid, d);
    if(R > mid) update(L, R, rs, mid + 1, pr, d);
    push_up(p);
}
ll query(ll L, ll R, ll p, ll pl, ll pr)
{
    if(L <= pl && R >= pr) return tree[p].sum;
    push_down(p, pl, pr);
    ll mid = (pl + pr) >> 1, res = 0;
    if(L <= mid) res += query(L, R, ls, pl, mid);
    if(R > mid) res += query(L, R, rs, mid + 1, pr);
    return res;
}
ll query2(ll L, ll R, ll p, ll pl, ll pr)
{
    if(L <= pl && R >= pr) return tree[p].cnt[1];
    push_down(p, pl, pr);
    ll mid = (pl + pr) >> 1;
    if(R <= mid) return query2(L, R, ls, pl, mid);
    else if(L > mid) return query2(L, R, rs, mid + 1, pr);
    else return max(max(query2(L, mid, ls, pl, mid), query2(mid + 1, R, rs, mid + 1, pr)), min(tree[ls].cntr[1], mid + 1 - L) + min(tree[rs].cntl[1], R - mid));
}
int main()
{
    ll n, m;
    scanf("%d%d", &n, &m);
    build(1, 1, n); 
    while(m--)
    {
        ll q, L, R, d;
        scanf("%d%d%d", &q, &L, &R);
        ++L, ++R;
        if(q == 3) printf("%d\n", query(L, R, 1, 1, n));
        else if(q == 4) printf("%d\n", query2(L, R, 1, 1, n));
        else update(L, R, 1, 1, n, q);
    }
    return 0;
}
//query2如下写法也行
// node query2(ll L, ll R, ll p, ll pl, ll pr)
// {
//     if(L <= pl && R >= pr) 
//     {
//         return tree[p];
//     }
//     push_down(p, pl, pr);
//     ll mid = (pl + pr) >> 1;
//     if(L <= mid && R > mid)
//     {
//         node lans, rans, ans;
//         lans = query2(L, R, ls, pl, mid);
//         rans = query2(L, R, rs, mid + 1, pr);
//         ans.cntl[1] = lans.cntl[1];
//         if(lans.cntl[1] == lans.len) ans.cntl[1] += rans.cntl[1];
//         ans.cntr[1] = rans.cntr[1];
//         if(rans.cntr[1] == rans.len) ans.cntr[1] += lans.cntr[1];
//         ans.cnt[1] = max(lans.cntr[1] + rans.cntl[1], max(lans.cnt[1], rans.cnt[1]));
//         return ans;
//     }
//     if(L <= mid) return query2(L, R, ls, pl, mid);
//     if(R > mid) return query2(L, R, rs, mid + 1, pr);
// }

```



#### 基于二叉树结构的二分搜索



```c++
1.Vases and Flowers
操作1：从A开始，只对空瓶插花，最多插F朵花或花瓶遍历完时停止；
操作2：相当于先 求区间和 再 清空区间。
操作2实现简单，而操作1则需要利用 线段树的二分结构 去 二分搜索 第一个 和 最后一个 空瓶的位置。
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
#define ls (p << 1)
#define rs (p << 1 | 1)
#define lc ls, pl, mid
#define rc rs, mid + 1, pr
#define ll int
const int N = 5e4 + 5;
ll n, m;
ll tree[N << 2], tag[N << 2];
void push_up(ll p) {tree[p] = tree[ls] + tree[rs];}
void push_down(ll p, ll pl, ll pr)
{
    if(tag[p] != -1)
    {
        ll mid = pl + pr >> 1;
        tree[ls] = tag[p] * (mid - pl + 1);
        tree[rs] = tag[p] * (pr - mid);
        tag[ls] = tag[rs] = tag[p];
        tag[p] = -1;
    }
}
void build(ll p, ll pl, ll pr)
{
    tree[p] = 0, tag[p] = -1;
    if(pl == pr) return;
    ll mid = pl + pr >> 1;
    build(lc);
    build(rc);
}
void update(ll L, ll R, ll p, ll pl, ll pr, ll d)
{
    if(L <= pl && R >= pr)
    {
        tree[p] = d * (pr - pl + 1);
        tag[p] = d;
        return;
    }
    push_down(p, pl, pr);
    ll mid = pl + pr >> 1;
    if(L <= mid) update(L, R, lc, d);
    if(R > mid) update(L, R, rc, d);
    push_up(p);
}
ll query_sum(ll L, ll R, ll p, ll pl, ll pr)
{
    if(L <= pl && R >= pr) return tree[p];
    push_down(p, pl, pr);
    ll mid = pl + pr >> 1, res = 0;
    if(L <= mid) res += query_sum(L, R, lc);
    if(R > mid) res += query_sum(L, R, rc);
    return res;
}
ll query_ind(ll st, ll cnt)//cnt为要找第几个0
{
    ll l = st, r = n, res = -1;
    while(l <= r)
    {
        ll mid = l + r >> 1;
        ll sum = mid - st + 1 - query_sum(st, mid, 1, 1, n);
        if(sum >= cnt) res = mid, r = mid - 1;//数量充分，收束右端
        else l = mid + 1;
    }
    return res;
}
int main()
{
    int T;
    scanf("%d", &T);
    while(T--)
    {
        scanf("%d%d", &n, &m);
        build(1, 1, n);
        while(m--)
        {
            ll op, a, b, posl, posr;
            scanf("%d%d%d", &op, &a, &b);
            ++a;
            if(op == 1)
            {
                posl = query_ind(a, 1);//第1个空花瓶位置
                if(posl == -1)//如果一个都没有就输出
                    printf("Can not put any one.\n");
                else
                {
                    ll sum = n - posl + 1 - query_sum(posl, n, 1, 1, n);//用于判断给的花是不是比空花瓶多
                    posr = query_ind(a, min(b, sum));//第b个空花瓶位置
                    printf("%d %d\n", posl - 1, posr - 1);
                    update(posl, posr, 1, 1, n, 1);
                }
            }
            else
            {
                ++b;
                printf("%d\n", query_sum(a, b, 1, 1, n));
                update(a, b, 1, 1, n, 0);
            }
        }
        printf("\n");
    }
    return 0;
}
```



### 模板

不带Lazy-tag

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
#define endl '\n'
#define gcd(a, b) __gcd(a, b)
#define mk(a, b) make_pair((a), (b))
#define lowbit(x) ((x) & -(x))
const int N = 2e5 + 10, inf = 1e9;
const ll linf = 1e18;

struct Info{
    ll val;

    Info(ll a = 0)
    {
        val = a;
    }
    friend Info operator +(Info a, Info b)
    {
        
        return a;
    }
};

class SegTree{

public:

    #define ls (p << 1)
    #define rs (p << 1 | 1)
    #define mid (pl + pr >> 1)

    int n;
    vector<Info> tree;
    
    SegTree() : n(0) {}
    SegTree(int nn, Info X = Info())
    {
        init(nn, X);
    }

    void init(int nn, Info X = Info()) // 初始化初值
    {
        n = nn;
        tree.assign(n << 2, Info());
        auto build = [&](auto self, ll p, ll pl, ll pr)
        {
            if(pl == pr)
            {
                tree[p] = X;
                return ;
            }
            self(self, ls, pl, mid);
            self(self, rs, mid + 1, pr);
            push_up(p);
        };
        build(build, 1, 1, n);
    }

    void setval(vector<Info> arr) // 置值
    {
        auto build = [&](auto self, ll p, ll pl, ll pr)
        {
            if(pl == pr)
            {
                tree[p] = arr[pl];
                return ;
            }
            self(self, ls, pl, mid);
            self(self, rs, mid + 1, pr);
            push_up(p);
        };
        build(build, 1, 1, n);
    }

    void push_up(ll p)
    {
        tree[p] = tree[ls] + tree[rs];
    }

    void update(ll x, ll p, ll pl, ll pr, Info X) // 单点修改
    {
        if(pl == pr)
        {
            tree[p] = X;
            return ;
        }
        if(x <= mid) update(x, ls, pl, mid, X);
        else update(x, rs, mid + 1, pr, X);
        push_up(p);
    }
    void update(ll x, Info X)
    {
        update(x, 1, 1, n, X);
    }

    Info query(ll x, ll p, ll pl, ll pr) // 单点查询
    {
        if(pl == pr)
        {
            return tree[p];
        }
        if(x <= mid) return query(x, ls, pl, mid);
        else return query(x, rs, mid + 1, pr);
    }
    Info query(ll x) 
    {
        return query(x, 1, 1, n);
    }

    Info query_range(ll L, ll R, ll p, ll pl, ll pr) // 区间查询
    {
        if(pl >= L && pr <= R)
        {
            return tree[p];
        }
        if(mid >= R) return query_range(L, R, ls, pl, mid);
        else if(mid < L) return query_range(L, R, rs, mid + 1, pr);
        return query_range(L, R, ls, pl, mid) + query_range(L, R, rs, mid + 1, pr);
    }
    Info query_range(ll L, ll R)
    {
        return query_range(L, R, 1, 1, n);
    }

    void _debug(ll p, ll pl, ll pr)
    {
        auto PRINT = [&](auto self, ll p, ll pl, ll pr)
        {
            if(pl == pr)
            {
                // cout << tree[p].val << " "; 
                return ;
            }
            self(self, ls, pl, mid);
            self(self, rs, mid + 1, pr);
        };
        PRINT(PRINT, p, pl, pr);
        cout << endl;
    }

    #undef ls
    #undef rs
    #undef mid
};

void Solve()
{
    ll n, q;
    cin >> n >> q;

    vector<Info> arr(n + 1);
    for(int i = 1; i <= n; ++i)
    {
        ll x; cin >> x;
        arr[i] = Info(x);
    }

    SegTree seg(n);
    seg.setval(arr);
    while(q--)
    {
        ll op, x, y;
        cin >> op >> x >> y;
        if(op == 1)
        {
            seg.update(x, Info(y));
        }
        else
        {
            cout << seg.query_range(x, y).val << endl;
        }
    }
}

int main()
{
    untie();
    int T = 1;
    // cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```

带Lazy-tag

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
#define endl '\n'
#define gcd(a, b) __gcd(a, b)
#define mk(a, b) make_pair((a), (b))
#define lowbit(x) ((x) & -(x))
const int N = 2e5 + 10, inf = 1e9;
const ll linf = 1e18;

struct Info{
    ll val;

    Info(ll a = 0) // 注意初值
    {
        val = a;
    }
    friend Info operator +(Info a, Info b)
    {
        
        return a;
    }
};

struct Tag{
    ll tag1;

    Tag(ll a = -1) // 注意初值
    {
        tag1 = a;
    }
};

class LazySegTree{

public:

    #define ls (p << 1)
    #define rs (p << 1 | 1)
    #define mid (pl + pr >> 1)

    int n;
    vector<Info> tree;
    vector<Tag> tag;
    
    LazySegTree() : n(0) {}
    LazySegTree(int nn, Info X = Info(), Tag Y = Tag())
    {
        init(nn, X, Y);
    }

    void init(int nn, Info X = Info(), Tag Y = Tag()) // 初始化初值
    {
        n = nn;
        tree.assign(n << 2, Info());
        tag.assign(n << 2, Tag());
        auto build = [&](auto self, ll p, ll pl, ll pr)
        {
            if(pl == pr)
            {
                tree[p] = X;
                tag[p] = Y;
                return ;
            }
            self(self, ls, pl, mid);
            self(self, rs, mid + 1, pr);
            push_up(p);
        };
        build(build, 1, 1, n);
    }

    void setval(vector<Info> arr) // 置值
    {
        auto build = [&](auto self, ll p, ll pl, ll pr)
        {
            if(pl == pr)
            {
                tree[p] = arr[pl];
                return ;
            }
            self(self, ls, pl, mid);
            self(self, rs, mid + 1, pr);
            push_up(p);
        };
        build(build, 1, 1, n);
    }

    void addTag1(ll p, ll pl, ll pr, ll d) // 记得标记对 tree[p] 的更新，以及标记间的影响
    {

    }
    void addTag2(ll p, ll pl, ll pr, ll d);
    void addTag3(ll p, ll pl, ll pr, ll d);
    void push_down(ll p, ll pl, ll pr) // 标记优先级越高，越先更新，以此覆盖优先级低的标记
    {
        if(tag[p].tag1 != -1)
        {
            addTag1(ls, pl, mid, tag[p].tag1);
            addTag1(rs, mid + 1, pr, tag[p].tag1);
            tag[p].tag1 = -1;
        }

    }

    void push_up(ll p)
    {
        tree[p] = tree[ls] + tree[rs];
    }

    void update(ll x, ll p, ll pl, ll pr, Info X) // 单点修改
    {
        if(pl == pr)
        {
            tree[p] = X;
            return ;
        }
        push_down(p, pl, pr);
        if(x <= mid) update(x, ls, pl, mid, X);
        else update(x, rs, mid + 1, pr, X);
        push_up(p);
    }
    void update(ll x, Info X)
    {
        update(x, 1, 1, n, X);
    }

    void update_range(ll L, ll R, ll p, ll pl, ll pr, ll op, ll d) // 区间修改（对第 op 个标记修改）
    {
        if(pl >= L && pr <= R)
        {
            if(op == 1) addTag1(p, pl, pr, d);

            return ;
        }
        push_down(p, pl, pr);
        if(L <= mid) update_range(L, R, ls, pl, mid, op, d);
        if(R > mid) update_range(L, R, rs, mid + 1, pr, op, d);
        push_up(p);
    }
    void update_range(ll L, ll R, ll op, ll d)
    {
        update_range(L, R, 1, 1, n, op, d);
    }

    Info query(ll x, ll p, ll pl, ll pr) // 单点查询
    {
        if(pl == pr)
        {
            return tree[p];
        }
        push_down(p, pl, pr);
        if(x <= mid) return query(x, ls, pl, mid);
        else return query(x, rs, mid + 1, pr);
    }
    Info query(ll x) 
    {
        return query(x, 1, 1, n);
    }

    Info query_range(ll L, ll R, ll p, ll pl, ll pr) // 区间查询
    {
        if(pl >= L && pr <= R)
        {
            return tree[p];
        }
        push_down(p, pl, pr);
        if(mid >= R) return query_range(L, R, ls, pl, mid);
        else if(mid < L) return query_range(L, R, rs, mid + 1, pr);
        return query_range(L, R, ls, pl, mid) + query_range(L, R, rs, mid + 1, pr);
    }
    Info query_range(ll L, ll R)
    {
        return query_range(L, R, 1, 1, n);
    }

    void _debug(ll p, ll pl, ll pr)
    {
        auto PRINT = [&](auto self, ll p, ll pl, ll pr)
        {
            if(pl == pr)
            {
                // cout << tree[p].val << " "; 
                return ;
            }
            self(self, ls, pl, mid);
            self(self, rs, mid + 1, pr);
        };
        PRINT(PRINT, p, pl, pr);
        cout << endl;
    }

    #undef ls
    #undef rs
    #undef mid
};

void Solve()
{
    ll n, q;
    cin >> n >> q;

    vector<Info> arr(n + 1);
    for(int i = 1; i <= n; ++i)
    {
        ll x; cin >> x;
        arr[i] = Info(x);
    }

    LazySegTree seg(n);
    seg.setval(arr);
    while(q--)
    {
        ll op, x, y;
        cin >> op >> x >> y;
        if(op == 1)
        {
            // seg.update(x, Info(y));
            seg.update_range(x, y, op, 1);
        }
        else
        {
            cout << seg.query_range(x, y).val << endl;
        }
    }
}

int main()
{
    untie();
    int T = 1;
    // cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```



### 例题



```c++
1.ABC343 F
要求维护最大值、次大值以及最大值个数、次大值个数。
涉及单点修改和区间查询。
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define bll __int128_t
#define all(v) v.begin(), v.end()
#define endl '\n'
#define gcd(a, b) __gcd(a, b)
#define mk(a, b) make_pair((a), (b))
#define lowbit(x) ((x) & -(x))

const int N = 2e5 + 10, inf = 1e9;
const ll linf = 1e18;
const bll mod = (bll)1e31 + 7177;

struct info{
    ll mx1, mx2;
    ll cnt1, cnt2;
    info(ll a = 0, ll b = 0, ll c = 0, ll d = 0)
    {
        mx1 = a, mx2 = b, cnt1 = c, cnt2 = d;
    }
    friend info operator +(info a, info b)
    {
        if(a.mx1 == b.mx1)
        {
            if(a.mx2 < b.mx2) swap(a, b);
            a.cnt1 += b.cnt1;
            if(a.mx2 == b.mx2)
            {
                a.cnt2 += b.cnt2;
            }
            return a;
        }
        if(a.mx1 < b.mx1) swap(a, b);
        if(b.mx1 > a.mx2)
        {
            a.mx2 = b.mx1;
            a.cnt2 = b.cnt1;
        }
        else if(b.mx1 == a.mx2)
        {
            a.cnt2 += b.cnt1;
        }
        return a;
    }
};

class SegTree{

public:

    #define ls (p << 1)
    #define rs (p << 1 | 1)

    int n;
    vector<info> tree;
    
    SegTree() : n(0) {}
    SegTree(int nn, info val = info())
    {
        init(nn, val);
    }

    void init(int nn, info val = info()) // 初始化初值
    {
        n = nn;
        tree.assign(n << 2, info());
        auto build = [&](auto self, ll p, ll pl, ll pr)
        {
            if(pl == pr)
            {
                tree[p] = val;
                return ;
            }
            ll mid = pl + pr >> 1;
            self(self, ls, pl, mid);
            self(self, rs, mid + 1, pr);
            push_up(p);
        };
        build(build, 1, 1, n);
    }

    void setval(vector<info> arr) // 置值
    {
        auto build = [&](auto self, ll p, ll pl, ll pr)
        {
            if(pl == pr)
            {
                tree[p] = arr[pl];
                return ;
            }
            ll mid = pl + pr >> 1;
            self(self, ls, pl, mid);
            self(self, rs, mid + 1, pr);
            push_up(p);
        };
        build(build, 1, 1, n);
    }

    void addtag(ll p, ll pl, ll pr)
    {

    }
    void push_down(ll p, ll pl, ll pr)
    {

    }
    void push_up(ll p)
    {
        tree[p] = tree[ls] + tree[rs];
    }

    void update(ll x, ll p, ll pl, ll pr, info d) // 单点修改
    {
        if(pl == pr)
        {
            tree[p] = d;
            return ;
        }
        push_down(p, pl, pr);
        ll mid = pl + pr >> 1;
        if(x <= mid) update(x, ls, pl, mid, d);
        else update(x, rs, mid + 1, pr, d);
        push_up(p);
    }
    void update(ll x, info d)
    {
        update(x, 1, 1, n, d);
    }

    void update_range(ll L, ll R, ll p, ll pl, ll pr, info d) // 区间修改
    {
        if(pl >= L && pr <= R)
        {

            return ;
        }
        push_down(p, pl, pr);
        ll mid = pl + pr >> 1;
        if(L <= mid) update_range(L, R, ls, pl, mid, d);
        if(R > mid) update_range(L, R, rs, mid + 1, pr, d);
        push_up(p);
    }
    void update_range(ll L, ll R, info d)
    {
        update_range(L, R, 1, 1, n, d);
    }

    info query(ll x, ll p, ll pl, ll pr) // 单点查询
    {
        if(pl == pr)
        {
            return tree[p];
        }
        push_down(p, pl, pr);
        ll mid = pl + pr >> 1;
        if(x <= mid) return query(x, ls, pl, mid);
        else return query(x, rs, mid + 1, pr);
    }
    info query(ll x) 
    {
        return query(x, 1, 1, n);
    }

    info query_range(ll L, ll R, ll p, ll pl, ll pr) // 区间查询
    {
        if(pl >= L && pr <= R)
        {
            return tree[p];
        }
        push_down(p, pl, pr);
        ll mid = pl + pr >> 1;
        if(mid >= R) return query_range(L, R, ls, pl, mid);
        else if(mid < L) return query_range(L, R, rs, mid + 1, pr);
        return query_range(L, R, ls, pl, mid) + query_range(L, R, rs, mid + 1, pr);
    }
    info query_range(ll L, ll R)
    {
        return query_range(L, R, 1, 1, n);
    }

    void _debug(ll p, ll pl, ll pr)
    {
        if(pl == pr)
        {
            // for(auto x : tree[p]) cout << x <<" "; cout << endl;
            return ;
        }
        ll mid = pl + pr >> 1;
        _debug(ls, pl, mid);
        _debug(rs, mid + 1, pr);
    }

    #undef ls
    #undef rs
};

void Solve()
{
    ll n, q;
    cin >> n >> q;

    vector<info> arr(n + 1);
    for(int i = 1; i <= n; ++i)
    {
        ll x; cin >> x;
        arr[i] = info(x, -1, 1, 0);
    }

    SegTree seg(n);
    seg.setval(arr);
    while(q--)
    {
        ll op, x, y;
        cin >> op >> x >> y;
        if(op == 1)
        {
            seg.update(x, info(y, -1, 1, 0));
        }
        else
        {
            cout << seg.query_range(x, y).cnt2 << endl;
        }
    }
}

int main()
{
    untie();
    int T = 1;
    // cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}



2.ABC341 E
涉及区间修改（翻转tag）和区间查询
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define bll __int128_t
#define all(v) v.begin(), v.end()
#define endl '\n'
#define gcd(a, b) __gcd(a, b)
#define mk(a, b) make_pair((a), (b))
#define lowbit(x) ((x) & -(x))
const int N = 2e5 + 10, inf = 1e9;
const ll linf = 1e18;
const bll mod = (bll)1e31 + 7177;

struct Info{
    ll ch[2];
    ll good;

    Info(ll a = 0, ll b = 0, ll c = 1) // 注意初值
    {
        ch[0] = a;
        ch[1] = b;
        good = c;
    }
    friend Info operator +(Info a, Info b)
    {
        if(a.good && b.good) a.good = (a.ch[1] != b.ch[0]);
        else a.good = 0;
        a.ch[1] = b.ch[1];
        return a;
    }
};

struct Tag{
    ll tag1;

    Tag(ll a = 0) // 注意初值
    {
        tag1 = a;
    }
};

class SegTree{

public:

    #define ls (p << 1)
    #define rs (p << 1 | 1)
    #define mid (pl + pr >> 1)

    int n;
    vector<Info> tree;
    vector<Tag> tag;
    
    SegTree() : n(0) {}
    SegTree(int nn, Info X = Info(), Tag Y = Tag())
    {
        init(nn, X, Y);
    }

    void init(int nn, Info X = Info(), Tag Y = Tag()) // 初始化初值
    {
        n = nn;
        tree.assign(n << 2, Info());
        tag.assign(n << 2, Tag());
        auto build = [&](auto self, ll p, ll pl, ll pr)
        {
            if(pl == pr)
            {
                tree[p] = X;
                tag[p] = Y;
                return ;
            }
            self(self, ls, pl, mid);
            self(self, rs, mid + 1, pr);
            push_up(p);
        };
        build(build, 1, 1, n);
    }

    void setval(vector<Info> arr) // 置值
    {
        auto build = [&](auto self, ll p, ll pl, ll pr)
        {
            if(pl == pr)
            {
                tree[p] = arr[pl];
                return ;
            }
            self(self, ls, pl, mid);
            self(self, rs, mid + 1, pr);
            push_up(p);
        };
        build(build, 1, 1, n);
    }

    void addTag1(ll p, ll pl, ll pr, ll d) // 记得标记对 tree[p] 的更新，以及标记间的影响
    {
        tag[p].tag1 ^= d;
        tree[p].ch[0] ^= d;
        tree[p].ch[1] ^= d;
    }
    void addTag2(ll p, ll pl, ll pr, ll d);
    void addTag3(ll p, ll pl, ll pr, ll d);
    void push_down(ll p, ll pl, ll pr) // 标记优先级越高，越先更新，以此覆盖优先级低的标记
    {
        if(tag[p].tag1 != 0)
        {
            addTag1(ls, pl, mid, tag[p].tag1);
            addTag1(rs, mid + 1, pr, tag[p].tag1);
            tag[p].tag1 = 0;
        }

    }

    void push_up(ll p)
    {
        tree[p] = tree[ls] + tree[rs];
    }

    void update(ll x, ll p, ll pl, ll pr, Info X) // 单点修改
    {
        if(pl == pr)
        {
            tree[p] = X;
            return ;
        }
        push_down(p, pl, pr);
        if(x <= mid) update(x, ls, pl, mid, X);
        else update(x, rs, mid + 1, pr, X);
        push_up(p);
    }
    void update(ll x, Info X)
    {
        update(x, 1, 1, n, X);
    }

    void update_range(ll L, ll R, ll p, ll pl, ll pr, ll op, ll d) // 区间修改（对第 op 个标记修改）
    {
        if(pl >= L && pr <= R)
        {
            if(op == 1) addTag1(p, pl, pr, d);

            return ;
        }
        push_down(p, pl, pr);
        if(L <= mid) update_range(L, R, ls, pl, mid, op, d);
        if(R > mid) update_range(L, R, rs, mid + 1, pr, op, d);
        push_up(p);
    }
    void update_range(ll L, ll R, ll op, ll d)
    {
        update_range(L, R, 1, 1, n, op, d);
    }

    Info query(ll x, ll p, ll pl, ll pr) // 单点查询
    {
        if(pl == pr)
        {
            return tree[p];
        }
        push_down(p, pl, pr);
        if(x <= mid) return query(x, ls, pl, mid);
        else return query(x, rs, mid + 1, pr);
    }
    Info query(ll x) 
    {
        return query(x, 1, 1, n);
    }

    Info query_range(ll L, ll R, ll p, ll pl, ll pr) // 区间查询
    {
        if(pl >= L && pr <= R)
        {
            return tree[p];
        }
        push_down(p, pl, pr);
        if(mid >= R) return query_range(L, R, ls, pl, mid);
        else if(mid < L) return query_range(L, R, rs, mid + 1, pr);
        return query_range(L, R, ls, pl, mid) + query_range(L, R, rs, mid + 1, pr);
    }
    Info query_range(ll L, ll R)
    {
        return query_range(L, R, 1, 1, n);
    }

    void _debug(ll p, ll pl, ll pr)
    {
        auto PRINT = [&](auto self, ll p, ll pl, ll pr)
        {
            if(pl == pr)
            {
                // cout << tree[p].val << " "; 
                return ;
            }
            self(self, ls, pl, mid);
            self(self, rs, mid + 1, pr);
        };
        PRINT(PRINT, p, pl, pr);
        cout << endl;
    }

    #undef ls
    #undef rs
    #undef mid
};

void Solve()
{
    ll n, q;
    cin >> n >> q;

    vector<Info> arr(n + 1);
    for(int i = 1; i <= n; ++i)
    {
        char ch; cin >> ch;
        ll x = ch - '0';
        arr[i] = Info(x, x, 1);
    }

    SegTree seg(n);
    seg.setval(arr);
    while(q--)
    {
        ll op, x, y;
        cin >> op >> x >> y;
        if(op == 1)
        {
            seg.update_range(x, y, 1, 1);
        }
        else
        {
            cout << (seg.query_range(x, y).good ? "Yes" : "No") << endl;
        }
    }
}

int main()
{
    untie();
    int T = 1;
    // cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```





















---

# 二. 图论

---

## 基础概念

顶点（Vertex）：图G中的数据元素称为顶点。一条边连接两个顶点。

无向图：若图G中的每条边都是没有方向的，则称图G为无向图。

有向图：若图G中的每条边都是有方向的，则称图G是有向图。

弧：即有向边。边的终点被称为弧头，起始点被称为弧尾。

度（degree）：一个顶点的度就是与该顶点相关联的边的数目。

出度：有向图中，顶点所连接的出边的数量。

入度：有向图中，顶点所连接的入边的数量。

环：环是一条只有第一个和最后一个顶点重复的非空路径。

有向无环图：又称 DAG图，即没有环的有向图。DAG图是动态规划和记忆化搜索的结构基础。

子图（Subgraph）：由图中部分节点以及这些节点间的边组成的图。

连通图（Connected Graph）：若图G中的任意两个结点 u 和 v，结点 u 与结点 v 相互可达，则成图G是连通图。

连通分量（Connected Component）：各节点间至少存在一条边可以连通。

树: 树就是无环连通图。互不相连的树组成的集合称为森林。具有n个顶点和n-1条边的连通图，是树的充要条件。如果一个无向连通图不包含回路(连通图中不存在环)，那么就是一棵树。

生成树：含有连通图全部顶点并有 n - 1 条边的连通子图。

邻接矩阵: 所谓矩阵其实就是二维数组。对于有n个顶点的图 G = (V, E) 来说，我们可以用一个 n × n 的矩阵 A 来表示 G 中各顶点的相邻关系。如果点 i 和 点 j 之间存在边(或弧)，则 `A[i][j] = 1`，否则 `A[i][j] = 0`。一个图的邻接矩阵是唯一的，矩阵的大小只与顶点个数N有关。

邻接表：邻接表的思想是，对于图中的每一个顶点，用一个数组来记录这个点和哪些点相连。由于相邻的点会动态的添加，所以对于每个点，我们需要用 vector 来记录。



## 图的存储

* **邻接矩阵**

  直接用 `gra[i][j] = w` 表示边 `(i, j)` 的权值 w。若为无权边，则用 1 表示有边，否则 0 表示无边

* 邻接表

  用链表`vector<Edge> e[N]` 存，e[u] = {邻居点 v 的集合 及 到该点的边权`<u, v>`}。

  无权图直接用 `vector<int> e[N]` 存即可

  ```c++
  #include <iostream>
  #include <algorithm>
  #include <vector>
  using namespace std;
  #define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
  const int N = 1e3 + 5;
  int n, m;//n个点，m条边         
  struct Edge{
      // int from, to, w; //from 一般不需要，直接去掉即可
      int to, w;
      Edge(){}
      Edge(int a, int b) { to = a, w = b;}
  };
  vector<Edge> e[N];
  void init()
  {
      for(int i = 0; i <= n; i++)
          e[i].clear();
  }
  int main()
  {
      untie();
      init();
      cin >> n >> m;
      for(int i = 0; i < m; i++)
      {
          int u, v, w;
          cin >> u >> v >> w;
          e[u].push_back(Edge(v, w));
          //无向边加上下句
          //e[v].push_back(Edge(u, w));
      }
      //遍历各节点的所有邻居
      for(int u = 0; u <= n; u++)
      {
          if(e[u].empty()) continue;
          cout << "node " << u << " 's neighbours: ";
          for(auto v : e[u])//或 for(int i = 0; i < e[u].size(); i++)
          {
              cout << v.to << " ";//不同于链式前向星，这里会按插入顺序输出
          }
          cout << '\n';
      }
      return 0;
  }
  ```

* 链式前向星

  ```c++
  //1.节点编号范围为 0 ~ n - 1
  #include <iostream>
  #include <algorithm>
  #include <vector>
  using namespace std;
  #define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
  const int N = 1e3 + 5, M = 2e3 + 5;                 //N为最大结点数，M为最多边数
  int n, m, head[N], cnt = 0;                         //head[u]指向 u 的一个邻居在edge[]中的存储位置 即 连着 u 的最后一条边(按录入顺序的最后者), cnt记录当前可存储位置
  struct node{
      int from, to, next;                             //from为边的起点(一般不需要)，to为边的终点，next为 u 的下一个邻居
      int w;                                          //边权
  }edge[M];
  void init()
  {
      for(int i = 0; i < N; i++) head[i] = -1;        //点初始化
      for(int i = 0; i < M; i++) edge[i].next = -1;   //边初始化
      cnt = 0;
  }
  void addedge(int u, int v, int w)
  {
      edge[cnt] = node{u, v, head[u], w};
      //edge[cnt].next = head[u] 将已录入位置存起，对于同一个起点 u，最终是从最后录入的位置连向最先录入的位置以遍历所有邻居
      head[u] = cnt++;
  }
  int main()
  {
      untie();
      init();
      cin >> n >> m;                                  //n个点，m条边
      for(int i = 0; i < m; i++)
      {
          int u, v, w;
          cin >> u >> v >> w;
          addedge(u, v, w);
      }
      //遍历各节点的所有邻居
      for(int u = 0; u <= n; u++)
      {
          if(!~head[u]) continue;
          cout << "node " << u << " 's neighbours: ";
          for(int i = head[u]; ~i; i = edge[i].next)//~i = -(i + 1) ，这里相当于 i != -1
              cout << edge[i].to << " ";
          cout << '\n';
      }
      return 0;
  }
  
  
  
  //*2.节点编号范围为 1 ~ n，这里就可以用 0 表示空，而不是 -1，省去 init() 函数
  #include <iostream>
  #include <algorithm>
  using namespace std;
  #define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
  const int N = 1e3 + 5, M = 2e3 + 5;
  int n, m, head[N], cnt = 1;
  struct Edge{
      int to, next, w;
      Edge(int a = 0, int b = 0, int c = 0) { to = a, next = b, w = c;}
  }e[M];
  void addedge(int u, int v, int w = 0)
  {
      e[cnt] = Edge(v, head[u], w);
      head[u] = cnt++;
  }
  int main()
  {
      untie();
      cin >> n >> m;                                  
      for(int i = 0; i < m; i++)
      {
          int u, v, w;
          cin >> u >> v >> w;
          addedge(u, v, w);
          // 无向边加上下句，并且注意判重
          // addedge(v, u, w);
      }
      //遍历各节点的所有邻居
      for(int u = 1; u <= n; u++)
      {
          if(!head[u]) continue;
          cout << "node " << u << " 's neighbours: ";
          for(int i = head[u]; i; i = e[i].next)//为 0 时停止
              cout << e[i].to << " ";
          cout << '\n';
      }
      return 0;
  }
  
  ```

  

---







---

## 树上基础问题

### 树基础

#### 引入

图论中的树和现实生活中的树长得一样，只不过我们习惯于处理问题的时候把树根放到上方来考虑。这种数据结构看起来像是一个倒挂的树，因此得名。树是非线性数据结构，它能很好的描述数据的层次关系，树这种结构的现实场景很常见，如文件目录、书本目录就是典型的树形结构，特别适合编码，而常常将一般的树转换为二叉树来处理。

#### 定义

一个没有固定根结点的树称为 **无根树**（unrooted tree）。无根树有几种等价的形式化定义：

- 有 $n$ 个结点，$n - 1$ 条边的连通无向图
- 无向无环的连通图
- 任意两个结点之间有且仅有一条简单路径的无向图
- 任何边均为桥的连通图
- 没有圈，且在任意不同两点间添加一条边之后所得图含唯一的一个圈的图

1. 简单来说，树就是“**没有圈的连通图**”。在无根树的基础上，指定一个结点称为 **根**，则形成一棵 **有根树**（rooted tree）。有根树在很多时候仍以无向图表示，只是规定了结点之间的上下级关系 ( 一棵树只有一个根, 且根到其他任何一个点有且仅有一条路径 ). 而无根树每个节点都可以作为树根。

2. 除了树根，每个节点都必须有且只有一个父结点，从根开始遍历时，必须从父节点到达子节点
3. 从根出发能遍历整棵树
4. 一棵有 $n$ 个节点的树，有 $n-1$ 条边
5. 去掉一条边，树被分成不连通的两棵树，去掉一个节点，树被分成不连通的两棵树或更多树
6. 在树中添加一条边后，出现一个圈
7. 从根出发到一个节点，有且仅有一条路径

注意：有根树和常见的概念“有向无环图”不同。在图论中，如果一个有向图不能从某一个顶点出发经过若干点和边回到该点，称这个图是一个有向无环图（Directed Acyclic Graph, DAG）。有根树都是DAG，但是DAG不一定是有根树，例如DAG中一个点 $u$ 可能经过两条路线到达另一个点 $v$ , $v$ 就有了两个父亲，不符合树的概念。 

#### 有关树的定义

**适用于无根树和有根树**

- **森林（forest）**：每个连通分量（连通块）都是树的图。按照定义，一棵树也是森林。
- **生成树（spanning tree）**：一个连通无向图的生成子图，同时要求是树。也即在图的边集中选择 $n - 1$ 条，将所有顶点连通。
- **无根树的叶结点（leaf node）**：度数不超过 $1$ 的结点。

<details class="question" open="" style="box-sizing: inherit; background-color: var(--md-admonition-bg-color); border: 0.05rem solid rgb(100, 221, 23); border-radius: 0.2rem; box-shadow: var(--md-shadow-z1); color: rgba(0, 0, 0, 0.87); display: flow-root; font-size: 0.64rem; margin: 1.5625em 0px; padding: 0px 0.6rem; break-inside: avoid; overflow: visible; font-family: &quot;Fira Sans&quot;, -apple-system, BlinkMacSystemFont, Helvetica, Arial, sans-serif; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary style="box-sizing: border-box; background-color: rgba(100, 221, 23, 0.1); border-top: none; border-right: none; border-bottom: none; border-left: 0.2rem none; border-image: initial; font-weight: 700; margin: 0px -0.6rem; padding: 0.4rem 1.8rem 0.4rem 2rem; position: relative; cursor: pointer; display: block; min-height: 1rem; border-top-left-radius: 0.1rem; border-top-right-radius: 0.1rem; -webkit-tap-highlight-color: transparent; outline: none;">为什么不是度数恰为 1<span>&nbsp;</span><mjx-container class="MathJax" jax="CHTML" style="box-sizing: inherit; line-height: 0;"><mjx-math class="MJX-TEX" style="box-sizing: inherit; display: inline-block; text-align: left; line-height: 0; text-indent: 0px; font-style: normal; font-weight: normal; font-size: 14.08px; letter-spacing: normal; border-collapse: collapse; overflow-wrap: normal; word-spacing: normal; white-space: nowrap; direction: ltr; padding: 1px 0px; font-family: MJXZERO, MJXTEX;"><mjx-mn class="mjx-n" style="box-sizing: inherit; display: inline-block; text-align: left;"><mjx-c class="mjx-c31" style="box-sizing: inherit; display: inline-block;"></mjx-c></mjx-mn></mjx-math><img src="data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7" title="1" style="box-sizing: inherit; border-style: none; width: 0px; height: auto; max-width: 100%;"></mjx-container>？</summary><p style="box-sizing: border-box; margin-bottom: 0.6rem;">考虑 n = 1 <span>&nbsp;</span><mjx-container class="MathJax" jax="CHTML" style="box-sizing: inherit; line-height: 0;"><mjx-math class="MJX-TEX" style="box-sizing: inherit; display: inline-block; text-align: left; line-height: 0; text-indent: 0px; font-style: normal; font-weight: normal; font-size: 14.08px; letter-spacing: normal; border-collapse: collapse; overflow-wrap: normal; word-spacing: normal; white-space: nowrap; direction: ltr; padding: 1px 0px; font-family: MJXZERO, MJXTEX;"><mjx-mi class="mjx-i" style="box-sizing: inherit; display: inline-block; text-align: left;"><mjx-c class="mjx-c1D45B TEX-I" style="box-sizing: inherit; display: inline-block; font-family: MJXZERO, MJXTEX-I;"></mjx-c></mjx-mi><mjx-mo class="mjx-n" space="4" style="box-sizing: inherit; display: inline-block; text-align: left; margin-left: 0.278em;"><mjx-c class="mjx-c3D" style="box-sizing: inherit; display: inline-block;"></mjx-c></mjx-mo><mjx-mn class="mjx-n" space="4" style="box-sizing: inherit; display: inline-block; text-align: left; margin-left: 0.278em;"><mjx-c class="mjx-c31" style="box-sizing: inherit; display: inline-block;"></mjx-c></mjx-mn></mjx-math><img title="n = 1" src="data:image/gif;base64,R0lGODlhAQABAIAAAAAAAP///yH5BAEAAAAALAAAAAABAAEAAAIBRAA7" style="box-sizing: inherit; border-style: none; width: 0px; height: auto; max-width: 100%;"></mjx-container>。</p></details>

- **有根树的叶结点（leaf node）**：没有子结点的结点

**只适用于有根树**

- **父亲（parent node）**：对于除根以外的每个结点，定义为从该结点到根路径上的第二个结点。 根结点没有父结点。
- **祖先（ancestor）**：一个结点到根结点的路径上，除了它本身外的结点。 根结点的祖先集合为空。
- **子结点（child node）**：如果 `u `是 `v` 的父亲，那么 `v` 是 `u` 的子结点。
  子结点的顺序一般不加以区分，二叉树是一个例外。
- **结点的深度（depth）**：到根结点的路径上的边数。
- **高度 ( height) **: 对于任意结点`n`, `n`  的高度是为从`n` 到叶子节点的最长路径长, 所有叶子的高度为 $0$ 
- **树的深度或高度（depth or height）**：所有结点的深度的最大值。
- **兄弟（sibling）**：同一个父亲的多个子结点互为兄弟。
- **后代（descendant）**：子结点和子结点的后代。
  或者理解成：如果 `u` 是 `v` 的祖先，那么 `v `是 `u` 的后代

![tree-definition.svg](https://oi-wiki.org/graph/images/tree-definition.svg)

- **子树（subtree）**：删掉与父亲相连的边后，该结点所在的子图。

![tree-definition-subtree.svg](https://oi-wiki.org/graph/images/tree-definition-subtree.svg)



#### 特殊的树

- **链（chain/path graph）**：满足与任一结点相连的边不超过 $2$ 条的树称为链。
- **菊花/星星（star）**：满足存在 `u` 使得所有除 `u` 以外结点均与 `u` 相连的树称为菊花。
- **有根二叉树（rooted binary tree）**：每个结点最多只有两个儿子（子结点）的有根树称为二叉树。常常对两个子结点的顺序加以区分，分别称之为左子结点和右子结点。
  大多数情况下，**二叉树** 一词均指有根二叉树。
- **完整二叉树（full/proper binary tree）**：每个结点的子结点数量均为 0 或者 2 的二叉树。换言之，每个结点或者是树叶，或者左右子树均非空。

![img](https://oi-wiki.org/graph/images/tree-binary-proper.svg)

- **完全二叉树（complete binary tree）**：只有最下面两层结点的度数可以小于 $2$，且最下面一层的结点都集中在该层最左边的连续位置上, 而且每个节点都与同高度的满二叉树中编号为`1 ~ n` 的节点一一对应. 

![img](https://oi-wiki.org/graph/images/tree-binary-complete.svg)

- **完美二叉树（perfect binary tree）**：树中的每层都含有最多的结点，树的叶子结点都集中在二叉树的最下一层，并且除叶子结点之外的每个结点度数均为2, 所有叶结点的深度均相同的二叉树称为完美二叉树, 一般完美二叉树也称为**满二叉树**

![img](https://oi-wiki.org/graph/images/tree-binary-perfect.svg)

<details class="warning" open="" style="box-sizing: inherit; background-color: var(--md-admonition-bg-color); border: 0.05rem solid rgb(255, 145, 0); border-radius: 0.2rem; box-shadow: var(--md-shadow-z1); color: rgba(0, 0, 0, 0.87); display: flow-root; font-size: 0.64rem; margin: 1.5625em 0px; padding: 0px 0.6rem; break-inside: avoid; overflow: visible; font-family: &quot;Fira Sans&quot;, -apple-system, BlinkMacSystemFont, Helvetica, Arial, sans-serif; font-style: normal; font-variant-ligatures: normal; font-variant-caps: normal; font-weight: 400; letter-spacing: normal; orphans: 2; text-align: start; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px; -webkit-text-stroke-width: 0px; text-decoration-thickness: initial; text-decoration-style: initial; text-decoration-color: initial;"><summary style="box-sizing: border-box; background-color: rgba(255, 145, 0, 0.1); border-top: none; border-right: none; border-bottom: none; border-left: 0.2rem none; border-image: initial; font-weight: 700; margin: 0px -0.6rem; padding: 0.4rem 1.8rem 0.4rem 2rem; position: relative; cursor: pointer; display: block; min-height: 1rem; border-top-left-radius: 0.1rem; border-top-right-radius: 0.1rem; -webkit-tap-highlight-color: transparent; outline: none;">Warning</summary><slot id="details-content"><p style="box-sizing: border-box; margin-bottom: 0.6rem;">Proper binary tree 的汉译名称不固定，且完全二叉树和满二叉树的定义在不同教材中定义不同，遇到的时候需根据上下文加以判断。</p></slot></details>

- **二叉排序树 ( Binary sort tree) :** 又称**二叉搜索树**, 二叉查找树, BST; 若任意节点的左子树不空, 则左子树上所有节点的值均小于它的根节点的值; 若任意节点的右子树不空, 则右子树上所有节点的值均大于它的根节点的值; 任意节点的左, 右子树也分别为二叉查找树; 没有键值相等的节点.

<img src="https://img-blog.csdnimg.cn/bd6b8feacb664716899f77ed87ae8d6b.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA56iL5bqPbGVl,size_20,color_FFFFFF,t_70,g_se,x_16" alt="img" style="zoom: 25%;" />

- **平衡二叉搜索树 ( Self-balancing binary search tree ) :** 又称 **AVL树** , 是**特殊的二叉排序树**, 它是一棵空树或它的左右两个子树的高度差的绝对值不超过1, 并且左右两个子树都是一棵平衡二叉树. 平衡二叉树的常用实现方法有红黑树, AVL, 替罪羊树, Treap, 伸展树等

<img src="https://img-blog.csdnimg.cn/032e5e76e2e342b193983342103ef99d.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA56iL5bqPbGVl,size_20,color_FFFFFF,t_70,g_se,x_16" alt="img" style="zoom: 50%;" />

**二叉树的性质**

注意: 二叉树和度为2的树，绝不可以混为一谈。度为2的树至少有3个结点，而二叉树可以为空树。此外，度为2的有序树的孩子结点的左右次序是相对于另一孩子结点而言的，如果某个结点只有一个孩子结点，这个孩子结点就无须区分其左右次序，而**二叉树无论其孩子数是否为2均需明确地确定其左右次序**，也就是说二叉树的结点次序不是相对于另一结点而言，而是确定的


**二叉树**: 

1. 非空二叉树上的叶子结点数等于度为`2`的结点数加`1`
2. 非空二叉树中**第 n 层的结点数**不超过 $2^{n - 1}$ 
3. 深度为h的非空二叉树的**总结点数**最多不超过 $2^{h - 1}$ 

**满二叉树**: 

1. 对于编号为`i`的结点, 若存在父结点, 则父节点的编号是 `2 / n`, 若存在左孩子, 则左孩子为 `n << 1`, 若存在右结点, 则右结点的编号为 `n << 1 | 1`
2. 每行总结点数为 $2^{n - 1}$ 

**完全二叉树**: 

1. 具有`n` 个结点的完全二叉树的高度满足 $2^{h - 1}-1 \lt n\leq 2^{h} - 1$ , 因此`n`个结点的完全二叉树高度 $h=\lceil\log_2(n+1)\rceil$ 或者 $h=\lfloor log_2(n) \rfloor + 1$ , 第`i`个结点所在层次或者高度也符合该公式
2. 叶子结点只可能在层次最大的两层的上出现。对于最大层次中的叶子结点，都依次排列在该层最左边的位置上。于是倒数第二层，若有叶子节点，一定都在右部连续位置
3. 按层序编号后，一旦出现某结点（其编号为i）为叶子结点或只有左孩子，则编号大于i的结点均为叶子结点
4. 当`(i << 1) <= n` 时, 结点`i`的左孩子编号为`i << 1`, 否则无左孩子; 当 `(i << 1 | 1) <= n`, 结点`i`的右孩子编号为 `i << 1 | 1`, 否则无右孩子
5. 同样结点数的二叉树, 完全二叉树的深度最小

#### 存储

**只记录父结点**

用一个数组 `parent[N]` 记录每个结点的父亲结点。

这种方式可以获得的信息较少，不便于进行自顶向下的遍历。常用于自底向上的递推问题中。

**邻接表**

- 对于无根树：为每个结点开辟一个线性列表，记录所有与之相连的结点。

  ```c++
  std::vector<int> adj[N]; 
  ```


- 对于有根树：

  - 方法一：若给定的是无向图，则仍可以上述形式存储。下文将介绍如何区分结点的上下关系。

  - 方法二：若输入数据能够确保结点的上下关系，则可以利用这个信息。为每个结点开辟一个线性列表，记录其所有子结点；若有需要，还可在另一个数组中记录其父结点。

    ```c++
    std::vector<int> children[N]; int parent[N];
    ```

    当然也可以用其他方式（如链表）替代`std::vector`


**左孩子右兄弟表示法**

**过程**

对于有根树，存在一种简单的表示方法。

首先，给每个结点的所有子结点任意确定一个顺序。

此后为每个结点记录两个值：其 **第一个子结点** `child[u]` 和其 **下一个兄弟结点** `sib[u]`。若没有子结点，则 `child[u]` 为空；若该结点是其父结点的最后一个子结点，则 `sib[u]` 为空。

**实现**

遍历一个结点的所有子结点可由如下方式实现

```c++
int v = child[u];  // 从第一个子节点开始
while (v != EMPTY_NODE)
{
    // ...
    // 处理子节点 v
    // ...
	v = sib[v];  // 转至下一个子节点, 即 v 的下一个兄弟
}
```

也可简写为以下形式

```C++
for (int v = child[u]; v != EMPTY_NODE; v = sib[v])
{
    // ...
    // 处理子节点
    // ...
}
```

**二叉树**

二叉树的一个节点的储存，包括节点的值、左右子节点，有静态和动态两种存储方法

（1） 动态二叉树，数据结构中一般这样定义二叉树，新建一个`Node`，用new运算符动态申请一个节点，使用完毕后应该使用`delete`命令释放它，否则会内存泄露，动态二叉树的优点是不浪费空间，缺点是需要管理，不小心会出错

```C++
struct Node
{
	int value;          //节点的值，可以定义多个值
	node *lson, *rson;  //指向左右子节点
};
```

（2）静态二叉树，竞赛中为了编码简单、加快速度，一般用静态数组实现二叉树

```c++
//定义一个大小为N的结构体数组
struct Node
{
	char value;      
	int lson, rson;    //左右孩子
}tree[N];
```



#### 树的遍历

**树上 DFS**

在树上 DFS 是这样的一个过程：先访问根节点，然后分别访问根节点每个儿子的子树。

可以用来求出每个节点的深度、父亲等信息。

**二叉树上 DFS**

**先序遍历**

![preorder](https://oi-wiki.org/graph/images/tree-basic-Preorder.png)

按照 **根，左，右** 的顺序遍历二叉树。

```c++
void preTrav(BitTree* root)
{
	if (root)
    {
        cout << root -> key << " ";
        preTrav(root -> left);
        preTrav(root -> right);
    }
}
```

**中序遍历**

![inorder](https://oi-wiki.org/graph/images/tree-basic-inorder.png)

按照 **左，根，右** 的顺序遍历二叉树，再二叉搜索树中中序遍历实现了排序功能

```C++
void midTrav(BiTree* root)
{
    if (root)
    {
        midTrav(root -> left);
        cout << root -> key << " ";
        midTrav(root -> right);
	}
}
```

**后序遍历**

![Postorder](https://oi-wiki.org/graph/images/tree-basic-Postorder.png)

按照 **左，右，根** 的顺序遍历二叉树。

```C++
void lastTree(BiTree* root)
{
	if (root)
	{
		lastTrav(root -> left);
		lastTrav(root -> right);
		cout << root -> key << " ";
	}
}
```

**反推**

已知**中序遍历序列和另外一个序列**可以求第三个序列。

![reverse](https://oi-wiki.org/graph/images/tree-basic-reverse.png)

1. 先序的第一个节点是 `root`，后序遍历的最后一个节点是 `root`。
2. 先确定根节点，然后根据中序遍历，在根左边的为左子树，根右边的为右子树。
3. 对于每一个子树可以看成一个全新的树，仍然遵循上面的规律。

**树上 BFS**

从树根开始，严格按照层次来访问节点，BFS 过程中也可以顺便求出各个节点的深度和父亲节点。

**无根树**

**过程**

树的遍历一般为深度优先遍历，这个过程中最需要注意的是避免重复访问结点。

由于树是无环图，因此只需记录当前结点是由哪个结点访问而来，此后进入除该结点外的所有相邻结点，即可避免重复访问。

**实现**

```c++
void dfs(int u, int from)
{
    // 递归进入除了 from 之外的所有子结点
    // 对于出发结点，from 为 -1，故会访问所有相邻结点，这与期望一致
    for (int v : adj[u])
    	if (v != from) dfs(v, u);
}

int EMPTY_NODE = -1;    // 一个不存在的编号
int root = 0;           // 任取一个结点作为出发点
dfs(root, EMPTY_NODE);
```

**有根树**

对于有根树，需要区分结点的上下关系。

考察上面的遍历过程，若从根开始遍历，则访问到一个结点时 `from` 的值，就是其父结点的编号。

通过这个方式，可以对于无向的输入求出所有结点的父结点，以及子结点列表。

#### 基本问题

关于树，有以下常见基本问题

1. 树的判断，判断一个图是否为树的基本方法是用DFS遍历
2. 树的存储，和图的存储一样
3. 距离问题，求树上点与点之间的路径长度，由于两点之间只有一条路径，无需使用图论中`Dijkstra` 等复杂寻路算法
   1. 求某个节点`u` 到所有其他点的距离，用DFS或BFS对整棵树搜索一遍，复杂度为 $O(n)$ 
   2. 求所有点对之间的距离，对每个点进行一次DFS或BFS，复杂度为 $O(n^2)$ ，用点分治法更快。
   3. 求两个节点之间的距离，可以利用最近公共祖先（LCA），有复杂度为 $O(log_2n)$ 的高级算法
4. 公共祖先和最近公共祖先（LCA）
5. 树的直径，树的重心
6. 多叉树和二叉树
7. 树上前缀和、树上差分，树上前缀和是指从根出发到某点的路径上的点（或者边）的权值之和，用DFS从根开始搜索到某点，逐个累加点或边的权值即可。



### 树的直径

#### 定义

**树上任意两节点之间最长的简单路径即为树的「直径」（diameter）**

显然，一棵树可以有多条长度相等直径。可以用两次 `DFS` 或者树形 `DP` 的方法在 $O(n)$ 时间求出树的直径。



**直径性质**
若树上所有边边权均为正，则树的所有直径中点重合
证明：使用反证法。设两条中点不重合的直径分别为 $\delta(s,t) 与 \delta(s',t')$，中点分别为 $x$ 与 $x'$。显然，$\delta(s,x) = \delta(x,t) = \delta(s',x') = \delta(x',t')$。

![无负权边的树所有直径的中点重合](https://oi-wiki.org/graph/images/tree-diameter4.svg)

有 $\delta(s,t') = \delta(s,x) + \delta(x,x') + \delta(x',t') > \delta(s,x) + \delta(x,t) = \delta(s,t)$，与 $\delta(s,t)$ 为树上任意两节点之间最长的简单路径矛盾，故性质得证。



#### 两种实现方法

现给定一棵 $n$ 个节点的树，求其直径的长度。$1 \leq n\leq 10^4$

##### **两次DFS**

首先从任意节点 $y$ 开始进行第一次 `DFS`，到达距离其最远的节点，记为 $z$，然后再从 $z$ 开始做第二次 `DFS`，到达距离 $z$ 最远的节点，记为$z^{'}$，则 $\delta(z,z^{'})$ 即为树的直径。

显然，如果第一次 `DFS` 到达的节点 $z$ 是直径的一端，那么第二次 DFS 到达的节点 $z^{'}$ 一定是直径的一端。我们只需证明在任意情况下 $z$  必为直径的一端。

**定理**：在一棵树上，从任意节点 $y$ 开始进行一次 `DFS`，到达的距离其最远的节点 $z$ 必为直径的一端。

注意：上述结论建立在所有路径均不为负的前提下。**如果树上存在负权边，则上述结论不成立**。故若存在负权边，则无法使用两次 DFS 的方式求解直径。

代码实现：

```c++
const int N = 10010;

int n, c, d[N];
vector<int> E[N];

void dfs(int u, int fa)
{
    for (int v : E[u])
    {
        if (v == fa) continue;
        d[v] = d[u] + 1;
        if (d[v] > d[c]) c = v;
        dfs(v, u);
    }
}
int main()
{
    cin >> n;
    for (int i = 1; i < n; i++)
    {
        int u, v; cin >> u >> v;
        E[u].push_back(v);
        E[v].push_back(u);
    }
    dfs(1, 0);
    d[c] = 0;
    dfs(c, 0);
    cout << d[c] << endl;
    return 0;
}
```

如果需要求出一条直径上所有的节点，则可以在第二次 DFS 的过程中，记录每个点的前序节点，即可从直径的一端一路向前，遍历直径上所有的节点。

##### **树形DP**

**过程**

我们记录当 $1$ 为树的根时，每个节点作为子树的根向下，所能延伸的最长路径长度 $d_1$ 与次长路径（**与最长路径无公共边**）长度$d_2$，那么直径就是对于每一个点，该点能 $d_1+d_2$ 取到的值中的最大值。

树形 DP 可以在**存在负权边**的情况下求解出树的直径。

<img src="D:\MarkdownText\image\图论\1.png" alt="1" style="zoom:50%;" />

如图，对于点 1 来说，遍历其儿子结点 2、3、4，取最大的两条路径即 `d1[2]` 和 `d1[3]`，加上与父亲 1 的边权（这里设为 1），得到目前最大的直径 `d = (d1[2] + 1) + (d1[3] + 1)`。

代码实现：

```c++
const int N = 10010;

int n, d = 0;
int d1[N], d2[N];
vector<int> E[N];

void dfs(int u, int fa)
{
    d1[u] = d2[u] = 0;//初始化为 0，若与儿子有负权边则不选择该儿子，至少可以保证权值为 0。故至少 d >= 0
    for (int v : E[u])
    {
        if (v == fa) continue;
        dfs(v, u);
        int t = d1[v] + 1;
        if (t > d1[u])
            d2[u] = d1[u], d1[u] = t;
        else if (t > d2[u])
            d2[u] = t;
    }
    d = max(d, d1[u] + d2[u]);
}
int main()
{
    cin >> n;
    for (int i = 1; i < n; i++)
    {
        int u, v; cin >> u >> v;
        E[u].push_back(v);
        E[v].push_back(u);
    }
    dfs(1, 0);
    cout << d << endl;
    return 0;
}
```

**直径路径输出：**

如果需要求出一条直径上所有的节点，则可以在 DP 的过程中，记录下每个节点能向下延伸的最长路径与次长路径（定义同上）所对应的子节点，在求 $d$ 的同时记下对应的节点 $u$，使得 $d = d_1[u] + d_2[u]$，即可分别沿着从 $u$开始的最长路径的次长路径对应的子节点一路向某个方向（对于无根树，虽然这里指定了 $1$ 为树的根，但仍需记录每点跳转的方向；对于有根树，一路向上跳即可），遍历直径上所有的节点。

#### 例题

```c++
1.cf 903 div.3 F
要求找到树的直径 (a, b)，并求从直径各端出发的最长路
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
#define endl '\n'
#define gcd(a, b) __gcd(a, b)
#define mk(a, b) make_pair((a), (b))
#define lowbit(x) ((x) & -(x))
const int N = 2e5 + 10, inf = 1e9;
const ll linf = 4e18;

void Solve()
{
    int n, k;
    cin >> n >> k;

    vector<int> mark(n + 5);
    for(int i = 0; i < k; ++i)
    {
        int u; cin >> u;
        mark[u] = 1;
    }

    vector<vector<int>> G(n + 5);
    for(int i = 1; i < n; ++i)
    {
        int u, v;
        cin >> u >> v;
        G[u].push_back(v);
        G[v].push_back(u);
    }
    
    vector<int> dis(n + 5, -1);
    auto oper = [&](int st) -> int
    {
        dis.assign(n + 5, -1);
        dis[st] = 0;
        auto dfs = [&](auto self, int u, int fa) -> void
        {
            for(auto v : G[u])
            {
                if(v == fa) continue;
                dis[v] = dis[u] + 1;
                self(self, v, u);
            }
        };
        dfs(dfs, st, -1);
        
        int p = -1;
        for(int i = 1; i <= n; ++i)
            if(mark[i] && (p == -1 || dis[i] > dis[p]))
                p = i;
        return p;
    };

    int a = oper(1);
    int b = oper(a);
    auto f = dis;
    oper(b);

    for(int i = 1; i <= n; ++i)
        f[i] = max(f[i], dis[i]);

    int ans = inf;
    for(int i = 1; i <= n; ++i)
        ans = min(ans, f[i]);
    cout << ans << endl;
}

int main()
{
    untie();
    int T = 1;
    cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}

// bfs 同理
void Solve()
{
    int n, k;
    cin >> n >> k;

    vector<int> mark(n + 5);
    for(int i = 0; i < k; ++i)
    {
        int u; cin >> u;
        mark[u] = 1;
    }

    vector<vector<int>> G(n + 5);
    for(int i = 1; i < n; ++i)
    {
        int u, v;
        cin >> u >> v;
        G[u].push_back(v);
        G[v].push_back(u);
    }
    
    vector<int> dis(n + 5, -1);
    auto bfs = [&](int st) -> int
    {
        queue<int> q;
        q.push(st);
        dis.assign(n + 5, -1);
        dis[st] = 0;
        while(!q.empty())
        {
            int u = q.front();
            q.pop();
            for(auto v : G[u])
            {
                if(dis[v] == -1)
                {
                    dis[v] = dis[u] + 1;
                    q.push(v);
                }
            }
        }
        int p = -1;
        for(int i = 1; i <= n; ++i)
            if(mark[i] && (p == -1 || dis[i] > dis[p]))
                p = i;
        return p;
    };

    int a = bfs(1);
    int b = bfs(a);
    auto f = dis;
    bfs(b);

    for(int i = 1; i <= n; ++i)
        f[i] = max(f[i], dis[i]);

    int ans = inf;
    for(int i = 1; i <= n; ++i)
        ans = min(ans, f[i]);
    cout << ans << endl;
}






```





### 树的重心

#### 定义

树的重心（barycenter）也称为树的质心，它是无根树（不含回路的无向图）上的一个应用。树的重心 $u$ 是这样一个节点：把树变成以节点 $u$ 为根的有根树时，**最大子树**（最大连通块）的**节点数最少**，那么 $u$ 就是树的重心。换句话说，删除节点 $u$ 后得到两棵树或者更多棵互不连通的子树，其中最大子树的节点数最少。$u$ 是树上**最平衡**的点。

<img src="D:\MarkdownText\image\图论\2.png" alt="2" style="zoom:33%;" />

如图，

```vue
当去掉点 1 后，树将分成两个连通块（子树）：
（2,4,5）
（3,6,7）
则最大的连通块包含节点个数为 3。


当去掉点 2 后，树将分成三个连通块（子树）：
（4）
（5）
（1,3,6,7）
则最大的连通块包含节点个数为 4。

以此类推，最后得到，点 1 为根下的子树最大节点数为 3 是最小的，故点 1 是该树的重心
```

<img src="D:\MarkdownText\image\图论\3.png" alt="3" style="zoom:50%;" />

我们删除节点2，可以得到3个连通分量：① ② ③

① ② 是节点2的子树，其节点数可以通过递归求得；

③是该节点上方的子树，该子树的节点数为 `n - f[i]`。

我们所求的节点 i 的最大连通分量的节点数为：

```erlang
dp(i) = max(f[j], n - f[i]) （j 是 i 的子树）
```

**应用**

删去重心后，生成的多棵树已尽可能平衡，在**树的点分治**中有重要的作用, 可以避免`O(n^2)`的极端复杂度（从退化链的一端出发），保证`O(nlogn)`的复杂度

#### **代码实现**

给定一棵 `n` 个节点的树, 求该树的重心. 

```c++
#include <bits/stdc++.h>
using namespace std;
const int N = 5e4 + 10;        //最大节点数
int n, cnt, maxn, bc;
struct Edge
{
    int to, next;
    Edge() {};
    Edge(int _to, int _next) {to = _to; next = _next;}
}e[N << 1];                    //无向边，开两倍
int head[N];
int f[N];                      //f[u]是以u为根的子树节点数量
void add_edge(int u, int v)
{
    e[++cnt] = Edge(v, head[u]);
    head[u] = cnt;
}
void dfs(int u, int fa)
{
    int tmp = 0;
    f[u] = 1;                   //递归到最底层时，节点数初始化为 1
    for (int i = head[u]; i; i = e[i].next)   //遍历u的子节点
    {
        int v = e[i].to;         
        if (v == fa) continue;   //不递归父亲
        dfs(v, u);               //递归子节点，计算v这个子树节点数量
        tmp = max(tmp, f[v]);    //计算以u为根的节点数量
        f[u] += f[v];            //记录u的最大子树的节点数量
    }
    tmp = max(tmp, n - f[u]);    //tmp = u的最大连通块的节点数
    if (tmp < maxn) bc = u, maxn = tmp;  //维护重心
}
int main()
{
    maxn = 0x3f3f3f3f;
    cin >> n;
    for (int i = 1; i < n; i++)
    {
        int u, v; cin >> u >> v;
        add_edge(u, v);
        add_edge(v, u);
    }
    dfs(1, 0);
    cout << bc << endl;
    return 0;
}
```





---



## 判环与环路径输出

[C - Find it!](https://atcoder.jp/contests/abc311/tasks/abc311_c)

```c++
// 写法1：并查集判环（最简洁高效）
// 该题为判单环，并查集实际上可以处理多个环
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 2e5 + 10;
int vis[N], ok = 0;
int st, ed; // 环的首尾结点
int path[N];
int ds[N];

int find_set(int x)
{
    return x == ds[x] ? x : (ds[x] = find_set(ds[x]));
}

void dfs(vector<int> G[], int u, int step) // step 记录步数
{
    vis[u] = 1;
    path[step] = u;
    if(u == ed)
    {
        cout << step << "\n";
        for(int i = 1; i <= step; ++i) cout << path[i] << " \n"[i == step];
        return;
    }
    for(int v : G[u])
        if(!vis[v]) dfs(G, v, step + 1);
}

void Solve()
{
    int n; cin >> n;
    vector<int> G[n + 5];
    for(int i = 1; i <= n; ++i) ds[i] = i;
    for(int u = 1; u <= n; ++u)
    {
        int v; cin >> v;
        int fu = find_set(u), fv = find_set(v);
        if(fu == fv) st = v, ed = u; // 此时 u -> v 是 尾结点 接 首结点 的边
        else ds[fu] = fv;
        G[u].push_back(v);
    }
    dfs(G, st, 1);
}

int main()
{
    // untie();
    int T = 1;
    // cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}



// 写法2：定义三种状态区分点的状态
// vis[i] 有 3 个状态：未遍历、正在遍历、已遍历
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 2e5 + 10;
int vis[N], ok = 0;
int pre[N]; // 前驱存储
vector<int> vec;

bool dfs(vector<int> G[], int u, int fa)
{
    vis[u] = 0; // 正在遍历
    pre[u] = fa;
    for(int v : G[u])
    {
        if(vis[v] == 0) // 遇到正在遍历的路径上的点，由于是有向图，说明 v 是路径的头结点，即已经接头了
        {
            while(u != v) // v 是路径头结点，倒序遍历到头结点
            {
                vec.push_back(u);
                u = pre[u];
            }
            vec.push_back(v); // 最后记得加入头节点
            return 1;
        }
        else if(vis[v] == -1)
        {
            // pre[v] = u;
            if(dfs(G, v, u)) 
            {
                return 1;
            }
        }
    }
    vis[u] = 1; // 已遍历
    return 0;
}

void Solve()
{
    int n; cin >> n;
    vector<int> G[n + 5];
    for(int u = 1; u <= n; ++u)
    {
        int v; cin >> v;
        G[u].push_back(v);
    }
    memset(vis, -1, sizeof(vis)); // 初始化为未遍历状态
    for(int i = 1; i <= n; ++i)
    {
        if(vis[i] == -1)
        {
           if(dfs(G, i, 0)) 
           {
                int sz = vec.size();
                reverse(all(vec));
                cout << sz << "\n";
                for(int k = 0; k < sz; ++k)
                    cout << vec[k] << " \n"[k == sz - 1];
                return;
           }
        }
    }
    
}

int main()
{
    // untie();
    int T = 1;
    // cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}




```



---

## 拓扑排序





---

## 最小生成树





---

## 最短路

| 所求最短路径对象     | 边权                  | 算法                                                         |
| -------------------- | --------------------- | ------------------------------------------------------------ |
| 一个起点，一个终点   | 非负；无边权或边权为1 | A*算法(<O((m + n)logn))<br />双向广搜(<O((m + n)logn))<br />贪心最优搜索(<O(m + n)) |
| 一个起点到其他所有点 | 无边权或边权为1       | 简单BFS(O(m + n))                                            |
|                      | 非负数                | Dijkstra算法-堆优化优先队列(O((m + n)logn))                  |
|                      | 含负数                | Bellman–Ford算法(O(m * n))，SPFA(<O(m * n))                  |
| 所有点对之间         | 含负数                | Floyd-Warshall(O(n ^ 3))                                     |
|                      | 只有 0 和 1           | 传递闭包-bitset优化的Floyd算法(O(n ^ 2))                     |



### 简单BFS

```c++
1.最短路计数
一个起点到所有其他点，且边权为 1 (无边权) -- 邻接表 + BFS
思路：边确定最短路距离，边更新符合最短路的路径个数
#include <iostream>
#include <algorithm>
#include <vector>
#include <queue>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 1e6 + 5, mod = 100003, INF = 0x3f3f3f3f;
int n, m, s = 1;
int dis[N], cnt[N], vis[N];
vector<int> e[N];
void bfs()
{
    memset(dis, 0x3f, sizeof(dis));
    cnt[1] = 1, vis[1] = 1;
    queue<int> q;
    q.push(1);
    while(!q.empty())
    {
        int now = q.front(); q.pop();
        for(int i = 0; i < e[now].size(); i++)
        {
            int to = e[now][i];
            if(!vis[to])//固定最短路距离
            {
                vis[to] = 1;
                dis[to] = dis[now] + 1;
                q.push(to);
            }
            if(dis[to] == dis[now] + 1)//最短路径数叠加，把 now点 的路径数全部加到 to点 上
                cnt[to] = (cnt[to] + cnt[now]) % mod;
        }
    }
}
int main()
{
    untie();
    cin >> n >> m;
    while(m--)
    {
        int u, v;
        cin >> u >> v;
        e[u].push_back(v);
        e[v].push_back(u); //无向图
    }
    bfs();
    for(int i = 1; i <= n; i++)
    {
        if(i != 1 && dis[i] == INF) cout << "0\n";
        else cout << cnt[i] % mod << "\n";
    }
    return 0;
}
```



### Floyd 算法

适用于**多源**最短路径问题，可求得所有点对之间的最短路径，复杂度 O(n^3) 只适用于小规模的稠密图(n < 300)

应用 动态规划 的思路，定义 `dp[k][i][j]` 为从第一个点遍历到第 k 个点时，从点 i 到点 j 的最短距离(最小边权和)。

从含有 k - 1 个点的子图拓展到含 k 个点的图，若经历点 k 使得距离更短，则将点 k 接入到 i -> j 路径上，假设 i -> k -> j

此时 distance(i -> k -> j) < distance(i -> j)，则 k 点应用到该状态上，转移方程为

 `dp[k][i][j] = min(dp[k - 1][i][j], dp[k - 1][i][k] + dp[k - 1][k][j])`

由于第一维状态只与第 k - 1 层状态有关，故可以滚动为: 

`dp[i][j] = min(dp[i][j], dp[i][k] + dp[k][j])`

```c++
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
typedef long long ll;
const int N = 305;
const ll INF = 0x3f3f3f3f3f3f3f3fLL;
int n, m, q;
ll dp[N][N];

void floyd()
{
    for(int k = 1; k <= n; k++) // 注意遍历中间点的循环在第一重，它可代表 dp 状态的层数
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= n; j++)
                dp[i][j] = min(dp[i][j], dp[i][k] + dp[k][j]);
}

int main()
{
    untie();
    memset(dp, 0x3f, sizeof(dp));
    cin >> n >> m >> q;
    for(int i = 1; i <= m; i++)
    {
        int u, v; ll w;
        cin >> u >> v >> w;
        dp[u][v] = dp[v][u] = min(dp[u][v], w); // 防止有重边
    }
    floyd();
    while(q--)
    {
        int s, t;
        cin >> s >> t;
        if(dp[s][t] == INF) cout << "-1";
        else if(s == t) cout << "0";
        else cout << dp[s][t];
        cout << '\n';
    }
    return 0;
}
```



#### 传递闭包

适用于处理所有点对间的**优先关系**，复杂度 O(n ^ 2)

在交际网络中，给定若干个元素和若干对二元关系，且关系具有传递性（假如a 与 b有关系，b与c有关系，那么a和c必定有关系）。”

通过传递性推导出尽量多的元素之间的关系"的问题被称为传递闭包。

建立邻接矩阵d，其中 d[i, j] = 1 表示 i 与 j 有关系，d[i, j] = 0表示 i 与 j 没有关系。特别的，d[i, i]始终为 1。

核心代码：

```c++
for (int k = 1; k <= n; k++)
    for (int i = 1; i <= n; i++)
        for (int j = 1; j <= n; j++)
            d[i][j] |= d[i][k] & d[k][j];
```

bitset优化(从n<500优化到接受n<=1000)：

```c++
bitset<N> dp[N];//N串长度为N的二进制，可以对一整串二进制操作，故省去第 j 层循环
for(int k = 1; k <= n; k++)//遍历所有点作中间点k，若经历k点能连通 i->k->j 则说明 i 与 j 连通
    for(int i = 1; i <= n; i++)
        if(dp[i][k]) 
            dp[i] |= dp[k];            
```

例题：

```c++
1.Cow Contest(传递闭包)
必须知道任一一头奶牛与其他所有奶牛之间的胜负关系，并依个判定其排名是否确认，最后输出个数即可
#include <iostream>
#include <algorithm>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 105;
int n, m;
bool dp[N][N];
void floyd()
{
    for(int k = 1; k <= n; k++)//遍历所有点作中间点k，若经历k点能连通 i->k->j 则说明 i 与 j 连通
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= n; j++)
                dp[i][j] |= dp[i][k] & dp[k][j];
                //即 if(dp[i][k] && dp[k][j]) dp[i][j] = 1;
}
int main()
{
    untie();
    cin >> n >> m;
    while(m--)
    {
        int s, t;
        cin >> s >> t;
        dp[s][t] = 1;
    }
    floyd();
    //判定点 i 是否以确定排名：点 i 与其他n-1个点是否都具有连通关系
    int ans = 0;
    for(int i = 1; i <= n; i++)
    {
        int cnt = 0;
        for(int j = 1; j <= n; j++)
            if(dp[i][j] || dp[j][i]) ++cnt;
        if(cnt == n - 1) ++ans;
    }
    cout << ans << endl;
    return 0;
}

bitset优化
#include <iostream>
#include <algorithm>
#include <bitset>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 105;
int n, m;
bitset<N> dp[N];//N串长度为N的二进制，可以对一整串二进制操作，故省去第 j 层循环
void floyd()
{
    for(int k = 1; k <= n; k++)//遍历所有点作中间点k，若经历k点能连通 i->k->j 则说明 i 与 j 连通
        for(int i = 1; i <= n; i++)
            if(dp[i][k]) 
                dp[i] |= dp[k];            
}
int main()
{
    untie();
    cin >> n >> m;
    while(m--)
    {
        int s, t;
        cin >> s >> t;
        dp[s][t] = 1;
    }
    floyd();
    //判定点 i 是否已确定排名：点 i 与其他n-1个点是否都具有连通关系
    int ans = 0;
    for(int i = 1; i <= n; i++)
    {
        int cnt = 0;
        for(int j = 1; j <= n; j++)
            if(dp[i][j] || dp[j][i]) ++cnt;
        if(cnt == n - 1) ++ans;
    }
    cout << ans;
    return 0;
}



2.MPI Maelstrom
*题意：本题在于题意难解，给一个邻接矩阵(输入给半边)，求从点 1 到 其他点 所花费最短时间集里面的的最大值
解法：由于 n <= 100 ，用编码简单的floyd更方便
#include <iostream>
#include <algorithm>
#include <cstring>
#include <cstdlib>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
typedef long long ll;
const int N = 105;
const ll INF = 0x4ffffffffLL;
int n, m;
ll dp[N][N];
char str[N];
void floyd()
{
    for(int k = 1; k <= n; k++)
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= n; j++)
                dp[i][j] = min(dp[i][j], dp[i][k] + dp[k][j]);
}
int main()
{
    untie();
    cin >> n;
    for(int i = 2; i <= n; i++)
        for(int j = 1; j < i; j++)
        {
            cin >> str;
            if(str[0] != 'x') dp[i][j] = dp[j][i] = 1LL * atoi(str);
            else dp[i][j] = dp[j][i] = INF;
        }
    floyd();
    ll _max = 0;
    for(int i = 1; i <= n; i++) _max = max(_max, dp[1][i]);
    cout << _max;
    return 0;
}
```



### Bellman-Ford算法

对于边 (u, v) 有类似于 Dijkstra算法 的松弛操作：`dis[v] = min(dis[v], dis[u] + w(u, v))`

Bellman–Ford 算法所做的，就是不断尝试对图上每一条边进行松弛。我们每进行一轮循环，就对图上所有的边都尝试进行一次松弛操作，当一次循环中没有成功的松弛操作时，算法停止。

由于一次松弛操作会使最短路的边数至少会 +1，而最短路的边数最多为 n - 1，因此整个算法最多执行 n - 1 轮松弛操作，故总时间复杂度为 O(n * m)。

值得注意的是，以 s 点为源点跑 Bellman–Ford 算法时，如果没有给出存在负环的结果，只能说明从 s 点出发不能抵达一个负环，而**不能说明图上不存在负环**。

因此如果需要判断整个图上是否存在负环，最严谨的做法是建立一个**超级源点**，向图上**每个节点**连一条权值为 0 的边，然后以超级源点为起点执行 Bellman–Ford 算法。

```c++
struct Edge {
    int u, v, w;
};

vector<Edge> edge;
int n, dis[N];

bool bellmanford(int s) 
{
    memset(dis, 0x3f, sizeof(dis));
    dis[s] = 0;
    bool flag = 0;  // 判断一轮循环过程中是否发生松弛操作
    for(int i = 1; i <= n; i++) // 最后第 n 轮用于判断点 s 是否能到达负环
    {
        flag = 0;
        for(int j = 0; j < edge.size(); j++) 
        {
            int u = edge[j].u, v = edge[j].v, w = edge[j].w;

            // 无穷大与常数加减仍然为无穷大
            // 因此最短路长度为 inf 的点引出的边不可能发生松弛操作
            if(dis[u] == inf) continue;
            
            if(dis[v] > dis[u] + w) 
            {
                dis[v] = dis[u] + w;
                flag = 1;
            }
        }
        // 没有可以松弛的边时就停止算法
        if (!flag) break;
    } 
    // 第 n 轮循环仍然可以松弛时说明 s 点可以抵达一个负环
    return flag;
}
```



#### SPFA算法

属于**队列优化**的Bellman-Ford算法

优化原理：很多时候我们并不需要那么多无用的松弛操作。只有上一次被松弛过的结点所连接的边，才有可能引起下一次松弛操作。故用队列来维护「哪些结点可能会引起松弛操作」，这样就能只访问必要的边。

特点：可以用于快速判断 s 点是否能抵达一个负环，只需记录最短路经过了多少条边，当经过了至少 n 条边时，说明 s 点可以抵达一个负环。

```c++
struct edge {
  int v, w;
};

vector<edge> e[N];
int n, dis[N], cnt[N], vis[N];

bool spfa(int s) 
{
    memset(dis, 63, sizeof(dis));
    dis[s] = 0;
    vis[s] = 1;
    queue<int> q;
    q.push(s);
    while(!q.empty()) 
    {
        int u = q.front();
        q.pop();
        vis[u] = 0;
        for (auto ed : e[u]) 
        {
            int v = ed.v, w = ed.w;
            if(dis[v] > dis[u] + w) 
            {
                dis[v] = dis[u] + w;
                cnt[v] = cnt[u] + 1;  // 记录最短路经过的边数

                // 在不经过负环的情况下，最短路至多经过 n - 1 条边
                // 因此如果经过了多于 n 条边，一定说明经过了负环
                if (cnt[v] >= n) return false;
                
                if (!vis[v]) q.push(v), vis[v] = 1;
            }
        }
    }
    return true;
}
```



### Dijkstra算法

适用于单源最短路径问题

稀疏图：n 较大，用 dijkstra + 优先队列，便于获取最短距离的点  O((m + n)logn)

稠密图：n^2 约等于 m，不用优先队列优化，直接在所有节点中找距离最短的点  O(n^2)

模板：

```c++
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>
#include <cstring>
#include <cstdio>
using namespace std;
typedef long long ll;

const ll inf = (1LL << 31) - 1;
const int N = 1e5 + 5;

struct edge{
    int to; ll w;
    edge(int a = 0, ll b = 0){ to = a, w = b;}
};
struct node{
    int id; ll dis;//dis 为该点到起点的距离
    node(int a = 0, ll b = 0){ id = a, dis = b;}
    bool operator <(const node &x)const{ return dis > x.dis;}
};

int n, m, s = 1;//起点s
int pre[N];//pre[i] 记录前驱节点即 点 i 的上一个点的编号，用于打印路径
ll dis[N];//记录 所有节点 到 起点s 的距离
bool vis[N];//记录是否已找到 节点i 的 最短距离
vector<edge> e[N];

void print_path(int t)//从 终点n 递归打印
{
    if(t == s) { printf("%d", s); return;}
    print_path(pre[t]);
    printf("%d ", t);
}

void dijkstra()
{
    for(int i = 0; i <= n; i++) {dis[i] = inf, vis[i] = 0;}
    dis[s] = 0;
    priority_queue<node> q;
    q.push(node(s, dis[s]));
    while(!q.empty())
    {
        node u = q.top(); q.pop();
        if(vis[u.id]) continue;
        vis[u.id] = 1;
        for(int i = 0; i < e[u.id].size(); i++)//遍历邻居
        {
            edge ne = e[u.id][i];
            if(!vis[ne.to] && dis[ne.to] > u.dis + ne.w)//更新邻居点的最短距离
            {
                dis[ne.to] = u.dis + ne.w;
                q.push(node(ne.to, dis[ne.to]));
                // pre[ne.to] = u.id;
            }
        }
    }
    // print_path(n);//打印路径
}

int main()
{
    scanf("%d%d%d", &n, &m, &s);
    for(int i = 0; i <= n; i++) e[i].clear();
    for(int i = 0; i < m; i++)
    {
        int u, v; ll w;
        scanf("%d%d%lld", &u, &v, &w);
        e[u].push_back(edge(v, w));
        // e[v].push_back(edge(u, w)); //双向边
    }
    dijkstra();
    for(int i = 1; i <= n; i++) 
    {
        if(i != 1) printf(" ");
        printf("%lld", dis[i]);
    }
    return 0;
}
```

例题：

```c++
1.Til the Cows Come Home
要分析出这是个无向图
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>
#include <cstring>
#include <cstdio>
using namespace std;
typedef long long ll;
const ll inf = (1LL << 31) - 1;
const int N = 1e3 + 5;

struct edge{
    int to; ll w;
};
struct node{
    int id; ll dis;//dis 为该点到起点的距离
    bool operator <(const node &x)const{ return dis > x.dis;}
};

int n, m, s = 1;//起点s
ll dis[N];//记录 所有节点 到 起点s 的距离
bool vis[N];//记录是否已找到 节点i 的 最短距离
vector<edge> e[N];

void dijkstra()
{
    for(int i = 1; i <= n; i++) dis[i] = inf;
    dis[s] = 0;
    priority_queue<node> q;
    node sta = {s, 0};
    q.push(sta);
    while(!q.empty())
    {
        node u = q.top(); q.pop();
        if(vis[u.id]) continue;
        vis[u.id] = 1;
        for(int i = 0; i < e[u.id].size(); i++)//遍历邻居
        {
            edge ne = e[u.id][i];
            if(!vis[ne.to] && dis[ne.to] > u.dis + ne.w)//更新邻居点的最短距离
            {
                dis[ne.to] = u.dis + ne.w;
                node next = {ne.to, dis[ne.to]};
                q.push(next);
            }
        }
    }
}

int main()
{
    scanf("%d%d", &m, &n);
    for(int i = 0; i < m; i++)
    {
        int u, v; ll w;
        scanf("%d%d%lld", &u, &v, &w);
        edge e1 = {v, w}, e2 = {u, w};
        e[u].push_back(e1);
        e[v].push_back(e2); //双向边
    }
    dijkstra();
    printf("%lld", dis[n]);
    return 0;
}



*2.Heavy Transportation
题意：图里有不同载重限度的路段，一条完整的路上由不同的路段连接，货车重量需能经过其中最小载重限度的路段，整条路才算连通。
求 1 ~ n 路上 各路段都采取最大限重连接后 的最小限重路段的限重，即邻居点找最长边连接。
最长路径问题，题目较特殊，采用非优先队列写法，用邻接矩阵存储方便。
实际上该题应用最大生成树更合适，但现在拿来练习 Dijkstra算法的变形写法，有点类似动态规划。
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>
#include <cstring>
#include <cstdio>
using namespace std;
typedef long long ll;
const int N = 1e3 + 5;
int n, m, s = 1;//起点s
int dp[N];//dp[i] 定义为 从 起点1 到 点i 路上的最小限重值
int gra[N][N];
bool vis[N];
void dijkstra()
{
    for(int i = 1; i <= n; i++) dp[i] = gra[1][i];//状态初始化为直连情况下的限重值，这样来对比非直连哪个限重更高
    memset(vis, 0, sizeof(vis));
    dp[1] = 0;
    vis[1] = 1;
    //最大生成树，遍历 n-1 次，即取前 n - 1 大的边来连通 n 个点
    for(int i = 1; i < n; i++)
    {
        int _max = -1, ne = 0;
        for(int j = 1; j <= n; j++)//寻找最大限重的中间点
            if(!vis[j] && dp[j] > _max)
                _max = dp[j], ne = j;
        vis[ne] = 1;
        for(int j = 1; j <= n; j++)//更新dp状态，如果经历 中间点ne 的 非直连路中最小限重值更大，就取该非直连路径
            dp[j] = max(dp[j], min(dp[ne], gra[ne][j]));
    }
}
int main()
{
    int T;
    scanf("%d", &T);
    for(int k = 1; k <= T; k++)j
    {
        scanf("%d%d", &n, &m);
        memset(gra, -1, sizeof(gra));//求最长路径，初始化为无穷小，可判断点 i 与 j 是否为邻居
        for(int i = 0; i < m; i++)
        {
            int u, v, w;
            scanf("%d%d%d", &u, &v, &w);// All streets can be travelled in both directions.无向图
            gra[u][v] = gra[v][u] = w;// gra[u][v] = gra[v][u] = max(gra[u][v], w);//该题无重边
        }
        dijkstra();
        printf("Scenario #%d:\n%d\n\n", k, dp[n]);//Terminate the output for the scenario with a blank line.案例之间有空行
    }
    return 0;
}

前向星或邻接表 + 优先队列
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>
#include <cstring>
#include <cstdio>
using namespace std;
const int inf = (1LL << 31) - 1;
const int N = 1e5 + 5;
struct edge{
    int to, w;
};
struct node{
    int id, dis;//dis 为该点到起点的距离
    bool operator <(const node &x)const{ return dis < x.dis;}
};
int n, m, s = 1;
int dis[N];//记录 所有节点 到 起点s 的距离
bool vis[N];//记录是否已找到 节点i 的 最短距离
vector<edge> e[N];
void dijkstra()
{
    for(int i = 0; i <= n; i++) {dis[i] = 0, vis[i] = 0;}
    dis[s] = inf;
    priority_queue<node> q;
    node sta = {s, dis[s]};
    q.push(sta);
    while(!q.empty())
    {
        node u = q.top(); q.pop();
        if(vis[u.id]) continue;
        vis[u.id] = 1;
        for(int i = 0; i < e[u.id].size(); i++)//遍历邻居
        {
            edge ne = e[u.id][i];
            if(vis[ne.to]) continue;
            if(dis[ne.to] < min(dis[u.id], ne.w))
            {
                dis[ne.to] = min(dis[u.id], ne.w);
                node next = {ne.to, dis[ne.to]};
                q.push(next);
            }
        }
    }
}
int main()
{
    int T;
    scanf("%d", &T);
    for(int k = 1; k <= T; k++)
    {
        scanf("%d%d", &n, &m);
        for(int i = 0; i <= n; i++) e[i].clear();
        for(int i = 0; i < m; i++)
        {
            int u, v, w;
            scanf("%d%d%d", &u, &v, &w);
            edge e1 = {v, w}, e2 = {u, w};
            e[u].push_back(e1);
            e[v].push_back(e2); //双向边
        }
        dijkstra();
        printf("Scenario #%d:\n%d\n\n", k, dis[n]);//Terminate the output for the scenario with a blank line.案例之间有空行
    }
    return 0;
}



*3.昂贵的聘礼（Dijkstra + 枚举限制区间）
题意：有多样物品，每样物品可能有若干件替代品(以更低代价购得原品)，寻找 购得第一件物品所需最低总价钱的购买方案
     并且 能否购买i物品 受限于 购买的前一样物品j 与 i物品的地位系数差，|p[i] - p[j]| <= limit 才能购买
分析：对于第一件物品，它是必购买的，也就是购买的终点，可直接购买(增设原点)，也可购买其替代品集合中的某一件以代替购买
    故图中含有 原点0到每个物品的单向边(边权为物品本身的价钱) 和 物品间具有替代关系的单向边(边权为优惠价)
重点：虽然对所有路径全域条件为 p[i] - limit <= p[j] <= p[i] + limit，但若直接以此为条件不符合题意，涉及区间长度并非 limit 而是 2*limit
     故由于地位系数都是整数，对于一整条路径，满足题意的条件有 [p[i] - limit, p[j]] [p[i] - limit + 1, p[j] + 1] ··· [p[i], p[j] + limit]
     所以存在多个基于不同松弛条件的最短路，现枚举不同的松弛条件(区间)，并从所得的所有最短路中选出最小代价的最短路。
     而每条最短路必定经过 终点-点1，那么 对所有路径的全域范围 可简化为 只针对所有最短路的全域范围 [p[1] - limit, p[1] + limit]
16ms
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>
#include <cstring>
#include <cstdio>
using namespace std;
typedef long long ll;
const ll INF = 0x3f3f3f3f3f3f3f3fLL;
const int N = 105;
struct edge{
    int to; ll w;
    edge(int a = 0, ll b = 0){ to = a, w = b;}
};
struct node{
    int id; ll dis;
    node(int a = 0, ll b = 0){ id = a, dis = b;}
    bool operator <(const node &x)const{ return dis > x.dis;}
};
int n, limit, s = 0;
int level[N];
ll dis[N];
bool vis[N];
vector<edge> e[N];
ll dijkstra(int l, int r)
{
    memset(dis, 0x3f, sizeof(dis));
    memset(vis, 0, sizeof(vis));
    dis[s] = 0;
    priority_queue<node> q;
    q.push(node(s, 0));
    while(!q.empty())
    {
        node u = q.top(); q.pop();
        if(vis[u.id]) continue;
        vis[u.id] = 1;
        for(int i = 0; i < e[u.id].size(); i++)
        {
            edge ne = e[u.id][i];
            if(!vis[ne.to] && level[ne.to] >= l && level[ne.to] <= r && dis[ne.to] > u.dis + ne.w)
            {
                dis[ne.to] = u.dis + ne.w;
                q.push(node(ne.to, dis[ne.to]));
            }
        }
    }
    return dis[1];//返回到达终点1的当前最小价值
}
int main()
{
    scanf("%d%d", &limit, &n);
    for(int v = 1; v <= n; v++)
    {
        int k; ll w;
        scanf("%lld%d%d", &w, &level[v], &k);
        e[0].push_back(edge(v, w));
        while(k--)
        {
            int u; ll c;
            scanf("%d%lld", &u, &c);
            e[u].push_back(edge(v, c));
        }
    }
    ll ans = INF;
    for(int l = level[1] - limit; l <= level[1]; l++)
        ans = min(ans, dijkstra(l, l + limit));
    printf("%lld", ans);
    return 0;
}
79ms(由于 N <= 100，完全可以用邻接矩阵，结合dp思想解决。)
#include <iostream>
#include <algorithm>
#include <cstring>
#include <cstdio>
using namespace std;
typedef long long ll;
const ll INF = 0x3f3f3f3f3f3f3f3fLL;
const int N = 105;
int n, limit, s = 0;
int level[N];
ll dis[N], gra[N][N];
bool vis[N];
ll dijkstra(int l, int r)
{
    memset(dis, 0x3f, sizeof(dis));
    memset(vis, 0, sizeof(vis));
    dis[s] = 0;
    for(int i = 0; i <= n; i++)//从 自设原点0 到 点n 共 n + 1 个点，也就是子图大小拓展至全图大小
    {
        int ind = -1;
        for(int j = 0; j <= n; j++)//寻找当前未使用的最短边，同时也是在确定以ind为终点的最短路是否已确定
            if(!vis[j] && (ind == -1 || dis[j] < dis[ind])) ind = j;
        vis[ind] = 1;//不论是直连(初始状态)或非直连(已经过更新)，以ind为终点的最短路此时便能确定
        for(int j = 1; j <= n; j++)//遍历所有终点的最短路，更新其状态，判断接入或不接 0与ind的直连边(或者是终点为ind的最短路径)
            if(level[j] >= l && level[j] <= r)
                dis[j] = min(dis[j], dis[ind] + gra[ind][j]);
    }
    return dis[1];
}
int main()
{
    memset(gra, 0x3f, sizeof(gra));
    scanf("%d%d", &limit, &n);
    for(int v = 1; v <= n; v++)
    {
        int u, k; ll c, w;
        scanf("%lld%d%d", &w, &level[v], &k);
        gra[0][v] = w;
        while(k--) scanf("%d%lld", &u, &c), gra[u][v] = c;
    }
    ll ans = INF;
    for(int l = level[1] - limit; l <= level[1]; l++)
        ans = min(ans, dijkstra(l, l + limit));
    printf("%lld", ans);
    return 0;
}



4.Frogger（最短路径中最长路段最小值 -- 最短路极大极小问题）
题意：求最短路中最长路段的最小值，起点1 与 终点2 均唯一确定
#include <iostream>
#include <algorithm>
#include <cstring>
#include <cstdio>
#include <cmath>
using namespace std;
typedef double db;
const int N = 205;
struct p{
    int x, y;
    p(int a = 0, int b = 0){ x = a, y = b;}
}point[N];
db getdis(p e1, p e2) { return 1.0 * (e1.x - e2.x) * (e1.x - e2.x) + 1.0 * (e1.y - e2.y) * (e1.y - e2.y);}
int n, T = 0, s = 1, sx, sy, ex, ey;
db dis[N];
bool vis[N];
db dijkstra()
{
    dis[s] = 0;
    memset(vis, 0, sizeof(vis));
    for(int i = 2; i <= n; i++) dis[i] = getdis(point[1], point[i]);
    for(int i = 1; i <= n; i++)//子图拓展
    {
        int ind = -1;
        for(int j = 1; j <= n; j++)//找现存最短路径
            if(!vis[j] && (ind == -1 || dis[j] < dis[ind])) ind = j;
        vis[ind] = 1;
        for(int j = 1; j <= n; j++)//更新最短路中最大路段的最小值
            dis[j] = min(dis[j], max(dis[ind], getdis(point[ind],point[j])));
    }    
    return sqrt(dis[2]);
}
int main()
{
    while(~scanf("%d", &n), n)
    {
        for(int i = 1, x, y; i <= n; i++)
            scanf("%d%d", &x, &y), point[i] = p(x, y);
        printf("Scenario #%d\nFrog Distance = %.3lf\n\n", ++T, dijkstra());
    }
    return 0;
}



*5.通往奥格瑞玛的道路 (最短路 + 二分答案 求解极大极小问题)
题意：在所有符合血量条件的路径中，都有一个城市最大收费 x，得出其在所有路径中的最小值
思路：用dijkstra维护血量消耗，尽量使其符合血量条件；二分最大收费 x，大于该 x 的二分值的城市不走，二分完毕后得到的便是最小值。
#include <iostream>
#include <algorithm>
#include <queue>
#include <vector>
#include <cstring>
#include <cstdio>
using namespace std;
typedef long long ll;
const int N = 1e4 + 5;
struct edge{
    int to; ll w;
    edge(int a = 0, ll b = 0){ to = a, w = b;}
};
struct node{
    int id; ll dis;//dis 为该点到起点的距离
    node(int a = 0, ll b = 0){ id = a, dis = b;}
    bool operator <(const node &x)const{ return dis > x.dis;}
};
int n, m, blood, s = 1;//起点s
ll dis[N], cost[N];
bool vis[N];
vector<edge> e[N];
bool dijkstra(ll mid)
{
    if(mid < cost[s]) return 0;//优化，若起点都走不了，就返回
    memset(dis, 0x3f, sizeof(dis));
    memset(vis, 0, sizeof(vis));
    dis[s] = 0;
    priority_queue<node> q;
    q.push(node(s, dis[s]));
    while(!q.empty())
    {
        node u = q.top(); q.pop();
        if(vis[u.id]) continue;
        vis[u.id] = 1;
        for(int i = 0; i < e[u.id].size(); i++)
        {
            edge ne = e[u.id][i];
            if(!vis[ne.to] && mid >= cost[ne.to] && dis[ne.to] > u.dis + ne.w)
            {
                dis[ne.to] = u.dis + ne.w;
                if(ne.to == n) return dis[n] <= blood;//到n点
                q.push(node(ne.to, dis[ne.to]));
            }
        }
    }
    return 0;//无法到达点 n
}
int main()
{
    ll l = 1, r = 1, ans = -1;
    scanf("%d%d%d", &n, &m, &blood);
    for(int i = 1; i <= n; i++) scanf("%lld", &cost[i]), r = max(r, cost[i]);
    for(int i = 0; i < m; i++)
    {
        int u, v; ll w;
        scanf("%d%d%lld", &u, &v, &w);
        e[u].push_back(edge(v, w));
        e[v].push_back(edge(u, w)); //双向边
    }
    while(l <= r)
    {
        ll mid = (l + r) >> 1;
        if(dijkstra(mid)) r = mid - 1, ans = mid;
        else l = mid + 1;
    }
    if(ans == -1) printf("AFK");//一个符合条件的路径都没有
    else printf("%lld\n", ans);
    return 0;
}
```



---

# 三. 字符串

---

## Manacher算法

---

### 应用场景

























---

## 字典树（Trie）

---

### 应用场景

现有一个字符串匹配问题：在 `n` 个字符串中，查找某个字符串，设字符串的平均长度为 `m`，若用暴力法逐个匹配每个字符串，复杂度为 `O(n * m)`，效率低下。

字典树解法（以仅考虑小写英文单词为例，即只有26个小写字母）：

* 模拟查字典，比如单词 `coder`，先查 `c` 的部分，再查 `o`，依次查到最后的 `r`，即查找了 5 次，故查找次数最多为该单词的长度，与单词数量无关（这是字典树效率高的原因），所以字典树可以进行高强度检索，检索复杂度为 `O(m)`，由于单词平均长度不到 10，故可视为常数级 `O(1)`。

---

### 基本概念

> 字典树（Re**trie**val Tree），又称前缀树。它是一颗多叉树，由26个小写英文字母组成的字典树是 26 叉树，由数字组成的字典树是 10 叉树。

<img src="https://oi-wiki.org/string/images/trie1.png" alt="trie1" style="zoom: 67%;" />

字典树依据多个单词存储时是**共用相同的前缀**，一个单词产生于**一整条链**，链的终点是根据结点上的**终止标志**（代表该节点是否是一个单词的末尾），如字符串 `caaa` 是由链 `1 -> 4 -> 8 -> 12 -> 15` 产生的。

* 优点：时间复杂度优秀，对插入和查找都是 `O(1)`。

* 缺点：空间复杂度较差，对每个结点至少都有一个静态数组，如英文单词的字典树一个结点需要 `son[26]` 的大小，然而大部分空间都不会用上，导致**空间浪费严重**。

常见应用：

* 字符串检索。
* 词频统计。即统计一个单词的出现次数。
* 字典序排序。插入时在树的平级**按字母表的顺序**插入，待字典树建好后，用**先序遍历**就能得到字典树的排序。
* 前缀匹配。

定义结点结构体：

```c++
struct nd{
    int son[26];//26个分叉
    int num;	//统计前缀出现次数		——前缀问题
    bool repeat;//前缀(或单词)是否查询过 ——判断是否重复查询
    bool isend; //判断词尾		 	   ——判断单词存在性
    //若为叶子结点，前缀即代表一个单词
};
```

基本操作：

* 插入

  ```c++
  void Insert(string s)
  {
      int p = 0;//从根结点开始搜
      for(int i = 0; i < s.size(); i++)
      {
          int id = s[i] - 'a';
          if(!tree[p].son[id])        //若不存在链 根节点 -> id 表示的前缀，则插入
              tree[p].son[id] = cnt++;//让它指向下一个空闲空间，延长链
          p = tree[p].son[id];        //获取链的下一个结点，沿着该链走
          //注意这里处理对象是下一个结点的数据，假设链 0 -> 1 表示字符'a'，'a'属于结点1的前缀，而并非是结点0的前缀
          tree[p].num++;              //前缀出现次数增加
      }
      tree[p].isend = 1;              //词尾标记
  }
  ```

* 查询

  ```c++
  int Find(string s)// 0-单词不存在，1-单词存在，2-单词查询重复了
  {
      int p = 0;
      for(int i = 0; i < s.size(); i++)
      {
          int id = s[i] - 'a';
          if(!tree[p].son[id])//链中断，该前缀不存在
              return 0;
          p = tree[p].son[id];
      }
      //此时 p 为单词终点，则结点p的前缀的存在性即为整个单词的存在性
      if(!tree[p].isend) return 0;
      if(tree[p].repeat) return 2;//重复
      tree[p].repeat = 1;
      return 1;
      //return tree[p].num;//查询单词出现次数
  }
  ```

完整模板代码：

[P2580 于是他错误的点名开始了](https://www.luogu.com.cn/problem/P2580)

```c++
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <string>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 1e6;

int n, m, cnt = 1;//编号0 留给根节点
string s;

struct nd{
    int son[26];//字母ch存在 son[ch - 'a] ≠ 0
    int num;    //统计这个前缀出现次数
    bool repeat;//这个前缀是否重复
    bool isend; //词尾标记，即该链遍历到该节点是否是一个完整的单词
}tree[N];

void Insert(string s)
{
    int p = 0;//从根结点开始搜
    for(int i = 0; i < s.size(); i++)
    {
        int id = s[i] - 'a';
        if(!tree[p].son[id])        //若不存在链 根节点 -> id 表示的前缀，则插入
            tree[p].son[id] = cnt++;//让它指向下一个空闲空间，延长链
        p = tree[p].son[id];        //获取链的下一个结点，沿着该链走
        //注意这里处理对象是下一个结点的数据，假设链 0 -> 1 表示字符'a'，'a'属于结点1的前缀，而并非是结点0的前缀
        tree[p].num++;              //前缀出现次数增加
    }
    tree[p].isend = 1;              //词尾标记
}

int Find(string s)
{
    int p = 0;
    for(int i = 0; i < s.size(); i++)
    {
        int id = s[i] - 'a';
        if(!tree[p].son[id])//链中断，该前缀不存在
            return 0;
        p = tree[p].son[id];
    }
    //此时 p 为单词终点，则结点p的前缀的存在性即为整个单词的存在性
    if(!tree[p].isend) return 0;
    if(tree[p].repeat) return 2;//重复
    tree[p].repeat = 1;
    return 1;
}

int main()
{
    untie();
    cin >> n;
    for(int i = 0; i < n; i++)
    {
        cin >> s;
        Insert(s);
    }
    cin >> m;
    for(int i = 0; i < m; i++)
    {
        cin >> s;
        int res = Find(s);
        if(res == 1) cout << "OK\n";
        else if(res == 2) cout << "REPEAT\n";
        else cout << "WRONG\n";
    }
    return 0;
}
```

### 查找\匹配问题

```c++
1.全文检索
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <string>
#include <cstring>
#include <set>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 1e6;
int n, m, cnt = 1;
string text, t;
vector<int> ans;
bool vis[N];
struct nd{
    int son[10];
    int id;     //关键字编号
}tree[N];
void Insert(string s, int id)
{
    int p = 0;
    for(int i = 0; i < s.size(); i++)
    {
        int id = s[i] - '0';
        if(!tree[p].son[id])
            tree[p].son[id] = cnt++;
        p = tree[p].son[id];
    }
    tree[p].id = id;
}
int Find(string s)
{
    int p = 0;
    for(int i = 0; i < s.size(); i++)
    {
        int id = s[i] - '0';
        p = tree[p].son[id];
        if(!p) break;                           //词尾的下一个结点p为空
        if(tree[p].id != 0) return tree[p].id;  //找到
    }
    return 0;                                   //没找到
}
int main()
{
    untie();
    cin >> n >> m;
    for(int i = 0; i < n; i++) cin >> t, text += t;//连起来，防止关键字跨行存在
    for(int i = 1; i <= m; i++)
    {
        string tt;
        cin >> tt >> tt >> tt >> t;//这里不能直接用getline(cin, t, ']');会Presentation Error
        //scanf("%*s%*s%d%*c%s", &num, str); 也可以这样， %* 代表仅读取吸收但不会赋给后面的变量
        Insert(t, i);
    }
    for(int i = 0; i < text.size(); i++)
    {
        int res = Find(text.substr(i));//枚举寻找，关键字长度不定，搜索后面时长度判断起来麻烦，干脆直接全取了
        if(res && !vis[res])
        {
            vis[res] = 1;
            ans.push_back(res);
        }
    }
    if(ans.empty()) cout << "No key can be found !\n";
    else
    {
        cout << "Found key:";
        for(int i = 0; i < ans.size(); i++) cout << " [Key No. " << ans[i] << "]";
        cout << '\n';
    }
    return 0;
}



2.阅读理解
//存在 n = 1000 的数据卡空间，结点内容器存储 id 或 在外面开 bool ans[N][1001] 都会MLE，若不为 1001 则会 WA
//需要用 bitset 极致地降低空间复杂度
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <string>
#include <cstring>
#include <bitset>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 5e5 + 10;
int n, m, cnt = 1;
string t;
struct nd{
    int son[26];
}tree[N];
bitset<1001> ans[N];//ans[p][id] = 1，表示单词 p 在第 id 行是存在的，比起将id信息存进结点，这样更省空间
void Insert(string s, int id)
{
    int p = 0;
    for(int i = 0; i < s.size(); i++)
    {
        int id = s[i] - 'a';
        if(!tree[p].son[id])
            tree[p].son[id] = cnt++;
        p = tree[p].son[id];
    }
    ans[p][id] = 1;
}
void Find(string s)
{
    int p = 0, ok = 1;
    for(int i = 0; i < s.size(); i++)
    {
        int id = s[i] - 'a';
        p = tree[p].son[id];
        if(!p) 
        {
            ok = 0;
            break;
        }
    }
    if(!ok) return;
    for(int i = 1; i <= n; i++)
        if(ans[p][i]) cout << i << " ";
}
int main()
{
    untie();
    cin >> n;
    for(int i = 1; i <= n; i++)
    {
        int num; cin >> num;
        while(num--)
        {
            cin >> t;
            Insert(t, i);
        }
    }
    cin >> m;
    while(m--)
    {
        cin >> t;
        Find(t);
        cout << '\n';
    }
    return 0;
}
//也可以给每行文本分别建一棵字典树
#include<iostream>
#include<string>
using namespace std;
namespace first{    
    const int N=5e3+100;
    int trie[1010][N][26]={0};
    int id[N]={0};
    int cnt[1010][N]={0};
    void insert(string str,int index)//第 index 个字典树
    {
        int p=0;
        for(int i=0;i<str.size();i++){
            int ch=str[i]-'a';
            if( !trie[index][p][ch] )
                trie[index][p][ch]=++id[index];
            p=trie[index][p][ch];
        }
        cnt[index][p]++;
    }
    int Find(string str,int index)//第 index 个字典树
    {
        int p=0;
        //trie编号
        for(int i=0;i<str.size();i++){
            int ch=str[i]-'a';
            p=trie[index][p][ch];
            if( !p )
                return 0;
        }
        return !!cnt[index][p];
    }
    void solve()
    {
        cin.tie(0)->sync_with_stdio(0);
        cout.tie(0)->sync_with_stdio(0);
        int n,m;
        cin>>n;
        for(int i=1;i<=n;i++){
            int T;
            cin>>T;
            while(T--){
                string temp;
                cin>>temp;
                insert(temp,i);
            }
        }
        cin>>m;
        for(int i=1;i<=m;i++)
        {
            string temp;
            cin>>temp;
            bool first=0;
            for(int j=1;j<=n;j++)//每次查找就遍历所有树
                if( Find(temp,j) )
                {
	                if(first)
	                    cout<<' '<<j;
	                else
	                	cout<<j,first=1;
				}
            cout<<'\n';
        }
    }
}
using namespace first;
signed main(void)
{
    solve();
    return 0;
}



3.Hat’s Words（单词串联问题）
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <string>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 5e5 + 10;
int cnt = 1, ind = 0;
string s[N];
struct nd{
    int son[26];
    bool isend;
}tree[N];
void Insert(string s)
{
    int p = 0;
    for(int i = 0; i < s.size(); i++)
    {
        int id = s[i] - 'a';
        if(!tree[p].son[id])
            tree[p].son[id] = cnt++;
        p = tree[p].son[id];
    }
    tree[p].isend = 1;
}
bool Find(string s)
{
    int p = 0, ok = 1;
    for(int i = 0; i < s.size(); i++)
    {
        int id = s[i] - 'a';
        p = tree[p].son[id];
        if(!p) return 0;
    }
    return tree[p].isend;
}
int main()
{
    untie();
    while(cin >> s[++ind]) Insert(s[ind]);
    for(int i = 1; i <= ind; i++)
        for(int len = 1; len < s[i].size(); len++)//枚举所有分解情况
            if(Find(s[i].substr(0, len)) && Find(s[i].substr(len)))
            {
                cout << s[i] << '\n';
                break;//防止重复输出同一个字符串
            }
    return 0;
}



4.Spy Syndrome 2（字典树 + dfs）
//题意：给定一个字符串 s，以及 m 个单词，而 s 是由原句经过 大写变小写、翻转、删除空格 转换的，现要求从 m 个单词和 s 推出原句 s0。
//思路：由于要匹配的文本 s 是逆序的，将 m 个单词取小写并翻转后再存入字典树。由于如 "ancestorandan" 和 设倒序后单词为"an"、"and"、"ancestor"，无法直接O(n)遍历推出 an 的位置是在哪里，因此需要 dfs 搜索每个单词的归属，输出可行方案。
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <string>
#include <cstring>
#include <cctype>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 1e6 + 10;
int n, m, cnt = 1;
string str, s[N];
vector<int> ans;
struct nd{
    int son[26];
    int id;
}tree[N];
void Insert(string s, int ind)
{
    int p = 0;
    for(int i = 0; i < s.size(); i++)
    {
        int id = s[i] - 'a';
        if(!tree[p].son[id])
            tree[p].son[id] = cnt++;
        p = tree[p].son[id];
    }
    tree[p].id = ind;
}
bool dfs(int now)//搜索长为 n 的文本 s
{
    if(now == n) 
    {
        for(auto i : ans)
            cout << s[i] << " ";
        return 1;
    }
    int p = 0;
    for(int i = now; i < str.size(); i++)
    {
        int id = str[i] - 'a';
        p = tree[p].son[id];
        if(!p) break;
        if(tree[p].id)//遍历到可匹配单词就深入搜索
        {
            ans.push_back(tree[p].id);
            if(dfs(i + 1)) return 1;
            ans.pop_back();//能到这里说明方案不行，回溯
        }
    }
    return 0;
}
int main()
{
    untie();
    cin >> n >> str >> m;
    for(int i = 1; i <= m; i++)
    {
        cin >> s[i];
        string t = s[i];
        reverse(t.begin(), t.end());
        for(auto &ch : t)//转小写并翻转存入字典树中
            if(isupper(ch)) ch += 'a' - 'A';
        Insert(t, i);
    }
    dfs(0);     
    return 0;
}
```



### 统计前缀问题

```c++
1.Shortest Prefixes
//题意：给 n 个字符串 s，输出每个字符串的最短唯一前缀 s0，即 s0 是且仅是 s 的前缀，而不是其他任何单词的前缀，且 s0 可以为 s，题目还提出若当 s0 == s 时 s0 还可能是其他单词的前缀的话仍是输出 s0。
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <string>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 1e6;
int n, m, cnt = 1;
string s[1005];
struct nd{
    int son[26];
    int num;    //统计这个前缀出现次数
}tree[N];
void Insert(string s)
{
    int p = 0;
    for(int i = 0; i < s.size(); i++)
    {
        int id = s[i] - 'a';
        if(!tree[p].son[id])
            tree[p].son[id] = cnt++;
        p = tree[p].son[id];
        tree[p].num++;
    }
}
string Find(string s)
{
    int p = 0;
    for(int i = 0; i < s.size(); i++)
    {
        int id = s[i] - 'a';
        p = tree[p].son[id];
        if(tree[p].num == 1)
        {
            return s.substr(0, i + 1);
        }
    }
    return s;
}
int main()
{
    untie();
    int ind = 0;
    while(cin >> s[++ind])
    {
        Insert(s[ind]);
    }
    for(int i = 1; i <= ind; i++)
    {
        cout << s[i] << ' ' << Find(s[i]) << '\n';
    }
    return 0;
}
    
    
    
2.Secret Message G（必须理解整个过程）
//题意：给定若干字符串s1，并询问若干字符串s2，问有多少s1能匹配s2，匹配机制是满足取两者 较小者 可当作 较大者的前缀，即实际上这里的前缀是较小者的一整个单词
//当s2长度大于s1时，即s2长度超出字典树最大可匹配子串的长度，肯定遍历不完s2就结束查询循环，只需统计cnt即有多少个s1单词是在s2链上的
//当s2长度小于等于s1时，此时s2整个单词作为s1的前缀，统计前缀s2的出现次数num，减去上述循环最后s1 == s2时重复计数的cnt
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <string>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 1e6;
int n, m, cnt = 1;
char s[N];
struct nd{
    int son[2];
    int num;    //统计这个前缀出现次数
    int cnt;    //统计单词的出现次数
    //对于一条完整链，即一个单词，到达词尾时，会有 num 和 cnt 的重复计数，因为此时前缀就是一整个单词
}tree[N];
void Insert(char *s, int len)
{
    int p = 0;
    for(int i = 0; i < len; i++)
    {
        int id = s[i] - '0';
        if(!tree[p].son[id])
            tree[p].son[id] = cnt++;
        p = tree[p].son[id];
        tree[p].num++;
    }
    tree[p].cnt++;
}
int Find(char *s, int len)
{
    int p = 0, res = 0;
    bool ok = 0;
    for(int i = 0; i < len; i++)
    {
        int id = s[i] - '0';
        if(!tree[p].son[id])//链中断，即没有遍历完的情况s2 > s1
        {
            ok = 1;
            break;
        }
        p = tree[p].son[id];
        res += tree[p].cnt;//此时s2 >= s1，加上s1整个单词的出现次数（若最后 s2 == s1，则这里是重复计数了）
    }
    if(!ok)//s2 <= s1
    {
        //直接加上有多少单词以s2为前缀，并减去重复计数（s2 == s1时）的cnt
        res += tree[p].num - tree[p].cnt;
    }
    return res;
}
int main()
{
    untie();
    cin >> n >> m;
    int x;
    while(n--)
    {
        cin >> x;
        for(int i = 0; i < x; i++) cin >> s[i];
        Insert(s, x);
    }
    while(m--)
    {
        cin >> x;
        for(int i = 0; i < x; i++) cin >> s[i];
        cout << Find(s, x) << '\n';
    }
    return 0;
}



3.Phone List（排序 + 字典树前缀统计）
//题意：给 T 个字符串组，每组有 n 个字符串，判断组内 n 个字符串 s 是否均独特（即 s 不与组内任一其他字符串公用前缀）。如 9135 和 91125 不组成 独特字符串组，因为 91 是它们的公共前缀。
//思路：标记单词前缀。因为只有比 s 短的字串才可能成为 s 的前缀，所以可以由短到长排序，边插入边判断之前插入的短字串是否为当前单词的前缀 即可。
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <string>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 1e5 + 10;
int n, m, cnt = 1;
string s[10005];
struct nd{
    int son[10];
    int num;    //统计前缀出现次数
}tree[N];
bool Insert(string s)
{
    int p = 0;
    for(int i = 0; i < s.size(); i++)
    {
        int id = s[i] - '0';
        if(!tree[p].son[id])
            tree[p].son[id] = cnt++;
        p = tree[p].son[id];
        tree[p].num++;
    }
    return tree[p].num > 1;//除了它自身还有别的字串
}
bool cmp(const string &s1, const string &s2) { return s1.size() > s2.size();}
void Solve()
{
    cnt = 1;
    memset(tree, 0, sizeof(tree));//记得初始化

    int n;
    cin >> n;
    for(int i = 0; i < n; i++) cin >> s[i];
    sort(s, s + n, cmp);
    for(int i = 0; i < n; i++)
    {
        if(Insert(s[i]))
        {
            cout << "NO\n";
            return;
        }
    }
    cout << "YES\n";
}
int main()
{
    untie();
    int T;
    cin >> T;
    while(T--) Solve();
    return 0;
}



4.Problem C（字典树多操作维护）
//三种操作：插入单词、删除所有前缀为s的单词、查询前缀为s的单词是否存在
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cstring>
using namespace std;
const int N = 1e5 + 10;
struct nd{
    int son[26];
    int num;    //前缀出现次数
    void init()
    {
        num = 0;
        memset(son, 0, sizeof(son));
    }
}trie[N << 5];
int ind = 1;
char s[100];
void Insert(char *s)
{
    int len = strlen(s), p = 0;
    for(int i = 0; i < len; i++)
    {
        int id = s[i] - 'a';
        if(!trie[p].son[id]) trie[p].son[id] = ind++;
        p = trie[p].son[id];
        trie[p].num++;
    }
}
int Find(char *s)
{
    int len = strlen(s), p = 0;
    for(int i = 0; i < len; i++)
    {
        int id = s[i] - 'a';
        p = trie[p].son[id];
        if(!p) return 0;
    }
    return trie[p].num;
}
void Delete(char *s)//d 为删去前缀的出现次数，对应其经过的每个字符上都要减去d
{
    int d = Find(s);
    if(!d) return;  //防止多次Del，先查是否存在

    int len = strlen(s), p = 0;
    for(int i = 0; i < len; i++)
    {
        int id = s[i] - 'a';
        p = trie[p].son[id];
        if(!p) return;
        trie[p].num -= d;//删除标记
    }
    trie[p].init();
}
int main()
{
    int n;
    scanf("%d", &n);
    while(n--)
    {
        char op[10];
        cin >> op >> s;
        if(op[0] == 'i') Insert(s);
        else if(op[0] == 'd') Delete(s);
        else cout << (!Find(s) ? "No\n" : "Yes\n");
    }
    return 0;
}
```

### 异或值问题（01 - Trie）

#### · 最大异或和问题

​	以 [The xor-longest Path](http://poj.org/problem?id=3764) 为例：

* 异或和 定义：`S(u, v)` 为 两点间所有边权的异或值，如 4 个点间（起于`u`，终于v）有 3 条边 `w1`、`w2`、`w3`，其 `S(u, v) = w1 ^ w2 ^ w3`，即 “和” 的含义在异或操作中是“异或结合”。

* 题意：

  定义边权树 `G(u, v)`，定义最长路径为 最长异或路径，即两点权值的异或值越大，路径越长，求任意两点路径上异或值和的最大值 `max{S(u, v)}` —— 两结点间的异或长度为两点之间 所有边权值相异或 得到的最大值。

* 分析：

  由于 `(a xor b) xor (b xor c) = a xor c` 即 `b xor b = 0`，假设有两段异或和 `S1`、`S2`，它们都经过一条公共路径 `S0`，

  分别取出各自的 `S0`，有 `S1 ^ S2 = (S1' ^ S0) ^ (S2' ^ S0) = S1' ^ S2'`，故直接 `S1 ^ S2` 就能取出它们间相异的部分 `S1' ^ S2'`。

  ![异或和](C:\Users\asus\Desktop\异或和.png)

  如图，起点到 `u` 和 起点到 `v` 两条路中，黑边为重合路段，红色为 `u -> v` 路段。

* 思路：

  * 那么我们可以定义并预处理 `S(0, u)` 和 `S(0, v)`，则容易计算出 `S(u, v) = S(0, u) ^ S(0, v)`。

  * 已知所给边权树必然两两可连通，故我们只需要保存 各点u 到 根节点（设为0）的 异或和 `dis[u]`，

    易于取得 `S(u, v) = dis[u] ^ dis[v]`。

  * 设为无向图方便任意起点深搜，先 dfs 获得 根节点 到所有其他点 `i` 的路径异或权值和 `dis[i]`，而且树本身不成环，只要防止原路返回就行。
  
  * 贪心思想 - 查询时能选择相反方向就选，否则只能选相同方向（如现在某位上是 `1`，尽量走 `0`，使得异或值最大）。
  
  * 字典树的用处：先存起所有 `dis[i]`，然后遍历每个 `dis[i]` 并搜索树中存起的相异性最大的路径 `dis[x]`，
  
    返回 `dis[i] ^ dis[x]`（这里 `res` 实际上是仅当相异时在某数位上取 `1`，其余默认为 `0`）。

```c++
//1.The xor-longest Path（最大异或和）
//用vector邻接表存不论 C++ 还是 G++ 都会TLE
//链式前向星 和 bin[i]预处理 加速，G++ 可过
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <string>
#include <cstring>
#include <cctype>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 1e5 + 10;

int n, m, cnt = 1, ans = 0;
int tree[N << 5][2], dis[N];
int head[N], bin[50];

struct Edge{
    int to, w, next;
    Edge(int a = 0, int b = 0, int c = 0) {to = a, w = b, next = c;}
}edge[N << 2];

void init()
{
    cnt = 1;
    ans = 0;
    memset(head, 0, sizeof(head));
    memset(tree, 0, sizeof(tree));
    memset(dis, 0, sizeof(dis));
}

void addedge(int u, int v, int w)
{
    edge[cnt] = Edge(v, w, head[u]);
    head[u] = cnt++;
}

void dfs(int u, int father)
{
    for(int i = head[u]; i; i = edge[i].next)//编号从 1 开始
    {
        int v = edge[i].to;
        if(v == father) continue;
        dis[v] = dis[u] ^ edge[i].w;
        dfs(v, u);
    }
}

void Insert(int x)
{
    int p = 0;
    for(int i = 31; i >= 0; --i)//因为最大w为2^31，故可定为31位二进制，从高位（高位数对异或值影响最大）搜索最大异或方案
    {
        bool id = x & bin[i];//取第 i 数位，转 bool，相当于 id = ((x & bin[i]) != 0)
        if(!tree[p][id]) tree[p][id] = cnt++;
        p = tree[p][id];
    }
}

int Find(int x)
{
    int p = 0, res = 0;
    for(int i = 31; i >= 0; --i)//如果 i 最大定为 32， bin[i] = 1 << i 会越界
    {
        bool id = x & bin[i];            
        if(tree[p][id ^ 1]) p = tree[p][id ^ 1], res += bin[i];//尽量往不同方向走，使得异或值在第i位为1
        else p = tree[p][id];
        if(!p) break;//这里其实不需要，该树上二分叉必有一叉可走
    }
    return res;
}

int main()
{
    bin[0] = 1;
    for(int i = 1; i <= 32; i++) bin[i] = bin[i - 1] << 1;
    untie();
    
    while(cin >> n)
    {
        init();

        for(int i = 1; i < n; i++)
        {
            int u, v, w;
            cin >> u >> v >> w;
            ++u, ++v;
            addedge(u, v, w);
            addedge(v, u, w);
        }
        
        dfs(1, 1);//设起点为1

        for(int i = 1; i <= n; i++) Insert(dis[i]);//插入 所有点i 到 零点 的异或和
        for(int i = 1; i <= n; i++) ans = max(ans, Find(dis[i]));
        cout << ans << '\n';
    }
    return 0;
}



//2.Consecutive Sum（最大最小异或和）
//重点：搜索 最小异或和 时，由于自己与自己异或为 0，故很容易搜索到自己然后只得到 0，所以搜 dis[i] 时字典树里不应该含有 dis[i] 本身。
//所以在搜索 dis[i] 之后再插入 dis[i] 来拓展字典树，这样是可以包揽所有组合情况的 C(n, 2)。
//如 1 2 3 4 5，dis[1] 查 0 -> dis[2] 从 {1} 查 -> dis[3] 从 {1 2} 查 -> dis[4] 从 {1 2 3} 查 -> dis[5] 从 {1 2 3 4}，所以可包揽所有两两组合
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
const int N = 5e4 + 100;
int n, T, cnt, mmin, mmax;
int dis[N], bin[N], trie[N << 5][9];//dis[i] 存 a0 ~ ai 的异或和
void init()
{
    cnt = 1, mmin = 2e9, mmax = 0;
    memset(trie, 0, sizeof(trie));
}
void Insert(int x)
{
    int p = 0;
    for(int i = 31; i >= 0; --i)
    {
        bool id = x & bin[i];
        if(!trie[p][id]) trie[p][id] = cnt++;
        p = trie[p][id];
    }
}
int Find_xor_min(int x)
{
    int p = 0, res = 0;
    for(int i = 31; i >= 0; --i)
    {
        bool id = x & bin[i];
        if(trie[p][id]) p = trie[p][id];
        else p = trie[p][id ^ 1], res += bin[i];
        if(!p) break;
    }
    return res;
}
int Find_xor_max(int x)
{
    int p = 0, res = 0;
    for(int i = 31; i >= 0; --i)
    {
        bool id = x & bin[i];
        if(trie[p][id ^ 1]) p = trie[p][id ^ 1], res += bin[i];
        else p = trie[p][id];
        if(!p) break;
    }
    return res;
}
int main()
{
    bin[0] = 1;
    for(int i = 1; i <= 31; ++i) bin[i] = bin[i - 1] << 1;
    scanf("%d", &T);
    for(int _ = 1; _ <= T; ++_)
    {
        init();
        
        scanf("%d", &n);
        for(int i = 1; i <= n; i++) 
        {
            scanf("%d", &dis[i]);
            dis[i] ^= dis[i - 1];
        }
        
        Insert(0);//由于所有最值都至少是 dis[i] 本身，所以加入 0 让它们能异或 0 而得到自己本身的值
        
        for(int i = 1; i <= n; i++) 
        {
            // cout << i << ": " << Find_xor_max(dis[i]) << " " << Find_xor_min(dis[i]) << '\n';
            mmin = min(mmin, Find_xor_min(dis[i]));
            mmax = max(mmax, Find_xor_max(dis[i]));
            Insert(dis[i]);
        }
        printf("Case %d: %d %d\n", _, mmax, mmin);
    }
    return 0;
}
```

#### · 极大极小异或值问题

以 [Choosing The Commander](https://codeforces.com/problemset/problem/817/E) 为例：

* 题意：

  士兵中选指挥官 j（具有 Pj 和 Lj），当且仅当 Pi ^ Pj < Lj 则其他士兵 i 对 j 尊敬，

  有三种操作：插入士兵 Pi、删除士兵 Pi、选指挥官（具有 Pj 和 Lj）并统计有多少士兵对其尊敬。

* 思路：

  正常进行插入和删除操作，对于询问操作我们搜索 Pi，并贪心地讨论 Lj 的每一位 x（即从高位开始）。

  举个例子，对数 1010110，我们从高位 1 开始找是否有比 1010110 小的数，明显该位上为 0 的数都可以；随后到 0，明显怎么找都找不到比 0 小的。

* 结论：

  若数位 x 为 1，则 Pi ^ Pj 有机会两个数该数位异或结果为 0 而使得 Pi ^ Pj < Lj （只要高位小于，必成立），加上同数位上同方向的num；

  然后此时对该 x 满足条件的士兵 Pj 已经全部加上，那么搜索 Pj 就得切换方向找其他士兵，不然会只是重复运算而已若数位 x 为 0，则不可能找得到异或后比 Lj 小的数，那只能沿着 pi 的路走（此时局部上 Pi ^ Pj == Lj）

```c++
*1.Choosing The Commander
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cstring>
using namespace std;
const int N = 1e5 + 10, MAXL = 2e9;
struct nd{
    int son[2];
    int num;   //前缀出现次数
}trie[N << 5];
int q, cnt = 1, bin[50];
void Insert(int x)
{
    int p = 0;
    for(int i = 30; i >= 0; --i)
    {
        bool id = x & bin[i];
        if(!trie[p].son[id]) trie[p].son[id] = cnt++;
        p = trie[p].son[id];
        ++trie[p].num;
    }
}
void Delete(int x)
{
    int p = 0;
    for(int i = 30; i >= 0; --i)
    {
        bool id = x & bin[i];
        p = trie[p].son[id];
        --trie[p].num;
    }
}
int Search(int x, int lim)//x 为待评估指挥官 Pj
{
    int p = 0, res = 0;
    for(int i = 30; i >= 0; --i)//搜索其他士兵 Pi
    {
        bool id = x & bin[i];
        bool limx = lim & bin[i];
        if(limx)
        {
            //加上数位相同的士兵
            res += trie[trie[p].son[id]].num;//重点：要往下走一格，因为当前对应次数存在下一个结点
            p = trie[p].son[id ^ 1];         //可以选不一样的数字，找其他可能满足小于条件的士兵
        }
        else
        {
            p = trie[p].son[id];             //limx = 0 只能查原路（此后 Pi ^ Pj <= lim，即存在小于的可能性），因为另一条路是恒为 Pi ^ Pj > lim 的
        }
        if(!p) break;//*重点：存在 trie[p].son[] = 0 即无效路径，故这里防止再回到根节点 0
    }
    return res;
}
int main()
{
    bin[0] = 1;
    for(int i = 1; i <= 30; ++i) bin[i] = bin[i - 1] << 1;

    scanf("%d", &q);
    while(q--)
    {
        int op, p, l;
        scanf("%d%d", &op, &p);
        if(op == 1) Insert(p);
        else if(op == 2) Delete(p);
        else
        {
            scanf("%d", &l);
            printf("%d\n", Search(p, l));
        }
    }
    return 0;
}



2.Chip Factory
// 题目：维护 max{ ( Si + Sj ) ^ Sk } = max{ sum ^ Sk } (i != j != k，且 sum 为 两数之和)
// 思路：求最大异或值，枚举 Si + Sj 的组合 并 搜索 Sk，由于 k != i != j 则搜索前要在字典树中删除一次 Si 和 Sj（强调是一次即可，即可能还有这个数，但下标不一样就可以了），搜索后再插入一次回来。
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cstring>
using namespace std;
const int N = 5e3 + 10;

struct nd{
    int son[2];
    int num;
}trie[N << 5];
int T, n, cnt = 1;
int s[N], bin[50];

void init()
{
    cnt = 1;
    memset(trie, 0, sizeof(trie));
}

void Insert(int x)
{
    int p = 0;
    for(int i = 30; i >= 0; --i)
    {
        bool id = x & bin[i];
        if(!trie[p].son[id]) trie[p].son[id] = cnt++;
        p = trie[p].son[id];
        ++trie[p].num;
    }
}

void Delete(int x)
{
    int p = 0;
    for(int i = 30; i >= 0; --i)
    {
        bool id = x & bin[i];
        p = trie[p].son[id];
        --trie[p].num;
    }
}

int Find_xor_max(int x)
{
    int p = 0, res = 0;
    for(int i = 30; i >= 0; --i)
    {
        bool id = x & bin[i];
        if(trie[trie[p].son[id ^ 1]].num)//查当前前缀次数，需要往下一个结点找
        {
            res |= bin[i];               //存起异或结果值
            p = trie[p].son[id ^ 1];
        }
        else
            p = trie[p].son[id];
        if(!p) break;
    }
    return res;
}

int main()
{
    bin[0] = 1;
    for(int i = 1; i <= 31; ++i) bin[i] = bin[i - 1] << 1;

    scanf("%d", &T);
    while(T--)
    {
        init();

        scanf("%d", &n);
        for(int i = 0; i < n; i++) scanf("%d", &s[i]), Insert(s[i]);

        int mmax = 0;
        for(int i = 0; i < n; i++)//按组合情况枚举 s[i] + s[j] 组合
        {
            Delete(s[i]);
            for(int j = i + 1; j < n; j++)
            {
                Delete(s[j]);
                mmax = max(mmax, Find_xor_max(s[i] + s[j]));
                Insert(s[j]);
            }
            Insert(s[i]);
        }
        printf("%d\n", mmax);
    }
    return 0;
}
```





---

## KMP字符串匹配算法

---

### 应用场景

给定一个**主串**（以 S 代替）和**模式串**（以 P 代替），要求找出 P 在 S 中出现的位置，此即串的模式匹配问题。

暴力匹配法：从 S 的第一个字符开始（设其指针为 `i`），逐个匹配 P 的每个字符（设其指针为 `j`），如果对 P 的首字符就失配则右移指针 `i`，否则 `i` 不动并右移 `j`继续匹配 —— 若能匹配到 P 的最后一个字符则成功，否则**回溯到 `i` 原位置的下一个位置** `last_i + 1`。由于 **`i` 回溯操作**的存在导致最坏的时间复杂度为 `O(m * n)`，使得该方法效率普遍很低。

KMP字符串匹配算法（仅适用单模即只有一个模式串 P）：时间复杂度稳定在`O(m + n)`。



### 算法分析

我们发现，暴力方法每次让 `i` 回溯都会重复对上次已匹配的子串再判断一次，说明这里有多余的重复操作

如何让 `i` 不回溯（避免重复的匹配操作）？

可对失配时刻前已匹配部分 P' 进行分类讨论：

* 记**最长公共前后缀**长度为 `lps`，即下图相同的前缀串 1 和 后缀串 2 的最大长度 `lps = len(s1) = len(s2)`，

* (1) P 在失配点之前的每个字符**都不同**。已知失配点前面的子串 P' 仅当适配时刻与 P 某部分匹配，而当失配后需要 P 右滑继续进行匹配操作时，整个 P' 都不可能再与 P 的该部分匹配，故可以直接跨过 P' 的长度重新开始匹配，`j = 0`。

  <img src="D:\MarkdownText\image\字符串\KMP算法\P 的每个字符都不同.png" alt="P 的每个字符都不同" style="zoom:33%;" />

  ```ini
  			  i = 3
  S = [A  B  C]  A  B  C  D
       ✔ ✔  ✔	  × 
  P = [A  B  C]  D
  			  j = 3
  P'= [A  B  C]
    
    ⬇
  
  			i = 3
  S = A  B  C  A  B  C  D
       		✔ ✔  ✔  ✔ 
  P = 		A  B  C  D
  		    j = 0
  此时 P' 中每个字符都是独立唯一的，则失配时 i 不变，j 直接从 0 开始，即令 j 为 0
  ```

  

* (2) P 在失配点之前的字符**有相同的部分**
  * ① 相同部分为**真前缀**和**真后缀**（即不含自身的前后缀）。此时不能直接跨过 P'，因为还存在 S 的后缀与 P 的前缀匹配时的情况，故此时再从不同的部分开始匹配，`j = lps`。

    <img src="D:\MarkdownText\image\字符串\KMP算法\相同部分是前缀和后缀.png" alt="相同部分是前缀和后缀" style="zoom: 33%;" />

    如下例子： 

    ```ini
    					  i
    S = [A  B  C  D  A  B]  C  D  A  B  E
         ✔ ✔  ✔ ✔  ✔  ✔  × 
    P = [A  B  C  D  A  B]  E
                            j
    P'= [A  B  C  D  A  B]
      
      ⬇
    
    					i
    S = A  B  C  D  A  B  C  D  A  B  E
         		   ✔  ✔  ✔ ✔ ✔  ✔ ✔
    P = 		   A  B  C  D  A  B  E
                          j
    此时应当从不同的部分（这里是"CD"）开始匹配，即令 j 为 lps (这里 P[j] 为 'C'，lps 即 "AB" 长度为 2)
    ```

  * ② 相同的部分不是前缀**或**后缀（即子串 1 和 2 **都**不含任意端点）。此时可以直接跨过 P'，因为红框部分**绝不可能匹配**（否则不满足前述前提条件，必至少有一个前缀或后缀），`j = 0`。

    <img src="D:\MarkdownText\image\字符串\KMP算法\相同部分不是前缀或后缀.png" alt="相同部分不是前缀或后缀" style="zoom:33%;" />
    
    如下例子：
    
    ```ini
    						 i
    S = [A  B  C  D  B  C  E]  A  B  C  D  B  C  E  L
         ✔ ✔  ✔ ✔  ✔  ✔  ✔  × 
    P = [A  B  C  D  B  C  E]  L
    						 j
    P'= [A  B  C  D  B  C  E]
      
      ⬇
    
    					   i
    S = A  B  C  D  B  C  E  A  B  C  D  B  C  E  L
     					   ✔ ✔  ✔ ✔  ✔  ✔  ✔ ✔
    P =                      A  B  C  D  B  C  E  L
    					   j
    实际上基本跟情况(1)一样，此时可视为 P'各字符都不同 来处理，则令 j 为 0
    ```
    

那么这种方法唯一需要处理的就是**最长公共前后缀和** `lps[]` 数组（注意"**和**"字，因为失配点可以是 P 串的任意处，所以它的**所有连续子串**都需要预处理对应 `lps` 值）。

### 基本操作

**lps[] 的预处理**

定义：`lps[j]` 的值等于 `P[0] ~ P[j]` 这部分连续子串的前缀集合和后缀集合的最长交集的长度 —— 最长公共前后缀。

（也有的是定义 `lps[j]` 指向 `p[0] ~ p[j - 1]`，具体写法上有细微区别）

朴素方法 `O(n ^ 3)`：

```c++
vector<int> prefix_function(string s) 
{
  	int n = (int)s.length();
  	vector<int> lps(n);
  	for (int i = 1; i < n; i++)//处理子串 p[0] ~ p[i - 1]
    	for (int j = i; j >= 0; j--)//逆序处理，首次得到的就是最长的长度
      		if (s.substr(0, j) == s.substr(i - j + 1, j)) //前缀 == 后缀
            {
                lps[i] = j;
                break;
            }
  	return lps;
}
```

第一个优化 `O(n ^ 2)`：

<img src="D:\MarkdownText\image\字符串\KMP算法\lps预处理第一个优化.png" alt="lps预处理第一个优化" style="zoom:80%;" />

此时将 `lps[i]` 当作下标即指向下一个字符（即当前待判断的**新增字符**） `s[i + 1] = s[lps[i]]`，由于 `lps[i + 1]` **最大**只可能比上一次匹配多 1，否则为 0 或 减少，故 `j` 直接从 `lps[i] + 1` 开始逆序找更快。

```c++
vector<int> prefix_function(string s) 
{
  	int n = (int)s.length();
  	vector<int> lps(n);
  	for (int i = 1; i < n; i++)
    	for (int j = lps[i - 1] + 1; j >= 0; j--)  // j = i  ---->  j = lps[i - 1] + 1
      		if (s.substr(0, j) == s.substr(i - j + 1, j)) //每次更新的lps[i]最大为 lps[i - 1] + 1，若符合就会在第一次判断就结束本次 lps[i] 的更新，否则再往下找
      		{
        		lps[i] = j;
        		break;
      		}
  	return lps;
}
```

*第二个优化 `O(n)`：

在第一个优化的基础上，我们不妨更全面地探讨剩余的情况：当 `s[i + 1] ≠ s[lps[i]]` 时。

当 `s[i + 1]` 与 `s[lps[i]]` 失配时，我们应当希望找到仅次于 `lps[i]` 的**第二长度 `j = lps[i - 1]`**，且保持在位置 `i` 的前缀性质，假设它满足条件即 `s[0 ... j - 1] = s[i - j + 1 ... i]`，有：

![lps预处理第二个优化](D:\MarkdownText\image\字符串\KMP算法\lps预处理第二个优化.png)

那么接下来仅需要再比较 `s[i + 1]` 和 `s[j]`（在上图中即比较 `s[0 ... 2]` 和 `s[i - 1 ... i + 1]`），若它们相等匹配，就能记录下 `lps[i + 1] = j + 1`，否则我们又需要继续寻找仅次于 `j` 的长度 **`j' = lps[lps[i - 1] - 1] = lps[j - 1]`**，以此类推下去直到找到匹配点 或 `j = 0` 时，当 `j = 0` 时就是最后连 `s[0]` 和 `s[i + 1]` 都失配，此时只有 `lps[i + 1] = 0`。

我们发现规律：每次失配时，都有跳转操作 **`j = lps[j - 1]`** 直到匹配或 `j = 0` 终止。

```c++
vector<int> prefix_function(string s)
{
  	int n = (int)s.length();
  	vector<int> lps(n);
  	for (int i = 1; i < n; i++)
    {
    	int j = lps[i - 1];
    	while (j > 0 && s[i] != s[j]) j = lps[j - 1];//不匹配则跳转到此次长度
    	if (s[i] == s[j]) j++;
    	lps[i] = j;
  	}
  	return lps;
}
```













[P3375 【模板】KMP字符串匹配](https://www.luogu.com.cn/problem/P3375)

```c++
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <vector>
#include <string>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 1e6 + 10;

int lps[N];//这里 lps[i] 是指 p[0] ~ p[i] 的lps
string s, p;

void getlps(string p)
{
    int plen = p.size();
    lps[0] = 0;//首字符没有真前后缀（不能为自身）
    for(int i = 1; i < plen; i++) // 或直接 for(int i = 1, j = 0; ···)，省去 int j = lps[i - 1]
    {
        int j = lps[i - 1];
        while(j > 0 && p[i] != p[j]) j = lps[j - 1];
        if(p[i] == p[j]) ++j;//新增字符与 s[j] 相等就增长
        lps[i] = j;
    }
}

void kmp(string s, string p)
{
    getlps(p);//预处理 lps[i]
    int slen = s.size(), plen = p.size();
    int j = 0;
    for(int i = 0; i < slen; i++)//在主串上匹配
    {
        while(j > 0 && s[i] != p[j]) j = lps[j - 1];
        if(s[i] == p[j]) ++j;
        if(j == plen) cout << i - plen + 2 << '\n';//若 P 完全匹配 S'，输出 S 的匹配起始位置
    }
    //输出 lps[i]
    for(int i = 0; i < plen; i++) cout << lps[i] << ' ';
}

int main()
{
    untie();
    cin >> s >> p;
    kmp(s, p);
    return 0;
}
```

### 例题

```c++
1.P1470 [USACO2.3]最长前缀 Longest Prefix（KMP + DP）
//kmp匹配求得集合中各元素在字符串 S 中的出现位置，再用 dp 求得最长前缀长度
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <string>
#include <cstring>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 2e5 + 10;

int lps[N], dp[N];
vector<string> v;
vector<int> id[N];//id[x] 表示 s 的长度为 x 的前缀的匹配成功的模串长度集
string s, x;

void getlps(string p, int plen)
{
    lps[0] = 0;
    for(int i = 1; i < plen; i++)
    {
        int j = lps[i - 1];
        while(j && p[i] != p[j]) j = lps[j - 1];
        if(p[i] == p[j]) ++j;
        lps[i] = j;
    }
}

void kmp(string s, string p)
{
    int slen = s.size(), plen = p.size();
    getlps(p, plen);
    for(int i = 0, j = 0; i < slen; i++)
    {
        while(j && s[i] != p[j]) j = lps[j - 1];
        if(s[i] == p[j]) ++j;
        if(j == plen) id[i + 1].push_back(plen);
    }
}

int main()
{
    untie();
    while(cin >> s)
    {
        if(s[0] == '.') break;
        v.push_back(s);
    }
    s = "";
    while(cin >> x) s += x;//可能有多行
    int slen = s.size();
    for(auto str : v) kmp(s, str);
    dp[0] = 1;//意义是对那些 plen 正好组成当前长度 i 记为可延伸状态。
    for(int i = 1; i <= slen; i++)//枚举到达第 i 个字符 s[i - 1] 时(枚举长度)
        for(int j = 0; j < id[i].size(); j++)//枚举可选择的方案
            if(dp[i - id[i][j]]) dp[i] = 1;//可从上一个状态上延长
    int res = 0;
    for(int i = 1; i <= slen; i++) 
        if(dp[i]) res = max(res, i);//组成串能到达的最长长度
    cout << res;
    return 0;
}



2.Radio Transmission 无线传输（最短循环节问题）
假设有长为 m 的循环节 k，分别讨论：
① 长为 n 的字符串 S 由若干个完整的 k 组成（以下举例由 4 个 k 组成）
    k	  k      k      k
|______|______|______|______|
|=========lps========|		  那么这就是 lps 的长度
    			    |===m==| 而这段就是单个循环节的长度
显然有 n - lps = m
② 最后一个 k 不完整，即只截取了 k 的小段长为 m0 的前缀 k0 （以下举例由 5 个 k 组成，其中第 5 个 k 不完整）
    k     k      k       k    k0
|______|______|______|______|___|
|===========lps==========|
    		 ⬇
不难发现，lps就是 3 * k + k0
    k     k      k    k0  k-k0 k0
|______|______|______|___|__|___|
|===========lps==========|
       |===========lps==========|
    				       |=m0|
不难发现此时 n - lps = (m - m0) + m0 = m
总结：最短循环节长度为 n - lps[n]
#include <cstdio>
#include <algorithm>
#include <cstring>
using namespace std;
const int N = 1e6 + 10;
int lps[N];
char s[N];
void getlps(char *s, int len)
{
    for(int i = 1; i < len; i++)
    {
        int j = lps[i - 1];
        while(j && s[i] != s[j]) j = lps[j - 1];
        if(s[i] == s[j]) ++j;
        lps[i] = j;
    }
}
int main()
{
    int n;
    scanf("%d%s", &n, s);
    getlps(s, n);
    printf("%d\n", n - lps[n - 1]);//这里 lps[n - 1] 代表 s[0] ~ s[n - 1] 即长度为 n 的前缀的 lps长度。
    return 0;
}



3.Period（最短循环节问题）
//对于 S 的所有前缀 s（包括它自己），若 s 是由若干个完整的最短循环节 k 组成，则输出 s 的长度 和 最短循环节在 s 中的个数
#include <cstdio>
#include <algorithm>
#include <string>
#include <iostream>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 1e6 + 10;

int n, lps[N];
string s;

void getlps(int slen)
{
    for(int i = 1; i < slen; i++)
    {
        int j = lps[i - 1];
        while(j && s[i] != s[j]) j = lps[j - 1];
        if(s[i] == s[j]) ++j;
        lps[i] = j;

        int len = i + 1, k = len - lps[i];//最短循环节长度 k
        if(lps[i] && len % k == 0) 		  //必须由若干个完整的 k 组成，强调 k 是完整出现的
            cout << len << ' ' << len / k << '\n';
    }
}

int main()
{
    untie();
    int T = 1;
    while(cin >> n)
    {
        if(!n) break;
        cin >> s;
        int slen = s.size();
        cout << "Test case #" << T++ << '\n';
        getlps(slen);
        cout << '\n';
    }
    return 0;
}



4.Theme Section（前后缀问题）
//问题相当于求 字符串s 的 lps 然后得到公共前后缀的长度 k，再判断 s 的中缀 k 是否存在（在前缀 k 后，且在后缀 k 前），存在则输出 k
#include <cstdio>
#include <algorithm>
#include <string>
#include <iostream>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 1e6 + 10;

int t, lps[N];
string s;

int getlps(int slen)
{
    for(int i = 1; i < slen; i++)
    {
        int j = lps[i - 1];
        while(j && s[i] != s[j]) j = lps[j - 1];
        if(s[i] == s[j]) ++j;
        lps[i] = j;                    
    }
    return lps[slen - 1];
}

int main()
{
    untie();
    cin >> t;
    while(t--)
    {
        cin >> s;
        int slen = s.size();
        int k = getlps(slen), ok = 0;
        k = min(slen / 2, k);//前后缀长度不超过 s 长度的一半，如 "aaa" 的 lps = 2，当实际前后缀长度为 1
        if(!k) 
        {
            cout << "0\n";
            continue;
        }
        for(int i = k; i < slen - k; i++)
        {
            if(lps[i] == k)//判断中缀 k 是否存在
            {
                ok = 1;
                break;
            }
        }
        if(ok) cout << k << '\n';
        else cout << "0\n";
    }
    return 0;
}    



5.hdu 2328 Corporate Identity（最长公共子串问题 - KMP + 暴力）
//求 n 个字符串的最长公共子串（优先字典序最小）
//既然是求公共子串 lcs，那么可以先取第一个字符串 p 作为模式串，并枚举它的所有后缀来求 lps
//这样相当于包含了所有可能的公共子串情况
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <string>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false); cout.tie(0);}
const int N = 4e3 + 10, M = 210;

int n, plen, lps[M];
string s[N], p;

void getlps(int st)
{
    for(int i = 1; i + st < plen; i++)
    {
        int j = lps[i - 1];
        while(j && p[i + st] != p[j + st]) j = lps[j - 1];
        if(p[i + st] == p[j + st]) ++j;
        lps[i] = j;
    }
}
int kmp(string s, int st)
{
    int res = 0, slen = s.size();
    for(int i = 0, j = 0; i < slen; i++)
    {
        while(j && s[i] != p[j + st]) j = lps[j - 1];
        if(s[i] == p[j + st]) ++j;
        res = max(res, j);
    }
    return res;
}
int Solve(int st)
{
    int len = M;
    getlps(st);
    for(int i = 0; i < n; i++) len = min(len, kmp(s[i], st));//模串后缀与其他字串的所有最长匹配长度的最小值，就是公共最长匹配长度。
    return len;
}
bool cmp(int st_now, int st_last, int len)//字典序比较
{
    for(int i = 0; i < len; i++)
        if(p[i + st_now] != p[i + st_last])
            return p[i + st_now] < p[i + st_last];
    return 0;
}

int main()
{
    untie();
    while(cin >> n)
    {
        if(!n) break;
        cin >> p;//模式串
        n--;
        plen = p.size();
        int ind = 0, len = 0;//最长公共子串在 p 中位置 ind 及其长度 len
        for(int i = 0; i < n; i++) cin >> s[i];
        for(int i = 0; i < plen; i++)//取 p 的后缀，每个后缀求前缀lps，这样才能枚举到所有子串情况
        {
            int res = Solve(i);
            if(res > len || (res == len && cmp(i, ind, len)))//若等于则看字典序先后
            {
                len = res, ind = i;
                // cout << "TEXT : " << len << " " << ind << '\n';
            }
        }
        if(!len) 
        {
            cout << "IDENTITY LOST\n";
            continue;
        }
        for(int i = ind; i < ind + len; i++) cout << p[i];
        cout << '\n';
    }
    return 0;
}



*6.动物园
//定义数组 cnt[i] 为 前 i 个字符可重叠的相同前后缀个数，用这个来推出不重叠的相同前后缀个数 num[i]
//如长度为 8 的字符串 s = "abacdaba"，我们注意 "aba" 的存在，首先易知对于 "aba" 有 cnt[3] = 1
//再看整个字符串 s，有 cnt[8] = 2，这是由于公共前后缀有 "a" 和 "aba" 两个，
//记前缀 "aba" 为 pre，后缀 "aba" 为 suf，在求解 lps 的过程中，已知 pre == suf 即 pre的前缀'a' 与 suf的前缀'a' 是相同的 或者看 pre的后缀'a' 与 suf的后缀'a' 是相同的，又已知 cnt[3] = 1 即 pre的前缀'a'和后缀'a'是相同的，故 pre的前缀'a' 和 suf的后缀'a'是相同的，那么对于 cnt[8] 除了 "aba" 的贡献外还有 'a' 的贡献，故 cnt[8] = 2。
//再如 s = "aaacdaaa" 等。。。
//故当前 cnt[i] = 1（前后缀相同） + 该前缀（后缀）自身的 cnt[j]
#include <cstdio>
#include <algorithm>
#include <iostream>
#include <cstring>
using namespace std;
const int N = 1e6 + 10, mod = 1e9 + 7;

int T, lps[N];
int cnt[N];
char s[N];

void getlps(int len)
{
    cnt[1] = 1, lps[0] = 0;
    for(int i = 1; i < len; i++)
    {
        int j = lps[i - 1];
        while(j && s[i] != s[j]) j = lps[j - 1];
        if(s[i] == s[j]) ++j;
        lps[i] = j;
        cnt[i + 1] = cnt[j] + 1;
    }
}

int main()
{
    scanf("%d", &T);
    while(T--)
    {
        scanf("%s", s);
        int len = strlen(s);
        long long res = 1;
        getlps(len);
        int j = 0;
        for(int i = 1; i < len; i++)
        {
            // int j = lps[i - 1]; 这里这样写会 TLE
            while(j && s[i] != s[j]) j = lps[j - 1];
            if(s[i] == s[j]) ++j;
            while(j > (i + 1) / 2) j = lps[j - 1];//只能取不重叠的次长度
            res = res * (cnt[j] + 1ll) % mod;
        }
        printf("%lld\n", res);
    }
    return 0;
}
```



---









# 四. 动态规划

---

## 线性dp

### 基础例题

```c++
1.P8707 [蓝桥杯 2020 省 AB1] 走方格
//行列号都从 1 开始，当行、列号都为偶数时不能走该格，且规定只能向下或向右一格格走。求从点 (1, 1) 走到点 (n, m) 的方案数。
//定义 dp[i][j] 为走到点 (i, j) 的方案数，答案为 dp[n][m]
#include <iostream>
using namespace std;
const int N = 100;
int dp[N][N];
int main()
{
    int n, m;
    cin >> n >> m;
    dp[1][1] = 1;
    for(int i = 1; i <= n; ++i)
    {
        for(int j = 1; j <= m; ++j)
        {
            if(i & 1) dp[i][j] += dp[i][j - 1];
            if(j & 1) dp[i][j] += dp[i - 1][j];
        }
    }
    cout << dp[n][m];
    return 0;
}



2.P8786 [蓝桥杯 2022 省 B] 李白打酒加强版
//初始有酒 2 斗，遇店加倍（0 斗酒加倍也是合法的），遇花喝 1 斗（0 斗酒遇花不合法），已知最后一次遇到的是花，并且恰好把酒喝完。
//问当遇到 n 次店，m 次花，计算一路上遇到店和花的顺序，有多少不同的可能。
//易知酒量的大小最多为 m（与花数相同），定义 dp[i][j][k] 状态为经过了 i 家店和 j 朵花后剩下了 k 斗酒的方案数
//若酒量为奇数，则上一次必定是遇到花；若酒量为偶数，则上一次可能遇到店，也可能遇到花。
//因为规定最后一次遇到的必须是花，且遇到前恰好剩 1 斗酒，答案为 dp[n][m - 1]。若为 dp[n][m][0] 则无法保证最后一次遇到的是花！
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int N = 150, mod = 1e9 + 7;
ll dp[N][N][N];
int main()
{
    ll n, m;
    cin >> n >> m;
    dp[0][0][2] = 1;//初始有 2 斗酒
    for(int i = 0; i <= n; ++i)//遍历店数
    {
        for(int j = 0; j <= m; ++j)//遍历花数
        {
            if(i == 0 && j == 0) continue;
            for(int k = 0; k <= m; ++k)//遍历酒量
            {
                if(k & 1) dp[i][j][k] = j ? dp[i][j - 1][k + 1] : 0;
                else dp[i][j][k] = (j ? dp[i][j - 1][k + 1] : 0) + (i ? dp[i - 1][j][k / 2] : 0);
                dp[i][j][k] %= mod;
            }
        }
    }
    cout << dp[n][m - 1][1] % mod;
    return 0;
}



3.P8784 [蓝桥杯 2022 省 B] 积木画
//有一块面积大小为 2 * n 的画布，它由 2 * n 个 1 * 1 的块构成。现需要用 I型积木 和 L型积木 将画布填满
//问总共有多少种不同的搭配方案（积木可以任意旋转）
//设 dp[n][3]，即每一列的有三种填补情况，则 dp[i][0]、dp[i][1]、dp[i][2] 表示第 i 列两格填满（或者说上下积木数相同）、仅填上格、仅填下格的方案数
//dp[i][0]可转移四种状态，dp[i][1] 和 dp[i][2] 都是两种
#include <bits/stdc++.h>
using namespace std;
const int N = 1e7 + 10, mod = 1e9 + 7;
int n;
int dp[N][3];//开 long long 会炸空间
int main()
{
    cin >> n;
    dp[0][0] = 1;//初始化，一列都还没填，此时第二维的 0 表示上下积木数相同
    for(int i = 1; i <= n; ++i)
    {
        //注意防 i - 2 越界
        dp[i][0] = ((dp[i - 1][0] + (i >= 2 ? dp[i - 2][0] : 0)) % mod + (dp[i - 1][1] + dp[i - 1][2]) % mod) % mod;
        dp[i][1] = (dp[i - 1][2] + (i >= 2 ? dp[i - 2][0] : 0)) % mod;
        dp[i][2] = (dp[i - 1][1] + (i >= 2 ? dp[i - 2][0] : 0)) % mod;
    }
    cout << dp[n][0] % mod;
    return 0;
}



4.P8725 [蓝桥杯 2020 省 AB3] 画中漂流
//dp[i][j] 表示当到时间 i 时 剩下体力 j 的方案数，上一秒到当前秒要么消耗了体力，要么不消耗
//由于要求体力必须花光，故答案为 dp[t][0]
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int N = 3e3 + 10, M = 15e2 + 10, mod = 1e9 + 7;
int d, t, m;
int dp[N][M];
int main()
{
    cin >> d >> t >> m;
    dp[0][m] = 1;
    for(int i = 1; i <= t; ++i)//枚举时间
    {
        for(int j = 0; j <= m; ++j)//枚举体力剩余量，则消耗量为 m - j
        {
            int now = d + (m - j) - (i - (m - j));
            if(now > 0) dp[i][j] = (dp[i - 1][j] + dp[i - 1][j + 1]) % mod;
        }
    }
    cout << (dp[t][0] % mod);
    return 0;
}



5.





```



### 进阶例题

```c++
1.ABC344 F
dp嵌套dp
https://www.cnblogs.com/onlyblues/p/18064351
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define bll __int128_t
#define all(v) v.begin(), v.end()
#define endl '\n'
#define gcd(a, b) __gcd(a, b)
#define mk(a, b) make_pair((a), (b))
#define lowbit(x) ((x) & -(x))
const int N = 2e5 + 10, inf = 1e9;
const ll linf = 1e18;
const bll mod = (bll)1e31 + 7177;

void Solve()
{
    int n;
    cin >> n;

    vector<vector<ll>> P(n + 5, vector<ll>(n + 5)), R(n + 5, vector<ll>(n + 5)), D(n + 5, vector<ll>(n + 5));
    for(int i = 1; i <= n; ++i)
        for(int j = 1; j <= n; ++j)
            cin >> P[i][j];
    for(int i = 1; i <= n; ++i)
        for(int j = 1; j < n; ++j)
            cin >> R[i][j];
    for(int i = 1; i < n; ++i)
        for(int j = 1; j <= n; ++j)
            cin >> D[i][j];

    vector<vector<ll>> f(n + 5, vector<ll>(n + 5, linf)), g(n + 5, vector<ll>(n + 5, 0));
    f[1][1] = 0;
    for(int i = 1; i <= n; ++i)
    {
        for(int j = 1; j <= n; ++j)
        {
            // 选择中间点 (u, v)，定义为最后一次进行操作 1 的点，使更新当前dp所需要的后层属性固定可求
            vector<vector<ll>> h(i + 5, vector<ll>(j + 5, linf));
            h[i][j] = 0;
            for(int u = i; u >= 0; --u)
                for(int v = j; v >= 0; --v)
                    if(!(u == i && v == j))
                        h[u][v] = min(h[u + 1][v] + D[u][v], h[u][v + 1] + R[u][v]);

            for(int u = 1; u <= i; ++u)
            {
                for(int v = 1; v <= j; ++v)
                {
                    ll c = (max(0LL, h[u][v] - g[u][v]) + P[u][v] - 1) / P[u][v];
                    ll t = f[u][v] + c + i - u + j - v;
                    ll m = g[u][v] + c * P[u][v] - h[u][v];
                    if(t < f[i][j])
                    {
                        f[i][j] = t;
                        g[i][j] = m;
                    }
                    else if(t == f[i][j])
                    {
                        g[i][j] = max(g[i][j], m);
                    }
                }
            }
        }
    }

    cout << f[n][n] << endl;
}

int main()
{
    untie();
    int T = 1;
    // cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```



---

## 背包dp

> 给定一组物品，每种物品都有自己的**重量**和**价值**，在限定的**总重量**内，我们如何选择，才能使得物品的**总价值最高**。

一般情况下，dp状态定义为 `dp[i][j]`表示:遍历到第 `i` 个物品，且体积不超过 `j` 时可以获得的最大价值。

初始化问题：

* 没有要求必须把背包装满：`dp[0 ... V]` 均初始化为 0，因为此时对于任意容量的背包都有一个合法解“什么都不装”，且该解的价值为 0。
* 背包必须要装满：`dp[0] = 0`，而 `dp[1 ... V] = -∞`。因为此时只有容量为 0 的背包在“什么都不装”的条件下相当于被“恰好装满”，其他容量的背包仍为未定义状态，暂时不具有合法解。

一般方法：

```c++
for 物品
    for 体积
        for 决策
```

---

### 01背包问题

> 01背包，顾名思义每件物品只有 0 和 1 即不选和选一次的状态，即**每件物品最多只能用一次**。

`dp[i][j]`：遍历到第 `i` 个物品，且体积不超过 `j` 时可以获得的最大价值。

<img src="D:\MarkdownText\image\动态规划\背包1.png" alt="背包1" style="zoom:67%;" />

dp转移方程：`f[i][j] = max(f[i - 1][j - v[i]] + w[i],f[i - 1][j])` -- 选当前物品 和 不选当前物品

优化：由于方程中第一维只涉及 `i - 1`，故可以将数组该层滚动掉，直接定义 `dp[j]`。

注意：由于方程中第二维涉及上一层 `i - 1` 的 `j - v[i]`，则第二层循环需要**逆向遍历**，防止更新本层时在**利用前**覆盖掉上层的状态，因为现在都被压到同一层，低位的代表上一层状态，我们从高位开始更新该层状态就能将上一层状态转移到该层。

```c++
1.Bone Collector
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int M = 1100;
int T, n, v, N[M], V[M], dp[M];
int main()
{
    untie();
    cin >> T;
    while(T--)
    {
        memset(dp, 0, sizeof dp);
        cin >> n >> v;
        for(int i = 1; i <= n; i++) cin >> N[i];
        for(int i = 1; i <= n; i++) cin >> V[i];
        for(int i = 1; i <= n; i++)
            for(int j = v; j >= V[i]; j--)
                dp[j] = max(dp[j], dp[j - V[i]] + N[i]);
        cout << dp[v] << '\n';
    }
    return 0;
}
```



---

### 完全背包问题

> 不同于 01 背包的是，**每件物品可以有无限次选择**。

形式上，我们只需要将 01 背包问题的「一维空间优化」解法中的「容量维度」遍历方向从「从大到小 改为 **从小到大**」就可以解决完全背包问题。但本质是因为两者进行状态转移时依赖了不同的格子：

* 01背包 依赖的是**「正上方上一行的格子」**和**「上一行左边的格子」**。
* 完全背包 依赖的是**「正上方上一行的格子」**和**「本行左边的格子」**。

dp状态定义不变，转移方程变为：`f[i, j] = max(f[i - 1][j], f[i][j - v] + w)`

不同之处在于决定选本件物品时`f[i][j - v] + w`是在利用本层状态

```c++
1.疯狂的采药
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 1e4 + 5;
typedef long long ll;
int t, m, T[N], val[N];
int main()
{
    untie();
    cin >> t >> m;
    vector<ll> dp(t + 1, 0);
    for(int i = 1; i <= m; i++) cin >> T[i] >> val[i];
    for(int i = 1; i <= m; i++)
        for(int j = T[i]; j <= t; j++)//正序遍历：刚装入背包的马上变成旧状态被利用，即前面的件数可以叠加到新状态，小背包可以装入大背包里
            dp[j] = max(dp[j], dp[j - T[i]] + val[i]);
    cout << dp[t];
    return 0;
}
```



---

### 多重背包问题

> 一样物品可以选多件，且不同物品有**不同的选择上限**。

转移方程：`f[i][j] = max(f[i - 1][j], f[i - 1][j - v] + w, f[i - 1][j - 2 * v] + 2 * w ,..., f[i - 1][j - s_i * v] + s_i * w`

以 [洛谷 P1776 宝物筛选](https://vjudge.csgrandeur.cn/problem/洛谷-P1776/origin) 为例：

法一：转化为 01背包 -- 枚举 k 件物品，把问题看成仅有一件的占用空间为 `k * Vi` ，价值为 `k * Wi` 的物品该不该拿

```c++
1.无优化(单例最高耗时922ms)
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
int n, W;
int main()
{
    untie();
    cin >> n >> W;
    vector<int> dp(W + 5, 0);
    for(int i = 1; i <= n; i++)
    {
        int v, w, sum;
        cin >> v >> w >> sum;
        for(int j = W; j >= 0; j--)
            for(int k = 1; k <= sum && k * w <= j; k++)
                dp[j] = max(dp[j], dp[j - k * w] + k * v);
    }
    int res = 0;
    for(int i = 1; i <= W; i++) res = max(res, dp[i]);
    cout << res;
    return 0;
}



2.二进制优化(最好用) -- 一种有一定数量 sum 的物品，拆分成 x 个基元，由这 x 个基元组成 1 ~ sum 所有的取件数情况 (单例最高耗时141ms) -- 优化效果显著
先按 2 的倍数升序拆分为 x - 1 个数，最后剩下一个余数，共 x 个基数，如 sum = 25，分成 1, 2, 4, 8, 10(10 < 2^4 = 16) 五个基元，并且由 5 个数任意自由组合得到 1 ~ 25 的所有数字。
多重背包问题 就转化成对 每种物品 的 每个基元数 的 选与不选，即 01背包问题。
#include <iostream>
#include <algorithm>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int MAXN = 1e6 + 5;//数组一定要够大
int n, m, dp[MAXN];
struct nd{
    int v, w;
}goods[MAXN];//存储一种物品分成的若干个01背包，规定在[1, cnt]
int main()
{
    untie();
    cin >> n >> m;
    for(int i = 1; i <= n; i++)//遍历每种物品
    {
        int v, w, sum, cnt = 0;
        cin >> v >> w >> sum;
        //二进制倍数枚举拆分
        for(int j = 1; j <= sum; j <<= 1)
        {
            goods[++cnt] = nd{v * j, w * j};
            sum -= j;//减去已拆分
        }
        //最后一个余数 处理
        if(sum) goods[++cnt] = nd{v * sum, w * sum};
        //01背包 -- 每种物品分成若干个 01背包
        for(int k = 1; k <= cnt; k++)
            for(int j = m; j >= goods[k].w; j--)//逆序
                dp[j] = max(dp[j], dp[j - goods[k].w] + goods[k].v);
    }
    cout << dp[m];
    return 0;
}



3.单调队列优化(最优耗时)

```

二法：转化为 01背包 + 完全背包 (3.8s，微小优化，仅供加深两种背包的理解)

```c++
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
int n, W;
int main()
{
    untie();
    cin >> n >> W;
    vector<int> dp(W + 5, 0);
    for(int i = 1; i <= n; i++)
    {
        int v, w, sum;
        cin >> v >> w >> sum;
        if(sum * w >= W)//完全背包,对于总体积为 W 的背包来说，此时 v 物体相当于可以无限选取，即不用担心超额
        {
            for(int j = w; j <= W; j++)
                dp[j] = max(dp[j], dp[j - w] + v);
        }
        else
        {
            for(int j = W; j >= w; j--)//01背包，必须逆序遍历
                for(int k = 1; k <= sum && k * w <= j; k++)//该层遍历顺序不影响结果
                    dp[j] = max(dp[j], dp[j - k * w] + k * v);
        }
    }
    cout << dp[W];
    return 0;
}
```

```c++
1.投资的最大效益
经典技巧降低时空复杂度：由于 a 是 1000 的倍数，那就全程用 a /= 1000 计算
#include <iostream>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
typedef long long ll;
const int N = 2e5 + 5;
int n, d, val[50], cost[50];
ll s, sum, dp[N];
int main()
{
    untie();
    cin >> s >> n >> d;
    sum = s;
    for(int i = 1; i <= d; i++) cin >> cost[i] >> val[i], cost[i] /= 1000;
    for(int i = 1; i <= n; i++)
    {
        ll tp = 0;
        memset(dp, 0, sizeof dp);
        for(int k = 1; k <= d; k++)
        {
            for(int j = 1; j <= sum / 1000; j++)
            {
                if(j >= cost[k]) dp[j] = max(dp[j], dp[j - cost[k]] + val[k]);
                tp = max(tp, dp[j]);
            }
        }
        sum += tp;
    }
    cout << sum;
    return 0;
}
```



---

### 混合背包问题



```c++
1.樱花
老实分算三个背包，并且二进制优化多重背包 (55ms)
#include <iostream>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
int n, T;
struct nd{
    int t, c;
}arr[10005];
string st, se;
int transform(string t)
{
    int h = 0, m = 0, pos = t.find(':');
    for(int i = 0; i < pos; i++) h = h * 10 + (t[i] - '0');
    for(int i = pos + 1; i < t.size(); i++) m = m * 10 + (t[i] - '0');
    return h * 60 + m;
}
int main()
{
    untie();
    cin >> st >> se >> n;
    T = transform(se) - transform(st);
    vector<int> dp(T + 1, 0);
    for(int i = 0; i < n; i++)
    {
        int t, c, p;
        cin >> t >> c >> p;
        if(p == 0 || t * p >= T)//完全背包
        {
            for(int j = t; j <= T; j++)
                dp[j] = max(dp[j], dp[j - t] + c);
        }
        else if(p == 1)//01背包
        {
            for(int j = T; j >= t; j--)
                dp[j] = max(dp[j], dp[j - t] + c);
        }
        else//多重背包
        {
            int cnt = 0;
            for(int j = 1; j <= p; j <<= 1)
            {
                arr[++cnt] = nd{t * j, c * j};
                p -= j;
            }
            if(p) arr[++cnt] = nd{t * p, c * p};
            for(int k = 1; k <= cnt; k++)//转为01背包
                for(int j = T; j >= arr[k].t; j--)
                    dp[j] = max(dp[j], dp[j - arr[k].t] + arr[k].c);
        }
    }
    cout << dp[T];
    return 0;
}



全部转化为01背包，用二进制优化(77ms)
#include <iostream>
#include <algorithm>
#include <vector>
#include <cstring>
#include <cstdio>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
int n, T, h1, h2, m1, m2;
struct nd{
    int t, c;
}arr[10005];
int main()
{
    scanf("%d:%d %d:%d %d", &h1, &m1, &h2, &m2, &n);
    T = (h2 - h1) * 60 + m2 - m1;
    vector<int> dp(T + 1, 0);
    for(int i = 0; i < n; i++)
    {
        int t, c, p, cnt = 0;
        scanf("%d%d%d", &t, &c, &p);
        if(!p) p = T / t;//完全背包的无限次转为有效次数即可
        for(int j = 1; j <= p; j <<= 1)
        {
            arr[++cnt] = nd{t * j, c * j};
            p -= j;
        }
        if(p) arr[++cnt] = nd{t * p, c * p};
        for(int k = 1; k <= cnt; k++)
            for(int j = T; j >= arr[k].t; j--)
                dp[j] = max(dp[j], dp[j - arr[k].t] + arr[k].c);
    }
    printf("%d\n", dp[T]);
    return 0;
}



亦可单调队列优化

```







---

### 二维费用的背包问题

> 指对于每件物品，具有**两种不同的代价**，选择这件物品必须**同时付出**这两种代价。对于每种代价都有**对应**一个可承受的最大值 （相当于背包容量），问怎样选择物品可以得到最大的价值。由于每个物品**只能选一次**，所以这实际也是个 **二维01背包问题**。

有时，"二维费用" 的条件是以这样一种隐含的方式给出的：**最多只能取 U 件物品**。这事实上相等于每件物品多了一种 "件数" 的费用，每个物品的件数费用均为 1，可以付出的最大件数费用为 U。

```c++
1.NASA的食物计划
定义 dp[i][j][k] 为遍历到第 i 个物品时体积不超过 j，质量不超过 k 的最大卡路里值
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
int H, T, n, dp[500][500];
int main()
{
    untie();
    cin >> H >> T >> n;
    for(int i = 0; i < n; i++)
    {
        int h, t, K;
        cin >> h >> t >> K;
        for(int j = H; j >= h; j--)
            for(int k = T; k >= t; k--)
                dp[j][k] = max(dp[j][k], dp[j - h][k - t] + K);
    }
    cout << dp[H][T];
    return 0;
}
```



---

### 分组背包问题

> 有 n 件物品和一个容量为 V 的背包。第 i 件物品的费用是 Ci ，价值是 Wi 。这些物品被**划分为 k 组**，每组中的物品互相冲突，**最多选一件**。本质上，对于**每个物品组**就是 **01背包**问题，又可以称为 多组01背包问题。

一般是遍历组数，然后在每个物品组中求解01背包即可。

```c++
1.通天之分组背包
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 1e3 + 5;
struct nd{
    int w, v;
};
int n, m, ind = 0, dp[N];
int main()
{
    untie();
    cin >> m >> n;
    vector<vector<nd> > goods(n + 1); 
    while(n--)
    {
        int w, v, grp;
        cin >> w >> v >> grp;
        goods[grp].push_back(nd{w, v});
        ind = max(ind, grp);
    }
    for(int i = 1; i <= ind; i++)//组数
        for(int j = m; j >= 0; j--)//01背包问题，逆序遍历体积（代价）
            for(auto x : goods[i])//方案数（物品数）
                if(j >= x.w)
                    dp[j] = max(dp[j], dp[j - x.w] + x.v);
    cout << dp[m];
    return 0;
}



2.ACboy needs your help
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
int n, m, a[105][105];
int main()
{
    untie();
    while(cin >> n >> m, n || m)
    {
        vector<int> dp(m + 1, 0);
        for(int i = 1; i <= n; i++)
            for(int j = 1; j <= m; j++)
                cin >> a[i][j];
        for(int i = 1; i <= n; i++)
            for(int j = m; j >= 0; j--)
                for(int k = 1; k <= m; k++)
                    if(j >= k)
                        dp[j] = max(dp[j], dp[j - k] + a[i][k]);
        cout << dp[m] << '\n';
    }
    return 0;
}
```

---

### 有依赖的背包问题

> **一个主件**及其**若干附件**将称为分组背包中的一个物品组，依赖性表现在附件选择的前提是**先选主件**。

我们可以将**每个附件**看成一个01背包问题，这样当我们给一个主件分配多少价钱的时候，就可以知道此时这个主件及其附件在对应的价钱可以获得的最大价值。

更一般的问题是:

依赖关系以图论中的 "森林" 形式给出，也就是说主件的**附件**依旧可以有自己的**附件集合**（如图）。

该问题的最佳解法是 **树形背包DP**

<img src="D:\MarkdownText\image\动态规划\背包3.png" alt="背包3" style="zoom:80%;" />

```c++
1.先处理附件价值状态，再往主件中加入附件 (230 ms)
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
struct nd{
    int v, p, pos;
};
int n, m;
int main()
{
    untie();
    cin >> n >> m;
    n /= 10;//降低复杂度
    vector<nd> goods[m + 1];//goods[0]为主件组，1 ~ m 为附件组
    vector<vector<int> > dp(m + 1, vector<int>(n + 1, 0));
    for(int i = 1; i <= m; i++)
    {
        int v, p, q;
        cin >> v >> p >> q;
        goods[q].push_back(nd{v / 10, p, i}); 
    }
    //dp[i][j]表示给第 i 个背包分配 j 钱数的最大价值
    for(int i = 1; i <= m; i++)//*遍历背包
        for(auto x : goods[i])//遍历物品(附件)
            for(int j = n; j >= x.v; j--)//遍历代价
                dp[i][j] = max(dp[i][j], dp[i][j - x.v] + x.v * x.p * 10);
    //遍历主件，给附件分配空间，据最大价值判断是否并入主件
    for(int i = 0; i < goods[0].size(); i++)
    {
        nd base = goods[0][i];
        for(int j = n; j >= base.v; j--)//遍历主件的代价
            for(int k = j - base.v; k >= 0; k--)//遍历加入附件的代价
                dp[0][j] = max(dp[0][j], dp[0][j - base.v - k] + base.p * base.v * 10 + dp[base.pos][k]);
    }
    cout << dp[0][n];
    return 0;
}



2.实际上也可以当成有 5 种操作的 01背包来写 (38 ms)
| 1.不变 | 2.只拿主件 | 3.主件 + 附件1 | 4.主件 + 附件2 | 5.主件 + 附件1 + 附件2 |
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
int n, m, v[65][3], p[65][3];//v[i][j] 第 i 个物品，若 j = 0，为主件；而 j = 1, 2 为附件 1，2
//物品价值定义为 v * p，直接将原值存入p数组中
int main()
{
    untie();
    cin >> n >> m;
    n /= 10;//降低复杂度
    vector<int> dp(n + 1, 0);
    for(int i = 1; i <= m; i++)
    {
        int a, b, grp;
        cin >> a >> b >> grp;
        if(grp) v[grp][2 - !v[grp][1]] = a / 10, p[grp][2 - !p[grp][1]] = a * b;//附件1位置未占用就存入位置1，已占用就会存入位置2
        else v[i][0] = a / 10, p[i][0] = a * b;
    }
    for(int i = 1; i <= m; i++)
        for(int j = n; j >= v[i][0]; j--)
        {
            dp[j] = max(dp[j], dp[j - v[i][0]] + p[i][0]);//情况1, 2
            if(j >= v[i][0] + v[i][1]) dp[j] = max(dp[j], dp[j - v[i][0] - v[i][1]] + p[i][0] + p[i][1]);//3
            if(j >= v[i][0] + v[i][2]) dp[j] = max(dp[j], dp[j - v[i][0] - v[i][2]] + p[i][0] + p[i][2]);//4
            if(j >= v[i][0] + v[i][1] + v[i][2]) dp[j] = max(dp[j], dp[j - v[i][0] - v[i][1] - v[i][2]] + p[i][0] + p[i][1] + p[i][2]);//5
        }
    cout << dp[n];
    return 0;
}


3.当作一种深度仅为 2 的特殊树形背包来解（最佳）
#include <stdio.h>
#include <algorithm>
#include <vector>
using namespace std;
const int N = 4e3 + 5, M = 65;
vector<int> ve[M];
int wei[M], imp[M];
int n, m;
int f[M][N];
void dfs(int u)
{
    //枚举物品
    for (int i = 0; i < ve[u].size(); i++)
    {
        int to = ve[u][i];
        dfs(to);
        for (int j = m; j >= 0; j--)
        {
            //枚举分给当前物品的体积(不包括当前物品的体积)
            for (int k = j; k >= 0; k--)
            {
                if (k - wei[to] >= 0)
                    f[u][j] = max(f[u][j], f[u][j - k] + f[to][k - wei[to]] + imp[to] * wei[to] * 10);
                else
                    break;
            }
        }
    }
}

int main()
{
    scanf("%d%d", &m, &n);
    m /= 10;
    for (int i = 1; i <= n; i++)
    {
        int c;
        scanf("%d%d%d", &wei[i], &imp[i], &c);
        wei[i] /= 10;
        ve[c].push_back(i);
    }
    dfs(0);
    int res = 0;
    res = f[0][m];
    printf("%d\n", res);
    return 0;
}

```



---

### 求第k优解

在原来 01背包 的基础上在加一维记录即可，即 `dp[i][j][k]` - 遍历到第 i 个物品，且体积不超过 j 时可得的第 k 大价值

当然可以滚动掉第一维，剩余 `dp[j][k]`。

* a 数组记录 不选第 i 个物品 的 k 个最大价值
* b 数组记录 选第 i 个物品 的 k 个最大价值

```c++
1.Bone Collector II
#include <iostream>
#include <algorithm>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int MAXN = 1e2 + 5;
int T, val[MAXN], vol[MAXN];
int main()
{
    untie();
    cin >> T;
    while(T--)
    {
        int n, v, k;
        cin >> n >> v >> k;
        vector<vector<int> > dp(v + 1, vector<int>(k + 1, 0));
        for(int i = 1; i <= n; i++) cin >> val[i];
        for(int i = 1; i <= n; i++) cin >> vol[i];
        for(int i = 1; i <= n; i++)
        {
            for(int j = v; j >= vol[i]; j--)
            {
                int ai = 1, bi = 1;
                vector<int> a(k + 2, 0), b(a);
                a[k + 1] = b[k + 1] = -1;
                //填充a, b
                for(int m = 1; m <= k; m++)
                {
                    a[m] = dp[j][m];
                    b[m] = dp[j - vol[i]][m] + val[i];
                }
                //合并a, b 取较大者存入 dp
                for(int m = 1; m <= k && (a[ai] != -1 || b[bi] != -1); )//相当于从 2*k 个数排序后取前 k 个数
                {
                    dp[j][m] = a[ai] > b[bi] ? a[ai++] : b[bi++];
                    if(dp[j][m] != dp[j][m - 1])//题意：两种不同方法得到相同价值是算作同一个种，即去除重复。若无该条件，则去掉该句即可
                        m++;//不重复说明该解占用有效
                }
            }
        }
        cout << dp[v][k] << "\n";
    }
    return 0;
}

```



---

### 背包问题求具体方案

一般而言，背包问题是要求一个最优值，如果要求输出这个最优值的方案，可以参照一般动态规划问题输出方案的方法: 记录下每个状态的最优质是由状态转移方程的哪一项推出来的。然后就可以根据这个状态继续往前推导。

```c++
1.Acwing 12	背包问题求具体方案
01背包问题，但是要求输出 字典序最小的方案数 。



```



---





---

## 区间dp

> 在**区间**上进行动态规划，求解一段区间上的最优解。主要是通过**合并小区间的最优解**进而得出整个**大区间上最优解**的dp算法

### 核心思路

把区间**分割成一个个小区间，求解每个小区间的最优解，再合并小区间得到大区间最优解**即可

1. 枚举区间长度`len`为每次分割成的小区间长度（由短到长不断合并），

2. 内层枚举该长度下的区间起点 `l`，并由 `len` 得到区间终点 `r`，

3. 然后在`[l, r]`之间枚举分割点 `k`，求解这段小区间在某个分割点下的最优解（即枚举出合成区间`[l, r]`的最优合并方式）

4. ***写出特定的状态转移方程***

5. 基础代码实现 `O(n^3)`：

   ```c++
   for(int len = 2; len <= n; ++len)//长度应从 2 开始
   {
       for(int l = 1; l + len - 1 <= n; ++l)//条件为右端点 r = l + len - 1  <=  n
       {
           int r = l + len - 1;
           //计算前初始化
           //dp[l][r] = inf 或 0
           for(int k = l; k < r; ++k)//枚举合并点
           {
               dp[l][r] = min(dp[l][r], dp[l][k] + dp[k + 1][r] + cost);//cost 为合并两连续子区间的代价
           }
       }
   }
   ```

### **四边形不等式优化** 

[四边形不等式优化讲解（详解）- CSDN博客](https://blog.csdn.net/NOIAu/article/details/72514812?ops_request_misc=%7B%22request%5Fid%22%3A%22168140090716800182768616%22%2C%22scm%22%3A%2220140713.130102334..%22%7D&request_id=168140090716800182768616&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-72514812-null-null.142^v83^pc_search_v2,239^v2^insert_chatgpt&utm_term=四边形不等式优化&spm=1018.2226.3001.4187)

思路：在查找最优分割点 `k` 的时候，浪费了大量时间。现把**最优分割点**存起，在查找的时候利用保存的最优分割点来优化查找过程。

基本概念：

* 记两个交叉区间 `A = [i, j]` 和 `B = [i', j']`，有 `A ∩ B = [i', j]`，`A ∪ B = [i, j']`;

* 区间包含的单调性：如果对于**`i ≤ i' < j ≤ j'`**，有 **`cost(i', j) ≤ cost(i, j')`**，那么说明 cost函数 具有区间包含的单调性。也有 `(A ∩ B) ≤ (A ∪ B)`（可以理解为**如果小区间 `[i', j]` 包含于大区间 `[i, j']` 中，那么小区间的cost函数值不超过大区间的cost函数值** ）。
* 四边形不等式：如果对于**`i ≤ i' < j ≤ j'`**，有 **`cost(i, j) + cost(i', j') ≤ cost(i', j) + cost(i, j')`**，我们称函数 cost 满足四边形不等式。也有 `A + B ≤ (A ∩ B) + (A ∪ B)`（可以理解为两个交叉区间的cost值和不超过小区间与大区间的cost值和——**交叉小于包含**）。

两个定理：

* 如果上述的 cost函数 同时满足**区间包含单调性**和**四边形不等式性质**，那么 函数dp 也满足四边形不等式性质，我们再定义`opt(i, j)`表示`dp(i, j)` 取得最优值时对应的下标 k（即 `i ≤ k ≤ j` 时，k 处的 dp 值为最优，则 `opt(i, j) = k`）。

* 假如 `dp(i, j)` 满足四边形不等式，那么 `opt(i, j)`(自变量为 i、j 两个变量) 单调，

  即`opt(i, j) ≤ opt(i, j + 1) ≤ opt(i + 1, j + 1)`，也即 `opt(i, j - 1) ≤ opt(i, j) ≤ opt(i + 1, j)`。

总结：

* dp转移方程：`dp(i, j) = min{dp(i, k - 1), dp(k, j)} + cost(i, j)，其中 opt(i, j - 1) ≤ k ≤ opt(i + 1, j)`
* 区间 `[l, r]`的最优分割点 k 在两个子区间 `[l, r - 1]` 和 `[l + 1, r]` 的最优分割点 k1，k2 间，即 `k ∈ [k1, k2]`。
* 算法复杂度优化至逼近 `O(n ^ 2)`。
* **注意四边形不等式的左界 `opt[][]` 是与 `dp[][]` 的下标一一对应的**

代码实现：

```c++
int opt[N][N];
for(int i = 1; i <= n; ++i) opt[i][i] = i;//初始化
for(int len = 2; len <= n; ++len)//从 len = 1 开始会出错，因为 opt[l][r - 1] 会存在 opt[1][0]，故出错
{
    for(int l = 1; l + len - 1 <= n; ++l)
    {
        int r = l + len - 1, cost = s[r] - s[l - 1];
        for(int k = opt[l][r - 1]; k <= opt[l + 1][r]; ++k)//四边形不等式优化
        {
            if(dp[l][r] > dp[l][k] + dp[k + 1][r] + cost)
            {
                dp[l][r] = dp[l][k] + dp[k + 1][r] + cost;
                opt[l][r] = k;//最优分割点更新
            }
        }
    }
}
```



### 二维区间dp

不同于一维区间dp（`dp[l][r]`）同时只管理一个类型的区间，而二维区间dp（`dp[l1][r1][l2][r2]`）可同时管理两种类型的区间，此时两种区间在某一状态下是相互影响的，产生的权值结果也是共同作用下的结果。







### **例题**

```c++
1.P1775 石子合并（弱化版）（模板）
//dp[l][r] 为合并区间[l, r]的最小代价（完成了 r - l 次合并）
//每次合并成区间[l, r]经历 len - 1 次合并，而最后一次合并花费 s[r] - sum[l - 1] 的代价
//基础做法
#include <bits/stdc++.h>
using namespace std;
const int N = 1e3 + 10;
int s[N], dp[N][N];
int main()
{
    int n;
    cin >> n;
    memset(dp, 0x7f, sizeof(dp));
    for(int i = 1; i <= n; ++i)
    {
        cin >> s[i];
        s[i] += s[i - 1];//前缀和方便取区间代价
        dp[i][i] = 0;//初始化，自合并的代价为 0
    }
    for(int len = 2; len <= n; ++len)   
    {
        for(int l = 1; l + len - 1 <= n; ++l)
        {
            int r = l + len - 1;
            int cost = s[r] - s[l - 1];//合并任意两个连续子区间 [l, k] 和 [k + 1, r] 所需的代价
            for(int k = l; k < r; ++k)
            {
                dp[l][r] = min(dp[l][r], dp[l][k] + dp[k + 1][r] + cost);
            }
        }
    }
    cout << dp[1][n];
    return 0;
}
//四边形不等式优化 O(n ^ 2)
#include <bits/stdc++.h>
using namespace std;
const int N = 1e3 + 10;
int s[N], dp[N][N], opt[N][N];
int main()
{
    int n;
    cin >> n;
    memset(dp, 0x7f, sizeof(dp));
    for(int i = 1; i <= n; ++i)
    {
        cin >> s[i];
        s[i] += s[i - 1];
        dp[i][i] = 0;
        opt[i][i] = i;//最优分割点初始化
    }
    for(int len = 2; len <= n; ++len)//从 len = 1 开始会出错，因为 opt[l][r - 1] 会存在 opt[1][0]，故出错
    {
        for(int l = 1; l + len - 1 <= n; ++l)
        {
            int r = l + len - 1, cost = s[r] - s[l - 1];
            for(int k = opt[l][r - 1]; k <= opt[l + 1][r]; ++k)//四边形不等式优化
            {
                if(dp[l][r] > dp[l][k] + dp[k + 1][r] + cost)
                {
                    dp[l][r] = dp[l][k] + dp[k + 1][r] + cost;
                    opt[l][r] = k;//最优分割点更新
                }
            }
        }
    }
    cout << dp[1][n];
    return 0;
}



2.P1880 [NOI1995] 石子合并（环上问题）
//不同于弱化版的链状问题，该问题基于一个环路，故需要转换为链上问题
//将长度为 n 的环路展开成链状，并延伸至 2 * n - 1 长度即可，在这上面取得的所有区间即所有可能的环区间
#include <bits/stdc++.h>
using namespace std;
const int N = 500;
int n, a[N], s[N], dp1[N][N], dp2[N][N];
int main()
{
    memset(dp1, 0x7f, sizeof(dp1));
    memset(dp2, -1, sizeof(dp2));
    cin >> n;
    int nn = (n << 1) - 1;
    for(int i = 1; i <= n; ++i) cin >> a[i];
    for(int i = n + 1; i <= nn; ++i) a[i] = a[i - n];
    for(int i = 1; i <= nn; ++i)
    {
        s[i] = s[i - 1] + a[i];
        dp1[i][i] = dp2[i][i] = 0;
    }
    for(int len = 2; len <= nn; ++len)
    {
        for(int l = 1; l + len - 1 <= nn; ++l)
        {
            int r = l + len - 1;
            int cost = s[r] - s[l - 1];
            for(int k = l; k < r; ++k)
            {
                dp1[l][r] = min(dp1[l][r], dp1[l][k] + dp1[k + 1][r] + cost);
                dp2[l][r] = max(dp2[l][r], dp2[l][k] + dp2[k + 1][r] + cost);
            }
        }
    }
    int xmin = 2e9, xmax = 0;
    for(int st = 1; st + n - 1 <= nn; ++st)
    {
        int ed = st + n - 1;
        xmin = min(xmin, dp1[st][ed]);
        xmax = max(xmax, dp2[st][ed]);
    }
    cout << xmin << "\n" << xmax;
    return 0;
}



3.Brackets
//定义 dp[l][r] 为区间[l, r]内最长匹配长度
//如果 s[l]为'('或'['，且对应 s[r]为')'或']'，则 dp[l][r] = dp[l + 1][r - 1] + 2 （内层区间加上匹配的最外一层） 
//如果 s[l] != s[r]，则至少可以取两连续子区间的合并
#include <iostream>
#include <algorithm>
#include <string>
#include <cstring>
using namespace std;
const int N = 200;
string s;
int dp[N][N];
int main()
{
    while(cin >> s)
    {
        if(s[0] == 'e') break;
        memset(dp, 0, sizeof(dp));
        int n = s.size();
        s = '*' + s;
        for(int len = 2; len <= n; ++len)
        {
            for(int l = 1; l + len - 1 <= n; ++l)
            {
                int r = l + len - 1;
                if((s[l] == '(' && s[r] == ')') || (s[l] == '[' && s[r] == ']')) //若可匹配则这样更新，但此时不一定是最优解
                    dp[l][r] = dp[l + 1][r - 1] + 2;
                for(int k = l; k < r; ++k)//不论可不可匹配都取最优分割（因为不一定直接匹配就比分割合并来的大），如 ()[]()，显然分割合并为6，比匹配左右界的4大
                {
                    dp[l][r] = max(dp[l][r], dp[l][k] + dp[k + 1][r]);
                }
            }
        }
        cout << dp[1][n] << '\n';
    }
    return 0;
}



4.Multiplication Puzzle
//每次删除一个卡片，现删除中间所有卡片，最后会留下左右两端的两个卡片，求最小代价
//记 dp[l][r] 表示区间[l, r]经过最佳删除方案后剩下卡片l、r的最小删除代价
//删除区间的长度至少为3，即中间元素至少有1个，遍历 k 分割两个区间时，显然 dp[l][k] 和 dp[k][r] 恰好是分开的连续子区间（因为两端点不算）
//dp[l][k] + dp[k][r] 就代表删除 [l + 1, k - 1] 和 [k + 1, r - 1] 元素后剩下卡片 l, k, r 的合并，即此时删除卡片 k 为一次合并
//重点在于初始状态的初始化
//初始化法1：
#include <iostream>
#include <algorithm>
#include <string>
#include <cstring>
using namespace std;
const int N = 200, inf = 0x7fffffff;
int n, c[N], dp[N][N];
int main()
{
    memset(dp, 0x7f, sizeof(dp));//初始化为无穷大
    cin >> n;
    for(int i = 1; i <= n; ++i) cin >> c[i];
    for(int l = 1; l + 1 <= n; ++l) dp[l][l + 1] = 0;//初始化 len = 2 时代价为 0，这样 len = 3 时能正确地取到初始合法值 dp[l][r] = c[l] * c[k] * c[r]
    for(int len = 3; len <= n; ++len)
    {
        for(int l = 1; l + len - 1 <= n; ++l)
        {
            int r = l + len - 1;
            for(int k = l + 1; k < r; ++k)
            {
                dp[l][r] = min(dp[l][r], dp[l][k] + dp[k][r] + c[l] * c[k] * c[r]);
            }
        }
    }
    cout << dp[1][n] << '\n';
    return 0;
}
//初始化法2：
#include <iostream>
#include <algorithm>
#include <string>
#include <cstring>
using namespace std;
const int N = 200, inf = 0x7fffffff;
int n, c[N], dp[N][N];
int main()
{
    // memset(dp, 0x7f, sizeof(dp)); //这里直接这样初始化是错的
    cin >> n;
    for(int i = 1; i <= n; ++i) cin >> c[i];
    for(int l = 1; l + 2 <= n; ++l) dp[l][l + 2] = c[l] * c[l + 1] * c[l + 2];//初始化 len = 3
    for(int len = 4; len <= n; ++len)//len = 3 已经被计算过了
    {
        for(int l = 1; l + len - 1 <= n; ++l)
        {
            int r = l + len - 1;
            dp[l][r] = inf;//首次计算前才需要重新初始化
            for(int k = l + 1; k < r; ++k)
            {
                dp[l][r] = min(dp[l][r], dp[l][k] + dp[k][r] + c[l] * c[k] * c[r]);
            }
        }
    }
    cout << dp[1][n] << '\n';
    return 0;
}



5.Cheapest Palindrome（最小花费回文串删除）
//题意：有加入一个字符或删除一个字符的操作（任意次），但它们都有对应代价，求由原串组成回文串的最小代价和
//可知对于任意一个子串如 bcc，对同一个字符 b，进行删除或者在对应对称位置插入它（cc 或 bccb），结果都是等效的，应直接取花费代价最小者。
//对于 s[l] == s[r]，则无需花费任何代价，直接继承小区间的状态 dp[l][r] = dp[l + 1][r - 1]。因为第 l 个和第 r 个字符相等就意味着把序列 [l, r] 变成回文串只需要把 l + 1 ~ r - 1 变成回文串即可，所需代价是相同的。
//而当 s[l] != s[r]，这时候 [l, r] 一定不是回文序列，只需看 l 和 r 上字符，又由于当前 len 是从小到大枚举的，此时必有至少一个 len - 1 的子串是回文序列
//现需从两个 len - 1 的子串中取最小代价，即 dp[l][r] = min(dp[l + 1][r] + cost[l], dp[l][r - 1] + cost[r])，如当前 len = 4 的 baca，找到已得到的 len = 3 回文子串中的最小代价方，这里显然是对 b 操作（取最小代价操作）即 dp[l][r] = dp[l + 1][r] + cost[l]。
//注意单个字符无代价即 dp[i][i] = 0，以及对于 len = 2 且 s[l] == s[r] 时也是无代价 —— 实际上 dp 数组初始值都为 0，然后在计算 dp[l][r] 前一刻先赋无穷大 inf 即可。
#include <iostream>
#include <algorithm>
#include <string>
#include <cstring>
#include <map>
using namespace std;
const int N = 2e3 + 10, inf = 0x7fffffff;
int t, m;
string s;
map<char, int> cost;
int dp[N][N];
int main()
{
    cin >> t >> m >> s;
    int n = s.size();
    s = ' ' + s;
    for(int i = 0; i < t; ++i)
    {
        char ch[3];
        int add, del;
        cin >> ch >> add >> del;
        cost[ch[0]] = min(add, del);
    }
    for(int len = 2; len <= n; ++len)
    {
        for(int l = 1; l + len - 1 <= n; ++l)
        {
            int r = l + len - 1;
            dp[l][r] = inf;//其他仍为 0，这样能得到初始长度 len = 2 的初始代价
            if(s[l] == s[r]) dp[l][r] = dp[l + 1][r - 1];//当 len = 2 时有 s[l] = s[r]，则代价初始化为 0（而正好 dp 数组原始值为 0）
            else dp[l][r] = min(dp[l + 1][r] + cost[s[l]], dp[l][r - 1] + cost[s[r]]);//两个 len - 1 的子串中选择最小操作代价，即分别对左右端字符讨论
        }
    }
    cout << dp[1][n] << '\n';
    return 0;
}



6.Zuma（最少删除次数）
// 据题意单个字符要消耗一次，即初始化 dp[i][i] = 1
// 当s[i] == s[i+1]时dp[i][i+1] = 1
// 当s[i] != s[i+1]时dp[i][i+1] = 2
// 当s[i] == s[j] 时 dp[i][j] = dp[i+1][j-1] 这是因为当移除[i+1,j-1]内的所有子串的最后一次时,如果s[i]==s[j]那么也同样可以同时移掉[i][j]这一层
// 剩下的情况我们枚举断点 k 更新即可
#include <bits/stdc++.h>
using namespace std;
const int N = 600, inf = 0x7fffffff;
int n, a[N], dp[N][N];
int main()
{
    cin >> n;
    for(int i = 1; i <= n; ++i) 
    {
        cin >> a[i];
        dp[i][i] = 1;
    }
    for(int len = 2; len <= n; ++len)
    {
        for(int l = 1; l + len - 1 <= n; ++l)
        {
            int r = l + len - 1;
            dp[l][r] = inf;
            if(len == 2) dp[l][r] = (a[l] == a[r] ? 1 : 2);//特判长度为 2 时
            else if(a[l] == a[r]) dp[l][r] = dp[l + 1][r - 1];//len > 2 时，直接继承中间区间的次数，不会增加删除操作次数
            for(int k = l; k < r; ++k)//分割的情况，如 1 2 1 4 4 分成 {1 2 1} 和 {4 4} 处理，即两次
            {
                dp[l][r] = min(dp[l][r], dp[l][k] + dp[k + 1][r]);
            }
        }
    }
    cout << dp[1][n] << '\n';
    return 0;
}



7.Clear the String
//题意：给定长度为 n 的字符串，每次可以删除一段只有一种字符的连续子串，求删完整个字符串所需的最少操作次数。
//特判若 len == 2，dp[l][r] = 1 + (s[l] != s[r])
//当 len > 2，若 s[l] == s[r]，则此时区间 [l, r - 1] 和 [l + 1, r] 是等效的，把其中一个端点字符 s[l] 或 s[r] 纳入中间区间 [l + 1, r- 1] 内，就可以得到加上 外层相同字符对<s[l], s[r]> 之后的删除次数，这样就能考虑到 s[l] 与 s[l + 1] 或者说 s[r] 与 s[r - 1] 的关系
//如 aabcba，对于子状态 aabcb 和 abcba，前者先删去 aa 再加上 bcb 的操作次数，后者先进行 bcb 的删除再删去 aa，它是等效的
#include <bits/stdc++.h>
using namespace std;
const int N = 600, inf = 0x7fffffff;
int n, dp[N][N];
string s;
int main()
{
    cin >> n >> s;
    s = ' ' + s;
    for(int i = 1; i <= n; ++i) dp[i][i] = 1;
    for(int len = 2; len <= n; ++len)
    {
        for(int l = 1; l + len - 1 <= n; ++l)
        {
            int r = l + len - 1;
            dp[l][r] = inf;
            if(len == 2) dp[l][r] = (s[l] == s[r] ? 1 : 2);
            else if(s[l] == s[r]) dp[l][r] = dp[l + 1][r];//或者 dp[l][r - 1] 也行
            for(int k = l; k < r; ++k)
            {
                dp[l][r] = min(dp[l][r], dp[l][k] + dp[k + 1][r]);
            }
        }
    }
    cout << dp[1][n] << '\n';
    return 0;
}
    

    
8.关路灯
//题意：要关 n 个路灯，每秒走一格，每秒每个仍开着的灯会消耗电能（功率 * 时间），走的时候可以掉转方向，现给定初始位置 x，问关完全部路灯所需最小功耗。
//一般地，先有 dp[l][r] 表示关完 [l, r] 上所有灯所需最小功耗。关灯后有两种决策：继续沿原方向关灯 和 掉头去关没关掉的灯。
//由于需要通过计算位置差来算功耗，我们还需要知道关完 [l, r] 的灯后所处的位置，也只有两种情况：在左端点 或 右端点。故添加方向状态维度即 dp[l][r][k]，则 dp[l][r][0] 或 dp[l][r][1] 分别表示关完灯在左端点或右端点所需的最小功耗。并且，存前缀和 sum[] 方便求取区间代价。定义 cost(t, x, y) 函数表示除去区间 [x, y] 上的灯外其他灯在 t 秒内的功耗。
//状态转移：不论是在左端点还是右端点，都可能是从上个状态 掉头 或 沿原方向走 来的：
//dp[l][r][0] = min{dp[l + 1][r][0] + cost(a[l + 1] - a[l], l + 1, r), dp[l + 1][r][1] + cost(a[r] - a[l], l + 1, r)} 不用掉头 与 要掉头
//dp[l][r][1] = min{dp[l][r - 1][1] + cost(a[r] - a[r - 1], l, r - 1), dp[l][r - 1][0] + cost(a[r] - a[l], l, r - 1)} 不用掉头 与 要掉头
//注：功耗计算是包括了那盏即将关掉的灯，不包括上一个状态已经关掉的灯
#include <bits/stdc++.h>
using namespace std;
const int N = 55, inf = 0x7fffffff;
int n, pos;
int a[N], sum[N], dp[N][N][2];
int cost(int t, int x, int y)
{
    return t * (sum[n] - (sum[y] - sum[x - 1]));
}
int main()
{
    memset(dp, 0x7f, sizeof(dp));//因为规定从 pos 开始，其余地方初值都为 inf
    cin >> n >> pos;
    dp[pos][pos][0] = dp[pos][pos][1] = 0;//第一个被关的灯无功耗
    for(int i = 1; i <= n; ++i)
    {
        cin >> a[i] >> sum[i];
        sum[i] += sum[i - 1];
    }
    for(int len = 2; len <= n; ++len)
    {
        for(int l = 1; l + len - 1 <= n; ++l)
        {
            int r = l + len - 1;
            dp[l][r][0] = min(dp[l + 1][r][0] + cost(a[l + 1] - a[l], l + 1, r), dp[l + 1][r][1] + cost(a[r] - a[l], l + 1, r));
            dp[l][r][1] = min(dp[l][r - 1][1] + cost(a[r] - a[r - 1], l, r - 1), dp[l][r - 1][0] + cost(a[r] - a[l], l, r - 1));
        }
    }
    cout << min(dp[1][n][0], dp[1][n][1]) << '\n';
    return 0;
}



*9.P4767 [IOI2000]邮局（决策单调性优化dp - 四边形不等式优化）
//本题优化方面较难，需要重新证明 cost 和 dp 满足的四边形不等式才写得对（而实际上四边形不等式只有两种写法，这里是第二种）
//题意：给定 n 个村庄和 m 个邮局，求 m 个邮局按最佳方案放置后，计算每个村庄和最近的邮局之间所有距离的最小可能的总和。
//定义 dp[i][j] 为共放置 i 个邮局后前 j 个村庄的最小距离总和。可知，邮局放在中间位置（偶数则中间两个位置都可）得到距离和最小，定义 cost() 函数计算。
//状态转移：枚举断点 k，从上一个状态（已经用 i - 1 个邮局规划好 [1, k] 的村庄）再安置一个邮局来规划 [k + 1, j] 的村庄，最后综合起来就是用 i 个邮局来规划 [1, j] 的村庄的最优方案，即dp[i][j] = min(dp[i - 1][k] + w(k + 1, j)) ，k ∈ [0, j)
//若不进行优化则 70pts，故需要用四边形不等式优化 断点k 的遍历（这里 opt 两个维度的变化互换了，或者可以将外层的两层循环顺序互换且内层逆序遍历，因为本题满足的凸性不同于其他题）
//70pts（供理解）
#include <bits/stdc++.h>
using namespace std;
const int N = 3e3 + 10, inf = 0x7fffffff;
int n, m;
int xx[N], sum[N], dp[N][N];
int cost(int x, int y)
{
    int mid = x + y >> 1, xmid = sum[mid] - sum[mid - 1], len1 = mid - x, len2 = y - mid;
    return sum[x - 1] + sum[y] + (len1 - len2) * xmid - sum[mid - 1] - sum[mid];
}
int main()
{
    memset(dp, 0x7f, sizeof(dp));
    cin >> n >> m;
    for(int i = 1; i <= n; ++i) cin >> xx[i];
    sort(xx + 1, xx + 1 + n);//坐标位置可能是乱序的
    for(int i = 1; i <= n; ++i) sum[i] = sum[i - 1] + xx[i];
    dp[0][0] = 0;
    for(int i = 1; i <= m; ++i)//枚举邮局数
    {
        for(int j = 1; j <= n; ++j)//前 j 个村庄
        {
            for(int k = 0; k < j; ++k)//分成已安排好的村庄区间（前 k 个） 和 待加入邮局的村庄区间[k + 1, j]，后者加入最优距离代价cost
            {
                dp[i][j] = min(dp[i][j], dp[i - 1][k] + cost(k + 1, j));
            }
        }
    }
    cout << dp[m][n] << '\n';
    return 0;
}
//优化后 AC
#include <bits/stdc++.h>
using namespace std;
const int N = 3e3 + 10, inf = 0x7fffffff;
int n, m;
int xx[N], sum[N], opt[N][N], dp[N][N];
int cost(int x, int y)
{
    int mid = x + y >> 1, xmid = sum[mid] - sum[mid - 1], len1 = mid - x, len2 = y - mid;
    // return len1 * xmid - (sum[mid - 1] - sum[x - 1]) + (sum[y] - sum[mid]) - len2 * mid;
    return sum[x - 1] + sum[y] + (len1 - len2) * xmid - sum[mid - 1] - sum[mid];
}
int main()
{
    memset(dp, 0x7f, sizeof(dp));
    cin >> n >> m;
    for(int i = 1; i <= n; ++i) cin >> xx[i];
    sort(xx + 1, xx + 1 + n);//坐标位置可能是乱序的
    for(int i = 1; i <= n; ++i) 
    {
        sum[i] = sum[i - 1] + xx[i];
        // opt[i][i] = i; //本题不需要该初始化
    }

    dp[0][0] = 0;
    for(int i = 1; i <= m; ++i)//枚举邮局数
    {
        opt[i][n + 1] = n;//重点初始化，因为开始时 opt[][n + 1] 是没计算过的，直接赋初始值为右端点
        for(int j = n; j > 0; --j)//前 j 个村庄，由于下面用到 opt[][j + 1]，故逆序遍历
        {
            //四边形不等式的左界 opt[i - 1][j] 与 dp[i - 1][k] 的下标一一对应
            for(int k = opt[i - 1][j]; k <= opt[i][j + 1]; ++k)//分成已安排好的村庄区间（前 k 个） 和 待加入邮局的村庄区间[k + 1, j]，后者加入最优距离代价cost
            {
                int now = dp[i - 1][k] + cost(k + 1, j);
                if(now < dp[i][j])
                {
                    dp[i][j] = now;
                    opt[i][j] = k;
                }
            }
        }
    }

    cout << dp[m][n] << '\n';
    return 0;
}
//或者定义 dp[i][j] 为前 i 个村庄放了 j 个邮局，并且将双层循环顺序交换，此处四边形不等式就是熟悉的，
//四边形不等式的左界 opt[i][j - 1] 与 dp[k][j - 1] 的下标一一对应
#include <bits/stdc++.h>
using namespace std;
const int N = 3e3 + 10, inf = 0x7fffffff;
int n, m;
int xx[N], sum[N], opt[N][N], dp[N][N];
int cost(int x, int y)
{
    int mid = x + y >> 1, xmid = sum[mid] - sum[mid - 1], len1 = mid - x, len2 = y - mid;
    return sum[x - 1] + sum[y] + (len1 - len2) * xmid - sum[mid - 1] - sum[mid];
}
int main()
{
    memset(dp, 0x7f, sizeof(dp));
    cin >> n >> m;
    for(int i = 1; i <= n; ++i) cin >> xx[i];
    sort(xx + 1, xx + 1 + n);//坐标位置可能是乱序的
    for(int i = 1; i <= n; ++i) 
    {
        sum[i] = sum[i - 1] + xx[i];
    }

    dp[0][0] = 0;
    for(int j = 1; j <= m; ++j)//枚举邮局
    {
        opt[n + 1][j] = n;
        for(int i = n; i > 0; --i)//枚举村庄
        {
            for(int k = opt[i][j - 1]; k <= opt[i + 1][j]; ++k)//分成已安排好的村庄区间（前 k 个） 和 待加入邮局的村庄区间[k + 1, j]，后者加入最优距离代价cost
            {
                int now = dp[k][j - 1] + cost(k + 1, i);
                if(now < dp[i][j])
                {
                    dp[i][j] = now;
                    opt[i][j] = k;
                }
            }
        }
    }

    cout << dp[n][m] << '\n';
    return 0;
}



*10.Coloring Brackets
//题意：给定长度为 n 的合法括号字符序列，每个括号有三种状态：不上色、红色 和 蓝色；
//要求匹配的一对括号中必须有且仅有一个有颜色，且任意两个相邻括号的颜色必须不同（但可以都无色）。求填色方案数。
//由于相邻括号有限制，必须增设左右端点颜色状态方便区间状态合并，即定义 dp[l][r][x][y] 为区间 [l, r] 上左端点颜色为 x，右端点颜色为 y 时的方案数
//已知每个括号符号都有其唯一对应的另一个括号符号，单纯的用 s[l] == '(' && s[r] == ')' 来判断配对区间显然是不可取的，
//故用 p[i] = j 记录与 i 上字符相匹配的字符的位置 j。
//当 p[l] == r 时，说明 [l, r] 为匹配区间，直接继承中间区间的状态
//当 p[l] != r 时，则需要遍历断点方案，且只需 p[l] = k 处作为断点即可，合并时采用乘法原理
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int N = 800, mod = 1e9 + 7;
int n, top = 0, sta[N], p[N];//p[i] 记录与 i 相匹配的字符的位置
ll dp[N][N][3][3];
string s;
void init()//预处理 p[i]，而 sta[i] 为接收括号字符 '(' 的栈，当遇到 ')' 时将一个 ')' 出栈
{
    for(int i = 1; i <= n; ++i)
    {
        if(s[i] == '(') sta[++top] = i;
        else
        {
            int pos = sta[top--];
            p[i] = pos;
            p[pos] = i;
        }
    }
}
int main()
{
    cin >> s;
    n = s.size();
    s = '*' + s;
    init();

    for(int len = 2; len <= n; ++len)
    {
        for(int l = 1; l + len - 1 <= n; ++l)
        {
            int r = l + len - 1;
            if(p[l] == r) //为匹配区间
            {
                if(len == 2) //特判长度为 2 时，初始化dp状态
                {
                    dp[l][r][0][1] = dp[l][r][0][2] = dp[l][r][1][0] = dp[l][r][2][0] = 1;
                    continue;
                }
                //分别继承中间区间的对应状态
                for(int i = 0; i < 3; ++i)
                { 
                    for(int j = 0; j < 3; ++j)
                    {
                        if(j != 1) dp[l][r][0][1] = (dp[l][r][0][1] + dp[l + 1][r - 1][i][j]) % mod;
                        if(j != 2) dp[l][r][0][2] = (dp[l][r][0][2] + dp[l + 1][r - 1][i][j]) % mod;
                        if(i != 1) dp[l][r][1][0] = (dp[l][r][1][0] + dp[l + 1][r - 1][i][j]) % mod;
                        if(i != 2) dp[l][r][2][0] = (dp[l][r][2][0] + dp[l + 1][r - 1][i][j]) % mod;
                    }
                }
            }
            else //不为匹配区间
            {
                int k = p[l];//仅此处能作为断点
                for(int x = 0; x < 3; ++x)
                    for(int y = 0; y < 3; ++y)
                        for(int p = 0; p < 3; ++p)
                            for(int q = 0; q < 3; ++q)
                                if(p == 0 || q == 0 || p != q) //相邻两个若有颜色，则颜色不能相同
                                    dp[l][r][x][y] = (dp[l][r][x][y] + dp[l][k][x][p] * dp[k + 1][r][q][y] % mod) % mod;//乘法原理
            }
        }
    }

    ll ans = 0;
    for(int i = 0; i < 3; ++i)
        for(int j = 0; j < 3; ++j)
            ans = (ans + dp[1][n][i][j]) % mod;
    cout << ans << '\n';
    return 0;
}





```



---

## 树形dp

> 树形 DP，即在**树上**进行的 DP。由于树固有的递归性质，树形 DP 一般都是 **dfs 递归** 进行的。

### 核心思路

- 树的遍历，用 **dfs** 从**根节点**开始进行记忆化搜索
- 从树**最深处**开始往回进行**dp**，用**子节点**dp值来更新**父节点**dp值









### 最大独立集问题

> 独立集是指图 G 中**两两互不相邻**的顶点构成的集合。最大独立集是该类顶点数量最多的独立集。

定义dp状态：

* `dp[u][0]`：以 u 为根的子树不选择 u 的最多选中结点数目或最大价值
* `dp[u][1]`：以 u 为根的子树选择 u 的最多选中结点数目或最大价值

状态转移：

* 不选 u ：在选与不选 v 中取最优解，即 `dp[u][0] += max(dp[v][0], dp[v][1])`
* 选中 u：由于相邻点不能同时选，故只能不选子结点 v，即 `dp[u][1] += dp[v][0]`

```c++
1.没有上司的舞会
//题意：有 n 名职员，编号为1 … n，他们的关系是一棵以老板为根节点的树，父节点就是其子节点的直接上司。每个职员有一个快乐指数 Hi，并给定从属关系，
//现在要召开一场舞会，使得没有职员和直接上司一起参会。主办方希望邀请一部分职员参会，使得所有参会职员的快乐指数总和最大，求这个最大值。
//思路：建图时是上司连向下属。对于一个结点要求父节点被选定后不能选，但父节点以上的结点被选定后仍能选，故这里需要dp进行状态转移。
//定义 dp[u][0/1] 代表以 u 为根的子树中不选与选 u 的最大权值和
//选择 u 时，以 u 为父节点的子节点 v 不能选，即 dp[u][1] += dp[v][0]
//不选 u 时，v 从选与不选两个状态中去最大者，即 dp[u][0] += max(dp[v][0], dp[v][1])
#include <bits/stdc++.h>
using namespace std;
const int N = 6e3 + 100, M = N;
int n, m, head[N], a[N], cnt = 1;
bool isson[N];
int dp[N][2];
struct Edge{
    int to, next, w;
    Edge(int a = 0, int b = 0, int c = 0) { to = a, next = b, w = c;}
    void add(int u, int v, int c = 0){ *this = Edge(v, head[u], c), head[u] = cnt++;}
}e[M];
void dfs(int u)
{
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        dfs(v);//先dfs到叶子节点，从子节点向上更新dp状态
        //更新父节点信息
        dp[u][1] += dp[v][0];
        dp[u][0] += max(dp[v][0], dp[v][1]);
    }
}
int main()
{
    cin >> n;
    m = n - 1;
    for(int i = 1; i <= n; ++i) cin >> dp[i][1];//初始化选 i 时的价值
    for(int i = 1; i <= m; ++i)
    {
        int v, u;
        cin >> v >> u;
        e[cnt].add(u, v);
        isson[v] = 1;//标记为非根节点
    }
    int root = 0;
    for(int i = 1; i <= n; ++i)//该图有且只有一个根节点
    {
        if(!isson[i])
        {
            root = i;
            break;
        }
    }
    dfs(root);
    cout << max(dp[root][0], dp[root][1]) << '\n';
    return 0;
}

// 用上模板类：
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int M = 6e3 + 10, N = 2 * M;

int n, m, cnt = 1;
int head[N];
ll dp[N][2];

template <class T = int>
class Edge
{
    public:
        int to, next;
        T w;
        Edge(){}
        Edge(int a, int b, T c) : to(a), next(b), w(c){}
        friend void addedge(Edge e[], int u, int v, T c = 0)
        {
            e[cnt] = Edge(v, head[u], c);
            head[u] = cnt++;
        }
        friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
};

Edge<int> e[M];
bool vis[N];

void dfs(int u)
{
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        dfs(v);
        dp[u][0] += max(dp[v][0], dp[v][1]);
        dp[u][1] += dp[v][0];
    }
}

int main()
{
    cin >> n;
    for(int i = 1; i <= n; ++i) cin >> dp[i][1];
    for(int i = 1; i < n; ++i)
    {
        int u, v;
        cin >> v >> u;
        addedge(e, u, v);
        vis[v] = 1;
    }
    int rt = 0;
    for(int i = 1; i <= n; ++i)
        if(!vis[i]) rt = i;
    dfs(rt);
    cout << max(dp[rt][0], dp[rt][1]) << "\n";
    return 0;
}



*2.P2607 [ZJOI2008] 骑士（最大独立集 + 基环树）
// 所给图没有规定是棵树，可能含有环和多个连通块，需要并查集统计全部连通块并逐个dfs求解。而每个连通块内可能含有环，需要进行断环操作：即把环找出来后从中间某边断开，分别以断边的两个端点为根做一次树形dp（最大独立集），取最大者累加到答案中。并注意断边的两端点在 dp 更新时无法互相选择。
#include <bits/stdc++.h>
using namespace std;
#define ll long long
#define inf 0x7fffffff
const int N = 1e6 + 10, M = 2 * N;

int n, m, k, cnt = 1;
int head[N], ds[N], a[N];
bool vis[N];
ll dp[N][2], ans = 0;

template <class T = int>
class Edge
{
    public:
        int to, next;
        T w;
        Edge(){}
        Edge(int a, int b, T c) : to(a), next(b), w(c){}
        friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
        friend void addedge(Edge e[], int u, int v, T c = 0)
        {
            e[cnt] = Edge(v, head[u], c);
            head[u] = cnt++;
        }
};

Edge<int> e[M];

void dfs(int u, int root)
{
    vis[u] = 1; // 标记整个连通块
    dp[u][1] = a[u];
    dp[u][0] = 0;
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        if(v == root) // 成环时防止重复遍历
        {
            dp[root][1] = -inf; // 因为是断边操作（减去 u -> root 这条边），则dfs过程中父节点 u 不能选择 root，因此把选择 root 的价值设为无穷小
            continue;
        }
        dfs(v, root);
        dp[u][0] += max(dp[v][0], dp[v][1]);
        dp[u][1] += dp[v][0];
    }
}

void work(int u)
{
    while(!vis[ds[u]]) // 向上寻找连通块的根 u
    {
        vis[u] = 1;
        u = ds[u]; 
    }
    dfs(u, u);
    ll tp = max(dp[u][0], dp[u][1]);
    u = ds[u]; // 如果是环，必定首尾相接，这里获得尾部编号
    dfs(u, u);
    ans += max(tp, max(dp[u][0], dp[u][1])); // 取两次 dfs 答案的最大者累计
}

int main()
{
    cin >> n;
    for(int i = 1; i <= n; ++i) ds[i] = i;
    for(int v = 1; v <= n; ++v)
    {
        int u;
        cin >> a[v] >> u;
        addedge(e, u, v);
        ds[v] = u; // 注意这里值是父节点，方便后续找根
    }
    for(int i = 1; i <= n; ++i)
        if(!vis[i]) work(i);
    cout << ans << "\n";
    return 0;
}






```

---

### 最小顶点覆盖问题

> 在树中选出**尽量少**的节点，使得树上**每一条边**都**至少有一端**的节点被选中。

定义dp状态：

* `dp[u][0]`：以 u 为根的子树不选择 u 的最少选中结点数目
* `dp[u][1]`：以 u 为根的子树选择 u 的最少选中结点数目

状态转移：

* 不选 u ：由于每条边至少需要一端选中，那么必须选子结点 v，即 `dp[u][0] += dp[v][1]`
* 选中 u：在选与不选 v 中取最优解，即 `dp[u][1] += min(dp[v][0], dp[v][1])`

```c++
1.P2016 战略游戏
//题意：在一颗树上的某些结点放置士兵，使得士兵们综合起来额能观察到树上所有的边（即所有边都与士兵点相连），求投入士兵的最少数量。
//思路：题意保证为一颗树，且题意是为无向图，但在树上可以当作有向图解决，故直接从根节点开始搜索。
// dp[u][1]:以 u 为根的子树在 u 上 放置 的士兵的最少所需的士兵数目
// dp[u][0]:以 u 为根的子树在 u 上不放置的士兵的最少所需的士兵数目
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 1e4, M = N;
int n, m, head[N], a[N], cnt = 1, root = 0;
bool isson[N];
int dp[N][2];
struct Edge{
    int to, next, w;
    Edge(int a = 0, int b = 0, int c = 0) { to = a, next = b, w = c;}
}e[M];
void addedge(int u, int v, int w = 0)
{
    e[cnt] = Edge(v, head[u], w);
    head[u] = cnt++;
}
void dfs(int u)
{
    //初始化
    dp[u][0] = 0;
    dp[u][1] = 1;
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        dfs(v);
        dp[u][1] += min(dp[v][0], dp[v][1]);//放的话，选择放了(也有可能成为最优解)和不放士兵的子树的最少数量者
        dp[u][0] += dp[v][1];//不放士兵就得加上放了士兵的子树
    }
}
int main()
{
    untie();
    cin >> n;
    for(int i = 1; i <= n; ++i)
    {
        int u, k, v;
        cin >> u >> k;
        ++u;
        while(k--)
        {
            cin >> v;
            ++v;
            addedge(u, v);
            isson[v] = 1;
        }
    }
    for(int i = 1; i <= n; ++i)
    {
        if(!isson[i])
        {
            root = i;
            break;
        }
    }
    dfs(root);
    cout << min(dp[root][0], dp[root][1]) << '\n';
    return 0;
}


// 类模板写法
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int M = 1e5 + 10, N = 2 * M;

int n, m, cnt = 1;
int head[N];
int dp[N][2];

template <class T = int>
class Edge
{
    public:
        int to, next;
        T w;
        Edge(){}
        Edge(int a, int b, T c) : to(a), next(b), w(c){}
        friend void addedge(Edge e[], int u, int v, T c = 0)
        {
            e[cnt] = Edge(v, head[u], c);
            head[u] = cnt++;
        }
        friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
};

Edge<int> e[M];
bool vis[N];

void dfs(int u, int fa = 0)
{
    dp[u][0] = 0;
    dp[u][1] = 1;
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        if(v == fa) continue;
        dfs(v, u);
        dp[u][0] += dp[v][1];
        dp[u][1] += min(dp[v][0], dp[v][1]);
    }
}

int main()
{
    cin >> n;
    for(int i = 1; i <= n; ++i)
    {
        int u, k; cin >> u >> k;
        u++;
        while(k--)
        {
            int v; cin >> v;
            v++;
            addedge_undirected(e, u, v);
            vis[v] = 1;
        }
    }
    int rt = 0;
    for(int i = 1; i <= n; ++i)
        if(!vis[i]) rt = i;
    dfs(rt);
    cout << min(dp[rt][0], dp[rt][1]) << "\n";
    return 0;
}
```

---

### 最小支配集问题

> 在树中选出**尽量少**的节点，使得树上**每个点**要么**被选**、要么被它的相邻点支配（即该点**有相邻点被选**）

以 [[P2899]Cell Phone Network G](https://www.luogu.com.cn/problem/P2899) 为例：

* 题意：给定一个有 n 个结点及 n - 1 条边的**无向无根**图，问选中结点的最少数量，使得所有结点要么自身被选中，要么其至少一个邻居结点被选中。
* 状态定义（以下dp状态都是认为结点 u 下的所有子树上的结点都已被覆盖，然后**讨论覆盖 u 的三种方式**）：
  *  `dp[u][0]`：结点 u 被**自己**覆盖（u 被选中）的最小花费。
  *  `dp[u][1]`：结点 u 被其**父节点 fa** 覆盖（即其父节点被选中，此时不用选中 u）的最小花费。
  *  `dp[u][2]`：结点 u 被其**子节点 v** 覆盖（即至少存在一个子节点被选中，此时不用选中 u）的最小花费。
* 状态转移：
  * 由于 u 下结点都已被覆盖，且 `dp[u][0]` 保证了 u 被选中即被覆盖，则直接从子节点转移出最小花费的状态，即 `dp[u][0] += min(dp[v][0], dp[v][1], dp[v][2])`。
  * 此时 u 没被选中，被父节点 fa 覆盖，则其子节点 v 要么自己覆盖自己，要么 v 的子节点覆盖 v，就是不能被 u 覆盖，即 `dp[u][1] += min(dp[v][0], dp[v][2])`。
  * 首先作为 v 的父节点 u 没被选中，那么至少状态 `dp[v][1]` 不用考虑转移到当前结点 u 上，初步得到 `dp[u][2] += min(dp[v][0], dp[v][2])`。但这样会产生一个问题，会出现 `dp[u][2]` 全是 `dp[v][2]` 转移过来的情况，而这样就说明 u 的**子节点全都没有被选中，也就不可能覆盖 u**，所以初步转移方程显然不够完善，则我们需要用 `flag` 标记判断是否全是 `dp[v][2]` 转移过来的：
    * `flag == 1` ：说明 `dp[u][2]` 至少一次由 `dp[v][0]` 转移过来，则无需多余操作。
    * `flag == 0` ：说明 `dp[u][2]` 全都由 `dp[v][2]` 转移过来，需要取一个最优花费的子节点 v 的状态 `dp[_v][0]`。为方便操作，每次由 `dp[v][2]` 转移时记录 `p = min(dp[v][0] - dp[v][2])`，最终 `p = dp[v][0] - dp[_v][2]`，然后需要重算时再给 `dp[u][2] += p`，这样就加上了 `dp[_v][0]` 还抵消了该递归层上错误转移的 `dp[_v][2]`（**反悔机制**）。

实现代码：

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 1e4 + 10, M = N, inf = N;
int n, m, head[N], cnt = 1;
int dp[N][3];
struct Edge{
    int to, next, w;
    Edge(int a = 0, int b = 0, int c = 0) { to = a, next = b, w = c;}
}e[M << 1];//记得无向边需要开两倍空间
int _min(int x1, int x2, int x3) { return x1 < x2 ? (x1 < x3 ? x1 : x3) : (x2 < x3 ? x2 : x3);}
void addedge(int u, int v, int w = 0)
{
    e[cnt] = Edge(v, head[u], w);
    head[u] = cnt++;
}
void dfs(int u, int fa = -1)// fa 防止 dfs 成环
{
    dp[u][0] = 1;
    dp[u][1] = dp[u][2] = 0;

    bool flag = 0;
    int p = inf;
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        if(v == fa) continue;
        dfs(v, u);
        dp[u][0] += _min(dp[v][0], dp[v][1], dp[v][2]);
        dp[u][1] += min(dp[v][0], dp[v][2]);
        if(dp[v][0] <= dp[v][2])
        {
            flag = 1;
            dp[u][2] += dp[v][0];
        }
        else//反悔 - 虽然现在选了 dp[v][2]，但可能反悔选回该层的 dp[v][0]
        {
            p = min(p, dp[v][0] - dp[v][2]);
            dp[u][2] += dp[v][2];
        }
    }
    if(!flag) dp[u][2] += p;
}
int main()
{
    untie();
    cin >> n;
    m = n - 1;
    int u, v;
    for(int i = 1; i <= m; ++i)
    {
        cin >> u >> v;
        addedge(u, v);
        addedge(v, u);//方向不定，无向图
    }
    dfs(u);//无根，随便选个点开始
    cout << min(dp[u][0], dp[u][2]) << '\n';
    return 0;
}


// 类模板写法
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int M = 2e5 + 10, N = 2 * M;

int n, m, cnt = 1;
int head[N];
int dp[N][3];

template <class T = int>
class Edge
{
    public:
        int to, next;
        T w;
        Edge(){}
        Edge(int a, int b, T c) : to(a), next(b), w(c){}
        friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
        friend void addedge(Edge e[], int u, int v, T c = 0)
        {
            e[cnt] = Edge(v, head[u], c);
            head[u] = cnt++;
        }
};

Edge<int> e[M];

void dfs(int u, int fa = 0)
{
    bool fl = 0;
    int p = 0x7fffffff;
    dp[u][0] = 1;
    dp[u][1] = dp[u][2] = 0;
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        if(v == fa) continue;
        dfs(v, u);
        dp[u][0] += min({dp[v][0], dp[v][1], dp[v][2]});
        dp[u][1] += min(dp[v][0], dp[v][2]);
        if(dp[v][0] <= dp[v][2])
        {
            fl = 1;
            dp[u][2] += dp[v][0];
        }
        else
        {
            p = min(p, dp[v][0] - dp[v][2]);
            dp[u][2] += dp[v][2];
        }
    }
    if(!fl) dp[u][2] += p;
}

int main()
{
    int u, v;
    cin >> n;
    for(int i = 1; i < n; ++i)
    {
        cin >> u >> v;
        addedge_undirected(e, u, v);
    }
    dfs(u);
    cout << min(dp[u][0], dp[u][2]) << "\n";
    return 0;
}
```

其他例题：

```c++

```



---

### 树上背包问题

> 将线性背包问题映射到**树结构**上求解，结点自身和结点之间的性质决定背包的种类。
>

普通背包dp定义：`dp[i][j]` 表示前 i 个物品占用 j 的空间所得最大价值。

树形背包dp定义：

* 按点：`dp[u][i][j]` 表示以结点 u 为**根节点**的子树下的前 i 个子树（以 u 的子节点为根）中选中 j 个结点所得最大价值。
* 按边：`dp[u][i]` 表示以结点 u 为根节点的子树选择 i 条边的最优权值花费。



```c++
1.二叉苹果树（树上01背包）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 1e3 + 10, inf = 0x3f3f3f3f;
struct Edge{
    int to, next, w;
    Edge(int a = 0, int b = 0, int c = 0) { to = a, next = b, w = c;}
}e[N << 1];
int n, m, root = 1, cnt = 1;
int head[N], dp[N][N], ans = 0;
void addedge(int u, int v, int w)
{
    e[cnt] = Edge(v, head[u], w);
    head[u] = cnt ++;
}
void dfs(int u, int fa = -1)
{
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to, w = e[i].w;
        if(v == fa) continue;
        dfs(v, u);//自底向上更新dp
        for(int j = m; j >= 1; --j)//枚举容量 j
        {
            for(int k = 0; k < j; ++k)//枚举留给子结点的容量（u 自己用 1 个单位容量，故 v 最大剩余容量 j - 1）
            {
                dp[u][j] = max(dp[u][j], dp[v][k] + dp[u][j - k - 1] + w);
                //注意结点 u 容量为 j 时，消耗容量 k 给子节点的同时，u -> v 也有一条边需要消耗，故转移的状态为 dp[u][j - k - 1]
            }
        }
    }
}
int main()
{   
    untie();
    cin >> n >> m;
    for(int i = 1; i < n; ++i)
    {
        int u, v, w;
        cin >> u >> v >> w;
        addedge(u, v, w);
        addedge(v, u, w);
    }
    dfs(root);
    cout << dp[root][m];
    return 0;
}

// 理解2（最佳）：
// dp[u][i] 表示以结点 u 为根节点的子树选择 i 条边的最优权值花费。
// 当某条边被保留下来时，从根节点到这条边的路径上的所有边也都必须保留下来
// dp 转移：dp[u][i] = max(dp[u][i], dp[u][i - (j + 1)] + dp[v][j] + w)
// 其中 dp[u][i - (j + 1)] 即留 j + 1 条边，其中 1 条为边 u - v，剩余 j 条留给子树 v
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int N = 1e2 + 10, M = 2 * N;

int n, m, cnt = 1;
int head[N];
int sz_edge[N]; // 存以结点 u 为根的子树所含边数
int dp[N][M];

template <class T = int>
class Edge
{
    public:
        int to, next;
        T w;
        Edge(){}
        Edge(int a, int b, T c) : to(a), next(b), w(c){}
        friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
        friend void addedge(Edge e[], int u, int v, T c = 0)
        {
            e[cnt] = Edge(v, head[u], c);
            head[u] = cnt++;
        }
};

Edge<int> e[M];

void dfs(int u, int fa = 0)
{
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to, w = e[i].w;
        if(v == fa) continue;
        dfs(v, u);
        sz_edge[u] += sz_edge[v] + 1; // 可以用以优化下面的 dp 过程
        for(int j = min(m, sz_edge[u]); j; --j) // 01背包，逆序遍历容量
            for(int k = min(j - 1, sz_edge[v]); k >= 0; --k) // 剩下 k 条留给子树 v，这一维正序倒序都无所谓，但是把取min放在初始化部分里可以减少运算次数，算是一个优化的小习惯
                dp[u][j] = max(dp[u][j], dp[u][j - (k + 1)] + dp[v][k] + w);
    }
}

int main()
{
    cin >> n >> m;
    for(int i = 1; i < n; ++i)
    {
        int u, v, w;
        cin >> u >> v >> w;
        addedge_undirected(e, u, v, w);
    }
    dfs(1);
    cout << dp[1][m] << "\n";
    return 0;
}



2.P2014 选课（树上有依赖的背包问题 -- 必须先选主件才能选附件）
题意：有 n 门课，第 i 门课的学分是 Si。每门课有一门或没有先修课，有则需要上了先修课才能上这门课。现要选 m 门课，使得学分总和最大。
思路：题目相当于给一个可能有多个入度为0的根节点即有多个不连通的树构成森林，故添加超级源点 0 连通那些没有先修课的课，连接起孤立的树。定义 dp[u][j] 为选中结点 u 的 j 个子节点的最大价值（即不包括 u）。
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int N = 1e3 + 10, M = 2 * N;

int n, m, cnt = 1;
int head[N];
int dp[N][M];

template <class T = int>
class Edge
{
    public:
        int to, next;
        T w;
        Edge(){}
        Edge(int a, int b, T c) : to(a), next(b), w(c){}
        friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
        friend void addedge(Edge e[], int u, int v, T c = 0)
        {
            e[cnt] = Edge(v, head[u], c);
            head[u] = cnt++;
        }
};

Edge<int> e[M];

void dfs(int u, int fa = 0)
{
    for(int i = head[u]; ~i; i = e[i].next)
    {
        int v = e[i].to, w = e[i].w;
        if(v == fa) continue;
        dfs(v, u);
        for(int j = m; j >= 1; --j) // 至少选一门即一定选结点 u，这样才能选其子树下的其他结点（依赖性）
            for(int k = 0; k < j; ++k) // 子节点可以选 0 ~ j - 1 个（第 j 个是 u）
                dp[u][j] = max(dp[u][j], dp[u][j - k] + dp[v][k]);
    }
}

int main()
{
    cin >> n >> m;
    ++m; // 题意所构成的树以点 0 为根
    memset(head, -1, sizeof(head));
    for(int v = 1; v <= n; ++v)
    {
        int u;
        cin >> u >> dp[v][1];
        addedge(e, u, v);
    }
    dfs(0);
    cout << dp[0][m] << "\n";
    return 0;
}



3.P1273 有线电视网（树上分组背包）
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int N = 3e3 + 10, M = 2 * N;

int n, m, cnt = 1;
int head[N];
int dp[N][N]; // dp[u][j] 表示根节点为 u 的子树中选 j 个结点的最优花费
int val[N];

template <class T = int>
class Edge
{
    public:
        int to, next;
        T w;
        Edge(){}
        Edge(int a, int b, T c) : to(a), next(b), w(c){}
        friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
        friend void addedge(Edge e[], int u, int v, T c = 0)
        {
            e[cnt] = Edge(v, head[u], c);
            head[u] = cnt++;
        }
};

Edge<int> e[M];

int dfs(int u, int fa = 0) // 返回叶子结点个数
{
    dp[u][0] = 0; // 初始化
    if(u > n - m) // 叶子结点
    {
        dp[u][1] = val[u];
        return 1;
    }
    int sum = 0;
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to, w = e[i].w, t = 0;
        if(v == fa) continue;
        t = dfs(v, u); // v 为根节点的子树的叶子节点个数 t
        sum += t;
        for(int j = sum; j; --j) // 前 i 个子树中选，前 i 个子树中共有 sum 个叶子节点（dp[u][i][j] 滚动到 dp[u][j]）
        {
            for(int k = min(j, t); k; --k) // 遍历该组方案（即遍历子树 v 的所有种选择 ——— 不选、选1个、选2个 ……）
            {
                dp[u][j] = max(dp[u][j], dp[u][j - k] + dp[v][k] - w);
            }
        }

    }
    return sum;
}

int main()
{
    memset(dp, -0x3f, sizeof(dp)); // 无穷小
    cin >> n >> m;
    for(int u = 1; u <= n - m; ++u)
    {
        int k, v, w;
        cin >> k;
        while(k--)
        {
            cin >> v >> w;
            addedge(e, u, v, w);
        }
    }
    for(int i = n - m + 1; i <= n; ++i) cin >> val[i];
    // for(int i = n - m + 1; i <= n; ++i) cin >> dp[i][1], dp[i][0] = 0; // 也可以直接初始化
    dfs(1);
    for(int j = m; j; --j)
    {
        if(dp[1][j] >= 0)
        {
            cout << j << "\n";
            break;
        }
    }
    return 0;
}





```





---

### 换根dp问题（二次扫描问题）

> 通常不会指定根结点，并且**根结点的变化**会对一些值，例如**子结点深度和、点权和**等产生影响。通常需要**两次 DFS**，第一次 DFS **预处理**诸如深度，点权和之类的信息，在第二次 DFS 开始运行**换根动态规划**。
>

以 [[P3478 POI2008] STA-Station](https://www.luogu.com.cn/problem/P2986) 为例：

* 题意：给定一棵有 n 个点的树，请求出一个结点，使得以这个结点为根时，所有结点的深度之和最大。

* 思路：已知树中所有点都是连通的，现

  * 定义结点 u 的子树结点数量（包括 u） `sz[u] = 1 + Σsz[v]` （v 为 u 的子节点）方便处理整个子树的深度变化
  * 定义dp状态 dp[u] 表示以结点 u 为根时**所有结点的深度和**即 `dp[u] = Σdepth[i] (1 <= i <= n)`

  * 初始化：用**第一次 dfs** 预处理 `sz[u]`，同时初始化dp首项 `dp[1]` 的值

  * 状态转移：**第二次 dfs** 考虑由父节点向子节点更新 `dp[u] -> dp[v]`（自顶向下）即**“根节点由 u 变成 v 时所有节点深度和的变化”**，则有：

    * 所有**在** v 的子树上的结点深度全部 -1（因为由 u -> v，这些结点更接近根节点了），则总深度减少 `sz[v]`
    * 所有**不在** v 的子树上的结点深度全部 +1（这些结点更远离根节点了），则总深度增加 `n - sz[v]`

    综合来说：进行自顶向下的更新，对子节点 v 有 `dp[v] = dp[u] - sz[v] + (n - sz[v]) = dp[u] + n - 2 * sz[v]`。（注意：该dp更新容易爆 int，一般开 **long long**）

实现代码：

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 1e6 + 100;
struct Edge{
    int to, next;
    Edge(int a = 0, int b = 0) { to = a, next = b;}
}e[N << 1];
int n, cnt = 1;
int ans = 1;//答案为结点编号
int head[N];
ll sz[N], dp[N];
void addedge(int u, int v)
{
    e[cnt] = Edge(v, head[u]);
    head[u] = cnt++;
}
ll pre_dfs(int u, int fa = -1, ll dep = 1)
{
    sz[u] = 1;
    ll sum = dep;
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        if(v == fa) continue;
        sum += pre_dfs(v, u, dep + 1);
        sz[u] += sz[v];
    }
    return sum;
}
void dp_dfs(int u, int fa = -1)
{
    if(dp[u] > dp[ans]) ans = u;//取最大深度和的结点
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        if(v == fa) continue;
        dp[v] = dp[u] + n - (sz[v] << 1LL);
        dp_dfs(v, u);
    }
}
int main()
{
    untie();
    cin >> n;
    for(int i = 1; i < n; ++i)
    {
        int u, v;
        cin >> u >> v;
        addedge(u, v);
        addedge(v, u);
    }
    //取结点1为入口
    pre_dfs(1);
    dp[1] = sz[1];//初始化自定义的dfs首结点
    dp_dfs(1);
    cout << ans << '\n';
    return 0;
}


// 类模板写法
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int M = 2e6 + 10, N = M;

int n, m, cnt = 1;
int head[N];
int sz[N];
int ans = 0;
ll dp[N];

template <class T = int>
class Edge
{
    public:
        int to, next;
        T w;
        Edge(){}
        Edge(int a, int b, T c) : to(a), next(b), w(c){}
        friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
        friend void addedge(Edge e[], int u, int v, T c = 0)
        {
            e[cnt] = Edge(v, head[u], c);
            head[u] = cnt++;
        }
};

Edge<int> e[M];

void pre_dfs(int u, int fa = 0)
{
    sz[u] = 1;
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        if(v == fa) continue;
        pre_dfs(v, u);
        sz[u] += sz[v];
    }
}

void dp_dfs(int u, int fa = 0)
{
    if(dp[u] > dp[ans]) ans = u;
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        if(v == fa) continue;
        dp[v] = dp[u] - sz[v] + (n - sz[v]);
        dp_dfs(v, u);
    }
}

int main()
{
    int u, v;
    cin >> n;
    for(int i = 1; i < n; ++i)
    {
        cin >> u >> v;
        addedge_undirected(e, u, v);
    }
    pre_dfs(u);
    dp[u] = sz[u];
    ans = u;
    dp_dfs(u);
    cout << ans << "\n";
    return 0;
}
```

换根dp 其他例题：

```c++
1.P2986 Great Cow Gathering G
//定义 sz[u] 为以 u 为根的子树下奶牛的总数量, len[u] 为以 u 为根的子树下总路径长度
//根节点 u -> v 时有 dp[u] -> dp[v]，则所有在 v 的子树上的子节点总距离减少 num[v] * w，不在则增加 (n - sz[v]) * w
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e5 + 100;
struct Edge{
    int to, next, w;
    Edge(int a = 0, int b = 0, int c = 0) { to = a, next = b, w = c;}
}e[N << 1];
int n, a[N], head[N], cnt = 1;
ll sz[N], num[N], dp[N], cow = 0;//cow 为总奶牛数
ll ans = 1e9;
void addedge(int u, int v, int w)
{
    e[cnt] = Edge(v, head[u], w);
    head[u] = cnt++;
}
ll pre_dfs(int u, int fa = -1)
{
    ll res = 0;
    num[u] = a[u];//必须在这初始化，不能写在下面，因为是自底向上更新，至少叶子结点需要先初始化
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to, w = e[i].w;
        if(v == fa) continue;
        res += pre_dfs(v, u) + num[v] * w;
        num[u] += num[v];
    }
    return res;
}
void dp_dfs(int u, int fa = -1)
{
    // if(dp[u] < ans) ans = dp[u];//当点 1 不存在时，该句导致 ans 被错误地初始化为 0，故不能这么写
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to, w = e[i].w;
        if(v == fa) continue;
        ll d = (cow - 2 * num[v]) * w;
        dp[v] = dp[u] + d;
        ans = min(ans, dp[v]);
        dp_dfs(v, u);
    }
}
int main()
{
    untie();
    cin >> n;
    for(int i = 1; i <= n; ++i) cin >> a[i], cow += a[i];
    for(int i = 1; i < n; ++i)
    {
        int u, v, w;
        cin >> u >> v >> w;
        addedge(u, v, w);
        addedge(v, u, w);
    }
    dp[1] = pre_dfs(1);
    ans = dp[1];
    dp_dfs(1);
    cout << ans << '\n';
    return 0;
}

// 类模板写法
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int M = 1e6 + 10, N = M;

int n, m, cnt = 1;
int head[N];
int a[N];
ll dp[N], amt[N], num = 0;

template <class T = int>
class Edge
{
    public:
        int to, next;
        T w;
        Edge(){}
        Edge(int a, int b, T c) : to(a), next(b), w(c){}
        friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
        friend void addedge(Edge e[], int u, int v, T c = 0)
        {
            e[cnt] = Edge(v, head[u], c);
            head[u] = cnt++;
        }
};

Edge<int> e[M];

ll pre_dfs(int u, int fa = 0)
{
    ll s = 0;
    amt[u] = a[u];
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        ll w = e[i].w;
        if(v == fa) continue;
        s += pre_dfs(v, u) + amt[v] * w;
        amt[u] += amt[v];
    }
    return s;
}

void dp_dfs(int u, int fa = 0)
{
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        ll w = e[i].w;
        if(v == fa) continue;
        dp[v] = dp[u] - w * amt[v] + w * (num - amt[v]);
        dp_dfs(v, u);
    }
}

int main()
{
    int u, v, w;
    cin >> n;
    for(int i = 1; i <= n; ++i) cin >> a[i], num += 1LL * a[i];
    for(int i = 1; i < n; ++i)
    {
        cin >> u >> v >> w;
        addedge_undirected(e, u, v, w);
    }
    dp[u] = pre_dfs(u);
    dp_dfs(u);
    cout << *min_element(dp + 1, dp + 1 + n) << "\n";
    return 0;
}



2.Hdu 2196 Computer 
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
#define ll long long
const int N = 1e4 + 10, M = 2 * N;

int n, m, cnt = 1;
int head[N], f[N][2];// f[u][0] 为 u 到叶子节点的最长距离，f[u][1] 为次长距离
int dp[N];

template <class T = int>
class Edge
{
    public:
        int to, next;
        T w;
        Edge(){}
        Edge(int a, int b, T c) : to(a), next(b), w(c){}
        friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
        friend void addedge(Edge e[], int u, int v, T c = 0)
        {
            e[cnt] = Edge(v, head[u], c);
            head[u] = cnt++;
        }
};

Edge<int> e[M];

void pre_dfs(int u, int fa = 0)
{
    f[u][0] = f[u][1] = 0;
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to, w = e[i].w;
        if(v == fa) continue;
        pre_dfs(v, u);
        int now = w + f[v][0];
        if(now > f[u][0])
        {
            f[u][1] = f[u][0];
            f[u][0] = now;
        }
        else if(now > f[u][1])
        {
            f[u][1] = now;
        }
    }
}

void dp_dfs(int u, int fa = 0)
{
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to, w = e[i].w;
        if(v == fa) continue;
        dp[v] = max(dp[u], f[v][0] + w == f[u][0] ? f[u][1] : f[u][0]) + w;
        dp_dfs(v, u);
    }
}

int main()
{
    while(cin >> n)
    {
        cnt = 1;
        memset(head, 0, sizeof(head));
        memset(dp, 0, sizeof(dp));

        for(int u = 2; u <= n; ++u)
        {
            int v, w;
            cin >> v >> w;
            addedge_undirected(e, u, v, w);
        }
        pre_dfs(1);
        dp_dfs(1);
        for(int i = 1; i <= n; ++i) cout << max(f[i][0], dp[i]) << "\n";
    }
    return 0;
}










```



---

### 其他

```c++
1.P3047 [USACO12FEB] Nearby Cows G （两次dfs更新 + 容斥定理）
// 这里两次 dfs 对应于从某个结点 u 出发的两个不同方向：子树方向 和 父节点方向
// dp[u][j] 只记录了 u 从上往下走 j 步的点权和，但忽略了 u 的父节点到达 u 的情况，故需要第二次 dfs 从上往下更新
// 第二次 dfs :
// 首先由于 j - 1 层包含其子树所有结点，利用容斥定理减去 j - 2 层就只剩下 j - 1 一层结点
// 然后，仿照第一次 dfs 的更新方法：dp[v][j] += dp[u][j - 1] ，从 u 到 v 差一步
// 先后顺序不能反转，防止待利用状态被覆盖
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int N = 1e5 + 10, M = 2 * N;

int n, m, k, cnt = 1;
int head[N];
int dp[N][25]; // dp[u][j] 表示根节点 u 向下走 j 步的点权和
int ans[N];

template <class T = int>
class Edge
{
    public:
        int to, next;
        T w;
        Edge(){}
        Edge(int a, int b, T c) : to(a), next(b), w(c){}
        friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
        friend void addedge(Edge e[], int u, int v, T c = 0)
        {
            e[cnt] = Edge(v, head[u], c);
            head[u] = cnt++;
        }
};

Edge<int> e[M];

void dp_dfs(int u, int fa = 0) // 在子树内的情况
{
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to, w = e[i].w, t = 0;
        if(v == fa) continue;
        dp_dfs(v, u);
        for(int j = 1; j <= k; ++j) dp[u][j] += dp[v][j - 1]; // v 向上走一步到达 u
    }
}

void dp_dfs2(int u, int fa = 0) // 不在子树内的情况（父节点方向）
{

    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to, w = e[i].w, t = 0;
        if(v == fa) continue;
        for(int j = k; j >= 2; --j) dp[v][j] -= dp[v][j - 2]; // 逆序遍历，否则导致待使用状态被覆盖
        for(int j = 1; j <= k; ++j) dp[v][j] += dp[u][j - 1]; // 父亲更新儿子
        dp_dfs2(v, u);
    }
}

int main()
{
    cin >> n >> k;
    for(int i = 1; i < n; ++i)
    {
        int u, v;
        cin >> u >> v;
        addedge_undirected(e, u, v);
    }
    for(int i = 1; i <= n; ++i) cin >> dp[i][0];
    dp_dfs(1);
    dp_dfs2(1);
    for(int i = 1; i <= n; ++i)
        for(int j = 0; j <= k; ++j)
            ans[i] += dp[i][j];
    for(int i = 1; i <= n; ++i) cout << ans[i] << "\n";
    return 0;
}



2.P3698 [CQOI2017] 小Q的棋盘
// 贪心
// 可以走 k 步，要尽可能保证每一次移动都到达一个新的节点，所以要先向深度最深的节点走。设最长链长度为 lmax
// k <= lmax，则直接沿着最长链走最优，每走一步就多一个节点，那么总节点数为 lmax + 1
// k > lmax，首先走完最长链后还有剩余步数 k - lmax，在走最长链的过程中若要添加一个邻点并回到最长链上，则需要两步
//          显然，添加 num 个邻点，这需要 2 * num 步，那么可添入邻点数量为 (k - lmax) / 2，故总节点数为 lmax + 1 + (k - lmax) / 2
//          注意，k 可能特别大，结果有可能超过 n，故最终答案为 min(n, lmax + 1 + (k - lmax) / 2)
#include <bits/stdc++.h>
using namespace std;
#define ll long long
const int N = 200 + 10, M = 2 * N;

int n, m, k, cnt = 1;
int head[N];
int lmax = 0;

template <class T = int>
class Edge
{
    public:
        int to, next;
        T w;
        Edge(){}
        Edge(int a, int b, T c) : to(a), next(b), w(c){}
        friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
        friend void addedge(Edge e[], int u, int v, T c = 0)
        {
            e[cnt] = Edge(v, head[u], c);
            head[u] = cnt++;
        }
};

Edge<int> e[M];

void dp_dfs(int u, int fa = 0, int len = 0)
{
    lmax = max(lmax, len);
    for(int i = head[u]; i; i = e[i].next)
    {
        int v = e[i].to;
        if(v == fa) continue;
        dp_dfs(v, u, len + 1);
    }
}

int main()
{
    cin >> n >> k;
    for(int i = 1; i < n; ++i)
    {
        int u, v;
        cin >> u >> v;
        ++u, ++v;
        addedge_undirected(e, u, v);
    }
    dp_dfs(1);
    
    if(k <= lmax) cout << k + 1 << "\n";
    else cout << min(n, lmax + 1 + (k - lmax) / 2) << "\n";
    return 0;
}



3.ABC341 F
基于01背包的树形dp —— dp[u] = max{ ∑dp[v] } + 1 (v ∈ S)
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define bll __int128_t
#define all(v) v.begin(), v.end()
#define endl '\n'
#define gcd(a, b) __gcd(a, b)
#define mk(a, b) make_pair((a), (b))
#define lowbit(x) ((x) & -(x))
const int N = 5e5 + 10, inf = 1e9;
const ll linf = 1e18;
const bll mod = (bll)1e31 + 7177;

void Solve()
{
    int n, m;
    cin >> n >> m;

    vector<vector<int>> G(n + 5);
    vector<pair<int, int>> E(m + 5);
    vector<ll> A(n + 5), cost(n + 5);
    while(m--)
    {
        int u, v; cin >> u >> v;
        E.emplace_back(u, v);
    }
    for(int i = 1; i <= n; ++i) cin >> cost[i];
    for(int i = 1; i <= n; ++i) cin >> A[i];
    for(auto [u, v] : E)
    {
        if(cost[u] > cost[v]) G[u].push_back(v);
        else if(cost[v] > cost[u]) G[v].push_back(u);
    }

    ll ans = 0;
    vector<ll> f(n + 5, 0);
    for(int i = 1; i <= n; ++i)
    {
        if(A[i] == 0) continue;
        auto dfs = [&](auto self, int u) -> ll
        {
            if (f[u]) return f[u];
            if(G[u].size() == 0) return 1;

            ll res = 0;
            vector<ll> dp(cost[u], 0);
            for(auto v : G[u])
            {
               for(int j = cost[u] - 1; j >= cost[v]; --j)
                {
                    dp[j] = max(dp[j], dp[j - cost[v]] + self(self, v));
                    res = max(res, dp[j]);
                }
            }
            return f[u] = res + 1;            
        };
        ans += A[i] * dfs(dfs, i);
    }
    cout << ans << endl;
}

int main()
{
    untie();
    int T = 1;
    // cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}





```





---

## 状压dp

> 通过**将 K 种状态压缩为 K进制整数**来达到优化转移的目的

举个例子，现有 n 个集合，每个集合有 m 个元素，设元素值为 1 ~ M，假设 M = 8。想知道如何组合最少个集合，来使得它们的并集含有  所有种元素值（即 1 ~ M 的元素值都有）。现保证至少有一种组合能选到所有种元素值。在动态规划的过程中，我们定义 `dp[j]` 表示构成该组合 j 所需最少集合个数，转移方程即 `dp[集合 i + 当前组合 j] = min(dp[集合 i + 当前组合 j], dp[当前组合 j] + 1)`，我们令下一个状态为 `to = 集合 i + 当前组合 j`，则有 `dp[to] = min(dp[to], dp[j] + 1)`。

问题就在于如何将 `dp[j]` 的下标 `j` 表达成一种组合情况，我们知道上述例子元素值的状态只有两种状态：有 或 没有，容易联想到 0 和 1，显然，用  **M 位二进制数**能表达其组合情况，如 `00001010` 可表示只有第 2、4 种元素值存在。

那么状压dp的精髓就在于：**用 M 位 K 进制数 表示 M 种元素（每种元素都有 K 种状态）的状态序列**

其缺点也很明显，空间复杂度较高。

```c++
1.P8687 [蓝桥杯 2019 省 A] 糖果
//状压dp，这里对每种糖果种类有取与不取两种状态，故用二进制。如 0110 表示第二、三种糖果取，其它不取
//dp[i] 定义为得到组合 i 所需最少包数，而得到全部糖果的组合为 (1 << m) - 1 即二进制每个数位都为 1，故答案为 dp[(1 << m) - 1]
#include <bits/stdc++.h>
using namespace std;
const int N = 2e6 + 10;
int a[105];
int dp[N];
int main()
{
    int n, m ,k;
    cin >> n >> m >> k;
    memset(dp, -1, sizeof(dp));
    for(int i = 1; i <= n; ++i)
    {
        int now = 0;
        for(int j = 1; j <= k; ++j)
        {
            int t; cin >> t;
            now |= (1 << (t - 1));
        }
        dp[now] = 1;//初始化该组合有一包
        a[i] = now;
    }
    for(int i = 1; i <= n; ++i)//加入第 i 包
    {
        for(int j = 0; j <= (1 << m) - 1; ++j)//枚举所有组合
        {
            if(dp[j] == -1) continue;
            int to = j | a[i];//若加入 i 包，下个状态为 to
            if(dp[to] == -1) dp[to] = dp[j] + 1;//如果未曾组合出该种组合，直接从组合 j 加上第 i 包
            else dp[to] = min(dp[to], dp[j] + 1);//如果组合过，就取最小者
        }
    }
    cout << dp[(1 << m) - 1];
    return 0;
}



2.








```

---

## 数位dp

**引入**

数位是指把一个数字按照个、十、百、千等等一位一位地拆开，关注它**每一位上的数字**。如果拆的是十进制数，那么每一位数字都是 0~9，其他进制可类比十进制。

数位 DP 用来解决一类特定问题，它们具有如下特征：

1. 要求统计满足一定条件的数的数量（即，最终目的为**计数**）；
2. 这些条件经过转化后可以使用**「数位」**的思想去理解和判断；
3. 输入会提供一个数字区间 `[a, b]`（有时也只提供**上界** `n`）来作为统计的限制；
4. 上界很大（比如 `1e18`），暴力枚举显然会超时。



**基本原理**

考虑人类计数的方式，最朴素的计数就是从小到大开始依次加一。但我们发现对于位数比较多的数，这样的过程中有许多重复的部分。例如，从 7000 数到 7999、从 8000 数到 8999、和从 9000 数到 9999 的过程非常相似，它们都是后三位从 000 变到 999，不一样的地方只有千位这一位，所以我们可以把这些过程归并起来，将这些过程中产生的计数答案也都存在一个通用的数组里。此数组根据题目具体要求设置状态，用递推或 DP 的方式进行状态转移。

数位 DP 中通常会利用常规计数问题技巧，比如把一个区间内的答案拆成两部分相减（即 `ans[l, r] = ans[0, r] - ans[0, l - 1]` ）

那么有了通用答案数组，接下来就是统计答案。统计答案可以选择**记忆化搜索**，也可以选择**循环迭代递推**。为了不重不漏地统计所有不超过上限的答案，要**从高到低枚举每一位**，再考虑每一位都可以填哪些数字，最后利用通用答案数组统计答案。

### 模板

```c++
// 如果一个正整数每一个数位都是 互不相同 的，我们称它是 特殊整数 。
// 给你一个 正整数 n (n >= 1)，请你返回区间 [1, n] 之间特殊整数的数目。
#include <bits/stdc++.h>
using namespace std;
class Solution 
{
public:
    // 记忆化搜索
    int dp[15][1050][2][2];
    int countSpecialNumbers(int n) 
    {
        string str = to_string(n);
        int len = str.size();
        // pos 表示当前遍历到的数位，此处 pos = 0 最高位，pos = len - 1 为个位
        // mask 表示前面数位所填入的数字集合(表示 0 ~ 9 的存在)
        // is_limit 表示前一位是否为数字 n 对应数位上的数值，即前一位是否达到上界
        // is_num 表示前一位是否填了数字，可避免计入前导零
        function<int(int, int, bool, bool)> dfs = [&](int pos, int mask, bool is_limit, bool is_num) -> int
        {
            if(dp[pos][mask][is_limit][is_num]) return dp[pos][mask][is_limit][is_num];
            if(pos == len) // 填完全部位数，特判是否全空
                return is_num;

            int res = 0;
            if(!is_num) // 仍然不填
                res += dfs(pos + 1, mask, false, false);
		   
            // 当前位上界 up 取决于上一位是否达到上界
            int up = is_limit ? str[pos] - '0' : 9;
            
            // 若前一位填过数字，则当前数位范围为 [0, up] 否则为 [1, up]
            for(int x = !is_num; x <= up; ++x) 
                if(((mask >> x) & 1) == 0) // 前面没有填过数字 x（判断整数特殊性）
                    res += dfs(pos + 1, mask | (1 << x), is_limit && x == up, true);
            return (dp[pos][mask][is_limit][is_num] = res);
        };
        // 从数字的高位到低位搜索
        return dfs(0, 0, true, false);
    }
};
```



### **例题**



```c++
1.ABC 336 E
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
#define endl '\n'
#define gcd(a, b) __gcd(a, b)
#define mk(a, b) make_pair((a), (b))
#define lowbit(x) ((x) & -(x))
const int N = 2e5 + 10, inf = 1e9;
const ll linf = 4e18;

ll dp[20][150][150][2][2], a[20];

void Solve()
{
    ll n;
    cin >> n;

    int len = 0;
    while(n)
    {
        a[++len] = n % 10;
        n /= 10;
    }

    // cur 为当前数字 % mod 后的值，sum 为其数位和
    function<ll(int, int, int, int, bool, bool)> dfs = [&](int pos, int cur, int sum, int mod, bool islimit, bool isnum) -> ll
    {
        if(pos == 0)
            return cur == 0 && sum == mod && isnum;

        if(~dp[pos][cur][sum][islimit][isnum]) return dp[pos][cur][sum][islimit][isnum];

        ll res = 0;
        if(!isnum)
            res += dfs(pos - 1, cur, sum, mod, false, false);

        int up = islimit ? a[pos] : 9;
        for(int x = !isnum; x <= up; ++x)
            res += dfs(pos - 1, (cur * 10LL + x) % mod, sum + x, mod, islimit && x == up, true);
        return (dp[pos][cur][sum][islimit][isnum] = res);
    };

    ll ans = 0;
    for(int i = 1; i <= 126; ++i)
    {
        memset(dp, -1, sizeof dp);
        ans += dfs(len, 0, 0, i, true, false);
    }
    cout << ans << endl;
}

int main()
{
    untie();
    int T = 1;
    // cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}

```





---

## 概率dp

### 概念

**概率运算**

加法：`P(A∪B) = P(A) + P(B) - P(A ∩ B)`，若两事件**互斥**，则有 `P(A∪B) = P(A) + P(B)`

乘法：`P(A ∩ B) = P(A) * P(B)`

**数学期望**

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

**概率DP与期望DP的区别**

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

使用模板如下

```c++
#include<bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 1e2 + 10;
const double eps = 1e-6; // 视题目数据调整
double a[N][N]; // 增广矩阵
double x[N];    // 解向量

//高斯消元解方程组(n 行，m + 1 列)
//返回值 -1 表示无解，0 表示有唯一解，大于 0 表示有无穷解，返回自由变元个数
int Gauss(int n, int m)
{
    int c = 0;
    int r;
    for(r = 0; r < n && c < m; r++, c++)
    {
        int maxr = r;
        for(int i = r + 1; i < n; i++)
            if(abs(a[i][c]) > abs(a[maxr][c]))
                maxr = i;
        if(maxr != r) swap(a[r], a[maxr]);
        if(fabs(a[r][c]) < eps)
        {
            r--;
            continue;
        }
        for(int i = r + 1; i < n; i++)
        {
            if(fabs(a[i][c]) > eps)
            {
                double t = a[i][c] / a[r][c]; // 误差来源
                for(int j = c; j < m + 1; j++) a[i][j] -= a[r][j] * t;
                a[i][c] = 0;
            }
        }
    } 
    for(int i = r; i < m; i++)
    	if(fabs(a[i][c]) > eps) 
            return -1;          //无解
    if(r < m) return m - r;     //无穷个解，返回自由元个数
    for(int i = m - 1; i >= 0; i--){
        for(int j = i + 1; j < m; j++) a[i][m] -= a[i][j] * x[j];
        x[i] = a[i][m] / a[i][i];
    }
    return 0;                   //有唯一解
}

int main()
{
    untie();
    int n;
    cin >> n;
    for(int i = 0; i < n; ++i)
        for(int j = 0; j <= n; ++j) // 注意 [0, n] 列
            cin >> a[i][j];
    if(Gauss(n, n) != 0)            // 不存在唯一解
    {
        cout << "No Solution\n";
        return 0;
    }
    for(int i = 0; i < n; ++i) cout << fixed << setprecision(2) << x[i] << "\n";
    return 0;
}
```



### 理解入门

以 [Kids and Prizes](https://codeforces.com/problemsets/acmsguru/problem/99999/495) 为例：

题意：n 个盒子里面装着奖品，m 个人随机选择盒子（对每个盒子的选择是等概率的 1 / n），选择后只取出奖品并放回空盒子（总盒子数不变），问得到的奖品数量的数学期望值。

* 定义 `dp[i]` 表示第 i 个人中奖的概率，思路是先通过概率DP求出每个人中奖的概率（即所得奖品数最多为 m 个）再统计数学期望，且每个人最多得到 1 个奖品，故 `E(X) = ∑(1 * dp[i])` 

* 第 i 个人中奖的状态来源：

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





### 例题

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



### 题解

~~~c++
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
~~~



























---

## DP优化

### 四边形不等式优化





#### 针对一维线性dp







#### 针对一维区间dp（二维dp）







---



# 五、搜索

## 搜索

### DFS

#### 记忆化搜索

```c++
1. P1464 Function
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 21;
ll a, b, c;
ll f[N][N][N];
ll w(ll a, ll b, ll c)
{
    if(a <= 0 || b <= 0 || c <= 0) return 1;
    else if(a > 20 || b > 20 || c > 20) return w(20, 20, 20);
    if(f[a][b][c]) return f[a][b][c];
    if(a < b && b < c) f[a][b][c] = w(a, b, c - 1) + w(a, b - 1, c - 1) - w(a, b - 1, c);
    else f[a][b][c] = w(a - 1, b, c) + w(a - 1, b - 1, c) + w(a - 1, b, c - 1) - w(a - 1, b - 1, c - 1);
    return f[a][b][c];
}
int main()
{
    while(~scanf("%lld%lld%lld", &a, &b, &c))    
    {
        if(a == -1 && b == -1 && c == -1) break;
        printf("w(%lld, %lld, %lld) = %lld\n", a, b, c, w(a, b, c));
    }
    return 0;
}



2. 计蒜客-T1408 矩形嵌套
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 1e3 + 10;
int n, ans = 0;
int mat[N][2], dp[N];
vector<int> G[N];

int dfs(int u)
{
    if(dp[u]) return dp[u];
    dp[u] = 1;
    for(int v : G[u]) dp[u] = max(dp[u], dfs(v) + 1);
    return dp[u];
}

int main()
{
    untie();
    int T; cin >> T;
    while(T--)
    {
        cin >> n;
        
        ans = 0;
        for(int i = 0; i < n; ++i) G[i].clear();
        memset(dp, 0, sizeof(dp));

        for(int i = 0; i < n; ++i) 
        {
            cin >> mat[i][0] >> mat[i][1];
            if(mat[i][0] > mat[i][1]) swap(mat[i][0], mat[i][1]);
        }
        for(int i = 0; i < n; ++i)
            for(int j = 0; j < n; ++j)
                if(mat[i][0] < mat[j][0] && mat[i][1] < mat[j][1])
                    G[i].push_back(j);
        for(int i = 0; i < n; ++i)
        {
            if(!dp[i]) dfs(i);
            ans = max(ans, dp[i]);
        }
        cout << ans << "\n";
    }
    return 0;
}



3. P1434 滑雪
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 110;
int n, m, dp[N][N];
int a[N][N];
int dir[5] = {-1, 0, 1, 0, -1};

int dfs(int x, int y)
{
    if(dp[x][y]) return dp[x][y];
    dp[x][y] = 1;
    for(int i = 0; i < 4; ++i)
    {
        int nx = x + dir[i], ny = y + dir[i + 1];
        if(nx >= 0 && nx < n && ny >= 0 && ny < m && a[nx][ny] < a[x][y])
        {
            dp[x][y] = max(dp[x][y], dfs(nx, ny) + 1);
        }
    }
    return dp[x][y];
}

int main()
{
    untie();
    cin >> n >> m;
    for(int i = 0; i < n; ++i)
        for(int j = 0; j < m; ++j)
            cin >> a[i][j];
    
    int ans = 0;
    for(int i = 0; i < n; ++i)
        for(int j = 0; j < m; ++j)
            dfs(i, j), ans = max(ans, dp[i][j]);
    cout << ans << "\n";
    return 0;
}



4. P3183 食物链
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 1e5 + 10;
int n, m, dp[N];
int deg[N];
vector<int> G[N];
bool vis[N];

int dfs(int u)
{
    vis[u] = 1;
    if(G[u].empty()) dp[u] = 1;
    if(dp[u]) return dp[u];
    for(int v : G[u])
    {
        dp[u] += dfs(v);
    }
    return dp[u];
}

int main()
{
    untie();
    cin >> n >> m;
    for(int i = 0; i < m; ++i)
    {
        int u, v; cin >> u >> v;
        G[u].push_back(v);
        ++deg[u];
        ++deg[v];
    }
    int ans = 0;
    for(int i = 0; i < n; ++i)
    {
        if(!vis[i]) ans += dfs(i);
        if(!deg[i]) --ans;
    }
    cout << ans << "\n";
    return 0;
}



5. P1025 数的划分
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
int dp[220][10][220];
int N, K;
int dfs(int n, int k, int last = 1)
{
    if(dp[n][k][last]) return dp[n][k][last];
    if(k == 1) return dp[n][k][last] = 1;
    for(int i = last; i <= n / k; ++i) // 最大的情况是剩余的数平分，才能保证不重复
        dp[n][k][last] += dfs(n - i, k - 1, i);
    return dp[n][k][last];
}

int main()
{
    untie();
    cin >> N >> K;
    cout << dfs(N, K) << "\n";
    return 0;
}



6. P2476 着色方案
// 定义 dp[c1][c2][c3][c4][c5][last] 表示当还能够涂 i 个格子的油漆还有 Ci 种，且上次涂的是还能涂 last 个格子的油漆的方案数
// 若 Ci != 0，则使用一次该油漆，故 Ci - 1，它的使用次数减少则有 Ci-1 + 1，即少一次的使用次数的油漆数增加
#include <cstdio>
#include <iostream>
#include <algorithm>
#include <vector>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 16, mod = 1e9 + 7;
int k, mp[6];
ll dp[N][N][N][N][N][6]; 

ll dfs(int c1, int c2, int c3, int c4, int c5, int last)
{
    if(dp[c1][c2][c3][c4][c5][last]) return dp[c1][c2][c3][c4][c5][last];
    ll res = 0;
    if(c1) res = (res + (c1 - (last == 2)) * dfs(c1 - 1, c2, c3, c4, c5, 1)) % mod; // 若上一次是次数为 2 的油漆，说明 c1 中含有与当前相邻位置颜色相同的油漆，需减去
    if(c2) res = (res + (c2 - (last == 3)) * dfs(c1 + 1, c2 - 1, c3, c4, c5, 2)) % mod;
    if(c3) res = (res + (c3 - (last == 4)) * dfs(c1, c2 + 1, c3 - 1, c4, c5, 3)) % mod;
    if(c4) res = (res + (c4 - (last == 5)) * dfs(c1, c2, c3 + 1, c4 - 1, c5, 4)) % mod;
    if(c5) res = (res + c5 * dfs(c1, c2, c3, c4 + 1, c5 - 1, 5)) % mod;
    return dp[c1][c2][c3][c4][c5][last] = res;
}

int main()
{
    untie();
    for(int i = 1; i <= 5; ++i) dp[0][0][0][0][0][i] = 1; // 无论上次取哪种油漆，只要使得所有 Ci = 0，那么他有且仅有一个方案
    cin >> k;
    for(int x; k; --k)
    {
        cin >> x;
        mp[x]++;
    }
    cout << dfs(mp[1], mp[2], mp[3], mp[4], mp[5], 0) << "\n";
    return 0;
}





```





### BFS



```c++
1. POJ 2110 Mountain Walking
// 二分答案 + 搜索 + 区间枚举
#include <iostream>
#include <algorithm>
#include <queue>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 110;
int n, minx = 1e9, maxx = 0;
int a[N][N];
int dir[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
bool vis[N][N];

bool bfs(int L, int R)
{
    if(a[1][1] < L || a[1][1] > R) return 0; // 少了这句会 WA
    memset(vis, 0, sizeof vis);
    queue<pair<int, int> > q;
    q.push(make_pair(1, 1));
    vis[1][1] = 1;
    while(!q.empty())
    {
        pair<int, int> p = q.front(); q.pop();
        int x = p.first, y = p.second;
        if(x == n && y == n) return 1;
        for(int i = 0; i < 4; ++i)
        {
            int nx = x + dir[i][0], ny = y + dir[i][1];
            if(nx >= 1 && ny >= 1 && nx <= n && ny <= n && !vis[nx][ny] && a[nx][ny] >= L && a[nx][ny] <= R)
            {
                vis[nx][ny] = 1;
                q.push(make_pair(nx, ny));
            }
        }
    }
    return 0;
}

bool check(int x)
{
    for(int i = 0; i + x <= N; ++i) // 对每个差值 x，枚举整个数据范围内所有[L, R]区间验证正确性
        if(bfs(i, i + x)) return 1;
    return 0;
}

int main()
{
    untie();
    cin >> n;
    for(int i = 1; i <= n; i++)
        for(int j = 1; j <= n; j++)
            cin >> a[i][j];
    int l = 0, r = N;
    while(l < r)
    {
        int mid = l + r >> 1;
        if(check(mid)) r = mid;
        else l = mid + 1;
    }
    cout << r << '\n';
    return 0;
}
```



















---

# 六、数论

---

## 基础知识

### 整除

定义：设 a, b ∈ Z，a ≠ 0，则可将 b 可以被 a 整除记作 a | b。



```c++
ABC341 D
求[1, X]中能被 N 所整除的正整数个数 -> X / N
那么能恰好被 N 和 M 其一所整除的数有 (X / N) + (X / M) - (X / lcm(N, M)) * 2 个
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define bll __int128_t
#define all(v) v.begin(), v.end()
#define endl '\n'
#define gcd(a, b) __gcd(a, b)
#define mk(a, b) make_pair((a), (b))
#define lowbit(x) ((x) & -(x))
const int N = 2e5 + 10, inf = 1e9;
const ll linf = 1e18;
const bll mod = (bll)1e31 + 7177;

void Solve()
{
    ll n, m, k;
    cin >> n >> m >> k;

    ll L = n * m / gcd(n, m);
    ll l = 0, r = 9LL * linf;
    while(l + 1 < r)
    {
        ll mid = l - (l - r) / 2;
        ll cur = mid / n + mid / m - 2 * (mid / L);
        if(cur < k) l = mid;
        else r = mid;
    }
    cout << r << endl;
}

int main()
{
    untie();
    int T = 1;
    // cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```



---

## 裴蜀定理

设 a, b 是不全为 0 的整数，对任意整数 x, y，满足 gcd(a, b) | ax + by，则存在整数 x, y，使得 `ax + by = gcd(a, b)`成立。



---

## 拓展欧几里得

```c++
#define ll long long
ll exgcd(ll a, ll b, ll &x, ll &y)
{
    /*
        ax + by = gcd(a, b)                   => 特解 x0, y0
        aX + bY = k  	    ，k % g == 0      => 特解 X0 = x0 * k / g, Y0 = y0 * k / g
        									  通解  X = X0 + (b / g) * t
        									        Y = Y0 - (a / g) * t
        									  t 为任意整数（可正可负）
        返回 gcd(a, b)，x = x0，y = y0
        注意依据题意转化为 aX + bY = k 的通解
    */
    if(!b)
    {
        x = 1;
        y = 0;
        return a;
    }
    ll g = exgcd(b, a % b, y, x);
    y -= a / b * x;
    return g;
}
```



---

## 线性代数



### 高斯消元法（Gauss）

思路：将增广矩阵中的系数矩阵化为**上三角矩阵**。

优点：

* 可以判断**无解**情况：某一行前 n 个数均为 0，第 n + 1 个数不为 0。

* 可以判断**无穷个解**情况：某一行所有 n + 1 个数均为 0。

* 亦可以判断**唯一解**情况，即不发生上述两种情况时得到的是唯一解。

  

缺点：

* 需要回代，实现起来较繁琐。

模板：

```c++
#include<bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 1e2 + 10;
const double eps = 1e-6; // 视题目数据调整
double a[N][N]; // 增广矩阵
double x[N];    // 解向量

//高斯消元解方程组(n 行，m + 1 列)
//返回值 -1 表示无解，0 表示有唯一解，大于 0 表示有无穷解，返回自由变元个数
int Gauss(int n, int m)
{
    int c = 0, r = 0;
    for(r = 0; r < n && c < m; r++, c++)
    {
        int maxr = r;
        for(int i = r + 1; i < n; i++)
            if(abs(a[i][c]) > abs(a[maxr][c]))
                maxr = i;
        if(maxr != r) swap(a[r], a[maxr]);
        if(fabs(a[r][c]) < eps)
        {
            r--;
            continue;
        }
        for(int i = r + 1; i < n; i++)
        {
            if(fabs(a[i][c]) > eps)
            {
                double t = a[i][c] / a[r][c]; // 误差来源
                for(int j = c; j < m + 1; j++) a[i][j] -= a[r][j] * t;
                a[i][c] = 0;
            }
        }
    } 
    for(int i = r; i < m; i++)
    	if(fabs(a[i][c]) > eps) 
            return -1;          //无解
    if(r < m) return m - r;     //无穷个解，返回自由元个数
    for(int i = m - 1; i >= 0; i--) // 依次回代求解向量
    {
        for(int j = i + 1; j < m; j++) a[i][m] -= a[i][j] * x[j];
        x[i] = a[i][m] / a[i][i];
    }
    return 0;                   //有唯一解
}

int main()
{
    untie();
    int n;
    cin >> n;
    for(int i = 0; i < n; ++i)
        for(int j = 0; j <= n; ++j) // 注意 [0, n] 列
            cin >> a[i][j];
    if(Gauss(n, n) != 0)            // 不存在唯一解
    {
        cout << "No Solution\n";
        return 0;
    }
    for(int i = 0; i < n; ++i) cout << fixed << setprecision(2) << x[i] << "\n";
    return 0;
}
```



### 高斯-约旦消元法（Gauss-Jordan）

思路：将增广矩阵中的系数矩阵化为**单位矩阵**。

步骤：

1. 选择一个尚未被选过的未知数作为主元，选择一个包含这个主元的方程。

2. 将这个方程主元的系数化为1。

3. 通过加减消元，消掉其它方程中的这个未知数。

4. 重复以上步骤，直到把每一行都变成只有一项有系数。

优点：

* 无需回代，实现简单
* 可以求**逆矩阵**

缺点：

* 只能判断是否存在唯一解，无法具体判断无解或无穷个解的情况。
* 要求系数矩阵一定是个**方阵**

模板：

```c++
#include<bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 1e2 + 10;
const double eps = 1e-6;
double a[N][N]; // 增广矩阵

//高斯-约旦消元解方程组(n 行，n + 1 列)
//返回值 0 表示无解或有无穷个解，1 表示有唯一解
//结果将存在增广矩阵的最后一列 j == n 中
int Gauss_Jordan(int n)
{
    for(int i = 0; i < n; ++i)            // 枚举列（变元）
    {
        int maxr = i;                     // 寻找第 c 列的最大项
        for(int j = i + 1; j < n; j++)
        	if(fabs(a[j][i]) > fabs(a[maxr][i])) 
                maxr = j;
        if(maxr != i) swap(a[i], a[maxr]);
        if(fabs(a[i][i]) < eps) return 0; // 该列全为 0，故无解或有无穷个解（即没有唯一解）
        double tp = a[i][i];              // 主元
        for(int j = 0; j < n; j++)        // 枚举所有行（除当前行）
        {
            if(j == i) continue;
            double t = a[j][i] / a[i][i]; // 第 i 行主元化 1 （1 / a[i][i]） ，第 j 行的该列元素化 0
            for(int k = 0; k < n + 1; k++) a[j][k] -= a[i][k] * t;
        }
    }
    // 化系数矩阵为单位矩阵，使得 a[i][n] 即为解向量值
    for(int i = 0; i < n; ++i) a[i][n] /= a[i][i];
    return 1;
}

int main()
{
    untie();
    int n;
    cin >> n;
    for(int i = 0; i < n; ++i)
        for(int j = 0; j <= n; ++j)
            cin >> a[i][j];
    if(Gauss_Jordan(n) == 0)
    {
        cout << "No Solution\n";
        return 0;
    }
    for(int i = 0; i < n; ++i) cout << fixed << setprecision(2) << a[i][n] << "\n";
    return 0;
}
```



#### 矩阵求逆

设原矩阵为 A，构造分块矩阵 (A | E) ~ (E | B)，B 即为 A 的逆矩阵。

```c++
#include<bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 410;
const ll mod = 1e9 + 7;
const double eps = 1e-6;
ll a[N][N << 1]; // 增广矩阵

ll quick_pow(ll base, ll index = mod - 2)
{
    ll ans = 1;
    base %= mod;
    for(; index; index >>= 1)
    {
        if(index & 1) ans = ans * base % mod;
        base = base * base % mod;
    }
    return ans % mod;
}

int Gauss_Jordan(int n)
{
    for(int i = 0; i < n; ++i)            // 枚举列（变元）
    {
        int maxr = i;                     // 寻找第 c 列的最大项
        for(int j = i + 1; j < n; j++)
        	if(a[j][i] > a[maxr][i]) 
                maxr = j;
        if(maxr != i) swap(a[i], a[maxr]);
        if(!a[i][i]) return 0;            // 该列全为 0，故无解或有无穷个解（即没有唯一解）
        ll tp = quick_pow(a[i][i]);       // 主元
        for(int j = 0; j < n; j++)        // 枚举所有行（除当前行）
        {
            if(j == i) continue;
            ll t = a[j][i] * tp % mod;    // 第 i 行化 1 （1 / a[i][i]） ，第 j 行的该列元素化 0
            for(int k = 0; k < (n << 1); k++) a[j][k] = (a[j][k] - a[i][k] * t % mod + mod) % mod;
        }
    }
    for(int i = 0; i < n; ++i) 			 // 化左矩阵为单位矩阵
    {
        ll tp = quick_pow(a[i][i]);
        for(int j = n; j < (n << 1); ++j)
            a[i][j] = a[i][j] * tp % mod;
    }
    return 1;
}

int main()
{
    untie();
    int n;
    cin >> n;
    for(int i = 0; i < n; ++i)
        for(int j = 0; j < n; ++j)
            cin >> a[i][j];
    for(int i = 0; i < n; ++i) a[i][n + i] = 1;
    if(Gauss_Jordan(n) == 0)
    {
        cout << "No Solution\n";
        return 0;
    }
    for(int i = 0; i < n; ++i)
        for(int j = n; j < (n << 1); ++j)
            cout << a[i][j] << " \n"[j == (n << 1) - 1];
    return 0;
}
```



### 线性基











---

# . 模板总结



* 链式前向星

  ```c++
  const int M = 1e6 + 10, N = 2 * M;
  int n, m, cnt = 1;
  int head[N];
  template <class T = int> // 创建数组仍需要指示类型 Edge<int> e[M];
  class Edge
  {
      public:
          int to, next;
          T w;
          Edge(){}
          Edge(int a, int b, T c) : to(a), next(b), w(c){}
          friend void addedge_undirected(Edge e[], int u, int v, T c = 0){ addedge(e, u, v, c), addedge(e, v, u, c);}
          friend void addedge(Edge e[], int u, int v, T c = 0)
          {
              e[cnt] = Edge(v, head[u], c);
              head[u] = cnt++;
          }
  };
  ```

  



































---



类的学习

```c++
1.Point(类与构造)
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
class Point{
    public:
        Point(){ x = 0, y = 0;}
        Point(double a, double b)
        {
            x = a, y = b;
        }
        double getX()
        {
            return x;
        }
        double getY()
        {
            return y;
        }
        void setX(double xx)
        {
            x = xx;
        }
        void setY(double yy)
        {
            y = yy;
        }
        double distanceToAnotherPoint(Point P)
        {
            double xx = P.x - x, yy = P.y - y;
            return sqrt(xx * xx + yy * yy);
        }
    private:
        double x, y;
};
int t;
int main()
{
    untie();
    cin >> t;
    while(t--)
    {
        Point p1, p2;
        double x1, y1, x2, y2;
        cin >> x1 >> y1 >> x2 >> y2;
        p1.setX(x1);
        p1.setY(y1);
        p2.setX(x2);
        p2.setY(y2);
        cout << fixed << setprecision(2) << "Distance of Point(" << p1.getX() << "," << p1.getY() << ") to Point(" << p2.getX() << "," << p2.getY() << ") is " << p1.distanceToAnotherPoint(p2) << '\n';
    }
    return 0;
}



2.Date(类与构造)
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
int mon[15] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
bool judge(int y)
{
    return y % 400 == 0 || (y % 4 == 0 && y % 100 != 0);
}
class Date{
    public:
  		Date(){year = 1900, month = 1, day = 1;}
        Date(int y, int m, int d)
        {
            year = y, month = m, day = d;
        }
        int getYear()
        {
            return year;
        }
        int getMonth()
        {
            return month;
        }
        int getDay()
        {
            return day;
        }
        void setDate(int y, int m, int d)
        {
            year = y, month = m, day = d;
        }
        void print()
        {
            cout << year << '/';
            if(month < 10) cout << "0";
            cout << month << '/';
            if(day < 10) cout << "0";
            cout << day << '\n';
        }
        void addOneDay()
        {
            int d = day + 1, m = month, y = year;
            if(d > mon[month] + (month == 2 ? judge(y) : 0)) ++m, d = 1;
            if(m > 12) ++y, m = 1;
            year = y, month = m, day = d;
        }
    private:
        int year, month, day;
};
int t;
int main()
{
    untie();
    cin >> t;
    for(int i = 1; i <= t; ++i)
    {
        Date now = Date();
        int y, m, d;
        cin >> y >> m >> d;
        if(i & 1) now = Date(y, m, d);
        else now.setDate(y, m, d); 
        cout << "Today is ";
        now.print();
        now.addOneDay();
        cout << "Tomorrow is ";
        now.print();
    }
    return 0;
}



3.分数类（类与构造）
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
class CFraction{
    public:
        CFraction(){ fz = 0, fm = 1;}
        CFraction(int fz_val, int fm_val)
        {
            fz = fz_val, fm = fm_val;
        }
        CFraction add(const CFraction &r)
        {
            return CFraction(fz * r.fm + r.fz * fm, fm * r.fm);
        }
        CFraction sub(const CFraction &r)
        {
            return CFraction(fz * r.fm - r.fz * fm, fm * r.fm);
        }
        CFraction mul(const CFraction &r)
        {
            return CFraction(fz * r.fz, fm * r.fm);
        }
        CFraction div(const CFraction &r)
        {
            return CFraction(fz * r.fm, fm * r.fz);
        }
        int getGCD()
        {
            int r = fm;
            int a = fz > 0 ? fz : -fz;
            int b = fm > 0 ? fm : -fm;
            while (a % b != 0)
            {
                r = a % b;
                a = b;
                b = r;
            }
            return b;
        }
        void print()
        {
            int gcd = getGCD();
            int flag = -1;
            fz /= gcd, fm /= gcd;
            if((fz < 0 && fm < 0) || (fz > 0 && fm > 0)) flag = 1;
            if(flag > 0) cout << fz << '/' << fm << '\n';
            else if(fz < 0) cout << fz << '/' << fm << '\n';
            else cout << '-' << fz << '/' << (-fm) << '\n';
        }
    private:
        int fz, fm;
};
int t;
int main()
{
    untie();
    cin >> t;
    while(t--)
    {
        char ch;
        int fz, fm;
        cin >> fz >> ch >> fm;
        CFraction c1(fz, fm);
        cin >> fz >> ch >> fm;
        CFraction c2(fz, fm);
        CFraction c3;
        c3 = c1.add(c2);
        c3.print();
        c3 = c1.sub(c2);
        c3.print();
        c3 = c1.mul(c2);
        c3.print();
        c3 = c1.div(c2);
        c3.print();
        cout << '\n';
    }
    return 0;
}



4.Point_Array(类+构造+对象数组)
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
class Point{
    public:
        Point()
        { 
            x = 0;
            y = 0;
            cout << "Constructor.\n";
        }
        Point(double xx, double yy)
        {
            x = xx, y = yy;
        }
        ~Point()
        {
            cout << "Distructor.\n";
        }
        double getX()
        {
            return x;
        }
        double getY()
        {
            return y;
        }
        void setXY(double x1, double y1)
        {
            x = x1, y = y1;
        }
        void setX(double xx)
        {
            x = xx;
        }
        void setY(double yy)
        {
            y = yy;
        }
        double getDisTo(Point &p)
        {
            double xx = x - p.getX(), yy = y - p.getY();
            return sqrt(xx * xx + yy * yy);
        }
    private:
        double x, y;
};
int t, n;
int main()
{
    untie();
    cin >> t;
    while(t--)
    {
        cin >> n;
        Point *p = new Point[n];
        for(int i = 0; i < n; ++i)
        {
            double x, y;
            cin >> x >> y;
            p[i].setXY(x, y);
        }
        int ii = 0, jj = 0;
        double dis = 0;
        for(int i = 0; i < n; ++i)
        {
            for(int j = 0; j < n; ++j)
            {
                if(i == j) continue;
                double now = p[i].getDisTo(p[j]);
                if(dis < now)
                {
                    ii = i;
                    jj = j;
                    dis = now;
                }
            }
        }
        if(ii > jj) swap(ii, jj);
        cout << fixed << setprecision(2) << "The longeset distance is " << dis << ",between p[" << ii << "] and p[" << jj << "].\n";
        delete[] p;
    }
    return 0;
}



5.Point_Array(类+构造+对象数组)
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
class CStack{
    public:
        CStack()
        {
            a = new int[10];
            top = 0;
            size = 10;
            cout << "Constructor.\n";
        }
        CStack(int sz)
        {
            a = new int[sz];
            top = 0;
            size = sz;
            cout << "Constructor.\n";
        }
        ~CStack()
        {
            delete[] a;
            cout << "Destructor.\n";
        }
        int get(int index)
        {
            return a[index];
        }
        void push(int n)
        {
            a[top++] = n;
        }
        int isEmpty()
        {
            return top == 0;
        }
        int isFull()
        {
            return top == size;
        }
        int pop()
        {
            return a[--top];
        }
    private:
        int *a;
        int size;
        int top;
};
int t, n;
int main()
{
    untie();
    cin >> t;
    while(t--)
    {
        cin >> n;
        CStack sta(n);
        for(int i = 0, x; i < n; ++i)
        {
            cin >> x;
            sta.push(x);
        }
        while(!sta.isEmpty())
        {
            cout << sta.pop();
            if(!sta.isEmpty()) cout << ' ';
        }
        cout << '\n';
    }
    return 0;
}



6.Equation(类与对象+构造)
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Equation{
    public:
        Equation(){ a = 1.0, b = 1.0, c = 0;}
        void set(double aa, double bb, double cc)
        {
            a = aa, b = bb, c = cc;
        }
        void getRoot()
        {
            double del = b * b - 4 * a * c;
            if(del > 0)
            {
                double x1 = (-b + sqrt(del)) / (2 * a), x2 = (-b - sqrt(del)) / (2 * a);
                cout <<fixed<<setprecision(2)<< "x1=" << x1 << " x2=" << x2 <<'\n';
            }
            else if(del == 0)
            {
                double x = (-b + sqrt(del)) / (2 * a);
                cout <<fixed<<setprecision(2)<< "x1=x2=" << x << '\n';
            }
            else
            {
                double x = -b / (2 * a), y1 = sqrt(-del) / (2 * a), y2 = -sqrt(-del) / (2 * a);
                cout <<fixed<<setprecision(2)<< "x1=" << x << "+" << y1 << "i " << "x2=" << x << y2 << "i\n";
            }
        }
    private:
        double a, b, c;
};

int t;
int main()
{
    untie();
    cin >> t;
    for(int i = 1; i <= t; ++i)
    {
        double a, b, c;
        cin >> a >> b >> c;
        Equation e;
        e.set(a, b, c);
        e.getRoot();
    }
    return 0;
}



7.对象是怎样构造的(拷贝构造函数)

#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class X{
    public:
        X(){val = 0;}
        X(int x)
        {
		    cout << "Constructed using one argument constructor, value = " << x << '\n';
            val = x;
        }
    private:
        int val;
};

int t;
int main()
{
    untie();
    cin >> t;
    for(int i = 1; i <= t; ++i)
    {
        int op;
        cin >> op;
        if(op == 0)
        {
            X a();
			cout << "Constructed by default, value = 0\n";
        }
        else if(op == 1)
        {
            int x;
            cin >> x;
            X a(x);
        }
        else
        {
            int x;
            cin >> x;
            X a(x);
            X b = a;
			cout << "Constructed using copy constructor, value = " << x << '\n';
        }
    }
    return 0;
}



8.电话号码升位(拷贝构造函数)
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CTelNumber{
    public:
       CTelNumber()
       {
            num = new char[15];
       }
       CTelNumber(char *ne)
       {
            num = new char[15];
            strcpy(num, ne);
       }
       CTelNumber(CTelNumber &ne)
       {
            num = new char[15];
            if(ne.num[0] >= '2' && ne.num[0] <= '4') num[0] = '8';
            else num[0] = '2';
            for(int i = 0; i < 7; ++i) num[i + 1] = ne.num[i];
       }
       ~CTelNumber()
       {
            free(num);
       }
       void Print()
       {
            for(int i = 0; i < 8; ++i)
            {
                cout << num[i];
            }
            cout << '\n';
       }
    private:
        char *num;
};
bool check(char *s)
{
    int len = strlen(s);
    if(len != 7) return 0;
    if(s[0] <= '1' || s[0] >= '9') return 0;
    for(int i = 0; i < len; ++i)
        if(!(s[i] >= '0' && s[i] <= '9'))
            return 0;
    return 1;
}
int t;
int main()
{
    untie();
    cin >> t;
    for(int i = 1; i <= t; ++i)
    {
        char s[15];
        cin >> s;
        if(!check(s))
        {
            cout << "Illegal phone number\n";
            continue;
        }
        CTelNumber m(s);
        CTelNumber n(m);
        n.Print();
    }
    return 0;
}



9.软件备份(拷贝构造函数)
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class software{
    public:
        software(char *nm, char tp, char med)
        {
            name = new char[strlen(nm) + 1];
            type = tp;
            media = med;
            strcpy(name, nm);
        }
        software(const software &s)
        {
            name = new char[strlen(s.name) + 1];
            strcpy(name, s.name);
            type = 'B';
            media = 'H';
        }
        ~software()
        {
            free(name);
        }
        void Print()
        {
            cout << "name:" << name << '\n';
            if (type == 'O') { cout << "type:original" << '\n'; }
            if (type == 'B') { cout << "type:backup" << '\n'; }
            if (type == 'T') { cout << "type:trial" << '\n'; }
            if (media == 'D') { cout << "media:optical disk" << '\n'; }
            if (media == 'H') { cout << "media:hard disk" << '\n'; }
            if (media == 'U') { cout << "media:USB disk" << '\n'; }
        }
    private:
        char *name;
        char type;//O B T
        char media;//D H U
};

class CDate
{
    public:
        CDate(){}
        CDate(int y, int m, int d);
        bool isLeapYear();
        int getYear();
        int getMonth();
        int getDay();
        int getDayofYear();
    private:
        int year, month, day;
};
CDate::CDate(int y, int m, int d)
{ 
    year = y, month = m,day = d;
}
bool CDate::isLeapYear()
{ 
    return (year % 4 == 0 && year % 100 != 0) || year % 400 == 0; 
}
int CDate::getYear()
{ 
    return year;
}
int CDate::getMonth() 
{ 
    return month;
}
int CDate::getDay() 
{ 
    return day;
}
int CDate::getDayofYear()
{
    int i, sum = day;
    int a[13] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
    if (isLeapYear())
    {
        a[2]++;
    }
    // 求日期的天数
    for (i = 0; i < month; i++)
    {
        sum += a[i];
    }
    return sum;
}

void check(int y, int m, int d)
{
    int tot, x;
    if(!y && !m && !d) cout << "this software has unlimited use\n";
    else if(y < 2015 || (y == 2015 && m < 4) || (y == 2015 && m == 4 && d < 8)) cout << "this software has expired\n";
    else
    {
        CDate now(y, m, d);
        x = now.getDayofYear();
        tot = x - 90 - 8;
        if(y > 2015) tot = x + 365 - 90 - 8;
        for(int i = 2016; i < y; ++i)
        {
            CDate tp(i, 12, 31);
            tot += tp.getDayofYear();
            break;
        }
        cout << "this software is going to be expired in " << tot << " days\n";
    }
    cout << '\n';
}

int t;
char name[50], type, media;
int main()
{
    untie();
    cin >> t;
    for(int i = 1; i <= t; ++i)
    {
        int year, month, day;
        cin >> name >> type >> media >> year >> month >> day;
        software now(name, type, media);
        now.Print();
        check(year, month, day);
        software ne(now);
        ne.Print();
        check(year, month, day);
    }
    return 0;
}



10.手机服务（构造+拷贝构造+堆）
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
class Date{
    public:
        Date(){}
        void set(int a, int b, int c) { year = a, month = b, day = c;}
        void Print()
        {
            cout << year << "." << month << "." << day << '\n';
        }
    private:
        int year, month, day;
};
class phone{
    public:
        phone(){ date = NULL;}
        phone(char tp, char *s, int sta)
        {
            strcpy(num, s);
            type = tp;
            status = sta;
            cout << "Construct a new phone " << num << '\n';
        }
        phone(const phone & ph)
        {
            strcpy(num, ph.num);
            num[11] = 'X', num[12] = '\0';
            type = 'D';
            status = ph.status;
            date = ph.date;
            cout << "Construct a copy of phone " << ph.num << '\n';
        }
        void Print()
        {
            cout << "类型=";
            if (type == 'A') cout << "机构";
            else if (type == 'B') cout << "企业";
            else if (type == 'C') cout << "个人";
            else if (type == 'D') cout << "备份";
            cout << "||号码=" << num << "||State=";
            if (status == 1) cout << "在用";
            else if (status == 2) cout << "未用";
            else if (status == 3) cout << "停用";
        }
        void stop(int y, int m, int d)
        {
            cout << "Stop the phone " << num << '\n';
            status = 3;
            date = new Date;
            date -> set(y, m, d);
            this -> Print();
            cout << "||停机日期=";
            date -> Print();
            delete date;
        }
    private:
        char type;//A B C D
        char num[15];
        int status;//1 2 3
        Date* date;
};

int t;
char type, num[15];
int sta;
int main()
{
    untie();
    cin >> t;
    for(int i = 1; i <= t; ++i)
    {
        int y, m, d;
        cin >> type >> num >> sta >> y >> m >> d;
        phone p1(type, num, sta);
        p1.Print();
        cout << '\n';
        phone p2(p1);
        p2.Print();
        cout << '\n';
        p1.stop(y, m, d);
        cout << "----\n";
    }
    return 0;
}



11.距离计算（友元函数）
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>

using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Point
{
    private:
        double x, y;
    public:
        Point(double xx, double yy)
        {
            x = xx, y = yy;
        }
        friend double Distance(Point &a, Point &b)
        {
            double xx = a.x - b.x, yy = a.y - b.y;
            return sqrt(xx * xx + yy * yy);
        }
};

int main()
{
    untie();
    int t;
    cin >> t;
    while(t--)
    {
        double x1, y1, x2, y2;
        cin >> x1 >> y1 >> x2 >> y2;
        Point p1(x1, y1), p2(x2, y2);
        cout << (int)Distance(p1, p2) << '\n';
    }
    return 0;
}



12.银行账户（静态成员与友元函数）
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>

using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Account
{
    private:
        static float count; // 账户总余额
        static float interestRate;
        string accno, accname;
        float balance;
    public:
        Account(string ac = "", string na = "", float ba = 0.0)
        {
            accno = ac, accname = na, balance = ba;
            count += ba;
        }
        ~Account(){}
        void deposit(float amount)
        {
            balance += amount;
            count += amount;
        }
        void withdraw(float amount)
        {
            balance -= amount;
            count -= amount;
        }
        float getBalance()
        {
            return balance;
        }
        void show()
        {
            cout << accno << " " << accname << " " << balance;
        }
        static float getCount()
        {
            return count;
        }
        static void setInterestRate(float rate)
        {
            interestRate = rate;
        }
        static float getInterestRate()
        {
            return interestRate;
        }
        friend void update(Account& a)
        {
            float tp = a.balance;
            a.balance += a.balance * Account::interestRate;
            count += a.balance - tp;
        }
};

float Account::count = 0;
float Account::interestRate = 0;

int main()
{
    untie();
    float rate, ans = 0;
    int n;
    cin >> rate >> n;
    Account **p = new Account*[n];
    Account::setInterestRate(rate);
    for(int i = 0 ; i < n ; ++i)
    {
        string acc, name;
        float bal, dep, witd;
        cin >> acc >> name >> bal >> dep >> witd;
        p[i] = new Account(acc, name, bal);
        p[i]->deposit(dep);
        p[i]->show();
        update(*p[i]);
        cout << " " << p[i]->getBalance();
        p[i]->withdraw(witd);
        cout << " " << p[i]->getBalance();
        cout << '\n';
        ans = p[i]->getCount();
        delete p[i];
    }
    cout << ans << '\n';
    delete[] p;
    return 0;
}



13.复数运算（友元函数）
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>

using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Complex
{
private:
	double real; // 实部
	double imag; // 虚部
public:
	Complex(){}
	Complex(double r, double i)
    {
        real = r, imag = i;
    }
	friend Complex addCom(const Complex& c1, const Complex& c2)
    {
        Complex t(c1.real + c2.real, c1.imag + c2.imag);
        return t;
    }
    friend Complex minusCom(const Complex& c1, const Complex& c2)
    {
        Complex t(c1.real - c2.real, c1.imag - c2.imag);
        return t;
    }
	// 友元函数，输出类对象c的有关数据(c为参数对象)
	friend void outCom(const Complex& c)
    {
        cout << "(" << c.real <<"," << c.imag << ")\n";
    }
    
};

int main()
{
    untie();
    double rl, ig;
    cin >> rl >> ig;
    Complex c(rl, ig);
    int n;
    cin >> n;
    while(n--)
    {
        char op[5];
        cin >> op >> rl >> ig;
        Complex ct(rl, ig);
        if(op[0] == '+') c = addCom(c, ct);
        else c = minusCom(c, ct);
        outCom(c);
    }
    return 0;
}

  
  
14.旅馆顾客统计（静态成员）
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>

using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Hotel
{
    private:
        static int totalCustNum; // 顾客总人数
        static float totalEarning; // 旅店总收入
        static float rent; // 每个顾客的房租
        char *customerName; // 顾客姓名
        int customerId; // 顾客编号
    public:
        Hotel(){}
        Hotel(char* customer)
        {
            Hotel::totalCustNum++;
            Hotel::totalEarning += rent;
            customerName = new char[strlen(customer) + 5];
            strcpy(customerName, customer);
        }
        ~Hotel()
        {
            delete customerName;
        }
        void Display()
        {
            printf("%s %04d %d %d\n", customerName, 20150000 + Hotel::totalCustNum, Hotel::totalCustNum, (int)totalEarning);
        }
        static void setRent(float rt)
        {
            Hotel::rent = rt;
        }
};

int Hotel::totalCustNum = 0;
float Hotel::totalEarning = 0;
float Hotel::rent = 0;

int main()
{
    untie();
    float rt;
    cin >> rt;
    Hotel::setRent(rt);
    char s[50];
    while(cin >> s)
    {
        if(s[0] == '0') break;
        Hotel h(s);
        h.Display();
    }
    return 0;
}



15.日期时间合并输出（友元函数）
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>

using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CDate;
class CTime
{
	private:
		int hour, min, sec;
	public:
		friend void Display(CDate &, CTime &);
		void input()
		{
			cin >> hour >> min >> sec;
		}
};
class CDate
{
	private:
		int year,month,day;
	public:
		friend void Display(CDate &,CTime &);
		void input()
		{
			cin >> year >> month >> day;
		}
};
void Display(CDate & date,CTime & time)
{
	cout << date.year << '-' << setfill('0') << setw(2) << date.month << '-' << setw(2) << date.day << ' ' << setw(2) << time.hour << ':' << setw(2) << time.min << ':' << setw(2) << time.sec << '\n';
}
int main()
{
    untie();
	int t;
	CDate date;
	CTime time;
	cin >> t;
	while(t--)
	{
		date.input();
		time.input();
		Display(date, time);
	}
    return 0;
}



16.向量1（类和对象）
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>

using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CVector{
    private:
        int *data;
        int n;
    public:
        CVector()
        {
            n = 5;
            data = new int[n];
            for(int i = 0; i < n; ++i) data[i] = i;
        }
        CVector(int n1, int *a)
        {
            n = n1;
            data = new int[n1];
            for(int i = 0; i < n1; ++i) data[i] = a[i];
        }
        ~CVector()
        {
            delete []data;
        }
        void Print()
        {
            for(int i = 0; i < n; ++i)
            {
                cout << data[i] << " \n"[i == n - 1];
            }
        }
};

int main()
{
    untie();
    int n, *a;
    cin >> n;
    a = new int[n];
    for(int i = 0; i < n; ++i) cin >> a[i];
    CVector c1 = CVector();
    CVector c2(n, a);
    c1.Print();
    c2.Print();
  	delete []a;
    return 0;
}



17.向量2（友元及拷贝构造）
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>

using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CVector{
    private:
        int *data;
        int n;
    public:
        CVector()
        {
            n = 5;
            data = new int[n];
            for(int i = 0; i < n; ++i) data[i] = i;
        }
        CVector(int n1, int *a)
        {
            n = n1;
            data = new int[n1];
            for(int i = 0; i < n1; ++i) data[i] = a[i];
        }
        CVector(const CVector &cv)
        {
            n = cv.n;
            data = new int[n];
            for(int i = 0; i < n; ++i) data[i] = cv.data[i];
        }
        ~CVector()
        {
            delete []data;
        }
        void Print()
        {
            for(int i = 0; i < n; ++i)
            {
                cout << data[i] << " \n"[i == n - 1];
            }
        }
        friend CVector add(const CVector v1, const CVector v2)
        {
            CVector res(v1);
            for(int i = 0; i < v2.n; ++i) res.data[i] += v2.data[i];
            return res;
        }
};

int main()
{
    untie();
    int t;
    cin >> t;
    while(t--)
    {
        int n, *a, *b;
        cin >> n;
        a = new int[n];
        b = new int[n];
        for(int i = 0; i < n; ++i) cin >> a[i];
        for(int i = 0; i < n; ++i) cin >> b[i];
        CVector c1(n, a);
        CVector c2(n, b);
        c1.Print();
        c2.Print();
        add(c1, c2).Print();
        delete []a;
        delete []b;
    }
    return 0;
}



18.向量3（静态成员）

#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>

using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CVector{
    private:
        int *data;
        int n;
        static int sum;
    public:
        CVector()
        {
            n = 5;
            data = new int[n];
            for(int i = 0; i < n; ++i) 
            {
                data[i] = i;
                sum += i;
            }
        }
        CVector(int n1, int *a)
        {
            n = n1;
            data = new int[n1];
            for(int i = 0; i < n1; ++i) 
            {
                data[i] = a[i];
                sum += a[i];
            }
        }
        ~CVector()
        {
            delete []data;
        }
        void Print()
        {
            for(int i = 0; i < n; ++i)
            {
                cout << data[i] << " \n"[i == n - 1];
            }
        }
        static void Print_sum()
        {
            cout << sum << '\n';
            sum = 0;
        }
};

int CVector::sum = 0;

int main()
{
    untie();
    int t;
    cin >> t;
    while(t--)
    {
        int cnt;
        cin >> cnt;
        while(cnt--)
        {
            int n, *a;
            cin >> n;
            a = new int[n];
            for(int i = 0; i < n; ++i) cin >> a[i];
            CVector c1(n, a);
            c1.Print();
            delete []a;
        }
        CVector::Print_sum();
    }
    return 0;
}



*19.向量4（类复合）

#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>

using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CVector{
    private:
        int *data;
        int n, sum;
    public:
        CVector()
        {
            n = 5;
            data = new int[n];
            for(int i = 0; i < n; ++i) 
            {
                data[i] = i;
                sum += i;
            }
        }
        CVector(int n1, int *a)
        {
            n = n1;
            data = new int[n1];
            sum = 0;
            for(int i = 0; i < n1; ++i) 
            {
                data[i] = a[i];
                sum += a[i];
            }
        }
        CVector(const CVector &cv)//拷贝构造函数
        {
            n = cv.n;
            data = new int[n];
            for(int i = 0; i < n; ++i) data[i] = cv.data[i];
        }
        ~CVector()
        {
            delete []data;
            data = NULL;
        }
        float Average()
        {
            return 1.0 * sum / n;
        }
        void Print()
        {
            for(int i = 0; i < n; ++i) cout << data[i] << " ";
        }
};

class CStudent{
    private:
        string name;
        CVector score;
    public:
        CStudent(){}
        CStudent(string name1, int n1, int *a1):score(n1, a1)//重点：这样写是深拷贝，防止浅拷贝造成指针Delete时报错
        {
            name = name1;
        }
        void Print()
        {
            cout << name << " ";
            score.Print();
            cout << fixed << setprecision(2)  << score.Average() << '\n';
        }
};

int main()
{
    untie();
    string s;
    while(cin >> s)
    {
        int n, *a;
        cin >> n;
        a = new int[n];
        for(int i = 0; i < n; ++i) cin >> a[i];
        CStudent stu(s, n, a);
        stu.Print();
        delete []a;
    }
    return 0;
}



20.向量5（友元类）
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <iomanip>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CVector{
    private:
        int *data;
        int n;
    public:
        friend class CMatrix;
        CVector()
        {
            n = 5;
            data = new int[n];
            for(int i = 0; i < n; ++i) 
            {
                data[i] = i;
            }
        }
        CVector(int n1, int *a = NULL)
            
        {
            n = n1;
            data = new int[n1];
            for(int i = 0; i < n1; ++i) 
            {
                data[i] = (a == NULL ? 0 : a[i]);
            }
        }
        CVector(const CVector &cv)
        {
            n = cv.n;
            data = new int[n];
            for(int i = 0; i < n; ++i) data[i] = cv.data[i];
        }
        ~CVector()
        {
            delete []data;
            data = NULL;
        }
        void Print()
        {
            for(int i = 0; i < n; ++i) cout << data[i] << " \n"[i == n - 1];
        }
};

class CMatrix{
    private:
        int n, **data;
    public:
        CMatrix(){}
        CMatrix(int nn, int **_data)
        {
            n = nn;
            data = new int* [nn];
            for(int i = 0; i < n; ++i) data[i] = new int[n];
            if(_data == NULL) return;
            for(int i = 0; i < n; ++i)
                for(int j = 0; j < n; ++j)
                    data[i][j] = _data[i][j];
        }
        ~CMatrix()
        {
            for(int i = 0; i < n; ++i) delete []data[i];
        }
        CVector multi(const CVector &v1)
        {
            CVector res(v1.n);
            for(int i = 0; i < v1.n; ++i)
                for(int j = 0; j < v1.n; ++j)
                    res.data[i] += v1.data[j] * data[i][j];
            return res;
        }
};

int main()
{
    untie();
    int t;
    cin >> t;
    while(t--)
    {
        int n, m, **data, *a;
        cin >> n;
        data = new int*[n];
        for(int i = 0; i < n; ++i) data[i] = new int[n];
        for(int i = 0; i < n; ++i)
            for(int j = 0; j < n; ++j)
                cin >> data[i][j];
        cin >> m;
        a = new int[m];
        for(int i = 0; i < m; ++i) cin >> a[i];
        if(n != m)
        {
            cout << "error\n";
        }
        else
        {
            CVector v1(m, a);
            CMatrix mat(n, data);
            mat.multi(v1).Print();//不创建临时变量而是直接使用，防止浅拷贝赋值写法带来Delete时的报错
        }
        for(int i = 0; i < n; ++i) delete []data[i];
        delete []a;
        a = NULL;
        data = NULL;
    }
    return 0;
}



21.机器人变身（类与对象）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Robot{
    private:
        char name[30], pat;
        int hp, attack, defense, level;
    public:
        Robot(){}
        Robot(char *nm, char tp, int lel) {  strcpy(name, nm), level = lel, pat = tp, hp = attack = defense = 5 * level;}
        void setPro(char ch)
        {
            pat = ch;
            switch(ch)
            {
                case 'A': attack = 10 * level; break;
                case 'D': defense = 10 * level; break;
                case 'H': hp = 50 * level; break;
            }
        }
        int getHp() { return hp;}
        int getAt() { return attack;}
        int getDf() { return defense;}
        char getPat() { return pat;}
        void Print()
        {
            cout << name << "--" << pat << "--" << level << "--" << hp << "--" << attack << "--" << defense << '\n';
        }
};

bool change(Robot *bot, char dest)
{
    bool res = true;
    if(bot->getPat() == dest) res = false;
    bot->setPro(dest);
    return res;
}

int main()
{
    untie();
    int T, cnt = 0;
    cin >> T;
    while(T--)
    {
        char nm[30], tp, dest;
        int lel;
        cin >> nm >> tp >> lel >> dest;
        Robot bot(nm, tp, lel);
        if(change(&bot, dest)) ++cnt;
        bot.Print();
    }
    cout << "The number of robot transform is " << cnt << '\n';
    return 0;
}



22.虚拟电话（构造与析构）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class PhoneNumber{
    private:
        int num;
        char pat;
    public:
        PhoneNumber(){}
        PhoneNumber(int n, char tp)
        {
            num = n, pat = tp;
            cout << num << " constructed.\n";
        }
        ~PhoneNumber()
        {
            cout << num << " destructed.\n";
        }
        int getNum(){ return num;}
        char getPat(){ return pat;}
};

class VirtualPhone{
    private:
        char name[30];
        int status;
        PhoneNumber pNum;
    public:
        VirtualPhone(){}
        VirtualPhone(int n, char tp, char *nm, int sta) : pNum(n, tp)
        {
            strcpy(name, nm);
            status = sta;
        }
        void Print()
        {
            cout << "Phone=" << pNum.getNum() << "--Type=" << pNum.getPat() << "--State=" << (status ? "use":"unuse") << "--Owner=" << name << '\n';
        }
        bool search(int _num)
        {
            if(pNum.getNum() == _num) return true;
            return false;
        }
};


int main()
{
    untie();
    int n, sta;
    char tp, nm[30];
    cin >> n >> tp >> sta >> nm;
    VirtualPhone p1(n, tp, nm, sta);
    cin >> n >> tp >> sta >> nm;
    VirtualPhone p2(n, tp, nm, sta);
    cin >> n >> tp >> sta >> nm;
    VirtualPhone p3(n, tp, nm, sta);
    int T;
    cin >> T;
    while(T--)
    {   
        int num, ok = 0;
        cin >> num;
        if(p1.search(num)) p1.Print(), ok = 1;
        if(p2.search(num)) p2.Print(), ok = 1;
        if(p3.search(num)) p3.Print(), ok = 1;
        if(!ok) cout << "wrong number.\n";
    }
    return 0;
}



23.银行账户（拷贝构造）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Account{
    private:
        int num;
        char pat;
        double count, rate;
    public:
        Account(){}
        Account(int n, char tp, double ct)
        {
            num = n, pat = tp, count = ct;
            rate = 0.005;
        }
        Account(const Account &a)
        {
            num = a.num + 50000000;
            pat = a.pat;
            count = a.count;
            rate = 0.015;
        }
        void Print1()
        {
            count *= 1.0 + rate;
            cout << "Account=" << num << "--sum=" << count << '\n';
        }
        void Print2()
        {
            cout << "Account=" << num << "--" << (pat == 'P' ? "Person" : "Enterprise") << "--sum=" << count << "--rate=" << rate << '\n';
        }
        void Print(char op)
        {
            if(op == 'C') Print1();
            else Print2();
        }
};

int main()
{
    untie();
    int T;
    cin >> T;
    while(T--)
    {   
        int num;
        char tp, op1, op2;
        double cnt;
        cin >> num >> tp >> cnt >> op1 >> op2;
        Account a1(num, tp, cnt);
        Account a2(a1);
        a1.Print(op1);
        a2.Print(op2);
    }
    return 0;
}



24.电视遥控（静态+友元）【期中模拟】
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Television{
    private:
        int sound, pat, num;
        static int numTV, numDVD;
    public:
        Television(){}
        Television(int p, int n, int ds)
        {
            pat = p;
            num = n;
            sound -= ds;
        }
        void setPro(int p, int n, int s)
        {
            pat = p;
            num = n;
            sound = s;
        }
        void Print()
        {
            cout << (pat == 1 ? "TV模式" : "DVD模式") << "--频道" << num << "--音量" << sound << '\n';
        }
        static void addTV(int val) { numTV += val;}
        static void addDVD(int val) { numDVD += val;}
        static int getNumTV() { return numTV;}
        static int getNumDVD() { return numDVD;}
        friend void Remote(Television &tv, int pat, int sound, int num)
        {
            if(tv.pat == 1 && pat == 2) Television::addDVD(1), Television::addTV(-1);
            if(tv.pat == 2 && pat == 1) Television::addDVD(-1), Television::addTV(1);
            tv.pat = pat;
            tv.num = num;
            tv.sound += sound;
            if(tv.sound < 0) tv.sound = 0;
            if(tv.sound > 100) tv.sound = 100;
        }
};

int Television::numTV = 0;
int Television::numDVD = 0;

int main()
{
    untie();
    int n, T;
    cin >> n >> T;
    Television *tvs = new Television[n + 1];
    for(int i = 1; i <= n; ++i) tvs[i].setPro(1, 0, 50);
    Television::addTV(n);
    while(T--)
    {   
        int id, k, num, d;
        cin >> id >> k >> num >> d;
        Remote(tvs[id], k, d, num);
        cout << "第" << id << "号电视机--";
        tvs[id].Print();
    }
    cout << "播放电视的电视机数量为" << Television::getNumTV() << '\n';
    cout << "播放DVD的电视机数量为" << Television::getNumDVD() << '\n';
    delete []tvs;
    return 0;
}



25. 圆和圆柱体计算（继承）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define M_PI 3.14

class CPoint{
    private:
        double x, y;
    public:
        CPoint(){}
        CPoint(double xx, double yy)
        {
            x = xx, y = yy;
        }
        void Print_Point()
        {
            cout << "(" << x << "," << y << ")";
        }
};

class CCircle : public CPoint{
    public:
        double r;
        CCircle(){}
        CCircle(double xx, double yy, double rr) : CPoint(xx, yy)
        {
            r = rr;
        }
        double getArea()
        {
            return M_PI * r * r;
        }
        void Print1()
        {
            cout << "Circle:";
            Print_Point();
            cout << "," << r << '\n';
        }
};

class CCylinder : public CCircle{
    private:
        double h;
    public:
        CCylinder(){}
        CCylinder(double xx, double yy, double rr, double hh) : CCircle(xx, yy, rr)
        {
            h = hh;
        }
        double getVol()
        {
            return getArea() * h;
        }
        void Print2()
        {
            cout << "Cylinder:";
            Print_Point();
            cout << "," << r << "," << h << '\n';
        }
};

int main()
{
    untie();
    double x1, y1, r1;
    cin >> x1 >> y1 >> r1;
    CCircle cir(x1, y1, r1);
    cir.Print1();
    cout << "Area:" << cir.getArea() << '\n';
    double x2, y2, r2, h;
    cin >> x2 >> y2 >> r2 >> h;
    CCylinder cyl(x2, y2, r2, h);
    cyl.Print2();
    cout << "Volume:" << cyl.getVol() << '\n';
    return 0;
}



26. 三维空间的点（继承）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define M_PI 3.14

class C2D{
    public:
        double x, y;
        C2D(){}
        C2D(double xx, double yy)
        {
            x = xx, y = yy;
        }
        double getDistance()
        {
            return sqrt(x * x + y * y);
        }
};

class C3D : public C2D{
    private:
        double z;
    public:
        C3D(){}
        C3D(double xx, double yy, double zz) : C2D(xx, yy)
        {
            z = zz;
        }
        double getDistance()
        {
            return sqrt(x * x + y * y + z * z);
        }
};

int main()
{
    untie();
    double x, y, z;
    cin >> x >> y;
    C2D p1(x, y);
    cout << p1.getDistance() << '\n';
    cin >> x >> y >> z;
    C3D p2(x, y, z);
    cout << p2.getDistance() << '\n';
    cin >> x >> y >> z;
    C3D p3(x, y, z);
    cout << p3.getDistance() << '\n';
    C2D p4(x, y);
    cout << p4.getDistance() << '\n';
    return 0;
}



27.时钟模拟(继承）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define M_PI 3.14

class Counter{
    protected:
        int value;
    public:
        Counter(){ value = 0;}
        Counter(int v)
        {
            value = v;
        }
        void increment()
        {
            ++value;
        }
};

class cyclicCounter : public Counter{
    private:
        int minValue, maxValue;
    public:
        cyclicCounter(){}
        void set(int val, int minVal, int maxVal)
        {
            this->value = val;
            minValue = minVal;
            maxValue = maxVal;
        }
        void increment()
        {
            if(++this->value > maxValue) this->value = minValue;
        }
        int getMinVal(){ return minValue;}
        int getMaxVal(){ return maxValue;}
        int getVal(){ return this->value;}
        void setVal(int x) { this->value = x;}
};

class Clock{
    private:
        cyclicCounter hour, minute, second;
    public:
        Clock(){}
        Clock(int h, int m, int s)
        {
            hour.set(h, 0, 24);
            minute.set(m, 0, 60);
            second.set(s, 0, 60);
        }
        void getTime(int ds)
        {
            for(int i = 0; i < ds; ++i)
            {
                second.increment();
                if(second.getVal() == second.getMaxVal())
                {
                    second.setVal(second.getMinVal());
                    minute.increment();
                    if(minute.getVal() == minute.getMaxVal())
                    {
                        minute.setVal(minute.getMinVal());
                        hour.increment();
                        if(hour.getVal() == hour.getMaxVal())
                        {
                            hour.setVal(hour.getMinVal());
                        }
                    }
                }
            }
            cout << hour.getVal() << ":" << minute.getVal() << ":" << second.getVal() << '\n';
        }
};

int main()
{
    untie();
    int n;
    cin >> n;
    while(n--)
    {
        int h, m, s, ds;
        cin >> h >> m >> s >> ds;
        Clock c(h, m, s);
        c.getTime(ds);
    }
    
    return 0;
}



28. 新旧身份证(继承)
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define M_PI 3.14
int mon1[15] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
int idcheck[17] = {7, 9, 10, 5, 8, 4, 2, 1, 6, 3, 7, 9, 10, 5, 8, 4, 2};
char code[11] = {'1', '0', 'X', '9', '8', '7', '6', '5', '4', '3', '2'};
char s[10];
class CDate{
    private:
        int year, month, day;
    public:
        CDate(){}
        CDate(int y, int m, int d) : year(y), month(m), day(d){}
        CDate(const CDate &cd)
        {
            year = cd.year, month = cd.month, day = cd.day;
        }
        bool check()//检验日期是否合法
        {
            if (year > 2021 || year == 2021 && month > 11 || year == 2021 && month == 11 && day > 9) return false;
            if(isLeap() && month == 2 && day > 28) return false;
            else if(day > mon1[month]) return false;
            return true;
        }
        bool isLeap()
        {
            if (year % 4 == 0 && year % 100 != 0 || year % 400 == 0) return true;
			return false;
        }
        void print()
        {
			cout << year << "年" << month << "月" << day << "日 ";
        }
        char* DatetoString()
        {
            for(int i = 3, y = year; i >= 0; --i, y /= 10) s[i] = '0' + y % 10;
            for(int i = 5, m = month; i >= 4; --i, m /= 10) s[i] = '0' + m % 10;
            for(int i = 7, d = day; i >= 6; --i, d /= 10) s[i] = '0' + d % 10;
            return s;
        }
        int getyear() { return year;}
		int getmonth() { return month;}
		int getday() { return day;}
};

class COldId{
    protected:
        char *pId15, *pName; //15位身份证号码，姓名
        CDate birthday; //出生日期
    public:
        COldId(){}
        COldId(char* pIdVal, char* pNameVal, CDate& day) : birthday(day)
        {
            pId15 = new char[strlen(pIdVal) + 1];
            pName = new char[strlen(pNameVal) + 1];
            strcpy(pId15, pIdVal);
            strcpy(pName, pNameVal);
        }
        bool check() //验证15位身份证是否合法
        {
            if(!birthday.check()) return false;
            if(strlen(pId15) != 15) return false;
            for(int i = 0; i < 15; ++i)
                if(!isdigit(pId15[i]))
                    return false;
            return true;
        }
        void print(){}
        ~COldId()
        {
            delete []pId15, pName;
        }
};

class CNewId : public COldId{
    private:
        char *pId18;
        CDate issueDay;
        int validYear;
    public:
        CNewId(){}
        CNewId(char *id18, CDate& issDay, int validy, char* pIdVal, char* pNameVal, CDate& day) : COldId(pIdVal, pNameVal, day), issueDay(issDay)
        {
            validYear = validy;       
            pId18 = new char[strlen(id18) + 1];
            strcpy(pId18, id18);
        }
        ~CNewId()
        {
            delete []pId18;
        }
        bool check()
        {
            if(!COldId::check()) return false;
            if(!issueDay.check()) return false;
            if(strlen(pId18) != 18) return false;
            if(strstr(pId18, this->birthday.DatetoString()) == NULL) return false;
            if(strncmp(pId15, pId18, 5) != 0 || strncmp(pId15 + 6, pId18 + 8, 8) != 0) return false;
            int temp = 0;
			for (int i = 0; i < 17; i++) temp += (pId18[i] - '0') * idcheck[i];
			temp %= 11;
			if (code[temp] != pId18[17])
				return false;
			if (issueDay.getyear() + validYear < 2021 || issueDay.getyear() + validYear == 2021 && issueDay.getmonth() > 11 || issueDay.getyear() + validYear == 2021 && issueDay.getmonth() == 11 && issueDay.getday() > 9)
				return false;
			return true;
        }
        void print()
        {
            cout << pId18 << " ";
            issueDay.print();
            if(validYear == 100) cout << "长期";
            else cout << validYear << "年";
            cout << '\n';
        }
};

int main()
{
    untie();
    int T;
    cin >> T;
    char name[50], id15[25], id18[25];
    int y, m, d, yy, mm, dd, time;
    while(T--)
    {
        cin >> name >> y >> m >> d >> id15 >> id18 >> yy >> mm >> dd >> time;
        CDate c1(y, m, d), c2(yy, mm, dd);
        CNewId now(id18, c2, time, id15, name, c1);
        cout << name << '\n';
        if(now.check()) now.print();
        else cout << "illegal id\n";
    }
    return 0;
}



29.学生成绩计算（继承）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define M_PI 3.14
class Person
{
	public:
		Person(string nm, int y)
		{
			name = nm;
			year = y;
		}
		void display()
		{
			cout << name << " " << year << " ";
		}
	private:
		string name;
		int year;
		
};

class Student1 : public Person
{
	public:	
		Student1(string nm, int y, int score1, int score2) : Person(nm, y)
		{
			scoreA = score2;
			scoreB = score1;
		}
		void display()
		{
			Person :: display();
			double m;
			m = scoreA * 0.4 + scoreB * 0.6;
			if(m >= 85) cout << "A";
			else if(m >= 75 && m < 85) cout << "B";
			else if(m >= 65 && m < 75) cout << "C";
			else if(m >= 60 && m < 65) cout << "D";
			else cout << "F";
            cout << '\n';
		}
	private:
		int scoreA;
		int scoreB;
};

class Student2 : public Person
{
	public:
		Student2(string name1,int year1,int score1):Person(name1,year1)
		{
			scoreB=score1;
		}
		void display()
		{
			Person::display();
			double m;
			m = scoreB;
			if(m >= 85) cout << "A";
			else if(m >= 75 && m < 85) cout << "B";
			else if(m >= 65 && m < 75) cout << "C";
			else if(m >= 60 && m < 65) cout << "D";
			else cout << "F";
            cout << '\n';			
		}
	private:
		int scoreB;
};

int main()
{
    untie();
    int T;
    cin >> T;
    char op;
    string name;
    int year, score1, score2;
    while(T--)
    {
        cin >> op;
        if(op == 'R')
		{
			cin >> name >> year >> score2 >> score1;
			Student1 Student3(name, year, score2, score1);
			Student3.display();
		}
		else if(op == 'S')
		{
			cin >> name >> year >> score1;
			Student2 Student4(name, year, score1);
			Student4.display();
	    }
    }
    return 0;
}


/*
https://blog.csdn.net/qq_42048450/article/details/117282640
（纯）虚函数概念解读：
定义一个函数为虚函数，不代表函数为不被实现的函数。
定义他为虚函数是为了允许用基类的指针来调用子类的这个函数。
定义一个函数为纯虚函数，才代表函数没有被实现。
定义纯虚函数是为了实现一个接口，起到一个规范的作用，规范继承这个类的程序员必须实现这个函数。
*/
30.图形面积（虚函数与多态）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Shape
{
    public:
        virtual void area(){}
};

class Circle : public Shape
{
    private:
        double r;
    public:
        Circle(){}
        Circle(double rr){ r = rr;}
        void area()
        {
            cout << 3.14 * r * r << '\n';
        }
};

class Square : public Shape
{
    private:
        double l;
    public:
        Square(){}
        Square(double _l){ l = _l;}
        void area()
        {
            cout << l * l << '\n';
        }
};

class Rectangle : public Shape
{
    private:
        double len, wid;
    public:
        Rectangle(){}
        Rectangle(double l, double w){ len = l, wid = w;}
        void area()
        {
            cout << len * wid << '\n';
        }
};

int main()
{
    untie();
    int T;
    cin >> T;
    Shape *p[3];
    while(T--)
    {
        double r, l, len, wid;
        cin >> r >> l >> len >> wid;
        Circle cir(r);
        Square sq(l);
        Rectangle rec(len, wid);
        p[0] = &cir;
        p[1] = &sq;
        p[2] = &rec;
        cout << fixed << setprecision(2);
        for(int i = 0; i < 3; ++i) p[i]->area();
    }
  	for(int i = 0; i < 3; ++i) p[i] = NULL;
    return 0;
}



31.汽车收费（虚函数和多态）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Vehicle
{
    protected:
        string no;
    public:
        Vehicle(){}
        Vehicle(string id) : no(id){}
        virtual void display() = 0;
};

class Car : public Vehicle
{ 
    private:
        int cnt, w;
    public:
        Car(){}
        Car(string s, int c, int ww) : Vehicle(s){ cnt = c, w = ww;}
        void display()
        {
            cout << no << " " << (8 * cnt + 2 * w) << '\n';
        }
};

class Truck : public Vehicle
{ 
    private:
        int w;
    public:
        Truck(){}
        Truck(string s, int ww) : Vehicle(s){ w = ww;}
        void display()
        {
            cout << no << " " << (5 * w) << '\n';
        }
};

class Bus : public Vehicle
{ 
    private:
        int cnt;
    public:
        Bus(){}
        Bus(string s, int c) : Vehicle(s){ cnt = c;}
        void display()
        {
            cout << no << " " << (30 * cnt) << '\n';
        }
};

int main()
{
    untie();
    int T;
    cin >> T;
    string id;
    while(T--)
    {
        int op, cnt, w;
        Vehicle *pv = NULL;
        cin >> op >> id;
        if(op == 1)
        {
            cin >> cnt >> w;
            Car c(id, cnt, w);
            pv = &c;
            pv -> display();
        }
        else if(op == 2)
        {
            cin >> w;
            Truck tru(id, w);
            pv = &tru;
            pv -> display();
        }
        else
        {
            cin >> cnt;
            Bus bus(id, cnt);
            pv = &bus;
            pv -> display();
        }
    }
    return 0;
}


         
32.支票账户（虚函数与多态）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class BaseAccount
{
    protected:
        string name, account;
        int balance;
    public:
        BaseAccount(){}
        BaseAccount(string s1, string s2, int b) : name(s1), account(s2), balance(b){}
        void deposit(int x)
        {
            balance += x;
        }
        virtual void withdraw(int x)
        {
            if(balance - x < 0) cout << "insufficient\n";
            else balance -= x;
        }
        virtual void display()
        {
            cout << name << " " << account << " Balance:" << balance << '\n';
        }
};

class BasePlus : public BaseAccount
{
    private:
        int limit, limitSum;
    public:
        BasePlus(){}
        BasePlus(string s1, string s2, int b) : BaseAccount(s1, s2, b){ limit = 5000, limitSum = 5000;}
        void withdraw(int x)
        {
            if(x > balance + limitSum) cout << "insufficient\n";
            else
            {
                if(x <= balance) balance -= x;
                else
                {
                    limitSum -= x - balance;
                    balance = 0;
                }
            }
        }
        void display()
        {
            cout << name << " " << account << " Balance:";
            if(balance < 0) cout << "0";
            else cout << balance << " limit:" << limitSum << '\n';
        }
};

int main()
{
    untie();
    int T;
    cin >> T;
    string s1, s2;
    BaseAccount *p = NULL;
    int dep1, wit1, dep2, wit2, bal;
    while(T--)
    {
        cin >> s1 >> s2 >> bal;
        cin >> dep1 >> wit1 >> dep2 >> wit2;
        if(s2[1] == 'A')
        {
            BaseAccount now(s1, s2, bal);
            p = &now;
            p -> deposit(dep1);
            p -> withdraw(wit1);
            p -> deposit(dep2);
            p -> withdraw(wit2);
            p -> display();
        }
        else
        {
            BasePlus now(s1, s2, bal);
            p = &now;
            p -> deposit(dep1);
            p -> withdraw(wit1);
            p -> deposit(dep2);
            p -> withdraw(wit2);
            p -> display();
        }
    }
  	p = NULL;
    return 0;
}



33.进位与借位（虚函数和多态）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Group
{
    public:
        Group(){}
        virtual int add(int x, int y) = 0;
	    virtual int sub(int x, int y) = 0;
};

class GroupA : public Group
{
    public:
        int add(int x, int y)
        {
            return x + y;
        }
        int sub(int x, int y)
        {
            return x - y;
        }
};

class GroupB : public Group
{
    public:
        GroupB(){}
        int add(int x, int y)
        {
            return x + y;
        }
        int sub(int x, int y)
        {
            int res = 0, base = 1;
            while(x || y)
            {
                int now = x % 10 - y % 10;
                if(x % 10 < y % 10) now += 10;
                x /= 10;
                y /= 10;
                res += now * base;
                base *= 10;
            }
            return res;
        }
};

class GroupC : public Group
{
    public:
        GroupC(){}
        int add(int x, int y)
        {
            int res = 0, base = 1;
            while(x || y)
            {
                int now = x % 10 + y % 10;
                if(x % 10 + y % 10 > 9) now -= 10;
                x /= 10;
                y /= 10;
                res += now * base;
                base *= 10;
            }
            return res;
        }
        int sub(int x, int y)
        {
            int res = 0, base = 1;
            while(x || y)
            {
                int now = x % 10 - y % 10;
                if(x % 10 < y % 10) now += 10;
                x /= 10;
                y /= 10;
                res += now * base;
                base *= 10;
            }
            return res;
        }
};

int main()
{
    untie();
    int T;
    cin >> T;
    string oper;
    Group *p = NULL;
    while(T--)
    {
        int type, ind = 0;
        char op;
        cin >> type >> oper;
        for(int i = 0; i < oper.size(); ++i)
        {
            if(!isdigit(oper[i]))
            {
                ind = i;
                op = oper[i];
            }
        }
        int x = stoi(oper.substr(0, ind)), y = stoi(oper.substr(ind + 1));
        if(type == 1)
        {
            GroupA now;
            p = &now;
            if(op == '+') cout << p -> add(x, y) << '\n';
            else cout << p -> sub(x, y) << '\n';
        }
        else if(type == 2)
        {
            GroupB now;
            p = &now;
            if(op == '+') cout << p -> add(x, y) << '\n';
            else cout << p -> sub(x, y) << '\n';
        }
        else
        {
            GroupC now;
            p = &now;
            if(op == '+') cout << p -> add(x, y) << '\n';
            else cout << p -> sub(x, y) << '\n';
        }
    }
    p = NULL;
    return 0;
}

        

34.动物园（虚函数与多态）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Animal
{
    protected:
        string name;
        int age;
    public:
        Animal(){}
        Animal(string s, int a) : name(s), age(a){}
        virtual void Speak() = 0;
};

class Tiger : public Animal
{
    public:
        Tiger(){}
        Tiger(string s, int a) : Animal(s, a){}
        void Speak()
        {
            cout << "Hello,I am " << name << ",";
            cout << "AOOO.\n";
        }
};

class Pig : public Animal
{
    public:
        Pig(){}
        Pig(string s, int a) : Animal(s, a){}
        void Speak()
        {
            cout << "Hello,I am " << name << ",";
            cout << "HENGHENG.\n";
        }
};

class Dog : public Animal
{
    public:
        Dog(){}
        Dog(string s, int a) : Animal(s, a){}
        void Speak()
        {
            cout << "Hello,I am " << name << ",";
            cout << "WangWang.\n";
        }
};

class Duck : public Animal
{
    public:
        Duck(){}
        Duck(string s, int a) : Animal(s, a){}
        void Speak()
        {
            cout << "Hello,I am " << name << ",";
            cout << "GAGA.\n";
        }
};


int main()
{
    untie();
    int T;
    cin >> T;
    Animal *p = NULL;
    string name, type;
    int age;
    while(T--)
    {
        cin >> type >> name >> age;
        if(type == "Tiger")
        {
            Tiger now(name, age);
            p = &now;
            p -> Speak();
        }
        else if(type == "Pig")
        {
            Pig now(name, age);
            p = &now;
            p -> Speak();
        }
        else if(type == "Dog")
        {
            Dog now(name, age);
            p = &now;
            p -> Speak();
        }
        else if(type == "Duck")
        {
            Duck now(name, age);
            p = &now;
            p -> Speak();
        }
        else
        {
            cout << "There is no " << type << " in our Zoo.\n";
        }
    }
    p = NULL;
    return 0;
}



35.OOP 学生综合评价（虚函数和多态）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

string code[10] = {"优秀", "良好", "一般", "及格", "不及格"};

class Student{
    protected:
        string name;
        int type; //学生类别:1表示本科生，2表示研究生
        int courses[3]; //3门课的成绩
        string courseGrade; //成绩等级
    public:
        Student(string n, int t, int a1, int a2, int a3)
        {
            name = n, type = t;
            courses[0] = a1;
            courses[1] = a2;
            courses[2] = a3;
        }
        virtual void calculateGrade() = 0;//计算成绩等级
        void Print()
        {
            if(type == 1) cout << name << ",本科生,";
            else cout << name << ",研究生,";
            cout << courseGrade << '\n';
        }
};

class Undergraduate : public Student
{
    public:
        Undergraduate(const string &n, int t, int a1, int a2, int a3) : Student(n, t, a1, a2, a3){}
        void calculateGrade()
        {
            double g = courses[0] + courses[1] + courses[2];
            g = g / 3;
            if (g >= 80 && g <= 100)
                courseGrade = "优秀";
            else if (g >= 70 && g < 80)
                courseGrade = "良好";
            else if (g >= 60 && g < 70)
                courseGrade = "一般";
            else if (g >= 50 && g < 60)
                courseGrade = "及格";
            else if(g < 50)
                courseGrade = "不及格";
        }
};

class Postgraduate : public Student
{
    public:
        Postgraduate(const string &n, int t, int a1, int a2, int a3) : Student(n, t, a1, a2, a3){}
        void calculateGrade()
        {
            double g = courses[0]+courses[1]+courses[2];
            g = g / 3;
            if (g >= 90 && g <= 100)
                courseGrade = "优秀";
            else if (g >= 80 && g < 90)
                courseGrade = "良好";
            else if (g >= 70 && g < 80)
                courseGrade = "一般";
            else if (g >= 60 && g < 70)
                courseGrade = "及格";
            else if(g < 60)
                courseGrade = "不及格";
        }
};

int main()
{
    untie();
    int n;
    cin >> n;
    Student **p = new Student*[n];
    string name;
    int tp, a, b, c;
    for(int i = 0; i < n; ++i)
    {
        cin >> name >> tp >> a >> b >> c;
        if(tp == 1) p[i] = new Undergraduate(name, tp, a, b, c);
        else p[i] = new Postgraduate(name, tp, a, b, c);
    }
    for(int i = 0; i < n; ++i) 
    {
        p[i] -> calculateGrade();
        p[i] -> Print();
    }
    for(int i = 0; i < n; ++i) delete p[i];
    delete []p;
    return 0;
}



36.在职研究生（多重继承）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CPeople{
    protected:
        string name;
        char sex;
        int age;
    public:
        CPeople(){}
        CPeople(string nm, char sx, int a) : name(nm), sex(sx), age(a){}
        void Print()
        {
            cout << "People:\n";
            cout << "Name: " << name << '\n';
            cout << "Sex: " << sex << '\n';
            cout << "Age: " << age << '\n';
            cout << '\n';
        }
};

class CStudent : public CPeople
{
    protected:
        int id;
        double grade;
    public:
        CStudent(){}
        CStudent(string nm, char sx, int a, int ID, double g) : CPeople(nm, sx, a){ id = ID, grade = g;}
        void Print()
        {
            cout << "Student:\n";
            cout << "Name: " << name << '\n';
            cout << "Sex: " << sex << '\n';
            cout << "Age: " << age << '\n';
            cout << "No.: " << id << '\n';
            cout << "Score: " << grade << '\n';
            cout << '\n';
        }
};

class CTeacher : public CPeople
{
    protected:
        string pos, department;
    public:
        CTeacher(){}
        CTeacher(string nm, char sx, int a, string p, string depart) : CPeople(nm, sx, a){ pos = p, department = depart;}
        void Print()
        {
            cout << "Teacher:\n";
            cout << "Name: " << name << '\n';
            cout << "Sex: " << sex << '\n';
            cout << "Age: " << age << '\n';
            cout << "Position: " << pos << '\n';
            cout << "Department: " << department << '\n';
            cout << '\n';
        }
};

class CGradOnWork : public CStudent, public CTeacher
{
    protected:
        string dir, guider;
    public:
        CGradOnWork(){}
        CGradOnWork(string nm, char sx, int a, int ID, double g, string p, string depart, string d, string gui) : CStudent(nm, sx, a, ID, g), CTeacher(nm, sx, a, p, depart)
        {
            dir = d, guider = gui;
        }
        void Print()
        {
            cout << "GradOnWork:\n";
            cout << "Name: " << CStudent::name << '\n';
            cout << "Sex: " << CStudent::sex << '\n';
            cout << "Age: " << CStudent::age << '\n';
            cout << "No.: " << id << '\n';
            cout << "Score: " << grade << '\n';
            cout << "Position: " << pos << '\n';
            cout << "Department: " << department << '\n';
            cout << "Direction: " << dir << '\n';
            cout << "Tutor: " << guider << '\n';
            cout << '\n';
        }
};

int main()
{
    untie();
    string name, pos, depart, dir, guider;
    char sex;
    int age, id;
    double grade;
    cin >> name >> sex >> age;
    CPeople p1(name, sex, age);
    p1.Print();
    cin >> id >> grade;
    CStudent p2(name, sex, age, id, grade);
    p2.Print();
    cin >> pos >> depart;
    CTeacher p3(name, sex, age, pos, depart);
    p3.Print();
    cin >> dir >> guider;
    CGradOnWork p4(name, sex, age, id, grade, pos, depart, dir, guider);
    p4.Print();
    return 0;
}

 

37.交通工具（多重继承）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CVehicle{
    protected:
        int max_speed, speed, weight;
    public:
        CVehicle(){}
        CVehicle(int a, int b, int c) : max_speed(a), speed(b), weight(c){}
        void display()
        {
            cout << "Vehicle:\n";
            cout << "max_speed:" << max_speed << '\n';
            cout << "speed:" << speed << '\n';
            cout << "weight:" << weight << '\n';
            cout << '\n';
        }
};

class CBicycle : public CVehicle
{
    protected:
        int height;
    public:
        CBicycle(){}
        CBicycle(int a, int b, int c, int h) : CVehicle(a, b, c){ height = h;}
        void display()
        {
            cout << "Bicycle:\n";
            cout << "max_speed:" << max_speed << '\n';
            cout << "speed:" << speed << '\n';
            cout << "weight:" << weight << '\n';
            cout << "height:" << height << '\n';
            cout << '\n';
        }
};

class CMotocar : public CVehicle
{
    protected:
        int seat_num;
    public:
        CMotocar(){}
        CMotocar(int a, int b, int c, int n) : CVehicle(a, b, c){ seat_num = n;}
        void display()
        {
            cout << "Motocar:\n";
            cout << "max_speed:" << max_speed << '\n';
            cout << "speed:" << speed << '\n';
            cout << "weight:" << weight << '\n';
            cout << "seat_num:" << seat_num << '\n';
            cout << '\n';
        }
};

class CMotocycle : public CBicycle, public CMotocar
{
    private:
    
    public:
        CMotocycle(){}
        CMotocycle(int a, int b, int c, int h, int n) : CBicycle(a, b, c, h), CMotocar(a, b, c, n){}
        void display()
        {
            cout << "Motocycle:\n";
            cout << "max_speed:" << CMotocar::max_speed << '\n';
            cout << "speed:" << CMotocar::speed << '\n';
            cout << "weight:" << CMotocar::weight << '\n';
            cout << "height:" << CBicycle::height << '\n';
            cout << "seat_num:" << CMotocar::seat_num << '\n';
            cout << '\n';
        }
};

int main()
{
    untie();
    int maxsp, sp, w, h, num;
    cin >> maxsp >> sp >> w >> h >> num;
    CVehicle p1(maxsp, sp, w);
    p1.display();
    CBicycle p2(maxsp, sp, w, h);
    p2.display();
    CMotocar p3(maxsp, sp, w, num);
    p3.display();
    CMotocycle p4(maxsp, sp, w, h, num);
    p4.display();
    return 0;
}


 
38.商旅信用卡（多重继承）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Tripcard{
    protected:
        int id;
        static int score;
    public:
        Tripcard(){}
        Tripcard(int ID, int s) : id(ID){ score = s;}
        void addscore(float x)
        {
            score += x;
        }
};

class Creditcard{
    protected:
        int id, limit;
        float count;
        string name;
        static float score;
    public:
        Creditcard(){}
        Creditcard(int ID, int ss, int lim, float ct, string nm)
        {
            id = ID, limit = lim, count = ct, name = nm, score = ss;
        }
        void consume(float m)
        {
            if(count + m <= limit) 
            {
                count += m;
                score += m;
            }
        }
        void getback(float m)
        {
            count -= m;
            score -= m;
        }
        void addscore(float x)
        {
            score += x;
        }
};

class TripCreditcard : public Tripcard, public Creditcard
{
    public:
        TripCreditcard(int ID1, int ID2, string nm, int lim, float ct, int s, int ss) : Tripcard(ID1, s), Creditcard(ID2, ss, lim, ct, nm){}
        void addscore(float m)
        {
            Tripcard::addscore(m);
            Creditcard::addscore(m);
            count += m;
        }
        void change(float x)
        {
            int s1, s2 = x / 2, s3;
            Tripcard::score += s2;
            Creditcard::score -= x;
        }
        void Print()
        {
            cout << Tripcard::id << " " << Tripcard::score << '\n' << Creditcard::id << " " << name << " " << count << " " << (int)Creditcard::score << '\n';
        }
};

float Creditcard::score = 0;
int Tripcard::score = 0;

int main()
{
    untie();
    int n, id1, id2, lim;
    string name, op;
    float m;
    cin >> id1 >> id2 >> name >> lim;
    TripCreditcard tcc(id1, id2, name, lim, 0, 0, 0);
    cin >> n;
    while(n--)
    {
        cin >> op >> m;
        char t = op[0];
        if(t == 'o')
			tcc.addscore(m);
		else if(t =='c')
			tcc.consume(m);
		else if(t =='q')
			tcc.getback(m);
		else if(t =='t')
			tcc.change(m);
    }
    tcc.Print();
    return 0;
}

 

*39.计算宝宝帐户收益(多重继承 + 虚基类)
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CPeople{
    protected:
        char id[20], name[10];
    public:
        CPeople(){}
        CPeople(char *ID, char *nm)
        {
            strcpy(id, ID);
            strcpy(name, nm);
        }
};

//这里定义虚基类，使得 CInternetUser::registerUser() 和 CBankCustomer::openAccount() 都是对同一个 CPeople 对象进行修改操作
class CInternetUser : virtual public CPeople
{
    protected:
        char password[20];
    public:
        void registerUser(char *ID, char *nm, char *pw)
        {
            strcpy(id, ID);
            strcpy(name, nm);
            strcpy(password, pw);
        }
        bool login(char *ID, char *pw)
        {
            return (!strcmp(ID, id)) && (!strcmp(pw, password));
        }
};

class CBankCustomer : virtual public CPeople
{
    protected:
        double balance;
    public:
        CBankCustomer(){}
        void openAccount(char *nm, char *ID)
        {
            strcpy(id, ID);
            strcpy(name, nm);
            balance = 0;
        }
        void deposit(double m)
        {
            balance += m;
        }
        bool withdraw(double m)
        {
            if(balance - m < 0) return 0;
            balance -= m;
            return 1;
        }
};

class CInternetBankCustomer : public CInternetUser, public CBankCustomer
{
    protected:
        double t_balance, y_balance;
        double earning;
        double t_interest, y_interest;
    public:
        CInternetBankCustomer()
        {
            t_balance = y_balance = earning = t_interest = y_interest = 0;
        }
        bool deposit(double m)
        {
            if(balance - m < 0) return 0;
            balance -= m;
            t_balance += m;
            return 1;
        }
        bool withdraw(double m)
        {
            if(balance - m < 0) return 0;
            balance += m;
            t_balance -= m;
            return 1;
        }
        void setInterest(double x)
        {
            y_interest = t_interest;
            t_interest = x;
        }
        void calculateProfit()
        {
            earning = y_balance * 0.0001 * y_interest;
            t_balance += earning;
            y_balance = t_balance;  
        }
        void print()
        {
            cout << "Name: " << CBankCustomer::name << " ID: " << CBankCustomer::id << '\n' << "Bank balance: " << balance << '\n' << "Internet bank balance: " << t_balance << '\n';
        }
        bool login(char *ID, char *pw)
        {
            return (!strcmp(ID, CInternetUser::id)) && (!strcmp(pw, CInternetUser::password)) && (!strcmp(CInternetUser::id, CBankCustomer::id) && !strcmp(CInternetUser::name, CBankCustomer::name));
        }
};

int main()
{
    untie();
    int t, no_of_days, i;
    char i_xm[20], i_id[20], i_mm[20], b_xm[20], b_id[20], ib_id[20], ib_mm[20];
    double money, interest;
    char op_code;
    //输入测试案例数t
    cin >> t;
    while (t--)
    {
        //输入互联网用户注册时的用户名,id,登陆密码
        cin >> i_xm >> i_id >> i_mm;
        //输入银行开户用户名,id
        cin >> b_xm >> b_id;
        //输入互联网用户登陆时的id,登陆密码
        cin >> ib_id >> ib_mm;
        CInternetBankCustomer ib_user;
        ib_user.registerUser(i_xm, i_id, i_mm);
        ib_user.openAccount(b_xm, b_id);
        if (ib_user.login(ib_id, ib_mm) == 0) //互联网用户登陆,若id与密码不符;以及银行开户姓名和id与互联网开户姓名和id不同
        {
            cout << "Password or ID incorrect" << endl;
            continue;
        }
        //输入天数
        cin >> no_of_days;
        for (i = 0; i < no_of_days; i++)
        {
            //输入操作代码, 金额, 当日万元收益
            cin >> op_code >> money >> interest;
            switch (op_code)
            {
                case 'S': //从银行向互联网金融帐户存入
                case 's':
                    if (ib_user.deposit(money) == 0)
                    {
                        cout << "Bank balance not enough" << endl;
                        continue;
                    }
                    break;
                case 'T': //从互联网金融转入银行帐户
                case 't':
                    if (ib_user.withdraw(money) == 0)
                    {
                        cout << "Internet bank balance not enough" << endl;
                        continue;
                    }
                    break;
                case 'D': //直接向银行帐户存款
                case 'd':
                    ib_user.CBankCustomer::deposit(money);
                    break;
                case 'W': //直接从银行帐户取款
                case 'w':
                    if (ib_user.CBankCustomer::withdraw(money) == 0)
                    {
                        cout << "Bank balance not enough" << endl;
                        continue;
                    }
                    break;
                default:
                    cout << "Illegal input" << endl;
                    continue;
            }
            ib_user.setInterest(interest);
            ib_user.calculateProfit();
            ib_user.print();
            cout << endl;
        }
    }
    return 0;
}
 


40.时钟调整（运算符前后增量）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CClock
{
    private:
	    int hour, minute, second;
    public:
        CClock(int hour, int minute, int second) :hour(hour), minute(minute), second(second) {}
        CClock& operator++()
        {
            second++;
            if (second > 59)
            {
                second = 0;
                minute++;
            }
            if (minute > 59)
            {
                minute = 0;
                hour++;
            }
            if (hour > 11)
                hour = 0;
            return *this;
        }
        CClock operator--(int)
        {
            CClock temp(hour, minute, second);
            second--;
            if (second < 0)
            {
                second = 59;
                minute--;
            }
            if (minute < 0)
            {
                minute = 59;
                hour--;
            }
            if (hour < 0)
                hour = 11;
            return temp;
        }
        void Print()
        {
            cout << hour << ':' << minute << ':' << second << endl;
        }
};
int main()
{
    untie();
	int hour, minute, second,t,count;
	cin >> hour >> minute >> second >> t;
	CClock test(hour, minute, second);
	while (t--)
	{
		cin >> count;
		if (count > 0)
		{
			while (count--) ++test;
			test.Print();
		}
		else
		{
			count = -count;
			while (count--) test--;
			test.Print();
		}
	}
    return 0;
}



41.矩阵（运算符重载）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CMatrix
{
    private:
        int n, m; // n-行，m-列
        int** data; // 存储矩阵数据
    public:
        CMatrix()
        {
            n = 0; 
            m = 0;
            data = NULL;
        }
        CMatrix(int n1, int m1);
        ~CMatrix();
        CMatrix &operator =(const CMatrix &a)
        {
            if(data != NULL)
            {
                for (int i = 0; i < n; i++) delete []data[i];
                delete []data;
            }
            n = a.n;
            m = a.m;
            data = new int*[n];
            for (int i = 0; i < n; i++)
            {
                data[i] = new int[m];
            }
            for(int i = 0; i < n; ++i)
				for(int j = 0; j < m; j++)
					data[i][j] = a.data[i][j];
            return *this;
        }
        int *const operator [](const int i)
        {
            return data[i];
        }
        int operator ()(int i, int j)
        {
            return data[i][j];
        }
};

CMatrix::CMatrix(int n1, int m1)
{
    n = n1;
    m = m1;
    // 分配n行m列的二维数组空间
    data = new int* [n];
    for (int i = 0; i < n; i++)
    {
        data[i] = new int[m];
    }
}

CMatrix::~CMatrix()
{
    // 释放空间
    for (int i = 0; i < n; i++)
    {
        delete[] data[i];
    }
    delete[] data;
}

int main()
{
    int t, n, m, i, j;
    cin >> t;
    while (t--)
    {
        cin >> n >> m;
        // 定义矩阵对象matrixA
        CMatrix matrixA(n, m);
        for (i = 0; i < n; i++)
        {
            for (j = 0; j < m; j++)
            {
                // 输入第i行第j列的数据
                cin >> matrixA[i][j];
            }
        }
        // 输出matrixA中的数据
        cout << "matrixA:" << endl;
        for (i = 0; i < n; i++)
        {
            for (j = 0; j < m; j++)
            {
                cout << matrixA(i, j) << " ";
            }
            cout << endl;
        }
        // 定义矩阵对象matrixB
        CMatrix matrixB;
        matrixB = matrixA;
        // 输出marixB中的数据
        cout << "matrixB:" << endl;
        for (i = 0; i < n; i++)
        {
            for (j = 0; j < m; j++)
            {
                cout << matrixB[i][j] << " ";
            }
            cout << endl;
        }
    }
    return 0;
}



42.矩形关系（运算符重载）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CPoint
{
    private:
        int x, y;
    public:
        CPoint(){}
        CPoint(int xx, int yy) : x(xx), y(yy){}
        int getX() { return x;}
        int getY() { return y;}
};

class CRectangle
{
    private:
        CPoint leftPoint, rightPoint;
    public:
        CRectangle(){}
        CRectangle(int x1, int y1, int x2, int y2) : leftPoint(x1, y1), rightPoint(x2, y2){}
        bool operator >(CPoint &p)
        {
            return p.getX() > leftPoint.getX() && p.getX() < rightPoint.getX() && p.getY() < leftPoint.getY() && p.getY() > rightPoint.getY();
        }
        bool operator >(CRectangle &c)
        {
            bool ok1 = c.leftPoint.getX() >= leftPoint.getX() && c.leftPoint.getX() <= rightPoint.getX() && c.leftPoint.getY() <= leftPoint.getY() && c.leftPoint.getY() >= rightPoint.getY();
            bool ok2 = c.rightPoint.getX() >= leftPoint.getX() && c.rightPoint.getX() <= rightPoint.getX() && c.rightPoint.getY() <= leftPoint.getY() && c.rightPoint.getY() >= rightPoint.getY();
            return ok1 && ok2;
        }
        bool operator ==(CRectangle &c)
        {
            return c.leftPoint.getX() == leftPoint.getX() && c.leftPoint.getY() == leftPoint.getY() && c.rightPoint.getX() == rightPoint.getX() && c.rightPoint.getY() == rightPoint.getY();
        }
        bool operator *(CRectangle &c)
        {
            if(c.leftPoint.getX() > rightPoint.getX() || c.leftPoint.getY() < rightPoint.getY() || c.rightPoint.getX() < leftPoint.getX() || c.rightPoint.getY() > leftPoint.getY())
                return false;
            return true;
        }
        operator int()
        {
            return (rightPoint.getX() - leftPoint.getX()) * (leftPoint.getY() - rightPoint.getY()); 
        }
        friend ostream &operator <<(ostream &cout, CRectangle &c)
        {
            cout << c.leftPoint.getX() << " " << c.leftPoint.getY() << " " << c.rightPoint.getX() << " " << c.rightPoint.getY();
            return cout;
        }
};

int main()
{
    int t, x1, x2, y1, y2;
    cin >> t;
    while (t--)
    {
        // 矩形1的左上角、右下角
        cin >> x1 >> y1 >> x2 >> y2;
        CRectangle rect1(x1, y1, x2, y2);
        // 矩形2的左上角、右下角
        cin >> x1 >> y1 >> x2 >> y2;
        CRectangle rect2(x1, y1, x2, y2);
        // 输出矩形1的坐标及面积
        cout << "矩形1:" << rect1 << " " << (int)rect1 << endl;
        // 输出矩形2的坐标及面积
        cout << "矩形2:" << rect2 << " " << (int)rect2 << endl;
        if (rect1 == rect2)
        {
            cout << "矩形1和矩形2相等" << endl;
        }
        else if (rect2 > rect1)
        {
            cout << "矩形2包含矩形1" << endl;
        }
        else if (rect1 > rect2)
        {
            cout << "矩形1包含矩形2" << endl;
        }
        else if (rect1 * rect2)
        {
            cout << "矩形1和矩形2相交" << endl;
        }
        else
        {
            cout << "矩形1和矩形2不相交" << endl;
        }
        cout << endl;
    }
    return 0;
}



43.货币加减（运算符重载）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CMoney
{
    private:
	    int n1, n2, n3;
    public:
        CMoney(){}
        CMoney(int x1, int x2, int x3) : n1(x1), n2(x2), n3(x3){}
        friend CMoney operator +(const CMoney &c1, const CMoney &c2)
        {
            int s = (c1.n1 + c2.n1) * 100 + (c1.n2 + c2.n2) * 10  + c1.n3 + c2.n3;
            CMoney tp(s / 100, s % 100 / 10, s % 100 % 10);
            return tp;
        }
        friend CMoney operator -(const CMoney &c1, const CMoney &c2)
        {
            int s = (c1.n1 - c2.n1) * 100 + (c1.n2 - c2.n2) * 10  + c1.n3 - c2.n3;
            CMoney tp(s / 100, s % 100 / 10, s % 100 % 10);
            return tp;
        }
        friend istream& operator >> (istream& cin, CMoney& a)
        {
            cin >> a.n1 >> a.n2 >> a.n3;
            return cin; 
        }
        friend ostream& operator << (ostream& cout, CMoney& a)
        {
            cout << a.n1 << "元" << a.n2 << "角" << a.n3 << "分" << endl;
            return cout;
        }
};
int main()
{
    untie();
	int T;
    cin >> T;
    while(T--)
    {
        int n1, n2, n3;
        string op;
        CMoney m, tp;
        cin >> m;
        while(cin >> op)
        {
            if(op == "stop") break;
            cin >> tp;
            if(op == "add") m = m + tp;
            else m = m - tp;
        }
        cout << m;
    }
    return 0;
}



44.X的放大与缩小（运算符重载）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CXGraph
{
    private:
        int n;
    public:
        CXGraph(){}
        CXGraph(int nn) : n(nn){}
        CXGraph &operator ++()
        {
            if(n < 21) n += 2;
            return *this;
        }
        CXGraph &operator --()
        {
            if(n > 1) n -= 2;
            return *this;
        }
        CXGraph operator ++(int)
        {
            CXGraph temp(n);
            if(n < 21) n += 2;
            return temp;
        }
        CXGraph operator --(int)
        {
            CXGraph temp(n);
            if(n > 1) n -= 2;
            return temp;
        }
        friend ostream &operator <<(ostream &cout, const CXGraph &g)
        {
            for(int i = g.n, j = 0; i > 1; i -= 2, j++) cout << string(j, ' ') + string(i, 'X') << '\n';
            for(int i = 1, j = g.n / 2; i <= g.n; i += 2, j--) cout << string(j, ' ') + string(i, 'X') << '\n';
            return cout;
        }
};

int main()
{
    untie();
	int t, n;
    string command;
    cin >> n;
    CXGraph xGraph(n);
    cin >> t;
    while (t--)
    {
        cin >> command;
        if (command == "show++")
        {
            cout << xGraph++ << endl;
        }
        else if(command == "++show")
        {
            cout << ++xGraph << endl;
        }
        else if (command == "show--")
        {
            cout << xGraph-- << endl;
        }
        else if (command == "--show")
        {
            cout << --xGraph << endl;
        }
        else if (command == "show")
        {
            cout << xGraph << endl;
        }
    }
    return 0;
}



45. 分数的加减乘除（运算符重载）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Fraction
{
    private:
        int fz, fm;
        int commonDivisor(){}
        void contracted(){}
    public:
        Fraction(int z = 0, int m = 1)
        {
            fz = z, fm = m;
        }
        Fraction(Fraction& fra)
        {
            fz = fra.fz;
            fm = fra.fm;
        }
        Fraction operator+(Fraction fra)
        {
            fz = fz * fra.fm + fm * fra.fz;
            fm *= fra.fm;
            return *this;
        }
        Fraction operator-(Fraction fra)
        {
            fz = fz * fra.fm - fm * fra.fz;
            fm *= fra.fm;
            return *this;
        }
        Fraction operator*(Fraction fra)
        {
            fz *= fra.fz;
            fm *= fra.fm;
            return *this;
        }
        Fraction operator/(Fraction fra)
        {
            fz *= fra.fm;
            fm *= fra.fz;
            return *this;
        }
        void set(int z = 0, int m = 1)
        {
            fz = z, fm = m;
        }
        void disply()
        {
            cout << "fraction=" << fz << "/" << fm << '\n';
        }
};

int main()
{
    untie();
    int fz, fm;
    cin >> fz >> fm;    
    Fraction f1(fz, fm);
    cin >> fz >> fm;
    Fraction f2(fz, fm);
    Fraction f3(f1);
    f1 = f1 + f2;
    f1.disply();
    f1 = f3;
    f1 = f1 - f2;
    f1.disply();
    f1 = f3;
    f1 = f1 * f2;
    f1.disply();
    f1 = f3;
    f1 = f1 / f2;
    f1.disply();
    return 0;
}



46. 复数的加减乘运算（运算符重载）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Complex
{
    private:
        float real, image;
    public:
        Complex(float x = 0, float y = 0)
        {
            real = x, image = y;
        }
        friend Complex operator+(Complex& c1, Complex& c2)
        {
            Complex res;
            res.real = c1.real + c2.real;
            res.image = c1.image + c2.image;
            return res;
        }
        friend Complex operator-(Complex& c1, Complex& c2)
        {
            Complex res;
            res.real = c1.real - c2.real;
            res.image = c1.image - c2.image;
            return res;
        }
        friend Complex operator*(Complex& c1, Complex& c2)
        {
            Complex res;
            res.real = c1.real * c2.real - c1.image * c2.image;
            res.image = c1.real * c2.image + c1.image * c2.real;
            return res;
        }
        void show()
        {
            cout << "Real=" << real << " Image=" << image << '\n';
        }
};

int main()
{
    untie();
    float a, b;
    cin >> a >> b;
    Complex c1(a, b);
    cin >> a >> b;
    Complex c2(a, b);
    Complex c3;
    c3 = c1 + c2;
    c3.show();
    c3 = c1 - c2;
    c3.show();
    c3 = c1 * c2;
    c3.show();
    return 0;
}



47. 三维坐标点的平移（运算符重载）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Point
{
    int x;
    int y;
    int z;        
    public:
        Point(int xx = 0, int yy = 0, int zz = 0)
        {
            x = xx, y = yy, z = zz;
        }
        friend Point operator ++(Point &p)
        {
            p.x++, p.y++, p.z++;
            return p;
        }
        friend Point operator ++(Point &p, int)
        {
            Point temp(p.x, p.y, p.z);
            p.x++, p.y++, p.z++;
            return temp;
        }
        friend Point operator --(Point &p)
        {
            p.x--, p.y--, p.z--;
            return p;
        }
        friend Point operator --(Point &p, int)
        {
            Point temp(p.x, p.y, p.z);
            p.x--, p.y--, p.z--;
            return temp;
        }
        void show()
        {
            cout << "x=" << x << " " << "y=" << y << " " << "z=" << z << '\n';
        }
};

int main()
{
    untie();
    int x, y, z;
    cin >> x >> y >> z;
    Point p1(x, y, z), tp(p1);

    Point p2 = p1++;
    p1.show();
    p2.show();

    p1 = tp;
    p2 = ++p1;
    p1.show();
    p2.show();

    p1 = tp;
    p2 = p1--;
    p1.show();
    p2.show();

    p1 = tp;
    p2 = --p1;
    p1.show();
    p2.show();
    return 0;
}



*48. 矩阵相乘（运算符重载）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CMatrix
{
    private:
        int n, m;
        int** data; 
    public:
        CMatrix()
        {
            n = 0; 
            m = 0;
            data = NULL;
        }
        CMatrix(int n1, int m1);
        ~CMatrix();
        CMatrix &operator =(const CMatrix &a)
        {
            if(data != NULL)
            {
                for (int i = 0; i < n; i++) delete []data[i];
                delete []data;
            }
            n = a.n;
            m = a.m;
            data = new int*[n];
            for (int i = 0; i < n; i++)
            {
                data[i] = new int[m];
            }
            for(int i = 0; i < n; ++i)
				for(int j = 0; j < m; j++)
					data[i][j] = a.data[i][j];
            return *this;
        }
        int *const operator [](const int i)
        {
            return data[i];
        }
        int operator ()(int i, int j)
        {
            return data[i][j];
        }
        friend CMatrix operator *(CMatrix &a, CMatrix &b)
        {
            int n = a.n;
            CMatrix ret(n, n);
            for(int i = 0; i < n; i++)
            {
                for(int j = 0; j < n; j++)
                {
                    int sum = 0;
                    for(int k = 0; k < n; k++) 
                        sum += a[i][k] * b[k][j];
                    ret[i][j] = sum;
                }
            }
            return ret;
        }
};

CMatrix::CMatrix(int n1, int m1)
{
    n = n1;
    m = m1;
    data = new int* [n];
    for (int i = 0; i < n; i++)
        data[i] = new int[m];
    for(int i = 0; i < n; ++i)
        for(int j = 0; j < n; ++j)
            data[i][j] = 0;
    for(int i = 0; i < n; ++i) 
        data[i][i] = 1;
}

CMatrix::~CMatrix()
{
    for (int i = 0; i < n; i++)
        delete[] data[i];
    delete[] data;
}

int main()
{
    int t, n;
    cin >> t >> n;
    CMatrix ans(n, n);
    while (t--)
    {
        CMatrix now(n, n);
        for (int i = 0; i < n; i++)
            for (int j = 0; j < n; j++)
                cin >> now[i][j];
        ans = ans * now;
    }
    for (int i = 0; i < n; i++)
        for (int j = 0; j < n; j++)
            cout << ans[i][j] << " \n"[j == n - 1];
    return 0;
}



*49. 学生生日差值计算（运算符重载）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Student
{
    public:
        string name;
        int year, mon, day;
        Student(){}
        Student(string nm, int yy, int mm, int dd) : name(nm), year(yy), mon(mm), day(dd){}
        bool IsLeap(int year)
        {
            return (year % 4 == 0 || year % 400 == 0) && (year % 100 != 0);
        }
        int operator-(Student& b)
        {
            int DAY[12] = { 31,28,31,30,31,30,31,31,30,31,30,31};
            int DAY1[12] = { 31,29,31,30,31,30,31,31,30,31,30,31};
            int length1=0, length2=0;
            
            if (IsLeap(year)) {
                for (int j = 0; j < mon-1; j++) {
                    length1 += DAY1[j];
                }	
            }
            else {
                for (int j = 0; j < mon-1; j++) {
                    length1 += DAY[j];
                }	
            }
            length1 += day;

            if (IsLeap(b.year)) {
                for (int j = 0; j < b.mon-1; j++) {
                    length2 += DAY1[j];
                }
            }
            else {
                for (int j = 0; j < b.mon-1; j++) {
                    length2 += DAY[j];
                }
            }
            length2 += b.day;

            if (year == b.year) {
                return abs(length1 - length2);
            }
            else if (year > b.year) {
                int year2 = b.year;
                while(year - year2) {
                    if (IsLeap(year2)) {
                        length1 += 366;
                    }
                    else {
                        length1 += 365;
                    }
                    year2++;
                }
                return length1-length2;
            }
            else {
                int year2 = year;
                while (b.year - year2) {
                    if (IsLeap(year2)) {
                        length2 += 366;
                    }
                    else {
                        length2 += 365;
                    }
                    year2++;
                }
                return length2 - length1;
            }
        }
};

int main()
{
    untie();
    int n;
    cin >> n;
    Student *p = new Student[n];
    string ans_name[2];
    int ans_date = 0;

    string name;
    int y, m, d;
    for(int i = 0; i < n; ++i)
    {
        cin >> name >> y >> m >> d;
        p[i] = Student(name, y, m, d);
    }
    for(int i = 0; i < n; ++i)
    {
        for(int j = i + 1; j < n; ++j)
        {
            int now = p[j] - p[i];
            if(now > ans_date)
            {
                ans_date = now;
                ans_name[0] = p[i].name;
                ans_name[1] = p[j].name;
            }
        }
    }
    cout << ans_name[0] << "和" << ans_name[1] << "年龄相差最大，为" << ans_date << "天。";
    delete []p;
    return 0;
}



50. 元素查找（函数模板）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

template <typename T>
int search(int n)
{
    T key;
    T *a = new T[n + 1];
    for(int i = 1; i <= n; ++i) cin >> a[i];
    cin >> key;
    for(int i = 1; i <= n; ++i)
        if(a[i] == key) return i;
    return 0;
}

int main()
{
    untie();
    int T;
    cin >> T;
    while(T--)
    {
        string s;
        int n;
        cin >> s >> n;
        if(s[0] == 'I') cout << search<int>(n);
        else if(s[0] == 'D') cout << search<double>(n);
        else if(s[0] == 'C') cout << search<char>(n);
        else if(s[0] == 'S') cout << search<string>(n);
        cout << "\n";
    }
    return 0;
}



51. 谁的票数最高（函数模板）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

template <typename T>
void search(int n)
{
    T *a = new T[n + 1];
    map<T, int> cnt;
    for(int i = 1; i <= n; ++i) cin >> a[i], cnt[a[i]]++;
    T res;
    int num = -1;
    for(auto [x, y] : cnt)
    {
        if(y > num)
        {
            num = y;
            res = x;
        }
    }
    cout << res << " " << num;
}

int main()
{
    untie();
    int T;
    cin >> T;
    while(T--)
    {
        string s;
        int n;
        cin >> s >> n;
        if(s[0] == 'I') search<int>(n);
        else if(s[0] == 'C') search<char>(n);
        else if(s[0] == 'S') search<string>(n);
        cout << "\n";
    }
    return 0;
}



52. 抢票验证码（函数模板）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

template <typename T>
void search()
{
    int n = 6;
    T *a = new T[n + 1];
    for(int i = 1; i <= n; ++i) cin >> a[i];
    for(int i = 2; i <= n; ++i)
    {
        if(a[i] < a[i - 1]) 
        {
            cout << "Invalid";
            return;
        }
    }
    cout << "Valid";
}

int main()
{
    untie();
    string s;
    while(cin >> s)
    {
        if(s[0] == 'i') search<int>();
        else if(s[0] == 'c') search<char>();
        else if(s[0] == 'f') search<float>();
        cout << "\n";
    }
    return 0;
}



53. 矩阵类模板（类模板）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

template <class T = int>
class CMatrix
{
    private:
        int n, m, maxn;
        T** data; 
    public:
        CMatrix()
        {
            n = 0; 
            m = 0;
            maxn = 0;
            data = NULL;
        }
        CMatrix(int n1, int m1)
        {
            n = n1;
            m = m1;
            maxn = max(n, m);
            data = new T*[maxn];
            for (int i = 0; i < maxn; i++)
                data[i] = new T[maxn];
            for(int i = 0; i < n; ++i)
                for(int j = 0; j < m; ++j)
                    cin >> data[i][j];
        }
        ~CMatrix()
        {
            for (int i = 0; i < maxn; i++)
                delete[] data[i];
            delete[] data;
        }
        CMatrix &operator =(const CMatrix &a)
        {
            if(data != NULL)
            {
                for (int i = 0; i < maxn; i++) delete []data[i];
                delete []data;
            }
            n = a.n;
            m = a.m;
            data = new T*[maxn];
            for (int i = 0; i < maxn; i++)
            {
                data[i] = new T[maxn];
            }
            for(int i = 0; i < n; ++i)
				for(int j = 0; j < m; j++)
					data[i][j] = a.data[i][j];
            return *this;
        }
        T *const operator [](const int i)
        {
            return data[i];
        }
        T operator ()(int i, int j)
        {
            return data[i][j];
        }
        void transport()
        {
            T** tp = new T*[maxn];
            for(int i = 0; i < maxn; ++i)
            {
                tp[i] = new T[maxn];
            }
            for(int j = 0; j < n; ++j)
                for(int i = 0; i < m; ++i)
                    tp[i][j] = data[j][i];
            swap(n, m);
            for(int i = 0; i < n; ++i)
                for(int j = 0; j < m; ++j)
                    data[i][j] = tp[i][j];
            for (int i = 0; i < maxn; i++)
                delete[] tp[i];
            delete[] tp;
        }
        void Print()
        {
            for(int i = 0; i < n; ++i)
				for(int j = 0; j < m; j++)
                    cout << data[i][j] << " \n"[j == m - 1];
        }
};

int main()
{
    untie();
    int T;
    cin >> T;
    while(T--)
    {
        int n, m;
        string s;
        cin >> s >> n >> m;
        if(s[0] == 'I')
        {
            CMatrix<int> mat(n, m);
            mat.transport();
            mat.Print();
        }
        else if(s[0] == 'C')
        {
            CMatrix<char> mat(n, m);
            mat.transport();
            mat.Print();
        }
        else if(s[0] == 'D')
        {
            CMatrix<double> mat(n, m);
            mat.transport();
            mat.Print();
        }
    }
    return 0;
}
/*
实际上该题这样也能运行：
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

template <class T = int>
class CMatrix
{
    private:
        int n, m, maxn;
        T** data; 
    public:
        CMatrix()
        {
            n = 0; 
            m = 0;
            maxn = 0;
            data = NULL;
        }
        CMatrix(int n1, int m1)
        {
            n = n1;
            m = m1;
            maxn = max(n, m);
            data = new T*[maxn];
            for (int i = 0; i < maxn; i++)
                data[i] = new T[maxn];
            for(int i = 0; i < n; ++i)
                for(int j = 0; j < m; ++j)
                    cin >> data[i][j];
        }
        ~CMatrix()
        {
            for (int i = 0; i < maxn; i++)
                delete[] data[i];
            delete[] data;
        }
        void transport()
        {
            T** tp = new T*[maxn];
            for(int i = 0; i < maxn; ++i)
            {
                tp[i] = new T[maxn];
            }
            for(int j = 0; j < n; ++j)
                for(int i = 0; i < m; ++i)
                    tp[i][j] = data[j][i];
            swap(n, m);
            for(int i = 0; i < n; ++i)
                for(int j = 0; j < m; ++j)
                    data[i][j] = tp[i][j];
            for (int i = 0; i < maxn; i++)
                delete[] tp[i];
            delete[] tp;
        }
        void Print()
        {
            for(int i = 0; i < n; ++i)
				for(int j = 0; j < m; j++)
                    cout << data[i][j] << " \n"[j == m - 1];
        }
};

int main()
{
    untie();
    int T;
    cin >> T;
    while(T--)
    {
        int n, m;
        string s;
        cin >> s >> n >> m;
        if(s[0] == 'I')
        {
            CMatrix<int> mat(n, m);
            mat.transport();
            mat.Print();
        }
        else if(s[0] == 'C')
        {
            CMatrix<char> mat(n, m);
            mat.transport();
            mat.Print();
        }
        else if(s[0] == 'D')
        {
            CMatrix<double> mat(n, m);
            mat.transport();
            mat.Print();
        }
    }
    return 0;
}
*/


        
54. 简单类模板(类模板)
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

template <class T = int>
class CList
{
    private:
        int n;
        T a[105];
    public:
        CList(){}
        CList(int nn)
        {
            n = nn;
            memset(a, -1, sizeof(a));
            for(int i = 0; i < n; ++i) cin >> a[i];
        }
        void Insert(int pos, T val)
        {
            for(int i = n + 1; i > pos; --i) a[i] = a[i - 1];
            a[pos] = val;
            ++n;
        }
        void Delete(int pos)
        {
            for(int i = pos; i < n - 1; ++i) a[i] = a[i + 1];
            a[n - 1] = -1;
            --n;
        }
        void Print()
        {
            for(int i = 0; i < n; ++i)
                cout << a[i] << " \n"[i == n - 1];
        }
};

int main()
{
    untie();
    int n, ins_pos, del_pos;
    int ins_val1;
    double ins_val2;

    cin >> n;
    CList<int> lis1(n);
    cin >> ins_pos >> ins_val1 >> del_pos;
    lis1.Insert(ins_pos, ins_val1);
    lis1.Delete(del_pos);
    lis1.Print();

    cin >> n;
    CList<double> lis2(n);
    cin >> ins_pos >> ins_val2 >> del_pos;
    lis2.Insert(ins_pos, ins_val2);
    lis2.Delete(del_pos);
    lis2.Print();
    
    return 0;
}



55. 音像制品（类与对象）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
string base[5] = {"", "黑胶片", "CD", "VCD", "DVD"};

class Product
{
    private:
        int type, rent, status;
        string name;
    public:
        Product(){}
        Product(int a, string s, int c, int d)
        {
            type = a, name = s, rent = c, status = d;
        }
        void getRent(int x)
        {
            if(status) cout << "当前租金为" << rent * x << '\n';
            else cout << "未产生租金\n";
        }
        void Print()
        {
            cout << base[type] << "[" << name << "]" << (status ? "已出租" : "未出租") << "\n";
        }
};

int main()
{
    untie();
    int n;
    cin >> n;
    for(int i = 0; i < n; ++i) 
    {
        int a, b, c, op;
        string name;
        cin >> a >> name >> b >> c >> op;
        Product p(a, name, b, c);
        p.Print();
        if(op) p.getRent(op);
    }
    return 0;
}



56. 拯救小明（多继承+友元）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class Date
{
    protected:
        int year, month, day;
    public:
        Date(){}
        Date(int a, int b, int c) : year(a), month(b), day(c){}
};

class Time
{
    protected:
        int hour, minute, second;
    public:
        Time(){}
        Time(int a, int b, int c) : hour(a), minute(b), second(c){}
};

class Work : public Date, public Time
{
    protected:
        int id;
    public:
        Work(){}
        Work(int a, int b, int c, int d, int e, int f, int x) : Date(a, b, c), Time(d, e, f), id(x){}
        friend bool before(const Work &w1, const Work &w2)
        {
            if(w1.year == w2.year)
            {
                if(w1.month == w2.month)
                {
                    if(w1.day == w2.day)
                    {
                        if(w1.hour == w2.hour)
                        {
                            if(w1.hour == w2.hour)
                            {
                                if(w1.minute == w2.minute)
                                {
                                    return w1.second < w2.second;
                                }
                                return w1.minute < w2.minute;
                            }
                            return w1.hour < w2.hour;
                        }
                        return w1.hour < w2.hour;
                    }
                    return w1.day < w2.day;
                }
                return w1.month < w2.month;
            }
            return w1.year < w2.year;
        }
        void Print()
        {
            cout << "The urgent Work is No." << id << ": " << year << "/" << (month < 10 ? "0" : "") << month << "/" << (day < 10 ? "0" : "") << day << " " << (hour < 10 ? "0" : "") << hour << ":" << (minute < 10 ? "0" : "") << minute << ":" << (second < 10 ? "0" : "") << second << '\n';
        }
};

int main()
{
    untie();
    int id, y, m, d, hour, min, sec;
    cin >> id >> y >> m >> d >> hour >> min >> sec;
    Work ans(y, m, d, hour, min, sec, id);
    while(cin >> id)    
    {
        if(!id) break;
        cin >> y >> m >> d >> hour >> min >> sec;
        Work now(y, m, d, hour, min, sec, id);
        if(before(now, ans)) ans = now;
    }
    ans.Print();
    return 0;
}



57. 宠物的生长（虚函数和多态）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CDate
{
    private:
        int year, month, day;
    public:
        CDate(){}
        CDate(int y, int m, int d) : year(y), month(m), day(d){}
        int getDays() // 以日期 1-1-1 为基点
        {
            auto isLeap = [](int y){ return (y % 4 == 0 && y % 100 != 0) || y % 400 == 0;};
            int d[13] = {0, 31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31};
            if(isLeap(year)) d[2]++;
            int res = day;
            for(int i = 1; i < year; ++i) res += 365 + isLeap(i);
            for(int i = 1; i < month; ++i) res += d[i];
            return res;
        }
        friend int count(CDate c1, CDate c2)
        {
            return c2.getDays() - c1.getDays();
        }
};

class Pet
{
    protected:
        string name;
        float length, weight;
        CDate current;
    public:
        Pet(){}
        Pet(string nm, float a, float b, int y, int m, int d) : current(y, m, d), name(nm), length(a), weight(b){}
        virtual void display(CDate day) = 0;
};

class Cat : public Pet
{
    public:
        Cat(){}
        Cat(string nm, float a, float b, int y, int m, int d) : Pet(nm, a, b, y, m, d){}
        void display(CDate day)
        {
            int sum = count(day, current);
            if(sum < 0) cout << "error\n";
            else cout << name << " after " << sum << " day: length=" << length + 0.1 * sum << ",weight=" << weight + 0.2 * sum << '\n';
        }
};

class Dog : public Pet
{
    public:
        Dog(){}
        Dog(string nm, float a, float b, int y, int m, int d) : Pet(nm, a, b, y, m, d){}
        void display(CDate day)
        {
            int sum = count(day, current);
            if(sum < 0) cout << "error\n";
            else cout << name << " after " << sum << " day: length=" << length + 0.2 * sum << ",weight=" << weight + 0.1 * sum << '\n';
        }
};

int main()
{
    untie();
    int T, y, m, d;
    string name;
    float len, wei;
    cin >> T >> y >> m >> d;
    cout << fixed << setprecision(2);
    CDate start = CDate(y, m, d);
    Pet *pt = NULL;
    while(T--)
    {
        int op;
        cin >> op >> name >> len >> wei >> y >> m >> d;
        if(op == 1) pt = new Cat(name, len, wei, y, m, d);
        else pt = new Dog(name, len, wei, y, m, d);
        pt -> display(start);
    }
    return 0;
}



58. 集合（运算符重载）
//做法1：友元重载二元运算符
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CSet
{
    private:
        int n;
        int *data;
    public:
        CSet(){ n = 0, data = NULL;}
        CSet(int nn, int *num = NULL) : n(nn)
        {
            data = new int[n];
            if(num != NULL)
                for(int i = 0; i < n; ++i) data[i] = num[i];
        }
        ~CSet()
        {
            delete []data;
            data = NULL;
        }
        friend CSet operator +(const CSet &c1, const CSet &c2)
        {
            map<int, bool> mp;
            CSet res(c1.n + c2.n, c1.data);
            for(int i = 0; i < c1.n; ++i) mp[c1.data[i]] = 1;
            int j = c1.n;
            for(int i = 0; i < c2.n; ++i)
                if(!mp.count(c2.data[i])) res.data[j++] = c2.data[i];
            res.n = j;
            return res;
        }
        friend CSet operator -(const CSet &c1, const CSet &c2)
        {
            map<int, bool> mp;
            CSet res(max(c1.n, c2.n));
            for(int i = 0; i < c2.n; ++i) mp[c2.data[i]] = 1;
            int j = 0;
            for(int i = 0; i < c1.n; ++i)
                if(!mp.count(c1.data[i])) res.data[j++] = c1.data[i];
            res.n = j;
            return res;
        }
        friend CSet operator *(const CSet &c1, const CSet &c2)
        {
            map<int, bool> mp;
            CSet res(min(c1.n, c2.n));
            for(int i = 0; i < c2.n; ++i) mp[c2.data[i]] = 1;
            int j = 0;
            for(int i = 0; i < c1.n; ++i)
                if(mp.count(c1.data[i])) res.data[j++] = c1.data[i];
            res.n = j;
            return res;
        }
        friend ostream &operator <<(ostream &cout, CSet &st)
        {
            for(int i = 0; i < st.n; ++i) cout << st.data[i] << " \n"[i == st.n - 1];
            return cout;
        }
};

int main()
{
    untie();
    int T;
    cin >> T;
    while(T--)
    {
        int n;
        cin >> n;
        int *num1 = new int[n];
        for(int i = 0; i < n; ++i) cin >> num1[i];
        CSet A(n, num1);

        cin >> n;
        int *num2 = new int[n];
        for(int i = 0; i < n; ++i) cin >> num2[i];
        CSet B(n, num2);
        CSet ans1 = A + B, ans2 = A * B, ans3 = (A - B) + (B - A);
        cout << "A:" << A;
        cout << "B:" << B;
        cout << "A+B:" << ans1;
        cout << "A*B:" << ans2;
        cout << "(A-B)+(B-A):" << ans3;
        if(T) cout << "\n";
        delete []num1;
        delete []num2;
    }
    return 0;
}
//做法2：引用返回类型重载二元运算符（但会导致第一个算子被改变）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CSet
{
    private:
        int n;
        int *data;
    public:
        CSet(){ n = 0, data = NULL;}
        CSet(int nn, int *num = NULL) : n(nn)
        {
            data = new int[n];
            if(num != NULL)
                for(int i = 0; i < n; ++i) data[i] = num[i];
        }
        ~CSet()
        {
            if(data != NULL)
                delete []data;
            data = NULL;
        }
        CSet &operator =(const CSet &c)
        {
            if(data != NULL) delete []data;
            n = c.n;
            data = new int[n];
            for(int i = 0; i < n; ++i) data[i] = c.data[i];
            return *this;
        }
        CSet &operator +(const CSet &c)
        {
            map<int, bool> mp;
            vector<int> v;
            for(int i = 0; i < n; ++i) mp[data[i]] = 1;
            for(int i = 0; i < c.n; ++i)
                if(!mp.count(c.data[i])) v.push_back(c.data[i]);
            CSet res(n + v.size(), data);
            for(int i = n; i < n + v.size(); ++i) res.data[i] = v[i - n];
            *this = res;
            return *this;
        }
        CSet &operator -(const CSet &c)
        {
            map<int, bool> mp;
            vector<int> v;
            for(int i = 0; i < c.n; ++i) mp[c.data[i]] = 1;
            for(int i = 0; i < n; ++i)
                if(!mp.count(data[i])) v.push_back(data[i]);
            CSet res(v.size());
            for(int i = 0; i < v.size(); ++i) res.data[i] = v[i];
            *this = res;
            return *this;
        }
        CSet &operator *(const CSet &c)
        {
            map<int, bool> mp;
            vector<int> v;
            for(int i = 0; i < c.n; ++i) mp[c.data[i]] = 1;
            for(int i = 0; i < n; ++i)
                if(mp.count(data[i])) v.push_back(data[i]);
            CSet res(v.size());
            for(int i = 0; i < v.size(); ++i) res.data[i] = v[i];
            *this = res;
            return *this;
        }
        friend ostream &operator <<(ostream &cout, CSet &st)
        {
            for(int i = 0; i < st.n; ++i) cout << st.data[i] << " \n"[i == st.n - 1];
            return cout;
        }
};



int main()
{
    untie();
    int T;
    cin >> T;
    while(T--)
    {
        int n;
        cin >> n;
        int *num1 = new int[n];
        for(int i = 0; i < n; ++i) cin >> num1[i];
        CSet A(n, num1), A1(n, num1), A2(n, num1), A3(n, num1);

        cin >> n;
        int *num2 = new int[n];
        for(int i = 0; i < n; ++i) cin >> num2[i];
        CSet B(n, num2), B1(n, num2);

        cout << "A:" << A;
        cout << "B:" << B;
        cout << "A+B:" << A + B;
        cout << "A*B:" << A1 * B;
        cout << "(A-B)+(B-A):" << (A2 - B) + (B1 - A3);

        if(T) cout << "\n";
        delete []num1;
        delete []num2;
    }
    return 0;
}



59. 最贵的书（重载+友元+引用）
// 尽量使用冲突少的 c++ 11，这里 c++ 98 会报错
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long

class CBook
{
    private:
        string name, editor, press;
        double price;
    public:
        CBook(){}
        CBook(string nm, string edit, double pri, string pre) : name(nm), editor(edit), price(pri), press(pre){}
        friend void find(CBook *book, int n, int &max1index, int &max2index)
        {
            double m1 = -1, m2 = -1;
            for(int i = 0; i < n; ++i)
                if(book[i].price > m1)
                    max1index = i, m1 = book[i].price;
            for(int i = 0; i < n; ++i)
                if(i != max1index && book[i].price > m2)
                    max2index = i, m2 = book[i].price;
        }
        friend istream &operator >>(istream &cin, CBook &b)
        {
            string s[4];
            for(int i = 0; i < 4; ++i) getline(cin, s[i], i == 3 ? '\n' : ',');
            b = CBook(s[0], s[1], stod(s[2]), s[3]);
            return cin;
        }
        friend ostream &operator <<(ostream &cout, CBook &b)
        {
            cout << b.name << "\n" << b.editor << "\n" << b.price << "\n" << b.press << "\n";
            return cout;
        }
};

int main()
{
    untie();
    int T;
    cin >> T;
    cout << fixed << setprecision(2);
    while(T--)
    {
        int n, ind1 = 0, ind2 = 0;
        cin >> n;
        CBook *p = new CBook[n];
        for(int i = 0; i < n; ++i) cin >> p[i];
        find(p, n, ind1, ind2);
        cout << p[ind1] << '\n';
        cout << p[ind2];
    }
    return 0;
}



60. 一、会员积分（期末模拟）
#include <iostream>
#include <algorithm>
#include <string>
#include <cstring>
#include <functional>
#include <iomanip>
using namespace std;

class Member
{
    protected:
        int id, score;
        string name;        
    public:
        Member(){}
        Member(int ID, int s, string nm) : id(ID), score(s), name(nm){}
        virtual void add(int m)
        {
            score += m;
        }
        virtual int exchange(int s)
        {
            if(score - s / 100 * 100 >= 0) score -= s / 100 * 100;
            return s / 100;
        }
        virtual void Print()
        {
            cout << "普通会员" << id << "--" << name << "--" << score << '\n';
        }
};

class VIP : public Member
{
    protected:
        int add_rate, exc_rate;
    public:
        VIP(){}
        VIP(int ID, int s, string nm, int r1, int r2) : Member(ID, s, nm)
        {
            add_rate = r1;
            exc_rate = r2;
        }
        virtual void add(int m)
        {
            score += add_rate * m;
        }
        virtual int exchange(int s)
        {
            if(exc_rate == 0) return 0;
            if(score - s / exc_rate * exc_rate >= 0) score -= s / exc_rate * exc_rate;
            return s / exc_rate;
        }
        virtual void Print()
        {
            cout << "贵宾会员" << id << "--" << name << "--" << score << '\n';
        }
};

int main()
{
    Member *pm;
    int id, score, cost, exc, r1, r2;
    string name;

    cin >> id >> name >> score;
    Member mm(id, score, name);
    pm = &mm;
    cin >> cost >> exc;
    pm->add(cost);
    pm->exchange(exc);
    pm->Print();

    cin >> id >> name >> score >> r1 >> r2;
    VIP vv(id, score, name, r1, r2);
    pm = &vv;
    cin >> cost >> exc;
    pm->add(cost);
    pm->exchange(exc);
    pm->Print();

    pm = NULL;
    return 0;
}



61. 二、金属加工（期末模拟）
#include <iostream>
#include <algorithm>
#include <string>
#include <cstring>
#include <functional>
#include <iomanip>
using namespace std;

class Metal
{
    private:
        int hard, weight, vol;
    public:
        Metal(){}
        Metal(int h, int w, int v) : hard(h), weight(w), vol(v){}
        friend Metal operator +(const Metal &m1, const Metal &m2)
        {
            return Metal(m1.hard + m2.hard, m1.weight + m2.weight, m1.vol + m2.vol);
        }
        friend Metal operator *(const Metal &m, int rate)
        {
            return Metal(m.hard, m.weight, m.vol * rate);
        }
        Metal &operator ++()
        {
            hard++;
            weight += weight / 10;
            vol += vol / 10;
            return *this;
        }
        Metal operator --(int)
        {
            Metal res(hard, weight, vol);
            hard--;
            weight -= weight / 10;
            vol -= vol / 10;
            return res;
        }
        void Print()
        {
            cout << "硬度" << hard << "--重量" << weight << "--体积" << vol << "\n";
        }
};

int main()
{
    int hard, w, v, n;
    cin >> hard >> w >> v;
    Metal m1(hard, w, v);
    cin >> hard >> w >> v;
    Metal m2(hard, w, v);

    Metal temp = m1 + m2;
    temp.Print();

    cin >> n;
    temp = m1 * n;
    temp.Print();

    ++m1;
    m1.Print();

    m2--;
    m2.Print();

    return 0;
}



62. 三、加密模板（期末模拟）
#include <iostream>
#include <algorithm>
#include <string>
#include <cstring>
#include <functional>
#include <iomanip>
using namespace std;

template<typename T>
T max(T num[], int n)
{
    T x = num[0];
    for(int i = 1; i < n; ++i)
        if(num[i] > x) x = num[i];
    return x;
}

template<typename T>
class Cryption
{
    private:
        T ptxt[100]; //明文
        T ctxt[100]; //密文
        T key; //密钥
        int len; //长度
    public:
        Cryption(T tk, T tt[], int n); //参数依次对应密钥、明文、长度
        void encrypt(); //加密
        void print() //打印，无需改造
        {
            int i;
            for (i = 0; i < len - 1; i++)
            {
                cout << ctxt[i] << " ";
            }
            cout << ctxt[i] << endl;
        }
};

template<typename T>
Cryption<T>::Cryption(T tk, T tt[], int n)
{
    key = tk;
    len = n;
    for(int i = 0; i < len; ++i) ptxt[i] = tt[i];
}

template<typename T>
void Cryption<T>::encrypt()
{
    T maxn = max<T>(ptxt, len);
    for(int i = 0; i < len; ++i) ctxt[i] = maxn - ptxt[i] + key;
}

//支持三种类型的主函数
int main()
{
	int i;
	int length; //长度
	int ik, itxt[100];
	double dk, dtxt[100];
	char ck, ctxt[100];
	//整数加密
	cin >> ik >> length;
	for (i = 0; i < length; i++)
	{
		cin >> itxt[i];
	}
	Cryption<int> ic(ik, itxt, length);
	ic.encrypt();
	ic.print();
	//浮点数加密
	cin >> dk >> length;
	for (i = 0; i < length; i++)
	{
		cin >> dtxt[i];
	}
	Cryption<double> dc(dk, dtxt, length);
	dc.encrypt();
	dc.print();
	//字符加密
	cin >> ck >> length;
	for (i = 0; i < length; i++)
	{
		cin >> ctxt[i];
	}
	Cryption<char> cc(ck, ctxt, length);
	cc.encrypt();
	cc.print();

	return 0;
}



63. *四、加湿风扇（期末模拟）-- 区分虚基类、基类、派生类
#include <iostream>
#include <algorithm>
#include <string>
#include <cstring>
#include <functional>
#include <iomanip>
using namespace std;

class EletricApp // 虚基类
{
    protected:
        int id, power;
    public:
        EletricApp(){}
        EletricApp(int ID, int p) : id(ID), power(p){}
        virtual void Print() = 0;// 抽象类
};

class Fan : virtual public EletricApp // 基类
{
    protected:
        int dir, force;
    public:
        Fan(){}
        Fan(int ID, int p, int d, int f) : EletricApp(ID, p), dir(d), force(f){}
        void dir_control(int x){ dir = x;}
        void force_control(int x){ force = x;}
};

class Humidifier : virtual public EletricApp // 基类
{
    protected:
        double cap_actual, cap_max;
    public:
        Humidifier(){}
        Humidifier(int ID, int p, double act, double maxn) : EletricApp(ID, p), cap_actual(act), cap_max(maxn){}
        int preWarn()
        {
            if(cap_actual >= cap_max / 2) return 1;
            else if(cap_actual >= cap_max / 10) return 2;
            return 3;
        }
};

class HumidifyFan : public Fan, public Humidifier // 派生类
{
    protected:
        int proper;
    public:
        HumidifyFan(){}
        HumidifyFan(int ID, int p, int d, int f, double act, double maxn, int pro) : EletricApp(ID, p), Fan(ID, p, d, f), Humidifier(ID, p, act, maxn), proper(pro){}
        void pro_control(int x)
        {
            proper = x;
            if(x != 0)
            {
                force_control(x);
                if(x == 1) dir_control(0);
                else dir_control(1);
            }
        }
        void Print()
        {
            string op[4] = {"", "正常", "偏低", "不足"};
            cout << "加湿风扇--档位" << proper << "\n";
            cout << "编号" << id << "--功率" << power << "W\n";
            cout << (dir ? "旋转吹风" : "定向吹风") << "--风力" << force << "级\n";
            cout << "实际水容量" << cap_actual << "升--水量" << op[preWarn()] << "\n";
        }
};

int main()
{
	int T;
    cin >> T;
    while(T--)
    {
        int id, power, dir, force, proper, proper_next;
        double cap_act, cap_max;
        cin >> id >> power >> dir >> force >> cap_act >> cap_max >> proper;
        HumidifyFan hFan(id, power, dir, force, cap_act, cap_max, proper);
        cin >> proper_next;
        hFan.pro_control(proper_next);
        hFan.Print();
    }
	return 0;
}



64. 五、计重转换（期末模拟）
#include <iostream>
#include <algorithm>
#include <string>
#include <cstring>
#include <functional>
#include <iomanip>
using namespace std;

class CN; //提前声明
class EN; //提前声明

// 抽象类
class Weight
{
    protected:
        char kind[20]; //计重类型
        int gram; // 克
    public:
        Weight(const char tk[] = "no name", int tg = 0)
        {
            strcpy(kind, tk);
            gram = tg;
        }
        virtual void print(ostream& out) = 0; // 输出不同类型的计重信息
};

// 中国计重
class CN : public Weight
{
	protected:
        int jin, liang, qian;
    public:
        CN(int a = 0, int b = 0, int c = 0, int tg = 0, const char tk[] = "no name") : Weight(tk, tg), jin(a), liang(b), qian(c){}
        void Convert(int g)
        {
            jin = g / 500;
            g %= 500;
            liang = g / 50;
            g %= 50;
            qian = g / 5;
            g %= 5;
            gram = g;
        }
        void print(ostream& out)
        {
            out << "中国计重:" << jin << "斤" << liang << "两" << qian << "钱" << gram << "克\n";
        }
};

// 英国计重
class EN : public Weight
{
	protected:
        int pound, ounce, duran;
    public:
        EN(int d = 0, int e = 0, int f = 0, int tg = 0, const char tk[] = "no name") : Weight(tk, tg), pound(d), ounce(e), duran(f){}
        void Convert(int g)
        {
            pound = g / 512;
            g %= 512;
            ounce = g / 32;
            g %= 32;
            duran = g / 2;
            g %= 2;
            gram = g;
        }
        void print(ostream& out)
        {
            out << "英国计重:" << pound << "磅" << ounce << "盎司" << duran << "打兰" << gram << "克\n";
        }
        operator CN()
        {
            int g = gram + duran * 2 + ounce * 32 + pound * 512;
            CN res;
            res.Convert(g);
            return res;
        }
};

// 以全局函数方式重载输出运算符，代码3-5行....自行编写
// 重载函数包含两个参数：ostream流对象、Weight类对象，参数可以是对象或对象引用
// 重载函数必须调用参数Weight对象的print方法
ostream &operator <<(ostream &out, Weight &w)
{
    w.print(out);
    return out;
}

// 主函数
int main()
{
	int tw;
	// 创建一个中国计重类对象cn
	// 构造参数对应斤、两、钱、克、类型，其中克和类型是对应基类属性gram和kind
	CN cn(0, 0, 0, 0, "中国计重");
	cin >> tw;
	cn.Convert(tw); // 把输入的克数转成中国计重
	cout << cn;

	// 创建英国计重类对象en
	// 构造参数对应磅、盎司、打兰、克、类型，其中克和类型是对应基类属性gram和kind
	EN en(0, 0, 0, 0, "英国计重");
	cin >> tw;
	en.Convert(tw); // 把输入的克数转成英国计重
	cout << en;
	cn = en; // 把英国计重转成中国计重
	cout << cn;
	return 0;
}

```

``
