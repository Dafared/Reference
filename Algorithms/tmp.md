### 2.最小改变次数

统计大写字母和小写字母的数量
如果第一个字母大写,大写字母数量-1
min(大写数量,小写数量)

### 3.每次将除第x个外其余翻倍

```c++
#include<bits/stdc++.h>
using namespace std;
const int mod=1e9+7;
int qpow(int a,int b,int mod){
    long long res=1,t=a;
    while(b){
        if(b&1){
            res=1ll*(res*t)%mod;
        }
        b>>=1;
        t=(t*t)%mod;
    }
    return res;
}
int main() {
    int n, q;
    cin >> n >> q;
    vector<int> a(n);
    for (int i = 0; i < n; i++) {
        cin >> a[i];
    }
    vector<int> s(n + 1, q);
    for (int i = 0; i < q; i++) {
        int x;
        cin >> x;
        s[x] -= 1;
    }
    long long res = 0;
    for (int i = 0; i < n; i++) {
        res=(res+1ll*qpow(2, s[i + 1],mod)* a[i] % mod)%mod;
    }
    cout << res << endl;
    return 0;
}
```

### 4.小美的加法

```c++
#include <iostream>
#include<vector>
using namespace std;

int main() {
    /*
        思路：
        朴实无华，就清清爽爽将相邻的两个数的乘积 - 和算出来暂存即可
    */

   
    long long num = 0;
    cin >> num;
   

    long long lastNum = 0;
    cin>>lastNum;

    long long sum = lastNum;

    //最大可能增加量
    long long maxAdd = 0;

    long long temp = -1;
    for(long long i = 0; i < num -1; i++)
    {
        cin>>temp;
        sum += temp;

        if( temp * lastNum - temp -lastNum > maxAdd)
        {
            maxAdd = temp*lastNum -temp -lastNum;
        }
       
        lastNum = temp;
    }
    cout<<sum + maxAdd;
    return 0;
}
```

### 5.选择两个元素,一个加 1,另一个减 1

```c++
#include <bits/stdc++.h>
using namespace std;
 
int main()
{
    int n;
    scanf("%d", &n);
    int v[n];
    long long sum = 0;
    int m = 0, M = 0;
    for (int i = 0; i < n; i++)
    {
        scanf("%d", v + i);
        sum += v[i];
        if (v[i] > v[M]) M = i;
        if (v[i] < v[m]) m = i;
    }
    function<long long(int, int)> cal = [&](int p, int idx)
    {
        long long res = 0;
        for (int i = 0; i < n; i++)
        {
            if (i == idx)
                continue;
            if (v[i] > p)
                res += v[i] - p;
        }
        return res;
    };
    long long ans = 0;
    if (sum % n == 0)
        ans = cal(sum / n, -1);
    else
    {
        long long k = (sum - v[M]) % (n - 1);
        ans = cal((sum - v[M]) / (n - 1), M);
        ans = min(ans, cal((sum - v[M]) / (n - 1) + 1, M) + n - 1 - k);
        k = (sum - v[m]) % (n - 1);
        ans = min(ans, cal((sum - v[m]) / (n - 1), m));
        ans = min(ans, cal((sum - v[m]) / (n - 1) + 1, m) + n - 1 - k);
    }
    cout << ans << endl;
    return 0;
}
```

### 6.01串翻转

```c++

#include <iostream>
#include <unordered_map>
#define endl '\n'
#define YES puts("YES")
#define NO puts("NO")
#define umap unordered_map
#pragma GCC optimize(3,"Ofast","inline")
#define ___G std::ios::sync_with_stdio(false),cin.tie(0), cout.tie(0)
using namespace std;
const int N = 2e6 + 10;
 
inline void solve()
{
	string s;
	getline(cin, s);
	int ans = 0;
	int n = s.size();
	for (int i = 0; i < n; ++i)
	{
		int res[2] = {0, 0};
 
		//	 从这里 j == i 开始就是也计算上了子串
		for (int j = i; j < n; ++j)
		{
			// 如果符合 11 或者 00 说明需要操作 一次,
			// 或者 01 的时候我们记录的是强制变成 10 情况，
			// 或者又将 10 强制变成 01的情况
			if ((j % 2 == 0 && s[j] == '1') || (j % 2 == 1 && s[j] == '0'))
			{
				res[0]++;
			}
			else
				res[1]++;
 
			// 累加我们所有子串的权值
			ans += min(res[0], res[1]);
		}
	}
 
	cout << ans << endl;
}
 
 
int main()
{
//	freopen("a.txt", "r", stdin);
	___G;
	int _t = 1;
//	cin >> _t;
	while (_t--)
	{
		solve();
	}
 
	return 0;
}
```

### 7.外卖订单编号

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
const int N=1e5+5;
int t,n,m;
int main(){
	scanf("%d",&t);
	while(t--){
		scanf("%d%d",&n,&m);
		if(m%n==0) printf("%d\n",n);
		else printf("%d\n",m%n);
	}
	return 0;
}
```

