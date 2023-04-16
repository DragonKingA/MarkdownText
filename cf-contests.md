# **codeforces contests**

---

# Div 1









---

# Div 2

## Edu Round 144

### A

```c++
//变化周期为 8，即"FBFFBFFB"，询问的子串最长为10，故存在跨越3个周期的可能，那么将周期字符串定为24位即可（即"FBFFBFFBFBFFBFFBFBFFBFFB"）
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
int T;
string s = "FBFFBFFBFBFFBFFBFBFFBFFB";
void Solve()
{
    int n;
    string t;
    cin >> n >> t;
    cout << (s.find(t) != -1 ? "YES\n" : "NO\n");
    return;
}
int main()
{
    untie();
    cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```

### B

```c++
//对于字符串 a 和 b 只有三种情况：
//1. a 和 b 首字母相同，都为 ch，样本即为 ch + "*"
//2. a 和 b 尾字母相同，都为 ch，样本即为 "*" + ch
//3. a 和 b 中间至少有连续两个字母 sub_s 相同（可能 sub_s 是其中一个字符串的首或尾，但一定是其中一个的中间），此时样本必然有两个 "*"，即为 "*" + sub_s + "*"。
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
int T;
void Solve() {
	string a, b;
	cin >> a >> b;
	int n = a.size(), m = b.size();
	if (a[0] == b[0])
    {
		cout << "YES\n";
		cout << a[0] << '*' << '\n';
		return;
	}
	if (a[n - 1] == b[m - 1])
    {
		cout << "YES\n";
		cout << '*' << a[n - 1] << '\n';
		return;
	}
	for (int i = 0; i + 1 < n; i++)
    {
		if (b.find(a.substr(i, 2)) != -1)
         {
			cout << "YES\n";
			cout << '*' << a.substr(i, 2) << '*' << '\n';
			return;
		}
	}
	cout << "NO\n";
}
int main()
{
    untie();
    cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```



## Edu Round 145

### A

发现规律，分类讨论

```c++
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
int main()
{
    untie();
  int t; cin >> t;
  while (t--)
  {
        char num[5];
        map<int, int> vis;
        for (int i = 1; i <= 4; i++) cin >> num[i], vis[num[i] - '1' + 1]++;
        if (vis.size() == 1) cout << "-1";
        else if (vis.size() == 2)
        {
        for (auto v : vis)
        {
            if (v.second != 2) cout << '6';
            else cout << '4';
            break;
        }
        }
        else cout << '4';
        cout << endl;
  }
}
```

### B

二分

```c++
//一块 x * x 的面积（此时 1 * 1 的面积代表一个点）表示正方形内所有点的个数，故代替求和公式
//注意二分条件
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
int T;
int main()
{
    untie();
    cin >> T;
    while(T--)
    {
        ll n;
        cin >> n;
        ll l = -1, r = 1e9;//二分答案设为 r，r ∈ [0, 1e9)  则 l ∈ [-1, 1e9)
        while(r - l > 1)
        {
            ll mid = l + r >> 1;
            ll cal = (mid + 1) * (mid + 1);
            if(cal < n) l = mid;
            else r = mid;
        } 
        cout << r << '\n';
    }
    return 0;
}

//原臭代码
//求和公式 + 二分答案
#include <iostream>
#include <cstdio>
#include <algorithm>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
int T;
int main()
{
    untie();
    cin >> T;
    while(T--)
    {
        ll n;
        cin >> n;
        ll ans1 = 0, ans2 = 0;

        ll l = 1, r = 1e9;
        while(l < r)
        {
            ll mid = l + r >> 1;
            ll cal = ((((mid - 1) << 3) * mid) >> 1);
            if(cal < n - 1) l = mid + 1;// ans1 = mid;
            else r = mid; 
        }
        ans1 = l;

        l = 1, r = 1e9;
        while(l < r)
        {
            ll mid = l + r >> 1;
            ll cal = (((8 + ((mid - 1) << 3)) * mid) >> 1);
            if(cal < n) l = mid + 1;// ans2 = mid;
            else r = mid;
        }
        ans2 = l;

        ans1 = (ans1 - 1) << 1;
        ans2 = (ans2 << 1) - 1;
        cout << min(ans1, ans2) << '\n';
    }
    return 0;
}
```



## Round 858

### A

签到题	

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <queue>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
int n;
int main()
{
    untie();
    cin >> n;
    while(n--) 
    {
        int a, b, c, d;
        cin >> a >> b >> c >> d;
        a = a + d - b;
        int res = d - b + (a -  c);
        if(a < c || d < b) cout << "-1\n";
        else cout << res << '\n';
    }
    return 0;
}
```

### B

思维题

先分为两种大情况：

* 答案为 **0**

  * 最小的数大于 0 （arr[1] > 0）或 非零数足够消去 0（num + 1 >= mp[0]）

* 答案不为0

  * 答案为 **1**

    * 当最大数大于 1 （arr[n] > 1）时，说明最小非零数无论是不是 1，都可以与最大数换位

      如 0 0 0  0 0 1 1 1 3，可以变成 0 0 0  0 0 3 1 1 1，这样 最小非零和 必然大于 1，

      即空出了 1 的位置，答案必然为 1

  * 答案为 最大数 + 1 （arr[n] + 1）

    * 当最大数等于 1 （arr[n] == 1）时，说明最小非零数也是 1，那么此时必有 两数和 的集合{0， 1}，

      答案不可能是 0 或 1。

      现在又只有两种情况：全是 0 或者 非零数有若干个 1 时

      * 前者，如 0 0 0 0 ，答案显然为 **1**
      * 后者，如 0 0 0 0 0 1 1 1，所有的 1 都能被 0 配对，即只有集合{0， 1}，空出了2，答案为 **2**

```c++
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <vector>
#include <queue>
#include <functional>
#include <map>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 2e5 + 10;
int t, n;
int arr[N], b[N];
map<int, int> mp;
int main()
{
    untie();
    cin >> t;
    while(t--)
    {
        mp.clear();
        cin >> n;
        int num = 0;
        for(int i = 1; i <= n; i++) cin >> arr[i];
        sort(arr + 1, arr + 1 + n);
        for(int i = 1; i <= n; i++) ++mp[arr[i]];
        int ind = mp[0] + 1;
        num = n - mp[0];
        if(arr[1] > 0 || num + 1 >= mp[0]) cout << "0\n"; 
        else if(arr[n] > 1) cout << "1\n";
        else cout << arr[n] + 1 << '\n';
        //arr[n] + 1 等价于：
        //if(arr[n] == 0) cout << "1\n";
        //else cout << "2\n";
    }
    return 0;
}
```

---

## Round 866

### A

模拟题

```c++
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
int T;
string s;
void Solve()
{
    ll ans = 0;
    cin >> s;
    int n = s.size();
    s = '*' + s;
    if(n == 1 && s[1] == '^')
    {
        cout << "1\n";
        return;
    }
    for(int i = 1; i <= n; ++i)
    {
        while(i <= n && s[i] == '^') ++i;
        int t = i;
        while(i <= n && s[i] == '_') ++i;
        ll num = i - t + 1;
        ans += num - (s[t - 1] == '^') - (s[i] == '^');
    }
    cout << ans << '\n';
}
int main()
{
    untie();
    cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```

### B

模拟题

```c++
//找规律，可以发现串成环处理后的最多连续1的个数与答案的关系。
//因为如"110011"，左右两边的1在一定次的变换下必能连续起来，如某行 "111100"，故可以认为 "110011" 等效为 "111100"。所以必须对原串延长出一个循环体或者说是成环处理。
//观察案例"011110"，如果一行中最多的连续1的个数为x， 则只有一行矩形的面积为x * 1 ，两行的面积为 (x - 1) * 2 ,三行的面积是(x - 2) * 3 。最终面积有一个最大值，也就是 (x - i - 1) * i, i 为 x / 2 向上取整。
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
string s;
void Solve()
{
    cin >> s;
    ll n = s.size();
    s += s;//使之成环
    ll cnt0 = 0, cnt1 = 0;
    for(int i = 0; i < n; ++i)
    {
        cnt0 += s[i] == '0';
        cnt1 += s[i] == '1';
    }
    if(cnt1 == 0)
    {
        cout << "0\n";
        return;
    }
    if(cnt0 == 0)
    {
        cout << n * n << '\n';//必须特判
        return;
    }

    n = s.size();
    ll ans = 0, len = 0;//ans 为最长连续1的长度
    for(int i = 0; i < n; ++i)
    {
        if(s[i] == '1') ++len;
        else len = 0;
        ans = max(len, ans);
    }
    if(ans & 1)
    {
        cout << (ans / 2 + 1) * (ans / 2 + 1) << '\n';
    }
    else
    {
        cout << (ans / 2 + 1) * (ans / 2) << '\n';
    }
}
int main()
{
    untie();
    int T;
    cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```

### C

模拟思维题

```c++
//思路：必要条件是要使得 a 没有 mex + 1 这个值，那么获取最长的两端 mex + 1 值的下标 l, r，将 [l, r] 变成 mex 值，同时这样就算不出 mex()
//也相当于给 mex + 1 提供可能性，最后再重新算一次 mex()，比对是否是 原mex值 + 1 即可
//注意，特判 a 是否包含 0 ~ n - 1 所有种数，即此时 mex == n，这样恒不可能出现 mex + 1
//此后还需要特判 mex + 1 是否本来就没出现过，那么必然符合条件。如 0 0，其中 mex = 1，而 2 没出现过，则 mex + 1 必存在
#include <bits/stdc++.h>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e5 + 10;
int n, a[N];
map<int, int> mp;
void Solve()
{
    cin >> n;

    int mex = 0, t = 0;
    mp.clear();
    for(int i = 1; i <= n; ++i) 
    {
        cin >> a[i];
        mp[a[i]] = i;
    }
    while(mp[mex]) ++mex;
    if(mex == n)
    {
        cout << "No\n";
        return;
    }

    t = mex;
    int R = mp[++mex];//mex + 1
    if(R == 0)
    {
        cout << "Yes\n";
        return;
    }
    bool ok = 0;
    for(int i = 1; i <= R; ++i)
    {
        if(a[i] == mex) ok = 1;
        if(ok) a[i] = t;
    }

    mex = 0;
    mp.clear();
    for(int i = 1; i <= n; ++i) mp[a[i]] = i;
    while(mp[mex]) ++mex;

    cout << ((t + 1 == mex) ? "Yes":"No") << '\n';
}
int main()
{
    untie();
    int T;
    cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```





---

# Div 3

## Round 863

### A ~ B

```c++
//A
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
int main()
{
    untie();
    int t;
    cin >> t;
    string s;
    while(t--)
    {
        int n;
        string  x;
        cin >> n >> x >> s;
        int ok = 0;
        for(int i = 0; i < s.size();i ++)
        {
            char num = s[i];
            if(num < x[0])
            {
                s = s.substr(0, i) + x + s.substr(i);
                ok = 1;
                break;
            }
        }
        if(!ok) s = s + x;
        cout << s << '\n';
    }
    return 0;
}



//B
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
int cal(int x, int y, int m)
{
    if(x <= m && y <= m) return max(abs(x - m), abs(y - m));
    else if(x <= m && y > m) return max(abs(x - m), abs(y - (m + 1)));
    else if(x > m && y <= m) return max(abs(x - (m + 1)), abs(y - m));
    else return max(abs(x - (m + 1)), abs(y - (m + 1)));
}
int main()
{
    untie();
    int t;
    cin >> t;
    while(t--)
    {
        int n, x1, y1, x2, y2;
        cin >> n >> x1 >> y1 >> x2 >> y2;
        int m = n / 2;
        cout << abs(cal(x1, y1, m) - cal(x2, y2, m)) << '\n';
    }
    return 0;
}
```

### C

```c++
//C 思维模拟
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cstring>
#include <string>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e5 + 10;
int a[N];
int main()
{
    untie();
    int t;
    cin >> t;
    int tex = 0;
    while(t--)
    {
        int n;
        cin >> n;
        n--;
        for(int i = 1; i <= n; i++) cin >> a[i];
        if(n == 1)
        {
            cout << a[1] << " 0\n";
            continue;
        }
        if(a[1] < a[2]) cout << "0 " << a[1] << " ";
        else cout << a[1] << " " << a[2] << " ";
        for(int i = 2; i < n; i++)
        {
            cout << min(a[i], a[i + 1]) << " ";
        }
        cout << a[n] << '\n';
    }
    return 0;
}
//7 3 4 4 4 5 
//7 3 3 4 4 4 5 

//0' 1 2 2 3 3 2 2 1 0'
//0  1 2 2 3 2 2 1 1
```

### E

```c++
//E. 数论
//我们知道对于十进制数来说它是由 0 ~ 9 的十个数字组成，现在我们要知道仅由 0 ~ 3 和 5 ~ 9 的九个数字组成(即类似九进制数)的第 k 个数。
//此时 k 的数值就相当于由十个数字组成的十进制数 k 的位置，问第 k 个类九进制数相当于问 k 转换为类九进制数的数值。
//普通九进制数相当于由 0 ~ 8 的九个数字组成 0 1 2 3 4 5 6 7 8
//而我们需要的是特殊的九进制数，由 0 1 2 3 5 6 7 8 9 这九个数字组成，对于 0 1 2 3 这段跟普通九进制一样，
//而对于 4 5 6 7 8 -> 5 6 7 8 9 相当于向右移一位（对应数位+1），故对于算出的普通九进制数的数位 x 若 x > 3 则需要 x++，得到类九进制数 
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cctype>
#include <cstring>
#include <string>
#include <cmath>
#include <vector>

using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
int T;

void Solve()
{
    ll k;
    cin >> k;
    vector<int> num;
    //先求出普通九进制数的各数位，并且先求出的数位是低位，即还需要翻转
    while(k)
    {
        num.push_back(k % 9);
        k /= 9;
    }
    reverse(num.begin(), num.end());
    for(auto x : num)
    {
        if(x > 3) cout << x + 1;
        else cout << x;
    }
    cout << '\n';
}

int main()
{
    untie();
    cin >> T;
    while(T--)
    {
        Solve();
    }
    return 0;
}
```







---

# Div 4

## Round 849

### A ~ D 、G1

签到题

```c++
//A
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <string>
#include <cstring>
#include <map>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
string s = "codeforces";
map<char, bool> mp;
int main()
{
    for(auto ch : s)
    {
        mp[ch] = 1;
    }
    untie();
    int t;
    char ch;
    cin >> t;
    while(t--)
    {
        cin >> ch;
        if(mp[ch]) cout << "YES\n";
        else cout << "NO\n";
    }
    return 0;
}



//B
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <string>
#include <cstring>
#include <map>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
int main()
{
    untie();
    int t, n;
    cin >> t;
    while(t--)
    {
        cin >> n;
        char ch;
        int x = 0, y = 0, ok = 0;
        while(n--)
        {
            cin >> ch;
            switch(ch)
            {
                case 'L': x--; break;
                case 'R': x++; break;
                case 'D': y--; break;
                case 'U': y++; break;
            }
            if(x == 1 && y == 1)
            {
                ok = 1;
            }
        }
        if(ok) cout << "YES\n";
        else cout << "NO\n";
    }
    return 0;
}



//C
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <string>
#include <cstring>
#include <map>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 2e4 + 10;
char s[N];
int main()
{
    untie();
    int t, n;
    cin >> t;
    while(t--)
    {
        cin >> n;
        cin >> s;
        int l = 0, r = n - 1;
        int ans = 0;
        // if(n & 1) ans = 1;
        int sum = 0;
        while(l < r)
        {
            if(s[l] != s[r])
            {
                ++sum;
            }
            else
            {
                break;
            }
            ++l, --r;
        }
        ans += n - 2 * sum;
        cout << ans << '\n';
    }
    return 0;
}



//D
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <string>
#include <cstring>
#include <map>
#include <set>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
const int N = 2e5 + 10;
string s;
int c1[200], c2[200];
int main()
{
    untie();
    int t, n;
    cin >> t;
    while(t--)
    {
        memset(c1, 0, sizeof(c1));
        memset(c2, 0, sizeof(c2));
        cin >> n;
        cin >> s;
        int len = s.size(), ans = 0;
        int cnt1 = 0, cnt2 = 0;
        for(int i = 0; i < len; i++) 
        {
            if(!c2[s[i]]) ++cnt2;
            c2[s[i]]++;
        }
        ans = cnt2;
        for(int i = 0; i < len; i++)
        {
            if(!c1[s[i]]) ++cnt1;
            c1[s[i]]++;
            c2[s[i]]--;
            if(!c2[s[i]]) --cnt2;
            ans = max(ans, cnt1 + cnt2);
        }
        cout << ans << '\n';
    }
    return 0;
}



//G1
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <string>
#include <cstring>
#include <map>
#include <set>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e5 + 10;
ll a[N];
int main()
{
    untie();
    int t, n;
    cin >> t;
    while(t--)
    {
        ll ans = 0, c;
        cin >> n >> c;
        for(int i = 1; i <= n; i++)
        {
            cin >> a[i];
            a[i] += i;
        }
        sort(a + 1, a + 1 + n);
        for(int i = 1; i <= n; i++)
        {
            if(c >= a[i])
            {
                ++ans;
                c -= a[i];
            }
            else
                break;
        }
        cout << ans << '\n';
    }
    return 0;
}
```

### E

思维转换 或 DP求解

```c++
//dp
//定义 dp[i][0] 为到 i 处的最大和（当前不翻转），dp[i][1] 为当前翻转后最大和 
//不翻转则直接继承最大的 1 ~ i-1 的状态即 dp[i][0] =  a[i] + max(dp[i - 1][0], dp[i - 1][1]);
//翻转，和值 减去翻转后差值 并 讨论a[i - 1]原符号是否改变（dp[i-1][1]说明改变过，故符号取反,即 dp[i][1] = -a[i] + max(dp[i - 1][0] - 2 * a[i - 1], dp[i - 1][1] + 2 * a[i - 1]);
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <string>
#include <cstring>
#include <map>
#include <set>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e5 + 10;
ll a[N], dp[N][2];
int main()
{
    untie();
    int t, n;
    cin >> t;
    while(t--)
    {
        memset(dp, 0, sizeof(dp));
        cin >> n;
        for(int i = 1; i <= n; i++) cin >> a[i];
        dp[2][0] = a[1] + a[2], dp[2][1] = -dp[2][0];
        for(int i = 3; i <= n; i++)
        {
            dp[i][0] =  a[i] + max(dp[i - 1][0], dp[i - 1][1]);//不翻转则直接继承最大的 1 ~ i-1 的状态
            dp[i][1] = -1LL * a[i] + max(dp[i - 1][0] - 2LL * a[i - 1], dp[i - 1][1] + 2LL * a[i - 1]);//翻转，和值 减去翻转后差值 并 讨论a[i - 1]原符号是否改变（dp[i-1][1]说明改变过，故符号取反）
        }
        cout << max(dp[n][0], dp[n][1]) << "\n";
    }
    return 0;
}



//思维 - 转换问题
//可以发现,翻转相邻两元素的符号,且不限制次数,其实就可以翻转数组内 任意两个元素 的符号(只要遍历到另一个元素期间一直翻转)
//故只需要负数两两配对,一起翻转即可将所有负数变成正数
//按负数个数分两种情况: 偶数个(直接两两配对) 和 奇数个(会剩下一个奇数没配对,那么只需要将转正后数组中 绝对值最小的 数 剩下来即可) 
//奇数个负数时,最后一个负数的翻转配对应该 是转正后数组中 绝对值最小的数,若绝对值最小是它自己,则它放弃翻转
//如 -4 -5 -6 1 2 3, 最后应将最后一个负数 与 1 配对
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <string>
#include <cstring>
#include <map>
#include <set>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
const int N = 2e5 + 10;
ll a[N];
int main()
{
    untie();
    int t, n;
    cin >> t;
    while(t--)
    {
        ll sum = 0, cnt = 0;
        cin >> n;
        for(int i = 0; i < n; i++) 
        {
            cin >> a[i];
            if(a[i] < 0)
            {
                a[i] = -a[i];
                ++cnt;
            }
            sum += a[i];
        }
        if(cnt & 1) sum -= 2 * *min_element(a, a + n);
        cout << sum << '\n';
    }
    return 0;
}
```

### F

区间和问题 - 树状数组 \ 线段树

```c++
//最大的数 99999999 变化一次后为 72 ,两次后为 9,故总变化次数不超过2两次
//只需要维护区间内操作次数和,求 ai 时按变化次数计算即可,这里用cnt[i]记录第i个元素变化次数
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <cmath>
#include <string>
#include <cstring>
#include <map>
#include <set>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll int
#define lowbit(x) ((x) & -(x))
const int N = 2e5 + 10;
ll t[N], a[N], cnt[N];
int T, n, q;
void update(ll x, ll d)
{
    for(int i = x; i <= n; i += lowbit(i)) t[i] += d;
}
ll query(ll x)
{
    ll res = 0;
    for(int i = x; i > 0; i -= lowbit(i)) res += t[i];
    return res;
}
ll cal(ll x)
{
    ll res = 0;
    for(; x; x /= 10) res += x % 10;
    return res;
}
int main()
{
    untie();
    cin >> T;
    while(T--)
    {
        memset(t, 0, sizeof(t));
        memset(cnt, 0, sizeof(cnt));
        cin >> n >> q;
        for(int i = 1; i <= n; i++) cin >> a[i];
        while(q--)
        {
            ll op, l, r, x;
            cin >> op;
            if(op == 1)
            {
                cin >> l >> r;
                update(l, 1);
                update(r + 1, -1);
            }
            else
            {
                cin >> x;
                ll tmp = query(x), ans = a[x];
                for(int i = cnt[x]; i < tmp && a[x] > 9; i++)//防止重复计算 和 无效计算
                {
                    a[x] = cal(a[x]);//变化直接应用到原数组上
                }
                cnt[x] = tmp;
                cout << a[x] << '\n';
            }
        }
    }
    return 0;
}
```





## [Round 859](https://codeforces.com/contest/1807)

### A ~ D

签到

```c++
//A
#include <iostream>
#include <cstdio>
#include <algorithm>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
int n;
int main()
{
    untie();
    int t;
    cin  >> t;
    while(t--)
    {
        int a, b, c;
        cin >> a >> b >> c;
        if(a + b == c)
        	cout << "+\n";
        else
            cout << "-\n";
    }
    return 0;
}



//B
#include <iostream>
#include <cstdio>
#include <algorithm>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll long long
int n;
int arr[200];
int main()
{
    untie();
    int t;
    cin  >> t;
    while(t--)    
    {
        cin >> n;
        ll s1 = 0, s2 = 0;
        for(int i = 1; i <= n; i++) 
        {
            cin >> arr[i];
            if(arr[i] % 2 == 0) s1 += arr[i];
            else s2 += arr[i];
        }
        if(s1 > s2) cout << "YES\n";
        else cout << "NO\n";
    }
    return 0;
}
 


//C
//可以假定第一个字符变成 0（因为如 010101 和 101010 在这里是完全等效的），定义 map<char, int> mp ，用 mp[ch] 来判断字符ch变成了 0 还是 1，用临时变量 ex 来判断下一个字符该变成 0 或 1。然后遍历字串，当到某个重复字符时可以判断 mp[ch] 是否为 ex，若不是则该字串不符合条件。
#include <iostream>
#include <cstdio>
#include <algorithm>
#include <map>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
int n;
char s[2050];
map<char, int> mp;
int main()
{
    untie();
    int t;
    cin  >> t;
    while(t--)    
    {
        mp.clear();
        cin >> n;
        for(int i = 1; i <= n; i++) cin >> s[i];
        mp[s[1]] = 0;	   //假定所有字符s[1]变成0
        int ex = 1, ok = 1;//ex = 1 表示下一个字符应变成 1，为0同理。
        for(int i = 2; i <= n; i++)
        {
            if(!mp.count(s[i])) mp[s[i]] = ex;
            else if(mp[s[i]] != ex)
            {
                ok = 0;
                break;
            }
            ex ^= 1;
        }
        if(ok) cout << "YES\n";
        else cout << "NO\n";
    }
    return 0;
}



//D
//题意：给定一段序列 A，现有 q 次操作，每次操作将 [L, R] 上每个数都变成 数k ，且每次操作后原序列不会变化即操作是独立的，问每次操作后序列 A 所有数之和是否为奇数
//显然，将序列处理为 前缀和序列 更加好操作，直接求出变化后的和再判奇数即可。
#include <iostream>
#include <cstdio>
#include <algorithm>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll unsigned long long
const int N = 2e5 + 10;
ll a[N];
int main()
{
    untie();
    int t;
    cin  >> t;
    while(t--)    
    {
        int n, q;
        cin >> n >> q;
        for(int i = 1; i <= n; i++) cin >> a[i];
        for(int i = 2; i <= n; i++) a[i] += a[i - 1];
        ll sum = a[n];
        while(q--)
        {
            int l, r;
            ll k;
            cin >> l >> r >> k;
            ll now = sum - (a[r] - a[l - 1]) + 1ull * (r - l + 1) * k;
            if(now & 1) cout << "YES\n";
            else cout << "NO\n";
        }
    }
    return 0;
}
```

### G1、G2

思维题

```c++
//G1、G2（同一套代码即可过题）
//题意：
//有一个序列 A，初始仅为 {1}，它可以根据当前集合情况进行如下操作：可以选择集合内任意个数（每个数只选一次）求和后将 和值s 加入序列。先给一个序列 C，问 A 经过任意次操作后是否能变成 C。
//分析：
//如 {1} -> {1 1} -> {1 1 2} - {1 1 2 2} 或 {1 1 2 3} 或 {1 1 2 4}，然后产生不同的分支，每个分支的最大值不同会影响其后面序列 最大变化值，如变成 {1 1 2 3} 那么下次最多就 {1 1 2 3 7}，而 {1 1 2 4} 至少可以变成 {1 1 2 4 8}。
//现在我们知道，新元素最大为 当前元素和 s，当当前元素 a[i] > s 时说明不存在正确方案，若 a[i] == s 那自然可以，但对应 a[i] 不总是取 s，一般在 a[i] < s 时，将 a[i] 加入序列 {1 1 2 ··· a[i]} 后，下一个元素加入时的最大变化值为 上一次的s + a[i] ---> 推出，s += last 递推变化。
#include <iostream>
#include <cstdio>
#include <algorithm>
using namespace std;
#define untie() {cin.tie(0)->sync_with_stdio(false), cout.tie(0);}
#define ll unsigned long long
const int N = 2e5 + 10;
int a[N];
int main()
{
    untie();
    int t;
    cin  >> t;
    while(t--)    
    {
        int n;
        cin >> n;
        for(int i = 1; i <= n; i++) cin >> a[i];
        sort(a + 1, a + 1 + n);
        ll cnt = 1, last = 0;
        int ok = 1;
        for(int i = 1; i <= n; i++)
        {
            if(a[i] > cnt)
            {
                ok = 0;
                break;
            }
            else
            {
                cnt = a[i];//重点：下一次可加入元素的最大限制为 cnt = a[i] + last
            }
            cnt += last;
            last = cnt;
        }
        if(ok) cout << "YES\n";
        else cout << "NO\n";
    }
    return 0;
}
```







