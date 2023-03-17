## trie树

> 是一种快速查找单词的数据结构
>
> 根节点是空的节点，编号为0，也是其他节点的指向节点

<img src="https://img-blog.csdn.net/20150509003807271" alt="trie树" style="zoom:100%;" />



**1. trie树的准备**

```c++
const int N=1e6+10;
int trie[N][26]; //trie 树 ,每个节点最多有26个衍生节点
int idx; //表示各个节点
int cnt[N]; //表示当前节点是否某个单词的结尾
```



**2. 构建trie树**

> 通过对每个字母进行遍历，在对数据结构的单词进行遍历，如果未出现则创建节点，反之继续搜索下去

```c++
void insert(string w){
        int p=0; //相当于一个指针，指向当前的节点
        for(auto c:w){
            int u=c-'a';
            if(!trie[p][u]) trie[p][u]= ++idx; //如果没出现过，则其值为一个节点
            p=trie[p][u];		//跳跃到当前的节点
            cnt[p]++;
        }
    }
```

**3. 查询trie树**

> 通过和构建trie树一样的操作来查询，借助cnt来判断单词出现的次数

```c++
  int query(string w){
        int p=0;
        int ans=0;
        for(auto c:w){           
            int u=c-'a';
        if(!trie[p][u]) return 0; //不存在该单词，就返回0
            p=trie[p][u];
        }
        return cnt[p];   
    }
```

