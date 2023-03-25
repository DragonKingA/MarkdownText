# **atcoder contests **
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

**ABC  294**

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

















