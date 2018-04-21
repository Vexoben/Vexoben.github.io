---
layout: post
title: 2015 Multi-University Contest 4
date: 2018-04-19 18:27:22 +0800
categories: contest
tags: 脑洞题 模拟 构造
img: https://vexoben.github.io/assets/images/Blog/2015-Multi-University-Contest-4.JPG
---

今天World Final。蒟蒻连去看直播的资格都没有啊只好来VP多校了。

2015年的XJ题，大佬辈出的年代啊。

题目：HDU5327~5338

## **A. Olympiad**

### **题意**

问[l,r]区间内有多少各位数字不相同的数。l,r<=1e5。

### **题解**

签到题。模拟即可。

```cpp
#include<set>
#include<map>
#include<queue>
#include<stack>
#include<cmath>
#include<cstdio>
#include<time.h>
#include<vector>
#include<cstring>
#include<stdlib.h>
#include<iostream>
#include<algorithm>
#define R register
#define LL long long
using namespace std;
const int N=1e5+10;
const int mod=1e9+7;
const int inf=0x3f3f3f3f;

int n,sum[N],bit[12];

namespace FastIO {
	template<typename tp> inline void read(tp &x) {
		x=0; register char c=getchar(); register bool f=0;
		for(;c<'0'||c>'9';f|=(c=='-'),c = getchar());
		for(;c>='0'&&c<='9';x=(x<<3)+(x<<1)+c-'0',c = getchar());
		if(f) x=-x;
	}
	template<typename tp> inline void write(tp x) {
	  if (x==0) return (void) (putchar('0'));
     if (x<0) putchar('-'),x=-x;
     int pr[20]; register int cnt=0;
     for (;x;x/=10) pr[++cnt]=x%10;
     while (cnt) putchar(pr[cnt--]+'0');
	}
	template<typename tp> inline void writeln(tp x) {
		write(x);
		putchar('\n');
	}
}
using namespace FastIO;

int check(int x) {
	memset(bit,0,sizeof(bit));
	while (x) {
		int r=x%10;
		if (bit[r]) return 0;
		bit[r]=1;
		x/=10;
	}
	return 1;
}

int main() {
	int t; read(t);
	for (int i=1;i<N;i++) {
		sum[i]=sum[i-1]+check(i);
	}
	for (int i=1;i<=t;i++) {
		int a,b; read(a); read(b); cout<<sum[b]-sum[a-1]<<endl;
	}
	return 0;
}
```

## **B. Problem Killer**

### **题意**

给定一个长为n的串，求最长的等差或等比子串。

### **题解**

签到题*2，模拟即可。

```cpp
#include <bits/stdc++.h>
#define int long long
#define fo(i, n) for(int i = 1; i <= (n); i ++)
#define out(x) cerr << #x << " = " << x << "\n"
#define type(x) __typeof((x).begin())
#define foreach(it, x) for(type(x) it = (x).begin(); it != (x).end(); ++ it)
using namespace std;
// by piano
template<typename tp> inline void read(tp &x) {
  x = 0;char c = getchar(); bool f = 0;
  for(; c < '0' || c > '9'; f |= (c == '-'), c = getchar());
  for(; c >= '0' && c <= '9'; x = (x << 3) + (x << 1) + c - '0', c = getchar());
  if(f) x = -x;
}
template<typename tp> inline void arr(tp *a, int n) {
  for(int i = 1; i <= n; i ++)
    cout << a[i] << " ";
  puts("");
}
namespace one {
  static const int N = 2e6 + 233;
  int n, ans, a[N], l, r;

  inline void upd(void) {
    ans = max(ans, r - l + 1);
  }

  int main(void) {
    ans = 0;
    read(n);
    for(int i = 1; i <= n; i ++) read(a[i]);
    if(n <= 1) return cout << "1\n", 0;
    l = 1, r = 2;
    upd();
    for(r = 3; r <= n; r ++) {
      int delta = a[r] - a[r - 1];
      while(l < r && a[l + 1] - a[l] != delta)
        ++ l;
      upd();
    }
    l = 1, r = 2;
    upd();
    for(r = 3; r <= n; r ++) {
      while(l < r && a[l + 1] * a[r - 1] != a[r] * a[l])
        ++ l;
      upd();
    }
    cout << ans << "\n";
  }
}
main(void) {
  int T; for(read(T); T --;) one::main();
}
```


## **H. Virtual Participation**

### **题意**

给定整数k<=1e9,构造一个串，使它不同的子串个数恰好为k，串长不大于100000。

### **题解**

大瞎搞题，糊了个和标算不同的做法。

看到题第一反应就是，每次将相同的数放若干次。

假设我们放第i个数xi次，总共放了r个数，那么不同的子串个数就是：

![][2]

但是这个式子很难处理。于是考虑缩减r的范围。

如果r=2，设放了x个1，y个2，那么子串个数就是(x+y+xy),因式分解一下：

![][3]

这个式子表明如果(k+1)可以分解为两个和不超过100002的整数的乘积，我们就可以构造出解。

这样不能处理(k+1)为质数。考虑把r扩展到3：

![][4]

枚举x=1,2后发现有规律。不妨把x当做常数，那么就有：

![][5]

这时只要k+(x+1)^2-x可以分解为两个和不超过100000的整数的乘积就行了。不妨将x从1开始枚举，只需很少的次数就可枚举出解。

```cpp
#include<set>
#include<map>
#include<queue>
#include<stack>
#include<cmath>
#include<cstdio>
#include<time.h>
#include<vector>
#include<cstring>
#include<stdlib.h>
#include<iostream>
#include<algorithm>
#define R register
#define LL long long
using namespace std;
const int N=1e5+10;
const int mod=1e9+7;
const int inf=0x3f3f3f3f;

int n,k,T;

namespace FastIO {
	template<typename tp> inline void read(tp &x) {
		x=0; register char c=getchar(); register bool f=0;
		for(;c<'0'||c>'9';f|=(c=='-'),c = getchar());
		for(;c>='0'&&c<='9';x=(x<<3)+(x<<1)+c-'0',c = getchar());
		if(f) x=-x;
	}
	template<typename tp> inline void write(tp x) {
	  if (x==0) return (void) (putchar('0'));
     if (x<0) putchar('-'),x=-x;
     int pr[20]; register int cnt=0;
     for (;x;x/=10) pr[++cnt]=x%10;
     while (cnt) putchar(pr[cnt--]+'0');
	}
	template<typename tp> inline void writeln(tp x) {
		write(x);
		putchar('\n');
	}
}
using namespace FastIO;

void Solve() {
	for (int x=1;x<=1000;x++) {
		int t=k+(x+1)*(x+1)-x;
		for (int i=x+1;i*i<=t;i++) {
			if (t%i) continue;
			int y=i-x-1,z=t/i-x-1;
			if (y<0||z<0) continue;
			if (x+y+z>100000) continue;
			printf("%d\n",x+y+z);
			for (int i=1;i<x;i++) putchar('1'),putchar(' ');
			if (!y) y=z,z=0;
			if (x) {
				putchar('1');
				if (y||z) putchar(' ');
			}
			for (int i=1;i<y;i++) putchar('2'),putchar(' ');
			if (y) {
				putchar('2');
				if (z) putchar(' ');
			}
			for (int i=1;i<z;i++) putchar('3'),putchar(' ');
			if (z) putchar('3'); 
			putchar('\n');
			return;
		}
	}
	throw;
}

main() {
	while (~scanf("%d",&k)) Solve();
	return 0;
}
```

## **I. Walk Out**

### **题意**

给定一个n*m的迷宫，每个格子上有0或1。可以上下左右移动，求(1,1)到(n,m)的最短路。

路径长度的定义是：将所有走过格子上的数字按次序写下后得到的二进制数的大小。

去前导零后输出二进制数，n,m<=1000。

### **题解**

VP时我切的第二题。不难但挺精彩的一道题。

不能跑最短路的瓶颈在于dist的比较是O(n)的。一开始想优化它但是没有想到好的办法。

后面突然想到：如果(1,1)上的数字是1，那最优解一定也是曼哈顿距离最小的解。

顺着这个思路想下去。如果走过的路径上已经有了1，那么只能向下或向右走；否则可以向四个方向走。

将搜索分为两个阶段：

1、只走0的格子。在可以走到的范围内找出离终点曼哈顿距离最小的1，将他们入队。

2、取出这些1，记录答案，再往下BFS。

显然每个点只会入队出队一次，复杂度O(n*m)。

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=1e3+10;
const int u[4]={1,0,-1,0};
const int v[4]={0,1,0,-1};

char s[N];
int T,n,m,cnt,ans_num,type;
int dis[N],a[N][N],ans[N*10],vis[N][N];
struct node {
	int x,y,dis,To_end;
}tmp[N];
queue<node> Q;

void Init() {
	memset(ans,0,sizeof(ans));
	memset(vis,0,sizeof(vis));
	memset(dis,0,sizeof(dis));
	cnt=0; ans_num=0;
}

void Getplace() {
	vis[1][1]=1;
	if (a[1][1]) return (void) (tmp[++cnt]=(node) {1,1,1,n-1+m-1});
	Q.push((node){1,1,1,n-1+m-1});
	while (!Q.empty()) {
		int x=Q.front().x,y=Q.front().y,d=Q.front().dis; Q.pop();
		for (int i=0;i<4;i++) {
			int xx=x+u[i],yy=y+v[i],To_end=n-xx+m-yy;
			if (xx>n||xx<=0||yy>m||yy<=0) continue;
			if (!vis[xx][yy]) {
				vis[xx][yy]=1;
				if (!a[xx][yy]) Q.push((node){xx,yy,d+1,To_end});
				else {
					if (cnt&&To_end<tmp[cnt].To_end) tmp[cnt=1]=(node) {xx,yy,d+1,To_end};
					else if (!cnt||To_end==tmp[cnt].To_end) tmp[++cnt]=(node) {xx,yy,d+1,To_end};
				}
			}
		}
	}
}

void Solve() {
	scanf("%d%d",&n,&m);
	for (int i=1;i<=n;i++) {
		scanf("%s",s+1);
		for (int j=1;j<=m;j++) a[i][j]=s[j]-'0';
	}
	type=a[n][m]; a[n][m]=1;
	Init();
	Getplace();
	while (cnt) {
		ans[++ans_num]=tmp[1].dis;
		for (int i=1;i<=cnt;i++) Q.push(tmp[i]);
		cnt=0;
		while (!Q.empty()) {
			int x=Q.front().x,y=Q.front().y,d=Q.front().dis; Q.pop();
			for (int i=0;i<2;i++) {
				int xx=x+u[i],yy=y+v[i],To_end=n-xx+m-yy;
				if (xx>n||xx<=0||yy>m||yy<=0) continue;
				if (!vis[xx][yy]) {
					vis[xx][yy]=1;
					if (!a[xx][yy]) Q.push((node){xx,yy,d+1,To_end});
					else {
						if (cnt&&To_end<tmp[cnt].To_end) tmp[cnt=1]=(node) {xx,yy,d+1,To_end};
						else if (!cnt||To_end==tmp[cnt].To_end) tmp[++cnt]=(node) {xx,yy,d+1,To_end};
					}
				}
			}
		}
	}
	int now=ans[1];
	for (int i=1;i<=ans_num;i++) {
		if (i>1) for (;now<ans[i];now++) putchar('0');
		if (i!=ans_num) putchar('1');
		else cout<<type;
		now++;
	}
	putchar('\n');
}

int main() {
	scanf("%d",&T);
	while (T--) Solve();
	return 0;
}
```

## **J. XYZ and Drops**

### **题意**

模拟游戏十滴水……

### **题解**

按题意模拟即可

跟DHL看了半天查不出错，一气之下重构代码就AC了

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=105;
const int u[4]={1,-1,0,0};
const int v[4]={0,0,1,-1};

int r,c,n,T,x[N],y[N];
int siz[N],tim[N],id[N][N];

struct drop {
	int x,y,dx,dy;
};
vector<int> wat;
vector<drop> vec,tmp;

void Push(int x,int y) {
	for (int i=0;i<4;i++) tmp.push_back((drop){x,y,u[i],v[i]});
}

void Init() {
	vec.clear(); tmp.clear(); wat.clear();
	memset(id,0,sizeof(id));
	memset(tim,0,sizeof(tim));
	for (int i=1;i<=n;i++) scanf("%d%d%d",&x[i],&y[i],&siz[i]);
	for (int i=1;i<=n;i++) id[x[i]][y[i]]=i;
	int xx,yy; scanf("%d%d",&xx,&yy); Push(xx,yy);
}

void Doit(int T) {
	vec.clear();
	for (int i=0;i<tmp.size();i++) vec.push_back(tmp[i]);
	tmp.clear();
	wat.clear();
	for (int i=0;i<vec.size();i++) {
		drop t=vec[i];
		int xx=t.x+t.dx,yy=t.y+t.dy;
		if (xx<=0||xx>r||yy<=0||yy>c) continue;
		int p=id[xx][yy];
		if (siz[p]) {
			if (++siz[p]>=5&&!tim[p]) {
				tim[p]=T;
				wat.push_back(p);
			}
		}
		else tmp.push_back((drop){xx,yy,t.dx,t.dy});
	}
	for (int i=0;i<wat.size();i++) {
		siz[wat[i]]=0;
		Push(x[wat[i]],y[wat[i]]);
	}
}

void Solve() {
	Init();
	for (int i=1;i<=T;i++) Doit(i);
	for (int i=1;i<=n;i++) {
		if (siz[i]) printf("1 %d\n",siz[i]);
		else printf("0 %d\n",tim[i]);
	}
}

int main() {
	while (cin>>r>>c>>n>>T) Solve();
	return 0;
}
```


## **L. ZZX and Permutations**

### **题意**

给定一个长为n的全排列，用括号将它分为若干个区间，使变换后全排列的字典序最大。

例如全排列:1 4 5 6 3 2,如果划分为(1,4,5,6)(3,2),表示将1写到6的位置，6写到5的位置，5写到4的位置……

### **题解**

从小到大考虑每个数的位置被哪个数所占据。这有三种情况：

1、被自己占据；

2、被左边可取范围内最大的数占据；

3、被右边第一个数占据。

对于前两种情况，我们可以直接确定一个括号区间。直接将它们写进答案后删除这个区间。注意左边最大数和这个数之前不能有被删除过的区间。

对于第三种情况，我们只能确定一个括号区间的左括号l。右括号的选择又有两种情况：

1、在后面考虑一个数x的时将[l,x]归为一个区间；

2、考虑(l+1)时确定右括号。

那么只需要让(l+1)不能为左括号就可以了。

```cpp
#include<bits/stdc++.h>
using namespace std;
const int N=1e5+10;

int n,m,T;
int a[N],id[N],mx[N<<2],num[N<<2],ans[N],used[N];
set<int> Set;

void Init() {
	scanf("%d",&n);
	memset(a,0,sizeof(int)*(n+3));
	memset(id,0,sizeof(int)*(n+3));
	for (int i=1;i<=n;i++) scanf("%d",&a[i]),id[a[i]]=i;
	Set.clear(); Set.insert(0);
	memset(used,0,sizeof(int)*(n+3));
}

void Pushup(int x) {
	if (mx[x<<1]<mx[x<<1|1]) mx[x]=mx[x<<1|1],num[x]=num[x<<1|1];
	else mx[x]=mx[x<<1],num[x]=num[x<<1];
}

void Build(int x,int l,int r) {
	if (l==r) return (void) (num[x]=l,mx[x]=a[l]);
	int mid=(l+r)>>1;
	Build(x<<1,l,mid); Build(x<<1|1,mid+1,r);
	Pushup(x);
}

int Query(int x,int l,int r,int lt,int rt) {
	if (rt<l||lt>r) return 0;
	if (lt<=l&&r<=rt) return num[x];
	int mid=(l+r)>>1;
	int u=Query(x<<1,l,mid,lt,rt),v=Query(x<<1|1,mid+1,r,lt,rt);
	return (a[u]<a[v])?v:u;
}

void Updata(int x,int l,int r,int pos) {
	if (l==r) return (void) (num[x]=mx[x]=0);
	int mid=(l+r)>>1;
	if (pos<=mid) Updata(x<<1,l,mid,pos);
	else Updata(x<<1|1,mid+1,r,pos);
	Pushup(x);
}

void Solve() {
	Init();
	Build(1,1,n);
	for (int i=1;i<=n;i++) {
		if (used[id[i]]) continue;
		int x=id[i];
		set<int>::iterator it=Set.lower_bound(x);
		int l=Query(1,1,n,*(--it)+1,x),r=x;
		if (a[l]<a[r+1]&&!used[r+1]) {
			ans[i]=a[r+1];
			Updata(1,1,n,r+1);
		}
		else {
			for (int i=l;i<=r;i++) {
				ans[a[i]]=a[(i==r)?l:i+1];
				Set.insert(i);
				Updata(1,1,n,i);
				used[i]=1;
			}
		}
	}
	for (int i=1;i<=n;i++) {
		printf("%d",ans[i]);
		putchar((i!=n)?' ':'\n');
	}
	memset(num,0,sizeof(int)*(2*n+10));
	memset(mx,0,sizeof(int)*(2*n+10));
}

int main() {
	scanf("%d",&T);
	while (T--) Solve();
	return 0;
}
```

[1]: http://acm.hdu.edu.cn/showproblem.php?pid=5327
[2]: https://vexoben.github.io/assets/images/Blog/2015-Multi-University-Contest-4(2).JPG
[3]: https://vexoben.github.io/assets/images/Blog/2015-Multi-University-Contest-4(3).JPG
[4]: https://vexoben.github.io/assets/images/Blog/2015-Multi-University-Contest-4(4).JPG
[5]: https://vexoben.github.io/assets/images/Blog/2015-Multi-University-Contest-4(5).JPG