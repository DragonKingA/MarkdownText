# [**atcoder contests **]()
---
# ABC

---

## **ABC 292**

A ��B ǩ��

### C
���⣺��һ������n�����ж�����(A, B, C, D)�������� A*B + C*D = n��

˼·��ת��Ϊ x + y = n���ȼ���ÿ�����������Եĸ������� arr[] �У��� x �ж��ٸ�(A, B)ʹ�� A*B = x����������֪��ÿ��(x, y)�ܲ�������(A, B, C, D)������Ȼ��ö�� x (y = n - x)���������ɡ�

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
void init()//���õ�д�� 12ms
{
    for(int i = 1; i < n; i++)//ö����������
        for(int j = 1; i * j <= n; j++)
            arr[i * j]++;//����˻����������1
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
���⣺��ͼ��ÿ����ͨ���Ƿ�����ÿ��ڶ��������߸�����ȣ���ͼ�����⣩

˼·���������鼯����ÿ����ͨ��

```c++
#include <iostream>
#include <algorithm>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 2e5 + 5;
struct component{
    int edge, node;
}cnt[N];//ÿ����ͨ��
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
            ds[vv] = uu;//��vv����uu���ڼ���
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

��������������������鷳��

���⣺��һ������ͼ����һ��Ŀ��ͼ����Ŀ��ͼ��ÿ����u���㣬u������ԭͼ�����ܵ�������е�v��Ҫ��ֱ���ߡ�

˼·��bfs����ÿ����u��Ϊ��㣬��ͨ·���ϵ��������е㶼Ҫ��u���ߣ�����cnt++������ȥ���б�������Ҫ�����Ĵ�����

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
���⣺��ѧ�������⣬�����(A, B)���ڲ��������ε�����ܱ߳���

- �� A >= B

- �����ȼ���AԶ����B����Ȼ��ʱ��BΪ�ߵ��������α߳���󣬼� 

  B / cos(pi / 6) = 2 * B / sqrt(3)                         																							(1)

- ����A = 12, B = 11�������ʱ�����������A����Ȼ�����������������۲���Ӧ�õ����� A > B

- ���賤ΪA����ΪB�ľ��Σ��涨�����������һ�������ھ������Ͻǣ�һ�������ھ��ε׳��ߣ���������������ڱ���Ҳ���ܲ��ڱ���

- ���������Ҳ���γ���һ���Ա�BΪ����һ��ֱ�Ǳߡ����������αߵ�б�ߵ�һ�����ֱ��������

- �����������һ�������B�γɽǦȣ������һ�������A�γɽ� (pi / 2 - �� - pi / 3) = pi / 6 - ��
  ������ΪL���õ��������� Lcos�� = B�� Lcos(pi / 6 - ��) = A�������� �� = arctan(2 * A / B - sqrt(3))
  ��ʱ���� 

  L = B / cos�� 																																				  (2)

- ���ֻҪ�жϽ��(1)�ͽ��(2)�ĸ��������ĸ����res1��res2�����ܻᳬ��A��Ҳֻ��һ���ᳬ��A����ʵ���Ͻ��(1)��(2)����ֻ��һ���Ǻ���ģ���𰸱���������Сֵ

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
    // ��ͬ�¾�
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

A��B ǩ��

### C

���⣺���������У��ֱ�Ϊ N��M ���ϸ񲻵����� A��B���ֽ����ǻ�ϳ����� C ���������򣬼�¼������ C �е�λ�ã����±� 1 ��ʼ�������������У�һ��Ϊ���� A ������ C �е�λ�ã�һ��Ϊ���� B ������ C �е�λ�ã���������ԭ���е�˳���������

˼·��ֱ��ģ�����⣬�������ռ������C����A��B����������������mapӳ��<ֵ�� ��C�е�λ��>���ɡ�

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

���⣺�� `N` ���˸����Լ��ı��`id`��`Q` ���¼���ÿ���˵�״̬�� ����û������绰��������û�������ߡ� ������ϡ������ֲ�����1.��û������绰����С��ŵ��˴�绰��2.�ѵ� `x` ���˽��ߣ�3.���Ѿ�������绰��û�����ߵ���С����ߴ�绰

˼·��������ĿԼ���У���֤����3һ���н⣬��һ�����Բ���1�Ѿ����й���ǰ�ᣬ����ʱ��С��ŵ���һ��������绰����������ֻ��Ҫά���������ߡ���״̬���� -- ������ `vis[N]` ά�����Բ���2�� `vis[x] = 1`���Բ���3��ά����С����˵�״̬������С���б��Ϊ `h`���� `vis[h] = 1` �����������ң�ֹͣ����� `h` ����

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

A��B��C ǩ��

### D��������







---

## ABC 297

### D

```c++
//���� a �� b������ÿ�ΰѴ������ȥС������ֱ�� a == b�������ڼ�Ĳ���������
//����շת�����
#include <bits/stdc++.h>
using namespace std;
#define ll long long
void Solve()
{
    ll a, b, ans = 0;
    cin >> a >> b;
    while(a != b)//���� a > b
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
//�󵥸�����������ϵĺ͵ĵ� k С��
//���ȶ��ж�̬ȡ�� k С��ÿ�μ��� n ����Ϻͣ����� map ����
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
    cout << now;//������� q.top()����Ϊ���Ѿ��� pop ��
    return 0;
}
```



---

## ABC 298

### B

multiset ��Ӧ�ã��������Զ������ҿ��Ա�����

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e5 + 10;
int n;
set<int> id[N];//id[num] �������ڵļ��ϱ�ż�
multiset<int> box[N];//box[] �ü��ϵ�������ظ����� 
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

���ݺ�ˮ��������ը stl �ĸ��Ӷȣ��Ժ�����������Ҫ��д

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

ģ����

���⣺������ʼΪ ��1���������ֲ��������Ҷ˼������� x��ɾȥ������֣����������

˼·�������� a[i] ����� i �����֣�������ָ�� l �� r ������ɾ������ӣ�����̬����������� ans��

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
//��Ϊģ��
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
// ���� + dfs
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 1e6;
const ll mod = 998244353;
ll n, inv;
map<ll, ll> mp; // mp[x] = �õ�x�ĸ���
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
    if(now == 1) return 1; // Ϊ 1 ʱֻ��һ�����
    if(mp.count(now)) return mp[now]; // ���仯
    mp[now] = 0; // ��ʼ��
    for(ll k = 2; k <= 6; ++k) // 1 û���κι��ף�����ֻ�� 5 �����
        if(now % k == 0) mp[now] = (mp[now] + dfs(now / k) * inv) % mod; 
    // ���ʵ����� P(now / k) * P(k)������ P(k) = 1 / 5 = inv
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
//�ĵ�����ģ��
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
//ģ�⣬����д��
//���ͣ��жϸ��ַ����µ��������Ƿ�Ϊ������
//top1д��
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 105;
int n, m;
string s[N], dest = "snuke";
//��Ӧ�����ң����£��������ϣ�ͬ��б���4������
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
//ֱ��ȫ���в����Ƿ���ڷ���������һ�����м��ɣ����Ӷ� 8! = 40320
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
//���⣺������������һ���� a, b ʹ������ |a - b| <= d���������� a + b
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
        // �ҵ�һ������ a + d ������ *iter > a + d �� *iter - a > d
        // �������� *--iter <= a + d �� *--iter - a <= d
        auto iter = sb.upper_bound(a + d);
        if(iter != sb.begin()) // iter == begin() �򲻿��ܴ��ڷ��������� b
        {
            ll b = *--iter;
            if(abs(a - b) <= d) // �п��� b ��С������ a - b > d ���Ի���Ҫ�ж�
                ans = max(ans, a + b);
        }
    }
    cout << ans << '\n';
    return 0;
}
```

### E

```c++
//STLģ��
//set[u] Ϊ��� u �����ĵ㼯��
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



//������룬���Ÿ��Ӷ�
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
    std::vector<int> del(n, 0), del_time(n, -1);//del[u]Ϊ u ɾ���߸�����del_time[u] Ϊ u ����2ʱ��ʱ���
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
//�Ѳ����������ò��鼯���������ͨ�飬����ĳ���߼�ʹ��������ͨ�黥��ɴ�
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
map<pair<int, int>, bool> mp;// mp[{fu, fv}] ��ʾ������ͨ���Ƿ�Ӧ����ͨ
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
        if(u != v && mp[make_pair(u, v)]) // �����жϲ���ͬһ����ͨ�飬���ж��Ƿ�������ͨ
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
STL���� + Ⱦɫ���䳤�ȵ�ǰ׺�ʹ洢
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
    //�˴��洢��ʽ���ص㣬������ǰ׺�͵���ʽ�������ǻ��ڷֶ�Ⱦɫ�����ϵ�ǰ׺��
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
// ���ȶ��� + bfs
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
    priority_queue<pair<int, int> > pq;//�����ߵĸ�Զ�ĵ�����ߣ�����ᱻ�ߵö̵ĵ�ռ�죬����WA
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
//B �����ݿӣ����ΧΪ unsigned long long��
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



//C ģ�⼴��
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
//C. �Զ������� + long double
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



//D. ����
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
// ǰ׺�� + ��׺��
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

������ `lambda` ���ʽ�������ȶ����е��Զ���ȽϺ����Լ��ؼ��� `decltype` ��ʹ�á�

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
// ���� dijkstra
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
// ��ͼ�� + dfs
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

### C �л�

```C++
// ��������AC�������������·������
// ��1�����鼯
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



// ��2���������� vis[u] ״̬
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

### D �෽��DFS�ļ���

```c++
// ���� vis[N][N][4] ʹ���ڸõ���ÿ�� dfs ���򶼶����ж�
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 200 + 10;

string s[N];
bool vis[N][N][4]; // �ĸ�����ĵ�״̬
int dir[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
int n, m, cnt = 0; 

void dfs(int x, int y, int d)
{
    if(vis[x][y][0] + vis[x][y][1] + vis[x][y][2] + vis[x][y][3] == 0) ++cnt;
    vis[x][y][d] = 1;
    int tx = x + dir[d][0], ty = y + dir[d][1];
    if(s[tx][ty] == '.') // �ظ÷����������
    {
        dfs(tx, ty, d);
    }
    else 				// ת��
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
    vector<vector<ll> > dp(n + 5, vector<ll>(m + 5, 1)); // ��ʼ��Ϊÿ�����ȡ
    while(k--)
    {
        int x, y; cin >> x >> y;
        dp[x][y] = 0; // �õ㲻��ȡ����������������� vis[][] �洢��
    }
    for(int i = 0; i <= n; ++i) dp[i][0] = 0; // ��ʼ����Щ�㲻��ȡ
    for(int j = 0; j <= m; ++j) dp[0][j] = 0; // ��ʼ����Щ�㲻��ȡ
    // ��(i, j)��ȡ��dpת��ֻת����С������Ϊ(i, j)��״̬��������״̬��ͬʱ��Լ������Ϊ 0
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
A. ģ��
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



B. ģ�⣨�Լ���� bitset ���������ȽϷ�����
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
#define all(v) v.begin(), v.end()
const int N = 63;
ll n;

bool check(bitset<N> bit) //�жϵ�һ��1�����е�λ��������Ӵ�С��
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

    int ind = bit._Find_first(); // ȡ��λ��һ��1λ��

    bit[ind] = bit[ind + 1] = bit[ind + 2] = 0;
    for(int index = ind; index >= 0; --index) //ö�ٵ�һ��1��λ�ã�ʵ����ö�ٵ�����1��λ�ã�
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
// A ģ��
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



// B ģ��
// ֻ�ı� a1 �� a2 ����
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

