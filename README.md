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
