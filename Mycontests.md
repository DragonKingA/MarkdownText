# **School Contests **
---
## 1

考点：**模拟除法**、二位费用背包dp、模拟

```c++
//G
//模拟除法
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e6;
string s = "";
int main()
{
    untie();
    ll n, k;
    cin >> n >> k;
    ll res = 0;
    for(int i = 0; i < n; ++i) 
    {
        ll x;
        cin >> x, res += x;
    }
    s += to_string(res / n) + ".";
    // cout << s << "\n";
    res %= n;
    //71 / 8 = 8.875
    //71 % 8 = 7
    //7 / 8 = 0.875
    //(int)70 / 8 = 0.8
    //70 % 8 = 6
    //6 / 8 = 0.75
    //(int)60 / 8 = 0.7
    //293 % 3 = 2
    // cout << res << '\n';
    //293 / 3 = 97
    //293 % 3 = 2
    //2 * 10 = 20
    //20 / 3 = 6
    //20 % 3 = 2
    for(int i = 0; i < k; ++i)
    {
        res *= 10;
        s.push_back(res / n + '0');
        res %= n;
    }
    cout << s << "\n";
    return 0;
}



//H
//二维费用的背包dp
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 500;
ll dp[N][N];
int main()
{
    untie();
    int n, H, S;
    cin >> n >> H >> S;
    for(int i = 0; i < n; ++i)
    {
        int h, s, w;
        cin >> h >> s >> w;
        for(int j = S; j >= 0; --j)
        {
            if(H + j - s <= 0) break;
            for(int k = H; k >= 0; --k)
            {
                if(k + j - s <= 0) break;
                if(j - s < 0 && k - h + (j - s) > 0) dp[j][k] = max(dp[j][k], dp[0][k - h + (j - s)] + w);
                else if(j - s >= 0 && k - h > 0) dp[j][k] = max(dp[j][k], dp[j - s][k - h] + w);
            }
        }
    }
    cout << dp[S][H] << '\n';
    return 0;
}



//M
//模拟
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 1e3;
int n, m;
string a[N], b[N];
int main()
{
    untie();
    cin >> n >> m;
    string base(m, '0');
    // cout << ">> " << base << '\n';
    for(int i = 0; i < n; ++i)
        cin >> a[i], b[i] = a[i];
    for(int i = 0; i < n; i += 2)
        for(int j = 0; j < m; ++j)
            a[i][j] = b[i + 1][j] = '1';
    for(int i = 0, ok = 0; i < n; ++i) 
    {
        if(a[i] == base) ok = 1;
        if(ok) cout << base << '\n'; 
        else cout << a[i] << '\n';
    }
    // for(int i = 0; i < n; ++i) cout << b[i] << '\n';
    for(int i = 0, ok = 0; i < n; ++i) 
    {
        if(i != 0 && b[i] == base) ok = 1;
        if(ok) cout << base << '\n'; 
        else cout << b[i] << '\n';
    }
    return 0;
}

/*
11111
00000
11111
00000
11111

00000
11111
00000
11111
00000


11111
00100
11111
01100
11111

00000
11111
01010
11111
00000
*/



//D
//模拟
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e5 + 10;
int n, visx[N], visy[N];
int main()
{
    untie();
    cin >> n;
    int ans1 = 0, ans2 = 0;
    while(n--)
    {
        int x, y;
        cin >> x >> y;
        int cnt = 0;
        if(visx[x]) ++cnt;
        ++visx[x];
        if(visx[x - 1] >= visx[x]) ++cnt;//左侧有相邻
        if(visx[x + 1] >= visx[x]) ++cnt;//右侧有相邻
        ans1 += 4 - 2 * cnt;

        cnt = 0;
        if(visy[y]) ++cnt;
        ++visy[y];
        if(visy[y - 1] >= visy[y]) ++cnt;
        if(visy[y + 1] >= visy[y]) ++cnt;
        ans2 += 4 - 2 * cnt;
        cout << ans1 << " " << ans2 << '\n';
    }
    return 0;
}
```

