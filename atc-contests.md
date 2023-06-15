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

## ABC 302(*ֵ���ٿ�)

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

## ABC 304(*)

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

