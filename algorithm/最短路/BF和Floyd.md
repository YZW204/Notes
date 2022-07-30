# BF

对每个点进行m次松弛操作，来求出到起点的最短距离

```c++
void bellman_ford(){
    memset(dis,0x3f, sizeof(dis));
    dis[1]=0;
    for(int i=0;i<m;i++){
        for(int j=0;j<n;j++){
            int a=e[j].a, b=e[j].b ,w=e[j].w;
            if(dis[b]>dis[a]+w){
                dis[b]=dis[a]+w;
            }   
        }
    }   
}
```

> 可以理解成对原点到其他点的Floyd（单点的最短路），然后更新的线段的条数为m（一般为n-1）
>
> 这里的  dis[a]其实也可能经过了 dis[aa]+w
>
> 每一个dis[]数组是每一到原点最短路的集合



## SPFA

> 其实是BF+队列优化

因为我们发现BF算法中许多的边都是不需要重新计算的，同时我们发现每一条边之所以户籍进行更新，是因为他的前继节点的最短路径更新。

因此，我们考虑每当一个点的最短路径更新了，那么我们会重新把他加入到队列，让他更新他的后继节点。同时，为了避免反复的加入队列，我们需要一个额外的数组st，来判断该节点是否已经在队列当中了（如果在队列当中就可以不用添加，因为更新的操作是一致的，且受前一个节点的影响）

由于是根据边来更新节点，所以图的存储采用链式向前星的存储方式

```c++
void spfa(){
    vector<int> q;
    q.push(1);
    st[1]=true;
    while(!q.empty()){
        int t=q.front();
        st[t]=false;
        for(int i=h[t];i!=-1;i=ne[i]){
            int b=ne[i];
            if(dis[b]>dis[t]+w[i]){
                dis[b]=dis[t]+w[i];
                if(!st[b]){
                    q.push(b);
                    st[b]=true;
                }
            }
            
        }
        if(dis[n]==0x3f3f3f3f) return -1;
        else return 1;
        
    }
}
```







# Floyd

会进行三重循环，求得任意两点间的最短路径

```c++
for(int k=1;k<=n;k++){
    for(int i=1;i<=n;i++){
        for(int j=1;j<=n;j++){
            f[i][j]=min(f[i][j],f[i][k]+f[k][j]);
        }
    }
}
```

这里面包含了动态规划的思想，由三维的 `f[k][i][j]`的数组降维到二维的`f[i][j]`

`f[i][j]` 之间最多包含的了k条线段，如何实现：

​	首先 `f[i][j]` 只由两个线段构成 `f[i][k]+f[k][j]` ,也就是直观上的 `f[i][k]`线段和 `f[k][j]` 线段。接下来，我们把 `f[i][k]` 当成是另一个 `f[i][j]`  ,那么 `f[i][k]` 就是由`f[i][kk]+f[kk]f[j]` 构成。

```c++
	f[i][j]=f[i][k]+f[k][j]
    
    f[i][k]=f[i][k1]+f[k1][k];
	f[k][j]=f[k][k2]+f[k2][j];
```

也就是每一个`f[i][k]` 都是计算了前面的k条线段的最短路径的集合，已经包含了k条路径。

`f[i][j]=min(f[i][j],f[i][k]+f[k][j])` 就是多计算一条线段的长度

