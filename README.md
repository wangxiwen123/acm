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
