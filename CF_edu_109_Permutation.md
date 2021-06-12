# Sorting Permutation
给定一个排列，可以将任意区间打乱，除了大区间。
输出要重排几次

1.当左端点排好或将右端点排好  一次即可
2.左端点和右端点都没有排好的时候需要两次
3.特殊情况  到1和N 位置交换的时候需要三次

```c++
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<cstring>
#include<cmath>
using namespace std;
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
int T;
int n;
int a[maxn];

int main(){
  T = read();

  while(T--){
    n = read();
    for(int i = 1; i <= n; i++){
      a[i] = read();

    }
    bool flg = false;
    for(int i = 1; i <= n; i++){
      if(a[i] != i){
        flg = true;
      }
    }
    if(!flg){
      cout << "0" << endl;
      continue;
    }
    if(a[1] == 1 || a[n] == n){
        cout << "1" << endl;
      } else if((a[1]==n)&&(a[n]==1)){
        cout << "3" << endl;
      } else{
        cout << "2" << endl;
      }
    

  }
}
```
