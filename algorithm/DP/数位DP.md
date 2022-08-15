## 1. 题目

> 如果一个正整数每一个数位都是 **互不相同** 的，我们称它是 **特殊整数** 。
>
> 给你一个 **正** 整数 `n` ，请你返回区间 `[1, n]` 之间特殊整数的数目。

[y总视频讲解](https://www.acwing.com/video/4217/)

## 2. 数位DP

> 对数的结构进行分析，直接拆分出来符合要求的数，并对最大的数进行特殊判断
>
> 左边的数一定满足阶乘，右边的数可能会有重复

![QQ浏览器截图20210315122504.png](https://img-blog.csdnimg.cn/img_convert/d9cff4c573361692ac8f6cef1b47a916.png)



```c++
class Solution {
public:
    int countSpecialNumbers(int n) {
        vector<int> num ;
        int res=0;
        while(n) num.push_back(n%10),n/=10;
        //小于 n 位的个数
        for(int i=1;i<num.size();i++){//小于 n 的各个长度
            int t=1;
            for(int j=1,k=9;j<=i;j++,k--) t*=k;// 每个长度的阶乘
            res+=t;

        }
        int st[10]={0};
        reverse(num.begin(),num.end());//从高位计算
        for(int i=0;i<num.size();i++ ){

            for(int j=!i;j<num[i];j++){ //小于当前位数的每个数的数的 相应的阶乘  
                if(st[j]) continue; //判断是否被用过
                int t=1;
                for(int k=0,u=9-i;k<num.size()-i-1;k++,u--) t*=u; //相应的阶乘
            res+=t;
            }
            
            if(st[num[i]]) break; //如果重复，则后面的数一定不成立
            st[num[i]]=1;
        }
        set<int> hash(num.begin(),num.end()); //本身是否符合要求
        if(hash.size()==num.size()) res++;
     return res;   
    }
    
};
```

