# 2023 年 03 月 - 碎碎念


### 2023.03.01
昨天晚上下班嘴贱喝了咖啡😩，现在真想给自己两个大逼兜，半夜 12 点过后咖啡因开始起作用，头脑大爆炸开始💥，从春秋战国想到俄乌战争，从上帝想到佛祖，从 Java 想到 JavaScript，从万有引力想到量子纠缠，
从 XXX 想到 YYY, 越想越睡不着，深受其扰，搞到好晚才睡😭。

### 2023.03.02
所有命运赠送的礼物，早已在暗中标好了价格。

### 2023.03.03
[举牌小人生成器](https://upuptoyou.com/)，还挺有意思的。

![upup](https://image.ericzzz.com/2023/03/05/6d7c96ac0f8110ab840eaac5713df202.webp)

### 2023.03.04
今天天气很好，随便一跑，一不小心就 PB 了😂

### 2023.03.05
看完了白日梦想家，很喜欢这部电影，更喜欢这首片尾曲。

美好从来不主动寻求关注，致敬平凡中的自己。

{{<youtube _HWRGKfSq3A>}}

### 2023.03.06
一定要等外界发生一些异常的悲剧时，我们才感激习以为常的东西吗？

我们怎么培养感激？
- 何不先审视平凡的一天？
- 有什么事是你不自觉地就会去处理的？
- 有什么事是毫不费力你就会全心投进去的？

### 2023.03.07
房租一交，立马感觉身体被掏空🤕

### 2023.03.08
培养下艺术细胞😂

![骑马的情侣](https://substackcdn.com/image/fetch/w_1456,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0bb61209-279c-4414-a879-05e907c83ed0_2537x2778.webp)

### 2023.03.09
算法优化中经常讲，用空间换时间，但选择不同空间，效率也不同，HashMap 一般以这种健值对存储的数据结构，有很快的访问速度，查询时间复杂度为 O(1)，也可以使用数组这种数据结构存储，可能会比 HashMap 使用更多的空间，但是在已知存储空间大小时，由于 HashMap 扩容机制会消耗一部分性能，总体来说数组性能会更好。

例： [leetcode 645. 错误的集合](https://leetcode.cn/problems/set-mismatch/) 这道题，分别使用 HashMap，数组，排序解题。

使用 HashMap 的解法：

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        HashMap<Integer, Integer> hashMap = new HashMap<>();
        int[] results = new int[2];
        for (int i = 0; i < nums.length; i++) {
            hashMap.put(nums[i], hashMap.getOrDefault(nums[i], 0) + 1);
        }
        for (int i = 1; i <= nums.length; i++) {
            if (hashMap.getOrDefault(i, 0) == 2) {
                results[0] = i;
            }
            if (hashMap.getOrDefault(i, 0) == 0) {
                results[1] = i;
            }
        }
        return results;
    }
}
```

使用数组的解法：

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        int[] t = new int[nums.length + 1];
        int[] results = new int[2];
        for (int i = 0; i < nums.length; i++) {
            t[nums[i]]++;
        }
        for (int i = 1; i <= nums.length; i++) {
            if (t[i] == 2) {
                results[0] = i;
            }
            if (t[i] == 0) {
                results[1] = i;
            }
        }
        return results;
    }
}
```
不使用额外空间，利用排序的解法：

```java
class Solution {
    public int[] findErrorNums(int[] nums) {
        Arrays.sort(nums);
        int[] results = new int[2];
        int l = nums[0] == 1 ? 2 : 1;
        for (int i = 1; i < nums.length; i++) {
            if (l == nums[i]) {
                l++;
            }
            if (nums[i - 1] == nums[i]) {
                results[0] = nums[i];
            }
        }
        results[1] = l;
        return results;
    }
}
```

最后效率最快依次为：数组 > 排序 > HashMap ，占用内存最多依次为：数组 > HashMap > 排序 。（ps. 使用了 HashMap 效率还没有排序快😓）

### 2023.03.10
走，去码头整点薯条😎

![去码头整点薯条](https://image.ericzzz.com/2023/03/10/c15a75c0ee7260b00a9c6588d40c1dcd.JPG)

### 2023.03.11
成都的天气，突然间热起来了🔥

### 2023.03.12
因为一首歌，而去看了一部电影，歌很好听，电影也很感人，这是 [电影链接](https://gimy.app/eps/202004-2-1.html)。

{{<youtube siQJhIp-UTU>}}
    
### 2023.03.13
今天早上马路上的交警没有了，哈哈哈，终于不用过提心吊胆的的日子了😂
    
### 2023.03.14
[全球城市生活成本数据库](https://www.numbeo.com/cost-of-living/)

### 2023.03.15
留给百度的时间不多了🤣

![chatgpt-4](https://image.ericzzz.com/2023/03/15/0e39c0ab-ab67-4307-9c7a-3bc2f2cfe55d.webp)

### 2023.03.16
最近用 ChatGPT 越来越频繁，这玩意确实好用，辅助编写代码时效率提升了不少。恐怕以后真的就一个人 + ChatGPT = 一个公司。

### 2023.03.17
开发者的福音，[codeium](https://codeium.com/)，代替 GitHub Copilot 的另一套解决方案，支持语言类型丰富，而且还免费，体验了一下感觉还不错。

### 2023.03.18
最近人工智能的强大，有些人感觉到焦虑，我觉得其实没有必要，如果你拿它当作工具，使你工作效率提升，那它就是你的朋友。如果你平常不学习，不思考，还想着一招鲜吃遍天，吃老本，那它可能就是你的敌人。

### 2023.03.19
愉快的周末咋过的这么快呢☹

### 2023.03.20
日常周一脑袋昏昏的一天。

### 2023.03.21
自从用了新耳机后，最近听梁祝越听越上头。(ps. 想起了小时候去姑姑家，一进门，表哥就说等你们来，都听了三遍梁祝了😄)

{{<bilibili BV1w54y1P7Wt>}}

### 2023.03.22
人言可畏，苍蝇不叮无缝的蛋，无蜜不招彩蝶蜂。
    
### 2023.03.23
先埋下 🌱 ，等秋天收获 🌲。

### 2023.03.24
晚上 App 发版，又是一个不眠之夜😫。

### 2023.03.25
十公里又 PB 了，马上就要打破五十分钟大关了😎。

### 2023.03.26
为啥早上看过的书，晚上却回忆不出来看过的内容了呢🤔？
    
### 2023.03.27
之前看 [Software-Engineering-at-Google](https://qiangmzsx.github.io/Software-Engineering-at-Google/#/zh-cn/Chapter-2_How_to_Work_Well_on_Teams/Chapter-2_How_to_Work_Well_on_Teams) 其中有一章讲述了`The Bus Factor 巴士因子`这一概念还挺有意思的，今天就深深地感受到了，那什么是巴士因子？意思就是在团队中，有多少人因为被巴士撞到而使项目停摆或失败。当然也可能是其他原因，例如：生病，请假，离职。.. 目前来看我们团队巴士因子很低啊😨，在资源有限的情况下如何提高巴士因子呢🤔
 
### 2023.03.28
昨天使用 Readwise Web 服务时，epub，pdf 文档突然都打不开，在网站提交 bug 之后，今天早上打开邮箱，连收两份 Readwise 团队邮件（ps. 其实是昨天晚上发送的），深深地感受到他们是对用户负责的，是把用户当成上帝的，除了表达歉意外，从 bug 记录，到反馈更新，用时 6 个小时，谁说国外人效率不高的😂，但愿出现 bug 的程序员不要被扣工资，哈哈哈~
    
![Readwise 道歉信](https://image.ericzzz.com/2023/03/28/fc7d582e-68a3-440f-acf2-6092aa6d3b36.webp)
    
### 2023.03.29
你得支棱起来啊🤪
    
![1](https://image.ericzzz.com/2023/03/29/efaffc84-723d-4772-aae0-f692d8cad117.webp)

![2](https://image.ericzzz.com/2023/03/29/9dce6320-ce02-11ed-afa1-0242ac120002.webp)    

### 2023.03.30
人生在世，只为两件事，第一身体舒适，第二灵魂自在。

### 2023.03.31
又到每月最后一天了，真快啊，今年的 1/4 过完了😳。

