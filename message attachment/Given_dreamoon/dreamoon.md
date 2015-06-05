<script src="http://yandex.st/highlightjs/7.3/highlight.min.js"></script>
<link rel="stylesheet" href="http://jmblog.github.io/color-themes-for-highlightjs/css/themes/tomorrow-night-eighties.css">
<script>
hljs.initHighlightingOnLoad();
</script> 

# 出題是種態度  #

## Description ##

> 「過去的小月是個常常寫假解 AC 後還暗自竊喜的小廢物，直到小月和球球相遇，小月才知道何謂 ** 演算法的正義 ** 」– 小月










* $$$−10^9 \le x_i,y_i \le 10^9$$$

## Original Output Format ##
對於每組測資輸出一行答案，最遠點距離的平方。
## Sample Input ##

### 範例 1 ###

```
2 
0 0 
10 0
```

### 範例 2 ###

```
-1000000000 1000000000
1000000000 -1000000000
1000000000 1000000000
```

## Sample Output ##

### 範例 1 ###

```
```

### 範例 2 ###

```
```

## Hacked Program ##

```
//bcw0x1bd2 {{{
#include<bits/stdc++.h>
using namespace std;
#define FZ(n) memset((n),0,sizeof(n))
#define FMO(n) memset((n),-1,sizeof(n))
#define MC(n,m) memcpy((n),(m),sizeof(n))
#define F first
#define S second
#define MP make_pair
#define PB push_back
#define FOR(x,y) for(__typeof(y.begin())x=y.begin();x!=y.end();x++)
#define IOS ios_base::sync_with_stdio(0); cin.tie(0);
// Let's Fight! }}}

const int MXN = 1000005;

struct Point{
  typedef long long T;
  T x, y;
  
  Point() : x(0), y(0) {}
  Point(T _x, T _y) : x(_x), y(_y) {}

  bool operator < (const Point &b) const{
    return tie(x,y) < tie(b.x,b.y);
  }
  bool operator == (const Point &b) const{
    return tie(x,y) == tie(b.x,b.y);
  }
  Point operator + (const Point &b) const{
    return Point(x+b.x, y+b.y);
  }
  Point operator - (const Point &b) const{
    return Point(x-b.x, y-b.y);
  }
  T operator * (const Point &b) const{
    return x*b.x + y*b.y;
  }
  T operator % (const Point &b) const{
    return x*b.y - y*b.x;
  }
  Point operator * (const T &b) const{
    return Point(x*b, y*b);
  }
  double abs(){
    return sqrt(abs2());
  }
  T abs2(){
    return x*x + y*y;
  }
};

long long cross(Point o, Point a, Point b){
  return (a-o) % (b-o);
}
vector<Point> convex_hull(vector<Point> pt){
  sort(pt.begin(),pt.end());
  int top=0;
  vector<Point> stk(2*pt.size());
  for (int i=0; i<(int)pt.size(); i++){
    while (top >= 2 && cross(stk[top-2],stk[top-1],pt[i]) <= 0)
      top--;
    stk[top++] = pt[i];
  }
  for (int i=pt.size()-2, t=top+1; i>=0; i--){
    while (top >= t && cross(stk[top-2],stk[top-1],pt[i]) <= 0)
      top--;
    stk[top++] = pt[i];
  }
  stk.resize(top-1);
  return stk;
}

int main(){
    int N;
    while (~scanf("%d", &N) && N){
        vector<Point> ip, ret;
        for (int i=0,x,y; i<N; i++){
            scanf("%d%d", &x, &y);
            ip.PB(Point(x,y));
        }
        ret = convex_hull(ip);
        long long ans = 0;
        for (auto p1 : ret)
            for (auto p2: ret)
                ans = max(ans, (p1-p2).abs2());
        printf("%lld\n", ans);
    }
    return 0;
}
```

## Judge Method ##

請構造出一組測試資料，使得使用上述程式碼凸包後，點數仍至少有 $$$1,000,000$$$ 個點。

## Sample Program ##

以下程式碼能產生本題合法但不一定 AC 的測試資料

```
#include<cstdio>
#include<cstdlib>
#define LL long long
const int MAX = 1000000000;
LL myrand(){
    LL res = ((LL)rand()<<48)^((LL)rand()<<32)^((LL)rand()<<16)^rand();
    if(res<0)res=-(res+1);
    return res;
}
int main(){
    int N = 1000000;
    printf("%d\n",N);
    for(int i=0;i<N;i++){
        printf("%d %d\n",(int)(myrand()%(2*MAX+1)-MAX),(int)(myrand()%(2*MAX+1)-MAX));
    }
    return 0;
}
```