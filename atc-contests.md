# **atcoder contests **
---
# ABC

---

## **ABC 292**

A 、B 签到

### C
题意：给一个整数n，问有多少种(A, B, C, D)数对满足 A*B + C*D = n。

思路：转化为 x + y = n，先计算每个数的因数对的个数存在 arr[] 中，如 x 有多少个(A, B)使得 A*B = x。这样就能知道每对(x, y)能产生多少(A, B, C, D)方案，然后枚举 x (y = n - x)，计数即可。

```c++
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
typedef long long ll;
const int N = 2e5 + 5;
ll cnt = 0;
ll arr[N] = {1};
int n;
// void init() //149ms
// {
//     for(int i = 1; i <= n; i++)
//     {
//         ll sum = 1, tp = 1;
//         for(int j = 2; j * j <= i; j++)
//         {
//             if(i % j == 0)
//             {
//                 ++sum;
//             }
//             tp = j;
//         }
//         arr[i] = 2 * sum - (tp * tp == i);
//     }
// }
void init()//更好的写法 12ms
{
    for(int i = 1; i < n; i++)//枚举两个因数
        for(int j = 1; i * j <= n; j++)
            arr[i * j]++;//这个乘积组合种数加1
}
int main()
{
    untie();
    cin >> n;
    init();
    for(int x = 1; x < n; x++) cnt += arr[x] * arr[n - x];
    cout << cnt;
    return 0;
}
```
### D
题意：问图中每个连通块是否都满足该块内顶点个数与边个数相等（简单图论问题）

思路：借助并查集管理每个连通块

```c++
#include <iostream>
#include <algorithm>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 2e5 + 5;
struct component{
    int edge, node;
}cnt[N];//每个连通块
int n, m;
int ds[N];
void init_set()
{ 
    for(int i = 1; i <= n; i++) 
    {
        ds[i] = i;
        cnt[i].edge = 0;
        cnt[i].node = 1;
    }
}
int find_set(int x){ return x == ds[x] ? x : (ds[x] = find_set(ds[x]));}
int main()
{
    untie();
    cin >> n >> m;
    init_set();
    for(int i = 1; i <= m; i++)
    {
        int u, v;
        cin >> u >> v;
        int uu = find_set(u), vv = find_set(v);
        if(uu == vv) cnt[uu].edge++;
        else
        {
            ds[vv] = uu;//将vv并入uu所在集合
            cnt[uu].edge += cnt[vv].edge + 1;
            cnt[uu].node += cnt[vv].node;
        }
    }
    for(int i = 1; i <= n; i++)
    {
        int x = find_set(i);
        if(cnt[x].edge != cnt[x].node)
        {
            cout << "No";
            return 0;
        }
    }
    cout << "Yes";
    return 0;
}
```
### E 

（本题题意理解起来较麻烦）

题意：给一个有向图，给一个目标图，该目标图的每个点u满足，u与其在原图中所能到达的所有点v都要有直连边。

思路：bfs遍历每个点u作为起点，连通路径上的其他所有点都要与u建边，计数cnt++，最后减去已有边数就是要操作的次数。

```c++
#include <iostream>
#include <algorithm>
#include <vector>
#include <queue>
#include <cstring>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 2e3 + 5;
int n, m, cnt = 0;
bool vis[N];
vector<int> G[N];
void bfs(int s)
{
    memset(vis, 0, sizeof(vis));
    queue<int> q;
    q.push(s);
    vis[s] = 1;
    while(q.size())
    {
        int u = q.front();
        q.pop();
        for(int i = 0; i < G[u].size(); i++)
        {
            int v = G[u][i];
            if(!vis[v])
            {
                vis[v] = 1;
                ++cnt;
                q.push(v);
            }
        }
    }
}
int main()
{
    untie();
    cin >> n >> m;
    for(int i = 0; i < m; i++)
    {
        int u, v;
        cin >> u >> v;
        G[u].push_back(v);
    }
    for(int i = 1; i <= n; i++) bfs(i);
    cnt -= m;
    cout << cnt;
    return 0;
}
```
### F
题意：数学几何问题，求矩形(A, B)的内部正三角形的最长可能边长。

- 设 A >= B

- 我们先假设A远大于B，显然此时以B为高的正三角形边长最大，即 

  B / cos(pi / 6) = 2 * B / sqrt(3)                         																							(1)

- 而当A = 12, B = 11此类情况时上述结果大于A，显然不成立，故上述结论不能应用到任意 A > B

- 假设长为A，宽为B的矩形，规定最大正三角形一个顶点在矩形右上角，一个顶点在矩形底长边，而第三个点可能在边上也可能不在边上

- 这样矩形右侧就形成了一个以边B为其中一条直角边、以正三角形边的斜边的一个封闭直角三角形

- 设三角形最近一条边与边B形成角θ，则最近一条边与边A形成角 (pi / 2 - θ - pi / 3) = pi / 6 - θ
  设最大边为L，得到两个方程 Lcosθ = B， Lcos(pi / 6 - θ) = A，联立得 θ = arctan(2 * A / B - sqrt(3))
  此时代入 

  L = B / cosθ 																																				  (2)

- 最后只要判断结果(1)和结果(2)哪个合理且哪个最大，res1和res2都可能会超过A，也只有一个会超过A，而实际上结果(1)和(2)有且只有一个是合理的，则答案便是两者最小值

```c++
//AC
#include <iostream>
#include <algorithm>
#include <cmath>
#include <cstdio>
using namespace std;
int main()
{
    int a, b;
    scanf("%d%d", &a, &b);
    if(a < b) swap(a, b);
    double res1 = 2.0 * b / sqrt(3.0);
    double res2 = 1.0 * b / cos(atan(2.0 * a / b - sqrt(3.0)));
    // if(res1 > a) printf("%.20lf\n", res2);
    // else printf("%.20lf\n",res1);
    // 等同下句
    printf("%.20lf\n", min(res1, res2));
    return 0;
}


//PA    57 pts / 70 pts 
#include <iostream>
#include <algorithm>
#include <cmath>
#include <cstdio>
using namespace std;
const long double MAXL = sqrt(6.0L) - sqrt(2.0L);
int main()
{
    int a, b;
    scanf("%d%d", &a, &b);
    if(a < b) swap(a, b);
    if(a > b) 
    {
        long double ml = 2.0L * b / sqrt(3.0L);
        if(ml > a)  printf("%.20Lf\n", 1.0L * b * MAXL);
        else printf("%.20Lf\n", ml);
    }
    else printf("%.20Lf\n", 1.0L * a * MAXL);
    return 0;
}
```
---

## **ABC  293**























---

**ABC  294**

A、B 签到

### C

题意：给两个序列，分别长为 N、M 的严格不等序列 A、B，现将它们混合成序列 C 并升序排序，记录它们在 C 中的位置（从下标 1 开始），随后输出两行，一行为序列 A 各数在 C 中的位置，一行为序列 B 各数在 C 中的位置（按它们在原序列的顺序输出）。

思路：直接模拟题意，开两倍空间的数组C存入A、B所有数并排序，再用map映射<值， 在C中的位置>即可。

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <set>
#include <map>
#include <queue>
#include <cctype>
#include <cmath>
#include <set>
#include <map>
#include <queue>
#include <cstring>
#include <string>
#include <vector>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 1e5 + 5;
int n, m;
int a[N], b[N], c[2 * N];
map<int, int> mp;
int main()
{
    untie();
    cin >> n >> m;
    int cnt = 0;
    for(int i = 1; i <= n; i++)
    {
        cin >> a[i];
        c[++cnt] = a[i];
    }
    for(int i = 1; i <= m; i++)
    {
        cin >> b[i];
        c[++cnt] = b[i];
    }
    sort(c + 1, c + 1 + cnt);
    for(int i = 1; i <= cnt; i++) mp[c[i]] = i;
    for(int i = 1; i <= n; i++)
    {
        cout << mp[a[i]];
        if(i != n) cout << " ";
        else cout << '\n';
    }
    for(int i = 1; i <= m; i++)
    {
        cout << mp[b[i]];
        if(i != m) cout << " ";
        else cout << '\n';
    }
    return 0;
}
```

### D

题意：有 `N` 个人各有自己的编号`id`，`Q` 次事件，每个人的状态由 “（没）被打电话”、“（没）被叫走” 任意组合。有三种操作：1.给没被打过电话中最小编号的人打电话；2.把第 `x` 个人叫走；3.给已经被打过电话但没被叫走的最小编号者打电话

思路：由于题目约定中，保证操作3一定有解，即一定是以操作1已经进行过的前提，即此时最小编号的人一定被打过电话，所以我们只需要维护“被叫走”的状态即可 -- 用数组 `vis[N]` 维护，对操作2有 `vis[x] = 1`。对操作3，维护最小编号人的状态，设最小可行编号为 `h`，当 `vis[h] = 1` 就重新往后找，停止后输出 `h` 即可

```c++
#include <cstdio>
#include <iostream>
#include <algorithm>
using namespace std;
#define ll int
const int N = 5e5 + 5;
bool vis[N];
int main()
{
    ll n, q;
    scanf("%d%d", &n, &q);
    int h = 1;
    while(q--)
    {
        ll op, x;
        scanf("%d", &op);
        if(op == 1);
        else if(op == 2)
        {
            scanf("%d", &x);
            vis[x] = 1;
        }
        else
        {
            while(vis[h]) ++h;
            printf("%d\n", h);
        }
    }
    return 0;
}
```

















