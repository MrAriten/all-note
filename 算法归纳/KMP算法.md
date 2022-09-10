# KMP算法

算法中最重要也是最难的一环就是next表及其构建。next表的作用是当字符串和子串不匹配时，向后移动的位数，本质上是“前后相同子串”的一种表达形式。比如 CHIBCHIDHIP中，前后两个CHI就是最大的前后子串，而三个HI也是前后相同的子串，这几个子串对next表的构建有很大意义。

```C++
vector<int> get_next(string needle)
    {
        int i = -1; //KMP是当前不匹配再根据前一项来判断next表的值！
        int j = 0;//所以要从-1 0开始比较
        vector<int> next(needle.size());
        next[0] = -1;//将next表第一个值设为-1
        while(j<needle.size()-1)
        {
            if((i<0) || (needle[i] == needle[j])) //当i为-1或者字符串匹配时
            {
                i++,j++;
                next[j] = (needle[j] == needle[i]) ? next[i] : i; //后面那个问号运算符表示，如果当前的字符串匹配了，而且前一个字符串也匹配，那就不能将next[j]设成j，这样等于无效比较，可以看数据结构中的next表优化就明白了
            }
            else
            {
                i=next[i]; //如果不匹配，则将i重置回去，并不是每个都重置到-1，而是要根据当前i的next值！因为前面也可能有前后匹配的子串！
            }
        }
        return next;
    }

    int strStr(string haystack, string needle)
    {
        vector<int> next = get_next(needle);
        int h = -1;//因为next表中有-1，所以最好也从-1开始
        int n = -1;
        while( (h != haystack.size()) && (n != needle.size()) )
        {
            if(n<0 || (haystack[h] == needle[n]) ) //n为-1说明要整体右移
                h++,n++;
            else
                n = next[n];//如果不相等，则根据next表调整n的值
        }
        if(n == needle.size())//当n到最后一位时，说明匹配成功了
            return h - n;//返回第一位的位置
        return -1;
    }
```

### ① next表的构建要从-1开始！

因为next表的内容，比如next[ j ]，其实取决于子串中第 j- 1 个字符的匹配关系，如果第 j - 1 个字符和第 i - 1 个匹配，且第 j 个字符串和第 i 个不同的话，那么next[ j ]的取值就为 i 。

意思是当在第 j 个字符处不匹配了，就将子串转移到第 i 个位置处。

所以从-1开始的原因是为了从头开始一个一个填满next表，每当检测到匹配或者 i = -1 时，都要i++,j++一次，这样是方便填入next[ j ]。

### ② next[j] = (needle[j] == needle[i]) ? next[i] : i;

这行代码的前半段已经在①中有所解释，那么为什么当needle[j] == needle[i]时，next[ j ]是等于next[ i ]而不是i呢？

这在数据结构那本书中有详细的阐释，因为如果只是单纯的等于i，那么如果前面已经相等的匹配会重复进行，为了减少不必要的匹配，如果下一个字符仍然是相等的，那么就不是转移到i处，而是转移到i处的next表！也就是进一步向前转移！避免多余的匹配。

