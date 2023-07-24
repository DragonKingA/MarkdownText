# [**atcoder contests **]()
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

## **ABC  294**

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

---

## ABC 295

A、B、C 签到

### D（待补）







---

## ABC 297

### D

```c++
//给定 a 和 b，操作每次把大的数减去小的数，直到 a == b，求这期间的操作次数，
//类似辗转相除法
#include <bits/stdc++.h>
using namespace std;
#define ll long long
void Solve()
{
    ll a, b, ans = 0;
    cin >> a >> b;
    while(a != b)//恒有 a > b
    {
        swap(a, b);
        if(a % b == 0)
        {
            ans += a / b - 1;
            break;
        }
        ans += a / b;
        a %= b;
    }
    cout << ans << '\n';
}
int main()
{
    // while(1)
    // {
        Solve();
    // }
    return 0;
}
```

### E

```c++
//求单个或若干数组合的和的第 k 小个
//优先队列动态取第 k 小，每次加入 n 个组合和，并用 map 判重
#include <bits/stdc++.h>
using namespace std;
#define ll long long
ll a[100];
map<ll, bool> vis;
priority_queue<ll, vector<ll>, greater<ll> > q;
int main()
{
    ll n, k, cnt = 0, now = 0;
    cin >> n >> k;
    for(int i = 0; i < n; ++i) cin >> a[i];
    q.push(0LL);
    while(cnt <= k)
    {
        now = q.top(); q.pop();
        if(vis[now]) continue;
        for(int i = 0; i < n; ++i) 
            q.push(now + a[i]);
        vis[now] = 1;
        ++cnt;
    }
    cout << now;//不能输出 q.top()，因为答案已经被 pop 了
    return 0;
}
```



---

## ABC 298

### B

multiset 的应用，它可以自动排序，且可以被遍历

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e5 + 10;
int n;
set<int> id[N];//id[num] 数字所在的集合编号集
multiset<int> box[N];//box[] 该集合的有序可重复数集 
int main()
{
    untie();
    int q, op, i, j;
    cin >> n;
    cin >> q;
    while(q--)
    {
        cin >> op;
        if(op == 1)
        {
            cin >> i >> j;
            box[j].insert(i);
            id[i].insert(j);
        }
        else if(op == 2)
        {
            cin >> j;
            for(auto x : box[j])
            {
                cout << x << " ";
            }
            cout << '\n';
        }
        else
        {
            cin >> i;
            for(auto x : id[i])
            {
                cout << x << " ";
            }
            cout << '\n';
        }
    }
    return 0;
}
```

### C

数据很水，不用怕炸 stl 的复杂度，以后遇到这种题要大胆写

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e5 + 10;

int n, q;
map<int, set<int> > mp;
vector<int> box[N];

int main()
{
    untie();
    cin >> n >> q;
    while(q--)
    {
        int op;
        int num, x;
        cin >> op;
        if(op == 1)
        {
            cin >> num >> x;
            box[x].push_back(num);
            mp[num].insert(x);
        }
        else if(op == 2)
        {
            cin >> x;
            priority_queue<int, vector<int>, greater<int> > pq;
            for(auto m : box[x])
            {
                pq.push(m);
            }
            while(!pq.empty())
            {
                cout << pq.top() << ' ';
                pq.pop();
            }
            cout << '\n';
        }
        else
        {
            cin >> num;
            for(auto x : mp[num])
            {
                cout << x << ' ';
            }
            cout << '\n';
        }
    }
    return 0;
}
```

### D

模拟题

题意：数串初始为 “1”，有三种操作：在右端加入数字 x，删去左端数字，输出数串。

思路：用数组 a[i] 存起第 i 个数字，用左右指针 l 和 r 来管理删除和添加，并动态管理数串结果 ans。

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 6e5 + 10, mod = 998244353;
ll q, num = 1, a[N];

ll quick_pow(ll base, ll index)//base = 10
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

int main()
{
    untie();
    cin >> q;
    int r = 1, l = 0;
    a[1] = 1;
    while(q--)
    {
        int op;
        cin >> op;
        if(op == 1)
        {
            cin >> a[++r];
            num = (num * 10 + a[r]) % mod;
        }
        else if(op == 2)
        {
            ++l;
            num = (num - a[l] * quick_pow(10, r - l) % mod + mod) % mod;
        }
        else
        {
            cout << num % mod << '\n';
        }
    }
    return 0;
}
```

---

## ABC 300

### B ~ D

```c++
//均为模拟
//B
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 100;
string s;
map<pair<int, int>, bool> mp1, mp2;
bool Solve()
{
    int n, m; cin >> n >> m;
    for(int i = 0; i < n; ++i)
    {
        cin >> s;
        for(int j = 0; j < m; ++j)
            if(s[j] == '#') mp1[make_pair(i, j)] = 1;
    }
    for(int i = 0; i < n; ++i)
    {
        cin >> s;
        for(int j = 0; j < m; ++j)
            if(s[j] == '#') mp2[make_pair(i, j)] = 1;
    }
    for(int dx = 0; dx < n; ++dx)
    {
        for(int dy = 0; dy < m; ++dy)
        {
            bool ok = 1;
            for(auto [now, _] : mp1)
            {
                int x = now.first >= dx ? (now.first - dx) : (n - dx + now.first);
                int y = now.second >= dy ? (now.second - dy) : (m - dy + now.second);
                if(!mp2.count(make_pair(x, y)))
                {
                    ok = 0;
                    break;
                }
            }
            if(ok) return 1;
        }
    }
    return 0;
}

int main()
{
    untie();
    cout << (Solve() ? "Yes\n" : "No\n");
    return 0;
}



//C
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 105;
int n, m, k;
char s[N][N];
int ans[N];
int dir[4][2] = {{-1, -1}, {-1, 1}, {1, -1}, {1, 1}};

int check(int x, int y)
{
    int res = 0;
    for(int d = 1; ; ++d)
    {
        bool ok = 1;
        for(int p = 0; p < 4; ++p)
        {   
            int nx = x + d * dir[p][0], ny = y + d * dir[p][1];
            if(!(nx > 0 && ny > 0 && nx <= n && ny <= m && s[nx][ny] == '#'))
            {
                ok = 0;
                return res;
            }
        }
        res += ok;
    }
    return 0;
}

int main()
{
    untie();
    cin >> n >> m;
    k = min(n, m);
    for(int i = 1; i <= n; ++i)
        for(int j = 1; j <= m; ++j)
            cin >> s[i][j];
    for(int i = 2; i < n; ++i)
        for(int j = 2; j < m; ++j)
            if(s[i][j] == '#') ans[check(i, j)]++;
    for(int i = 1; i <= k; ++i) cout << ans[i] << " \n"[i == k];
    return 0;
}



//D
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 1e6;
ll n, ans = 0;
vector<ll> p;
bool isprime(ll x)
{
    for(ll i = 2; i * i <= x; ++i)
        if(x % i == 0) return 0;
    return x != 1;
}
bool check(ll a, ll b, ll c)
{
    if(a > n || b > n || c > n || a * b > n || b * c > n || a * c > n || a * b * c > n) return 0;
    return 1;
}
int main()
{
    untie();
    cin >> n;
    for(ll i = 2; i <= N; ++i)
        if(isprime(i)) p.push_back(i);
    int sz = p.size();
    for(int i = 0; i < sz - 2; ++i)
    {
        ll a = p[i] * p[i];
        if(!check(a, p[i + 1], p[i + 2] * p[i + 2])) break;
        for(int j = i + 1; i < sz - 1; ++j)
        {
            ll b = p[j];
            if(!check(a, b, p[j + 1] * p[j + 1])) break;
            for(int k = j + 1; k < sz; ++k)
            {
                ll c = p[k] * p[k];
                if(!check(a, b, c)) break;
                ++ans;
            }
        }
    }
    cout << ans << '\n';
    return 0;
}
```

### E

```c++
// 概率 + dfs
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 1e6;
const ll mod = 998244353;
ll n, inv;
map<ll, ll> mp; // mp[x] = 得到x的概率
ll quick_pow(ll base, ll index = mod - 2)
{
    ll ans = 1;
    for(; index; index >>= 1)
    {
        if(index & 1) ans = ans * base % mod;
        base = base * base % mod;
    }
    return ans;
}
ll dfs(ll now)
{
    if(now == 1) return 1; // 为 1 时只有一种情况
    if(mp.count(now)) return mp[now]; // 记忆化
    mp[now] = 0; // 初始化
    for(ll k = 2; k <= 6; ++k) // 1 没有任何贡献，所以只有 5 种情况
        if(now % k == 0) mp[now] = (mp[now] + dfs(now / k) * inv) % mod; 
    // 概率叠加上 P(now / k) * P(k)，其中 P(k) = 1 / 5 = inv
    return mp[now];
}
int main()
{
    untie();
    inv = quick_pow(5); // 1 / 5
    cin >> n;
    cout << dfs(n);
    return 0;
}
```



---

## ABC 301

### A ~ D

```c++
//四道都是模拟
//A
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 1e4;

void Solve()
{
    int n;
    string s;
    cin >> n >> s;
    map<char, int> mp;
    for(auto ch : s)
        mp[ch]++;
    if(mp['T'] > mp['A']) cout << "T\n";
    else if(mp['T'] < mp['A']) cout << "A\n";
    else cout << (s[n - 1] == 'T' ? 'A' : 'T') << '\n';
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



//B
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 1e3;
int n, a[N];
void Solve()
{
    cin >> n;
    for(int i = 1; i <= n; ++i) cin >> a[i];
    cout << a[1] << " ";
    for(int i = 2; i <= n; ++i)
    {  
        if(abs(a[i] - a[i - 1]) != 1)
        {
            if(a[i - 1] < a[i])
            {
                for(int j = a[i - 1] + 1; j < a[i]; ++j)
                    cout << j << " ";
            }
            else
            {
                for(int j = a[i - 1] - 1; j > a[i]; --j)
                    cout << j << " ";
            }
        }
        cout << a[i] << " ";
    }
    cout << '\n';
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



//C
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 1e3;

void Solve()
{
    map<char, int> mp, cnts, cntt;
    string s, t;
    cin >> s >> t;
    mp['@'] = mp['a'] = mp['t'] = mp['c'] = mp['o'] = mp['d'] = mp['e'] = mp['r'] = 1;
    for(auto ch : s) ++cnts[ch];
    for(auto ch : t) ++cntt[ch];
    for(auto ch : s)
    {
        if(ch == '@') continue;
        if(cntt[ch] > 0 && cnts[ch] > 0) --cnts[ch], --cntt[ch];
    }
    for(auto ch : s)
    {
        if(ch == '@') continue;
        if(cnts[ch] > 0 && mp[ch] && cntt['@'] > 0) --cnts[ch], --cntt['@'];
    }
    for(auto ch : t)
    {
        if(ch == '@') continue;
        if(cntt[ch] > 0 && mp[ch] && cnts['@'] > 0) --cntt[ch], --cnts['@'];
    }
    bool ok = 1;
    for(auto ch : s) if(ch != '@' && cnts[ch] > 0) ok = 0;
    for(auto ch : t) if(ch != '@' && cntt[ch] > 0) ok = 0;
    if(ok) cout << "Yes\n";
    else cout << "No\n";
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



//D
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
ll base[100];
int main()
{
    untie();
    string s;
    ll n, now = 0;
    cin >> s >> n;
    int sz = s.size();
    s = ' ' + s;
    base[0] = 1;
    for(int i = 1; i <= sz; ++i) base[i] = base[i - 1] << 1LL;
    for(int i = 1; i <= sz; ++i)
        if(s[i] != '?') now |= (s[i] - '0') * base[sz - i];
    if(now > n)
    {
        cout << "-1\n";
        return 0;
    }
    for(int i = 1; i <= sz; ++i)
        if(s[i] == '?' && now + base[sz - i] <= n)
            now |= base[sz - i];
    cout << now << '\n';    
    return 0;
}
```







---

## (*)ABC 302

### B

```c++
//模拟，容易写臭
//题型：判断各种方向下的连续串是否为给定串
//top1写法
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 105;
int n, m;
string s[N], dest = "snuke";
//对应：向右，向下，向左，向上，同理斜向的4个方向
int dx[8] = {0, 1, 0, -1, 1, 1, -1, -1};
int dy[8] = {1, 0, -1, 0, 1, -1, 1, -1};
int main()
{
    untie();
    cin >> n >> m;
    for(int i = 0; i < n; ++i) cin >> s[i];
    for(int i = 0; i < n; ++i)
    {
        for(int j = 0; j < m; ++j)
        {
            for(int k = 0; k < 8; ++k)
            {
                bool ok = 1;
                for(int t = 0; t < 5; ++t)
                {
                    int x = i + dx[k] * t, y = j + dy[k] * t;
                    if(x < 0 || y < 0 || x >= n || y >= m || s[x][y] != dest[t])
                    {
                        ok = 0;
                        break;
                    }
                }
                if(ok)
                {
                    for(int t = 0; t < 5; ++t)
                    {
                        int x = i + dx[k] * t, y = j + dy[k] * t;
                        cout << x + 1 << " " << y + 1 << '\n';
                    }
                    return 0;
                }
            }
        }
    }
    return 0;
}
```

### C

```c++
//直接全排列查找是否存在符合条件的一次排列即可，复杂度 8! = 40320
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 10;
int n, m;
string s[N];
int main()
{
    untie();
    cin >> n >> m;
    for(int i = 0; i < n; ++i) cin >> s[i];
    sort(s, s + n);
    do{
        bool ok = 1;
        for(int i = 1; i < n; ++i)
        {
            int cnt = 0;
            for(int j = 0; j < m; ++j)
                if(s[i - 1][j] != s[i][j]) ++cnt;
            if(cnt != 1)
            {
                ok = 0;
                break;
            }
        }
        if(ok)
        {
            cout << "Yes\n";
            return 0;
        }
    }while(next_permutation(s, s + n));
    cout << "No\n";
    return 0;
}
```

### D

```c++
//题意：两组序列中找一对数 a, b 使得满足 |a - b| <= d，并求最大的 a + b
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
set<ll> sa, sb;
inline ll myabs(ll x) { return x > 0 ? x : -x;}
int main()
{
    untie();
    ll n, m, d;
    cin >> n >> m >> d;
    for(int i = 0; i < n; ++i)
    {
        ll x; cin >> x;
        sa.insert(x);
    }
    for(int i = 0; i < m; ++i)
    {
        ll x; cin >> x;
        sb.insert(x);
    }
    ll ans = -1;
    for(auto a : sa)
    {
        // 找第一个大于 a + d 的数即 *iter > a + d 即 *iter - a > d
        // 这样就有 *--iter <= a + d 即 *--iter - a <= d
        auto iter = sb.upper_bound(a + d);
        if(iter != sb.begin()) // iter == begin() 则不可能存在符合条件的 b
        {
            ll b = *--iter;
            if(abs(a - b) <= d) // 有可能 b 过小，导致 a - b > d 所以还需要判断
                ans = max(ans, a + b);
        }
    }
    cout << ans << '\n';
    return 0;
}
```

### E

```c++
//STL模拟
//set[u] 为与点 u 相连的点集合
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 3e5 + 10;
int n, q;
set<int> G[N];
int main()
{
    untie();
    cin >> n >> q;
    int now = n;
    while(q--)
    {
        int op, u, v;
        cin >> op;
        if(op == 1)
        {
            cin >> u >> v;
            now -= G[u].empty() + G[v].empty();
            G[u].insert(v);
            G[v].insert(u);
        }
        else
        {
            cin >> v;
            if(!G[v].empty())
            {
                ++now;
                for(int u : G[v])
                {
                    G[u].erase(v);
                    if(G[u].empty()) ++now;
                }
                G[v].clear();
            }
        }
        cout << now << '\n';
    }
    return 0;
}



//刘哥代码，最优复杂度
#include <iostream>
#include <string>
#include <vector>
#include <array>
#include <algorithm>
typedef long long ll;

int main()
{
    std::ios::sync_with_stdio(false);
    std::cin.tie(0);
    std::cout.tie(0);
    int n, q;
    std::cin >> n >> q;
    std::vector<int> del(n, 0), del_time(n, -1);//del[u]为 u 删除边个数，del_time[u] 为 u 操作2时的时间点
    std::vector<std::vector<std::array<int, 2> > > gra(n);
    int ans = n;
    for (int i = 0; i < q; i++)
    {
        int k, u, v;
        std::cin >> k >> u;
        u--;
        if (k == 1)
        {
            std::cin >> v;
            v--;
            gra[u].push_back({ v, i });
            gra[v].push_back({ u, i });
            if (gra[v].size() - del[v] == 1) --ans;
            if (gra[u].size() - del[u] == 1) --ans;
        }
        else
        {
            if (gra[u].size() - del[u] != 0) ans++;
            for (auto [nex, t] : gra[u])
            {
                if (t < del_time[nex]) continue;
                del[nex]++;
                if (gra[nex].size() - del[nex] == 0) ans++;
            }
            gra[u].clear();
            del_time[u] = i;
            del[u] = 0;
        }
        std::cout << ans << "\n";
    }
    return 0;
}
```



---

## (*)ABC 304

### D

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e5 + 10;

int x[N], y[N], a[N], b[N];
map<pair<int, int>, int> mp;

int main()
{
    untie();
    int w, h, n, A, B;
    cin >> w >> h >> n;
    for(int i = 0; i < n; ++i) cin >> x[i] >> y[i];
    cin >> A;
    for(int i = 0; i < A; ++i) cin >> a[i];
    cin >> B;
    for(int i = 0; i < B; ++i) cin >> b[i];
    
    int ans1 = 1e9, ans2 = 0;
    for(int i = 0; i < n; ++i)
    {
        int xx = lower_bound(a, a + A, x[i]) - a;
        int yy = lower_bound(b, b + B, y[i]) - b;
        mp[make_pair(xx, yy)]++;
    }
    for(auto [p, num] : mp)
    {
        ans1 = min(ans1, num);
        ans2 = max(ans2, num);
    }
    if((A + 1) * (B + 1) > mp.size()) ans1 = 0;
    cout << ans1 << " " << ans2;
    return 0;
}
```



### E

```c++
//把操作对象利用并查集变成两个连通块，加上某条边即使得两个连通块互相可达
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e5 + 10;
int n, m, k, q;
int sta[N], ds[N];
vector<int> G[N];
unordered_set<int> jud[N];
bool vis[N];
map<pair<int, int>, bool> mp;// mp[{fu, fv}] 表示两个连通块是否不应该连通
int find_set(int x)
{
    return x == ds[x] ? x : (ds[x] = find_set(ds[x]));
}
int main()
{
    untie();
    cin >> n >> m;
    for(int i = 1; i <= n; ++i) ds[i] = i;
    for(int i = 0; i < m; ++i)
    {
        int u, v;
        cin >> u >> v;
        u = find_set(u), v = find_set(v);
        if(u != v) ds[u] = v;
    }
    cin >> k;
    for(int i = 0; i < k; ++i)
    {
        int u, v;
        cin >> u >> v;
        u = find_set(u), v = find_set(v);
        mp[make_pair(u, v)] = mp[make_pair(v, u)] = 1;
    }
    cin >> q;
    while(q--)
    {
        int u, v;
        cin >> u >> v;
        u = find_set(u), v = find_set(v);
        if(u != v && mp[make_pair(u, v)]) // 首先判断不在同一个连通块，再判断是否不允许连通
            cout << "No\n";
        else
            cout << "Yes\n";
    }
    return 0;
}
```





## ABC 305

### *D

```c++
STL二分 + 染色区间长度的前缀和存储
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 2e5 + 10;
int a[N], n, q;
map<ll, int> mp;
void Solve()
{
    cin >> n;
    for(int i = 1; i <= n; ++i) cin >> a[i];
    //此处存储方式是重点，总体是前缀和的形式，但是是基于分段染色区间上的前缀和
    for(int i = 1; i <= n; ++i)
    {
        if(i & 1) mp[a[i]] = mp[a[i - 1]] + a[i] - a[i - 1];
        else mp[a[i]] = mp[a[i - 1]];
        // cout << i << " " << a[i] << " " << mp[a[i]] << '\n';
    }
    cin >> q;
    while(q--)
    {
        int l, r;
        cin >> l >> r;
        int x = upper_bound(a + 1, a + 1 + n, l) - a;
        int y = upper_bound(a + 1, a + 1 + n, r) - a - 1;
        ll ans = mp[a[y]] - mp[a[x]];
        if(l < a[x] && x % 2 == 1) ans += a[x] - l;
        if(r > a[y] && y % 2 == 0) ans += r - a[y];
        cout << ans << '\n';
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

### E

```c++
// 优先队列 + bfs
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 2e5 + 10;

void Solve()
{
    int n, m, k;
    cin >> n >> m >> k;
    vector<int> G[n + 1];
    while(m--)
    {
        int u, v; cin >> u >> v;
        G[u].push_back(v);
        G[v].push_back(u);
    }
    priority_queue<pair<int, int> > pq;//让能走的更远的点继续走，否则会被走得短的点占领，导致WA
    while(k--)
    {
        int v, w; cin >> v >> w;
        pq.push({w, v});
    }
    vector<bool> vis(n + 10, 0);
    while(!pq.empty())
    {
        pair<int, int> now = pq.top(); pq.pop();
        int u = now.second, w = now.first;
        vis[u] = 1;
        if(w == 0) continue;
        for(int v : G[u])
        {
            if(vis[v]) continue;
            vis[v] = 1;
            pq.push({w - 1, v});
        }
    }
    vector<int> ans;
    for(int i = 1; i <= n; ++i)
        if(vis[i]) ans.push_back(i);
    cout << ans.size() << '\n';
    for(int x : ans) cout << x << " ";
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



## ABC 306

### B ~ C

```c++
//B 有数据坑（最大范围为 unsigned long long）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll unsigned long long
const int N = 100;
ll p[N];
int main()
{
    untie();
    for(int i = 0; i < 64; ++i) p[i] = (1ULL << i);
    ll ans = 0;
    for(int i = 0; i < 64; ++i)
    {
        int x; cin >> x;
        if(x) ans += p[i];
    }
    cout << ans;
    return 0;
}



//C 模拟即可
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 3e5 + 100;
int n, a[N], ans[N];
bool vis[N];
int main()
{
    untie();
    cin >> n;
    for(int i = 1; i <= 3 * n; ++i) cin >> a[i];
    for(int i = 1; i <= 3 * n; ++i)
    {
        if(vis[a[i]] == 1 && !ans[a[i]]) ans[a[i]] = i;
        vis[a[i]] = 1;
    }
    vector<pair<int, int> > v;
    for(int i = 1; i <= n; ++i) v.push_back(make_pair(ans[i], i));
    sort(v.begin(), v.end());
    for(int i = 0; i < n; ++i) cout << v[i].second << " \n"[i == n - 1];
    return 0;
}
```

### D

```c++
// dp
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 3e5 + 10;
ll x[N], y[N], dp[2];
int main()
{
    untie();
    int n;
    cin >> n;
    for(int i = 1; i <= n; ++i) cin >> x[i] >> y[i];
    for(int i = 1; i <= n; ++i)
    {
        if(x[i]) dp[1] = max(dp[0] + y[i], dp[1]);
        else dp[0] = max(dp[0], max(dp[0], dp[1]) + y[i]);
    }
    cout << max(dp[0], dp[1]) << '\n';
    return 0;
}
```



## ABC 308

### C ~ D

```c++
//C. 自定义排序 + long double
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
#define double long double
const int N = 2e5 + 10;
bool cmp(pair<double, int> p1, pair<double, int> p2)
{
    if(p1.first == p2.first) return p1.second < p2.second;
    return p1.first > p2.first;
}
void Solve()
{
    int n; cin >> n;
    vector<pair<double, int> > v;
    for(int i = 0; i < n; ++i)
    {
        double a, b;
        cin >> a >> b;
        v.push_back(make_pair(a / (a + b), i));
    }
    sort(all(v), cmp);
    for(int i = 0; i < n; ++i)
        cout << v[i].second + 1 << " \n"[i == n - 1];

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



//D. 搜索
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
#define double long double
const int N = 600;
int n, m;
char mp[N][N];
bool vis[N][N];
int dir[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
map<char, char> check;
bool bfs()
{
    memset(vis, 0, sizeof(vis));
    queue<array<int, 2> > q;
    q.push({1, 1});
    vis[1][1] = 1;
    while(!q.empty())
    {
        array<int, 2> now = q.front(); q.pop();
        for(int i = 0; i < 4; ++i)
        {
            int nx = now[0] + dir[i][0], ny = now[1] + dir[i][1];
            if(nx >= 1 && nx <= n && ny >= 1 && ny <= m && !vis[nx][ny] && check[mp[now[0]][now[1]]] == mp[nx][ny])
            {
                q.push({nx, ny});
                vis[nx][ny] = 1;
            }
        }
    }
    return vis[n][m];
}
void Solve()
{
    check['s'] = 'n';
    check['n'] = 'u';
    check['u'] = 'k';
    check['k'] = 'e';
    check['e'] = 's';
    cin >> n >> m;
    for(int i = 1; i <= n; ++i)
        for(int j = 1; j <= m; ++j)
            cin >> mp[i][j];
    cout << (bfs() ? "Yes\n" : "No\n");
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

### E

```C++
// 前缀和 + 后缀和
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
#define double long double
const int N = 2e5 + 10;
int a[N];

int cal(int a, int b, int c)
{
    for(int i = 0; i < 4; ++i)
        if(i != a && i != b && i != c) return i;
}
void Solve()
{
    ll ans = 0;
    int n; cin >> n;
    vector<vector<int>> cntl(n + 5, vector<int>(3, 0)), cntr(n + 5, vector<int>(3, 0));
    for(int i = 1; i <= n; ++i) cin >> a[i];
    string s; cin >> s; s = ' ' + s;
    for(int i = 1; i <= n; ++i)
    {
        cntl[i] = cntl[i - 1];
        if(s[i] == 'M') cntl[i][a[i]]++;
    }
    for(int i = n; i >= 0; --i)
    {
        cntr[i] = cntr[i + 1];
        if(s[i] == 'X') cntr[i][a[i]]++;
    }
    for(int i = 1; i <= n; ++i)
    {
        if(s[i] != 'E') continue; 
        for(int j = 0; j < 3; ++j)
            for(int k = 0; k < 3; ++k)
                ans += 1LL * cntl[i][j] * cntr[i][k] * cal(a[i], j, k);
    }
    cout << ans << '\n';
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

### F

关于用 `lambda` 表达式重载优先队列中的自定义比较函数以及关键字 `decltype` 的使用。

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
#define pii pair<int, int>
const int N = 2e5 + 10;

void Solve()
{
    ll ans = 0;
    int n, m; cin >> n >> m;
    vector<int> p(n, 0);
    vector<pii > v(m);
    for(int &x : p) cin >> x, ans += x;
    for(auto &x : v) cin >> x.first;
    for(auto &x : v) cin >> x.second;
    sort(all(p));
    sort(all(v));
    auto fun = [](pii p1, pii p2) -> bool {return p1.second < p2.second;};
    priority_queue<pii, vector<pii >, decltype(fun)> pq(fun);
    int idx = 0;
    for(int x : p)
    {
        while(idx < m && v[idx].first <= x) pq.push(v[idx++]);
        if(pq.empty()) continue;
        ans -= pq.top().second;
        pq.pop();
    }
    cout << ans << '\n';
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



## ABC 309

### D

```c++
// 两次 dijkstra
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 3e5 + 10;
const ll inf = (1LL << 31) - 1;

struct edge{
    int to; int w;
    edge(int a = 0, int b = 0){ to = a, w = b;}
};
struct node{
    int id; int dis;
    node(int a = 0, int b = 0){ id = a, dis = b;}
    bool operator <(const node &x)const{ return dis > x.dis;}
};
int n1, n2, m;
bool vis[N];
vector<edge> e[N];

void dijkstra(vector<int> &dis, int ns, int ne, int s)
{
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
            if(!vis[ne.to] && dis[ne.to] > u.dis + ne.w)
            {
                dis[ne.to] = u.dis + ne.w;
                q.push(node(ne.to, dis[ne.to]));
            }
        }
    }
}

void Solve()
{
    int n1, n2, m; cin >> n1 >> n2 >> m;
    vector<int> dp(n1 + n2 + 1, inf - 1);
    while(m--)
    {
        int u, v; cin >> u >> v;
        e[u].push_back(edge(v, 1));
        e[v].push_back(edge(u, 1));
    }
    dijkstra(dp, 1, n1, 1);
    dijkstra(dp, n1 + 1, n1 + n2, n1 + n2);
    cout << *max_element(dp.begin() + 1, dp.begin() + n1 + 1) + *max_element(dp.begin() + n1 + 1, dp.begin() + n1 + n2 + 1) + 1 << "\n";
}

int main()
{
    untie();
    int T = 1;
    // int T = 1;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```

### E

```C++
// 简单图论 + dfs
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 3e5 + 10;
vector<int> G[N];
ll lim[N], ans = 0;

void dfs(int u, ll d = -2e9)
{
    d = max(d, lim[u]);
    if(d >= 0) ++ans;
    for(int v : G[u]) dfs(v, d - 1);
}

void Solve()
{
    int n, m; cin >> n >> m;
    for(int i = 2; i <= n; ++i)
    {
        int j; cin >> j;
        G[j].push_back(i);
    }
    memset(lim, -0x3f, sizeof(lim));
    while(m--)
    {
        ll x, y; cin >> x >> y;
        lim[x] = max(lim[x], y);        
    }
    dfs(1);
    cout << ans << "\n";
}

int main()
{
    untie();
    int T = 1;
    // int T = 1;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```







## (*)ABC 311

### C 判环

```C++
// 本来可以AC，但是忘记输出路径长度
// 法1：并查集
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 2e5 + 10;
int vis[N], ok = 0;
int st, ed;
int path[N];
int ds[N];

int find_set(int x)
{
    return x == ds[x] ? x : (ds[x] = find_set(ds[x]));
}

void dfs(vector<int> G[], int u, int step)
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
        if(fu == fv) st = v, ed = u;
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



// 法2：定义三种 vis[u] 状态
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 2e5 + 10;
int vis[N], ok = 0;
int pre[N];
vector<int> vec;

bool dfs(vector<int> G[], int u, int fa)
{
    vis[u] = 0;
    pre[u] = fa;
    for(int v : G[u])
    {
        if(vis[v] == 0) 
        {
            while(u != v)
            {
                vec.push_back(u);
                u = pre[u];
            }
            vec.push_back(v);
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
    vis[u] = 1;
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
    memset(vis, -1, sizeof(vis));
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

### D 多方向DFS的技巧

```c++
// 定义 vis[N][N][4] 使得在该点上每种 dfs 方向都独立判定
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 200 + 10;

string s[N];
bool vis[N][N][4]; // 四个方向的点状态
int dir[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
int n, m, cnt = 0; 

void dfs(int x, int y, int d)
{
    if(vis[x][y][0] + vis[x][y][1] + vis[x][y][2] + vis[x][y][3] == 0) ++cnt;
    vis[x][y][d] = 1;
    int tx = x + dir[d][0], ty = y + dir[d][1];
    if(s[tx][ty] == '.') // 沿该方向继续遍历
    {
        dfs(tx, ty, d);
    }
    else 				// 转向处
    {
        for(int i = 0; i < 4; ++i)
        {
            int nx = x + dir[i][0], ny = y + dir[i][1];
            if(s[nx][ny] == '.' && !vis[nx][ny][i])
            {
                dfs(nx, ny, i);
            }
        }
    }
    
}

void Solve()
{
    cin >> n >> m;
    for(int i = 0; i < n; ++i) cin >> s[i];
    for(int i = 0; i < 4; ++i) dfs(1, 1, i);
    cout << cnt << "\n";
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

### E

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 3e3 + 10;
int n, m, k;

void Solve()
{
    cin >> n >> m >> k;
    vector<vector<ll> > dp(n + 5, vector<ll>(m + 5, 1)); // 初始化为每个点可取
    while(k--)
    {
        int x, y; cin >> x >> y;
        dp[x][y] = 0; // 该点不可取（或者用另外的数组 vis[][] 存储）
    }
    for(int i = 0; i <= n; ++i) dp[i][0] = 0; // 初始化这些点不可取
    for(int j = 0; j <= m; ++j) dp[0][j] = 0; // 初始化这些点不可取
    // 若(i, j)可取，dp转移只转移最小量，因为(i, j)的状态受这三方状态的同时制约；否则为 0
    for(int i = 1; i <= n; ++i)
        for(int j = 1; j <= m; ++j)
            dp[i][j] += (dp[i][j] ? min({dp[i - 1][j - 1], dp[i][j - 1], dp[i - 1][j]}) : 0);
    ll ans = 0;
    for(int i = 1; i <= n; ++i)
        for(int j = 1; j <= m; ++j)
            ans += dp[i][j];
    cout << ans << "\n";
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

# ARC

---

## ARC 161

### A ~ B

```c++
A. 模拟
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 2e4 + 10;

void Solve()
{
    int n, x;
    cin >> n;
    vector<int> a(n + 1);
    for(int i = 1; i <= n; ++i) cin >> a[i];
    sort(all(a));
    for(int i = 1, j = n / 2 + 2; j <= n; i++, ++j)
    {
        if(!(a[j] > a[i] && a[j] > a[i + 1]))
        {
            cout << "No\n";
            return;
        }
    }
    cout << "Yes\n";
}

int main()
{
    untie();
    Solve();
    return 0;
}



B. 模拟（自己想的 bitset 方法，但比较繁琐）
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 63;
ll n;

bool check(bitset<N> bit) //判断第一个1在所有低位的情况（从大到小）
{
    int ind = bit._Find_first();
    bit[ind] = 0;
    for(int i = ind; i >= 0; --i)
    {
        bit[i] = 1;
        ll now = bit.to_ullong();
        if(now <= n)
        {
            cout << now << '\n';
            return 1;
        }
        bit[i] = 0;
    }
    return 0;
}

void Solve()
{
    ll base = 7;
    cin >> n;
    if(n < 7)
    {
        cout << "-1\n";
        return;
    }

    while(base <= n) base <<= 1;
    bitset<N> now(n), bit(base);

    int ind = bit._Find_first(); // 取低位第一个1位置

    bit[ind] = bit[ind + 1] = bit[ind + 2] = 0;
    for(int index = ind; index >= 0; --index) //枚举第一个1的位置（实际是枚举第三个1的位置）
    {
        bit[index + 2] = 1;
        for(int i = index; i >= 0; --i)
        {
            bit[i] = bit[i + 1] = 1;
            if(check(bit)) return;
            bit[i] = bit[i + 1] = 0;
        }
        bit[index + 2] = 0;
    }
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


```



## ARC 163

### A ~ B

```c++
// A 模拟
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 1e4;

void Solve()
{
    int n;
    string s;
    cin >> n >> s;
    for(int i = 0; i < n; ++i)
        if(s.substr(0, i + 1) < s.substr(i + 1))
        {
            cout << "Yes\n";
            return;
        }
    cout << "No\n";
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



// B 模拟
// 只改变 a1 和 a2 即可
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 2e5 + 10;
int a[N];

void Solve()
{
    int n, m, ans = 2e9;
    cin >> n >> m;
    for(int i = 1; i <= n; ++i) cin >> a[i];
    sort(a + 3, a + 1 + n);
    for(int i = 3; i + m - 1 <= n; ++i)
    {
        int l = a[i], r = a[i + m - 1], now = 0;
        if(l < a[1]) now += a[1] - l;
        if(r > a[2]) now += r - a[2];
        ans = min(ans, now);
    }
    cout << ans << '\n';
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

