# 教你用js拿到小姐姐微信
最近在某司机群，偶然有个老司机发了个图片。        
![图片描述](//img1.sycdn.imooc.com/5cb5306a000199f907580800.jpg)      
emmm......这是要走上人生巅峰了吗? 开始行动，一步一步来。     

## 1、找到微信号
不管怎么样，先找到微信号再说。      
小姐姐说了，微信号是由 `NY + 数字` 组成，其中`数字`又可以拆分为质数`a`和`b`，且`a&gt;b`，再且`a * b = 707829217`。      
emmm......笔算几乎无法下手，只能用机器算了。老老实实干吧，毕竟关系到小姐姐的幸福。      
**开工：**先把小于 **707829217** 的质数都找出来，然后嵌套循环找出满足`a * b = 707829217`的两个质数，So easy!      
程序写好后，运行，emmm......计算机不给力，过了几分钟没有出结果。    
反省一下，这个算法的时间复杂度是`O(n * logn)`（总之很慢就是了），不知道 `707829217` 这个以亿为单位的数，要算多久。而且把每一个质数都储存下来，这个内存空间消耗也很大。速度慢，空间大，这怎么行，**我们的目标是更快、更小** 。      
再想想，`a * b = 707829217`，也就是 `a = 707829217 / b` 啊。只要找到 一个可以把`707829217`整除的`b`，然后再判断 `b`和`707829217 / b`是不是质数就行了，这样就不需要判断每个数是不是质数了。算法分析  [传送门:将n分解为2个质数的乘积](https://github.com/KawayAlpaka/calculation/blob/master/%E5%B0%86n%E5%88%86%E8%A7%A3%E4%B8%BA2%E4%B8%AA%E8%B4%A8%E6%95%B0%E7%9A%84%E4%B9%98%E7%A7%AF/README.md)      

javascript代码：
```javascript
const isPrime = function (n) {
  if (n &lt; 2) {
    return false;
  }
  const max = Math.sqrt(n);
  for (let i = 3; i &lt; max; i = i + 2) {
    if (n % i === 0) {
      return false;
    }
  }
  return true;
};
const find2number = function (n) {
  const max = Math.sqrt(n);
  for (let b = 2; b &lt;= max; b++) {
    if (n % b === 0) {
      if (isPrime(b)) {
        const a = n / b;
        if (isPrime(a)) {
          return [a, b];
        }
      }
    }
  }
  return false;
};
const r = find2number(707829217);
console.log("微信号：NY"+r[0]+r[1]);   // 微信号：NY866278171
```
因为是`javascript`，浏览器就可以运行它。       
运行方法：打开`chrome`、`360浏览器`、`firefox`、`搜狗浏览器`、`qq浏览器` 其中一个，按下`F12`，再进入`Console`面板，把代码贴到下方，按下`回车`，就可以看到微信号了，`微信号：NY866278171`。      
微信号到手了：NY**866278171**，然后干嘛呢? 还能干嘛，**当然是做附加题啊**。      
## 2、计算附加题
统计由奇数`n`组成的序列 `1 &lt;= n &lt;= 866278171` 的序列，中 `3` 字符出现的次数。      
这不是更简单了吗？循环 `1` 到 `866278171`的奇数，求它们中含有`3`字符的个数的和，ez !     
运行：2分钟后，得到答案：**441684627**。      
答案是有了，可是为什么这么慢。这么简单的问题，其他小哥哥一定也能答出来。**这样不行，一定要做到最快**。      
仔细分析，可以把问题转化成：求小于等于`n`的奇数序列中`3`分别在`个`、`十`、`百`、`千`等位数上出现的次数的和。      
此算法分析过程稍微长，给有兴趣了解算法细节的小哥哥一个 [传送门:求奇数序列中x出现的次数](https://github.com/KawayAlpaka/calculation/blob/master/%E6%B1%82%E5%A5%87%E6%95%B0%E5%BA%8F%E5%88%97%E4%B8%ADx%E5%87%BA%E7%8E%B0%E7%9A%84%E6%AC%A1%E6%95%B0/README.md)        

这里直接上 javascript 代码：
```javascript
const count2 = function (n, x) {
  let sum = 0, factor = 1, higher = 0, current = 0, lower = 0, time = 1;
  for (; Math.floor(n / factor) != 0; factor *= 10) {
    higher = Math.floor(n / (factor * 10));
    current = Math.floor((n / factor)) % 10;
    lower = n - Math.floor((n / factor)) * factor;
    if (factor &gt; 1) { time = 0.5;
    } else if (x % 2 === 0) { continue;}
    if (x === 0) {  higher--;  }
    if (current === x) {
      let _t = factor === 1 ? 1 : Math.ceil(lower * time);
      sum += higher * factor * time + _t;
    } else if (current &gt; x) {
      sum += (higher + 1) * factor * time;
    } else if (current &lt; x) {
      sum += higher * factor * time;
    }
  }
  return sum;
};
console.log("附加题：" + count2(866278171,3));   // 附加题：441684627
```
运行，程序输出了`附加题：441684627`。

题目都解出来了，这就开始加微信，打开微信，搜索`NY866278171`，添加到通讯录，验证信息：`附加题答案：441684627`。仔细想想，这样怎么能体现我的算法更优越呢？于是补上一句：**不到0.01秒完事**。想想我还真特么机智。
![图片描述](//img1.sycdn.imooc.com/5cb530770001fec501640136.png)

