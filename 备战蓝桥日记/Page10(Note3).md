#### [P3378 【模板】堆 ](https://www.luogu.com.cn/problem/P3378)

二叉堆，作为一个基本的数据类型，可以用来排序，今天既然随到了，就来分析一下。

首先，根据数据结构中学到的关于大根堆和小根堆的知识，我们知道，堆是一颗完全二叉树，其次堆的顶端一定是“最大”或者“最小”的元素。那么这道题的要求已经完成了，那么现在就是想办法建立一个小根堆就好了。这里可以选择手写一个，或者是使用STL中自带的模板。

```cpp
#include<bits/stdc++.h>
using namespace std;
priority_queue<int, vector<int>,greater<int> >q;
int n, a, b;
int main(){
    cin >> n;
    for(int i = 1; i <= n; i++){
        cin >> a;
        if(a == 1){
            scanf("%d", &b);
            q.push(b);            
        }
        if(a == 2){
            int ans = q.top();
            cout << ans << endl;
        }
        if(a == 3){
            q.pop();
        }
    }
    return 0;
}
```

> 这道题只能算是一个新手教程，用来帮我了解优先队列的。在这里面，最大和最小可以不是传统意义上的大小，在priority_queue的使用场景中，我们可以通过重载运算符的方式来让我们认为大/小的元素在堆顶，来获取最大/小元素。

---