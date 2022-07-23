#### [P1321 单词覆盖还原](https://www.luogu.com.cn/problem/P1321)

这道题很的解法很巧妙，我一开始准备一位位的判断，但是想到了比较麻烦，就放弃了。

主要的思想就是我们在移位判断的时候只需要判断从当前开始的三个字符或者四个字符是不是属于boy或者girl中的一部分就行。

如果是，那么直接对结果++即可。而且由于boy和girl这两个单词中并没有相同或者重复的部分，那么就可以用这种方式来解决问题。

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){
    string l;
    cin >> l;
    int boy = 0, girl = 0;
    for(int i = 0; i <= l.length(); i++){
        if(l[i] == 'b' || l[i+1] == 'o' || l[i+2] == 'y')
            boy ++;
        if(l[i] == 'g' || l[i+1] == 'i' || l[i+2] == 'r' || l[i+3] == 'l')
            girl ++; 
    }
    cout << boy << endl;
    cout << girl << endl;
    return 0;
}
```

---

#### [P1590 失踪的7](https://www.luogu.com.cn/problem/P1590)

这真的是一道入门题吗，令人怀疑……

首先，这道题和找2那道题目一样，但是找2那道题目的数据没有这么复杂，通过取余的方法就能解决，但是这道题的数据比较苛刻，我只过了前10个点，之后的三个点全部TLE了。更离谱的是我在看题解的时候发现了和我一个思路的题解，我还以为是我的程序有啥问题，按着人家的那个改了改发现还是TLE，结果我直接点开评论，发现全都是TLE……

好了，首先把TLE的代码贴上来

```cpp
#include<bits/stdc++.h>
using namespace std;
int a[10005];
int main(){
    int t;
    cin >> t;
    for(int i = 1; i <= t; i++){
        int num, ans = 0;
        cin >> num;
        a[i] = num;
        for(int j = 1; j <= num; j++){
            int k = j;
            while(k){
                if(k%10 == 7){
                    a[i]--;
                    break;
                }
                k /= 10;
            }
        }
    }
    for(int i = 1; i <= t; i++){
        cout << a[i] << endl;
    }
    return 0;
}
```

具体实现的方法就是遍历小于n的数，之后找到数字中含有7的部分，但是中间的while循环加上外层的for循环，时间复杂度实在是太大了，当我写好一版之后，提交的时候就觉得可能要TLE了。

之后我一个人思考了一下，可以使用记录的办法减少时间复杂度，比如说我已经知道前10000个数中有几个少7的数字了，那么就可以大大减少时间复杂度，但是实现的方法感觉比较麻烦，就没有修改。那么如果用字符串的方式来看呢，结果也是不行。

这时候只能看题解了，发现这道题需要用一种进制的思想，因为我们不想在一串数中看到7，那么就只有012345689这9个数字，可以模拟为九进制。

但是这道题真的好难啊，我先把代码放下，之后学成再分析。

```cpp
#include<bits/stdc++.h>
using namespace std;
long long k ,s = 1, sum = 0, ans = 0;
char a[1005];
int main(){
    int t;
    cin >> t;
    while(t--){
        ans = sum = 0;
        s = 1;
        cin >> a;
        k  = strlen(a);
        for(int i = 0; i < k; i++){
            if(a[i] == '7'){
                a[i] = '6';
                for(int j = i + 1; j < k; j++)
                    a[j] = '9';
                break;
            }
        }
        for(int i = k-1; i >= 0; i--){
            if(a[i] > '7') sum += s;
            s *= 9;
        }
        for(int i = 0; i < k; ++i){
            ans = ans * 9 + a[i] - '0';
        }
        cout << ans - sum << endl;
    }
    return 0;
}
```

---

#### [P1917 三子棋II](https://www.luogu.com.cn/problem/P1917)

这道题目很有意思，是关于下棋的，大部分的分析都是关于下棋部分的，和代码关系不大。那么接下来就先分析一下如何才能赢游戏吧。

首先，我们确定一下，三子棋先手玩的好的话必赢，那么后手一定是赢不了的。这里说了一定是以当前的最好状态来下棋，那么也就是说答案一定是XIAOA赢或者是不知道结果。

由于XIAOA第一步下在了棋盘的正中心，其实自己试一试就可以知道了，如果UIM第二步没有下到对角线上，那么XIAOA赢的概率就是100%。如果说下到了对角线上，那么结果我们就不知道了。所以当我们把当前的棋面读入之后，我们就只需要判断当前四个角是不是UIM下的X棋子就好了。

由于题目中还需要统计当前所下的棋数，所以还需要一个ANS来统计一下，这步操作在读入的时候做就好了。

代码如下：

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){
    char a[3][3];
    string b;
    int ans = 0;
    for(int i = 0; i < 3; i++){
        getline(cin,b);
        for(int j = 0; j < b.length(); j++){
            a[i][j] = b[j];
            if(b[j]=='X'||b[j]=='O') ans ++;
        }
    }
    if(a[0][0] == 'X'||a[0][2] == 'X'||
    a[2][0] == 'X'||a[2][2] == 'X'||ans <= 1){
        cout << "Dont know." << endl;
        cout << ans;
    }else{
        cout << "xiaoa will win." << endl;
        cout << ans;
    }

    return 0;
}
```

> 这道题在做的时候，有两个地方使我WA，首先，题面中说赢的话输出"xiaoa will win."结尾有个标点符号，没看见，WA了一次；还有一次就是在判断的地方，我只判断了当四个角不是X的时候，不知道结果，那么如果棋盘上只有中间XIAOA下的O，那么结果也是未知的，换句话说，棋子数量小于1的时候，结果也是未知的。

---

#### [P8220 [WFOI - 02] I wanna win the race（比赛）](https://www.luogu.com.cn/problem/P8220)

这道题好坑啊，还是捆绑测试，看不了数据，搞得我都开始怀疑我的思路了。

首先简单分析一下题目，我读题的时候最先关注的就是关于A类点变B类点的事情，从题目中不难发现，如果说$c<n$的话，那么我们就可以从最上面绕过B类点，直接算就行了。这么说的话，如果$c>=n$的话，我们只需要加上那几列B类点就好了。

但是当我提交OJ之后发现有两个点不过，又因为是捆绑测试，所以测试数据也看不到，所以只能死盯着程序看看到底是那一部分有特例，还别说，真让我找出来了，就是当$c>=n$的时候。

那么究竟是为什么会出现这种错误呢？我又整理了一遍思路，发现的确没有问题。于是迫于无奈，只好去看题解。

结果最坑的地方就来了，看了题解才发现这和我想的还不一样。这并不是一个用墙围起来的赛场，而是整个第一象限，那么当B类点列太多的时候，我们完全可以绕过去，从绕与不绕之中选一个花费小的。我真的无语……

```cpp
#include<bits/stdc++.h>
using namespace std;
int main(){
    long long  n;
    cin >> n;
    long long a, b, c;
    cin >> a >> b >> c;
    if(n>c){
        cout << 2*n-1;
    }
    else{
        long long temp = (b-a) + 1;
        cout << min(2 * n + temp - 1, 2 * c + 1);
    }
    return 0;
}
```

代码很简单。

> 其实回头去看题面的时候，就能发现出题人留的提示。如果按照开始的思路的话，我们只需要能够向x, y轴的正方向移动就行了，但是题目中说道，可以向所在点的四面八方移动。那么就证明了我们需要向后走。所以说信息也不是白给的……
