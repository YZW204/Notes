## KMP

> 花了一下午终于又搞懂了这个算法，显然这个算法还是太抽象了。QWQ
>
> KMP 是一种以$O(N)$ 的时间复杂度来求解字符串匹配的算法

## 1. 问题

- 对于一个文本 txt，查找是否包含字符 s

  > 如  ：
  >
  > txt：ababababcabacdababc
  >
  >    s：abacdababc  

## 1. 暴力求解

> 对于每一个txt的字符，都进行遍历，查找是否有长度为 s.size() 的字符和 s 完全相同，时间复杂度 $O(NK)$ (N:txt的长度，K：s的长度)

## 2. KMP

> 优化暴力的方式，如果我们发现当前字符串不匹配，我们最多可以把 s 向后移动几位，使得匹配失败前的这一段依旧匹配?

- 引入：最大公共前后缀（自封的名字）

> 对于一个字串 s ，他的最大公共前后缀是指：`s[0 : k]==s[s.size()-k : s.size()]`，即前面的 k 位 和 倒数的 k 位完全相同。

- 优化暴力求解过程中移动的次数，找到当前子串中已经匹配的小子串的最大前缀，使得重新匹配的次数减少，而这个过程，我们是可以预处理当前的子串 S,得到各个长度的子串的最大前后缀的长度，用数组存储 记作next[]（由于下标从0开始，因为next[i]，表示的是前 i 为的最大公共前后缀，所以当第 i 为不匹配时，就可以跳到next[i]，上去继续进行匹配）

## 3. next[]数组的思路

> $P_k$ 是指子串[ 0 : k]，$P_j$是指子串[ 0 : j ]，即 表示 j 长度的子串

1. ###### 当$P_k = P_j$ 相同时 next[ j+1 ] 就是 next[ j ]+1

2. 当$P_k !=P_j$，k 就需要回退到next[k]

   > 因为当最后一个不匹配时，我们会尝试着缩小当前的匹配长度 K ,找到最短的缩小方案。这个最小方案就是当前已经匹配的$P_{j-k}……P_j$ 的最长公共前后缀，而这个长度为K的子串，恰好和 [0……k]的子串是完全一样的（因为已经匹配过了），那么求后面的最长公共前后缀就是求 next[k]，直到找到成功匹配或者为开头

3. next 数组的代码

```c++
int getnext(int next[],string t ){
    int j=0,k=-1;
    int next[0]=-1;
    whilt(j<t.size()-1){
        
        if(k==-1||t[j]==t[k]){
            j++,k++;
            next[j]=k; //前面 j-1 个的最大匹配数是 k 
        }
        else k=next[k]; //查找当前已经匹配的最大匹配数，减少移动
    }
}
```

4. kmp代码

> kmp 中只需要进行判断，如果不匹配，则子串进行回退即可

```c++
int kmp(strign s,string t){
    int i=0,j=0;
    int next[t.size()];
    getnext(next,t);
    whilt(i<s.size()&&j<t.size()){
        if(j==-1||s[i]==t[j]){
            i++,j++; 	//匹配成功，都++
        }
        else j=next[j]; //失败，进行回退，找到最大公共前后缀
    }
    if(j>t.size()) return (i-t.size());
    else return -1;
}

```

