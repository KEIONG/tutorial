#Armchair
给定一排椅子，椅子有一些别占用了，有一些是空闲的

你可以将一个人从一个椅子移到另一个椅子上，花费$\large |i-j|$
目标状态是使得所有人从一开始的椅子上离开

考虑dp, 将一开始的输入分成两组

$$
\ dp[i][j]=min(dp[i-1][j-1]+|i-j|,dp[i-1][j]);
$$

标识前$i$个占用的椅子用了前$j$个空闲的椅子去配对

```c++
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<cstring>
#include<cmath>
using namespace std;
#define int long long
const int N = 5100;
const int maxn = 1e6 + 10;
int read(){
  int x=0;
  int f=1;
  char ch=getchar();
  while(ch<'0'||ch>'9'){
    if(ch=='-')
    f=-1;
    ch=getchar();
  }
  while(ch>='0'&&ch<='9'){
    x=x*10+ch-'0';
    ch=getchar();
  }
  return x*f;
}
int dp[N][N];
int a[maxn];
int used[maxn];
int emp[maxn];
int cnt1 = 0;
int cnt2 = 0;
int n;
void init(){
  memset(dp, 0x3f3f3f3f, sizeof(dp));
  for(int i = 0; i <= cnt1; i++){
    dp[0][i] = 0;
  }

}
signed main(){
  n = read();
  for(int i = 1; i <= n; i++){
    a[i] = read();
    if(!a[i]){
      emp[++cnt1] = i;
    } else {
      used[++cnt2] = i;
    }
  }
  init();
  int ans = 0x3f3f3f3f;
  for(int i = 1; i <= cnt2; i++){
    for(int j = 1; j <= cnt1; j++){

      int tmp = abs(used[i] - emp[j]);
      dp[i][j] = min(dp[i][j-1], dp[i-1][j-1] + tmp);
      // if(i == n){
      //   ans = min(ans, dp[i][j]);
      // }
    }
  }
  cout << dp[cnt2][cnt1] << endl;

}`
```