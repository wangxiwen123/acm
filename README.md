vj春季第二次训练第D题
题目大致：有n个任务和k次机会，第一次任务完成后才能做可以反复做的任务，算最大的经验值。
解题思路：从第一个开始一直往下走，遇到比当前大的b[i]后进行比较替换。

#include <bits/stdc++.h>
using namespace std;

void test() {
    int n, k;
    cin >> n >> k;
    int a[n + 1];
    int b[n + 1];
    for (int i = 1; i <= n; i++)
        cin >> a[i];
    for (int i = 1; i <= n; i++)
        cin >> b[i];
    int min1 = min(n, k);
    int ma = 0;
    int no = 0;
    int mar = 0;
    for (int i = 1; i <= min1; i++) {
        ma = max(ma, b[i]);
        //选出比上个大的的b[i];
        no += a[i];
        //往前走
        mar = max(mar, no + ma * (k - i));
        //判断当前的a[i]和加上所解锁的最大的b[i]乘上剩余步数
    }
    cout << mar << endl;
}

int main () {
    ios::sync_with_stdio(false);
    cin.tie(nullptr);
    cout.tie(nullptr);

    int t = 1;
    cin >> t;
    while (t--) {
        test();
    }
}
vj春季第二次训练第A题
题目大致：统计需要至少多少次能将所有的村落连接，例如1与2连接，2与3连接，那么认为1与3也连接
解题思路：利用并查集统计次数，总共的连接数为n-1次，每输入一次数据，就减去一次连接数，如果输入的数据已经连接，则回退减去的一次
#include <iostream>
#include <bits/stdc++.h>
using namespace std;
int p[1005];

//并查集
int find (int x) {
	while (p[x] != x) {
		x = p[x];
	}
	return x;
}

// 初始化
void cs(int n) {
	for (int i = 1; i <= n; i++)
		p[i] = i;
}

int main () {
	ios::sync_with_stdio(false);
	cin.tie(nullptr);
	cout.tie(nullptr);
	int n;
	//n是村落数
	while (cin >> n, n != 0) {
		int m;
		cin >> m;
		//m是连接次数
		cs(n);
		int sum = n - 1;
		//n个村子最少的连接数是n-1.
		int x, y;
		for (int i = 1; i <= m; i++) {
			cin >> x >> y;
			sum--;
			//先减去一，因为输入的两个村落会连接
			if (find(x) == find(y)) {
				sum++;
				//因为x,y已经连接，所以回退减去的那次
				continue;
			}
			p[find(x)] = find(y);
			//将两个点的父节点进行连接
		}
		cout << sum << endl;
	}
}
vj春季第三次训练第C题
题目大意：找到两棵树的最近公共祖先
解题思路：利用倍增的思想，先将两个保持同一深度，然后在共同上有先找到有公共祖先，然后在向下寻找最近祖先
#include <iostream>
#include <cstring>
#include <bits/stdc++.h>
#include <algorithm>
using namespace std;
#define IOS ios::sync_with_stdio(false);cin.tie(nullptr);cout.tie(nullptr);
const int maxn = 5e5 + 10;

//创建结构体存储树
struct z {
    int t, nex;
} e[maxn * 10];
int head[maxn], cnt;

//存储
void add(int x, int y) {
    e[++cnt].t = y;
    e[cnt].nex = head[x];
    head[x] = cnt;
}
int deep[maxn], up[maxn][20], lg[maxn];

//通过dfs的找到每个节点的深度
//利用倍增的思想将需要查找的两个节点向上寻找
//从大到小的进行查找，因为任何一个数都能用2的I次方表示出来
//从大到小的话就能准确的找到节点，如果从大到小进行，可能会出现悔棋的情况
//例如5从大到小是5=4+1，而从小到大为5！=1 + 2+4，会出现回退的情况
void dfs(int x, int y) {
    up[x][0] = y;
    deep[x] = deep[y] + 1;
    for (int i = 1; i <= lg[deep[x]]; i++)
        up[x][i] = up[up[x][i - 1]][i - 1];
    for (int i = head[x]; i; i = e[i].nex)
        if (e[i].t != y)
            dfs(e[i].t, x);
}

int LCA(int x, int y) {
    //同步深度
    if (deep[x] < deep[y])
        swap(x, y);//保持x为更深的一方
    while (deep[x] > deep[y])
        x = up[x][lg[deep[x] - deep[y]] - 1];
    if (x == y)//特判无拐点的情况
        return x;
    //一起上游
    for (int k = lg[deep[x]] - 1; k >= 0; --k)
        if (up[x][k] != up[y][k])
            x = up[x][k], y = up[y][k];
    return up[x][0];
}

void test() {
    int n, m, s;
    cin >> n >> m >> s;
    for (int i = 1; i <= n - 1; ++i) {
        int x, y;
        cin >> x >> y;
        add(x, y);
        add(y, x);
    }
    for (int i = 1; i <= n; ++i)
        lg[i] = lg[i - 1] + (1 << lg[i - 1] == i);
    dfs(s, 0);
    for (int i = 1; i <= m; ++i) {
        int x, y;
        cin >> x >> y;
        cout << LCA(x, y) << endl;
    }
}

int main () {
    IOS
    int t = 1; //cin>>t;
    while (t--) {
        test();
    }
}
