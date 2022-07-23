#### [P1116 车厢重组 ](https://www.luogu.com.cn/problem/P1116)

读题之后就可以发现，这道题其实就是一个很简单的冒泡排序。

```cpp
#include<bits/stdc++.h>
using namespace std;
int N; //车厢总数
int a[1005];
int main(){
    cin >> N;
    int temp, ans = 0;
    for(int i = 1; i <= N; i++){
        scanf("%d",&a[i]);
    }
    for(int i = 1; i <= N; i++){
        for(int j = i + 1; j <= N; j++){
            if(a[i] > a[j]){
                temp = a[j];
                a[j] = a[i];
                a[i] = temp;
                ans++;
            }
        }
    }
    cout << ans;
    return 0;
}
```

但是我在题解中看到一个高赞答案：

```cpp
#include <iostream>
using namespace std;
int n, sum;
int main()
{
    cin >> n;
    int a[n];
    for (int i = 0; i < n; ++i)
        cin >> a[i];
    for (int i = 0; i < n; ++i)
        for (int j = 0; j < i; ++j)
            if (a[j] > a[i])
                ++sum;
    cout << sum;
    return 0;
}
```

这里的思想就是因为题目中并没有要求我们最后输出排序后的结果，那么我们就可以偷个懒，选择看一下每个数前面有几个比它大的数字，类似于求逆序数，由于这道题的数据不是很严格，所以并没体现优势，但是如果数据量很大的话，相比冒泡排序还是比较好的。

---

#### [P1075 质因数分解](https://www.luogu.com.cn/problem/P1075)

这道题是一道和质数相关的题目。

首先要明确的是，题目中所给的数n有两个不同的质因数，并不需要考虑所求的因数是不是质数。所以问题就变成了求几个因数之中最大的那一个

这里简便的方法是，因数总是成对出现的，平方数除外，所以我们就可以直接找到最小的那个质因数，然后用n去计算出ans即可。

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){
    int n;
    cin >> n;
    for(int i = 2; i <= n; i++){
        if(n % i == 0){
            cout << n / i;
            break;
        }
    }
    return 0;
}
```

---

#### [P1150 Peter 的烟 ](https://www.luogu.com.cn/problem/P1150)

首先贴上我的思路，有多少烟，就有多少烟屁，那么就直接把烟的数量先加上，然后有多少烟屁，就有$(多少/k)$根烟，那么把烟的值更新，套入while循环中，一直到$n<0$即可。但是这里需要注意一个点，就是浪费掉的烟屁，比如说这次正好有$k-1$个烟屁，但是我们没有把他计算到新烟的数量中，如果下一轮凑够$k$个的话，那么这$k-1$的烟屁就算被浪费了，所以我们需要一个temp来存放多余出的烟屁。

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){
    int n, k;
    cin >> n >> k;
    int ans = 0;
    int temp = 0;
    while(n > 0){
        ans += n;
        temp += n % k;
        n = n / k;
        if(temp >= k){
            ans ++;
            temp -= k;
        }
    }
    cout << ans;
    return 0;
}
```

当我看题解的时候，发现这道题还有更巧妙的解法。

首先，假设最终抽到了$x$根烟，那么其中$(x-n)$根烟都是用烟屁换来的，而又因为抽了$x$根烟，则自始至终一共拥有$x$个烟屁，但是抽到的最后一根烟是不能用来换烟的，那么能用来换烟的烟屁只有$x-1$个，则换来烟的数量为$x-n={{x-1}\over k}$

最终通过化简得到：$x = n + {(n-1)\over (k-1)}$

```cpp
#include <iostream>

int main() {
    unsigned n, k;
    std::cin >> n >> k;
    std::cout << n + (n - 1) / (k - 1) << '\n';
}
```

---

#### [P1317 低洼地](https://www.luogu.com.cn/problem/P1317)

这道题目很简单，但是我写完看题解的时候看到了一个比较有趣的答案。首先来简单分析一下这道题目。要求洼地，那么我们只需要判断一个点左面和右面的数都比它大就OK了，但是会有连续的部分，那么该怎么办呢，我在输入的时候就处理了，如果是$a[i] == a[i-1]$那么就让下标和$n$都减一就好了。

```cpp
#include<bits/stdc++.h>
using namespace std;
int n;
int a[10005];
int main(){
    cin >> n;
    for(int i = 1; i <= n; i++){
        cin >> a[i];
        if(a[i] == a[i-1]){
            i--;
            n--;
        }
    }
    int ans = 0;
    for(int i = 2; i < n; i++){
        if(a[i-1] > a[i] && a[i+1] > a[i]){
            ans ++;
        }
    }
    cout << ans;
    return 0;
}
```

但是题解一个比较好的思路就是：

要求洼地，那么必然左面有一个下降，右面有一个上升。那么就在输入的时候判断即可，用一个临时变量记录下来下坡和上坡就好，如下：

```cpp
for(int z=1;z<=n;z++)
{
    cin >> b;
    if(b<a) {l=1;}
    if(b>a&&l==1) {ans++;l=0;}
    a=b;
}
```

---

#### [P1319 压缩技术 ](https://www.luogu.com.cn/problem/P1319)

这道题比较有趣，主要是在判定结束的时候需要思考一下。

先上代码：

```cpp
#include<bits/stdc++.h>
using namespace std;
int n;
int main(){
    cin >> n;
    int temp, num = 0;
    bool flag = false;
    int total = 0;
    while(temp < n*n){
        for(int i = 0; i < temp; i++){
            if(flag == true)
                cout << "1";
            else cout << "0";
            num ++;
            if(num == n){
                cout << endl;
                num = 0;
            }
        }
        if(flag) flag = false;
        else flag = true;
        total += temp;
    }
    return 0;
}
```

输出部分我就不细说了，大概就是读到一个数$n$，然后输出$n$次$0$或者是$1$，这里的01用flag来判定。那么什么时候结束程序呢，一开始我想的是这样

```cpp
while(scanf("%d",&temp) != '\n')
```

也就是说，只要不敲回车，那么就一直判断下去，但是本地的话还可以，输入完成后直接敲回车没问题，但是提交到OJ就会发现TLE了。

这是我又读了一遍题，发现题目中已经说了，交替的个位数之和就是$n^2$，那么我只需要把所有读入的数加起来，如果等于，就跳出循环了。

---
