## 淘宝APP用户行为分析

数据集：阿里云天池数据  <https://tianchi.aliyun.com/dataset/dataDetail?dataId=46>

使用pandas进行数据的处理与分析，matplotlib进行数据的可视化

- user_id：用户id
- item_id：商品id
- behavior_type：用户行为（1表示浏览，2表示收藏。3表示加入购物车，4表示支付）
- time：时间戳
- user_geohash字段暂时不做研究

数据集主要是2014.11.18-2014.12.18期间的数据，接下来我们从几个维度进行分析：

1. 分析用户行为的漏斗模型
2. 不同时间尺度下用户行为模式分析
3. 不同商品种类的用户行为
4. RFM模型找出有价值的用户

### 数据清洗
- 缺失值处理
- 重复值处理
- 列名修改 id  item  behavior  category  time
- 行为数字修改为pv  collect  cart  pay
- 时间戳日期和小时分开，因为月份和小时要分别研究
 
### 构建模型
**1.分析用户行为的漏斗模型**

利用AARRR模型分析用户行为，此处数据主要涉及用户刺激和购买转化的环节，通过用户从浏览到最终购买整个过程的流失情况，包括浏览、收藏、加入购物车和购买环节，得到一个月内的各项指标如下：
 
APP访客总数（uv）：9988

页面总访问量（pv）：11182815

人均浏览多少页面  pv/uv：1119.625050060072

跳失率=只点击一次浏览的用户数量/总用户访问量：0.0004 

用户总行为数漏斗分析（转化率：浏览-收藏+加购-支付）

![](https://raw.githubusercontent.com/sunandflowering/user_behavor_taobao_app/master/pic/1.png)

**分析：**

1. 从浏览到收藏或者加入购物车的转化率仅5%左右，转化率较低；
2. 从收藏或加入购物车到购买的转化率为20%左右；   
3. uv，pv等值可以查看APP流量，跳失率可以可以按天按周按月，如果过低，可以对页面进行改善，提高用户黏性；
从上面数据可以发现，浏览到收藏或者加入购物车环节的转化率较低，应该着重把从浏览到收藏或者加入购物车环节的转化率改善，增加销售额。

**2.不同时间尺度下用户行为模式分析**

分析一个月中用户每天的行为（四项用户指标：浏览量，收藏量，加购量，支付量）

![](https://raw.githubusercontent.com/sunandflowering/user_behavor_taobao_app/master/pic/2.png)

**分析：**

1. 从图中我们发现在双12当天，由于大促，四项用户指标均达到了高峰；
2. 双12促销期间，因为各种满减活动，会促使用户大量购买，导致购买数涨幅最大；
3. 收藏通常是与购买行为异步的用户行为，在购买行为发生前一段时间才会出现，因此提高幅度不如其他几项指标；
4. 在购买行为发生之前一般都会有加入购物车批量购买，因此加入购物车的行为发生次数同样大幅增加。

分析一周内每日的用户行为：取出12.8-14号的数据进行分析
 
![](https://github.com/sunandflowering/user_behavor_taobao_app/raw/master/pic/3.png)
![](https://github.com/sunandflowering/user_behavor_taobao_app/raw/master/pic/4.png)

**分析：**

我们取双十二及与之相隔较远的另一周的七日内用户行为进行比较，可以看到明显不同。

1. 在平时，周五为一周内各项指标最低的一天，而到周末达到最高峰。推测是上班族周五下班后忙着放松和休息，而周末有充足的精力，购买能力增加；
2. 而双十二当天为周五，所以周五的购买力最高，促销结束后周末的用户活跃度最低；
综上平日运营可以将活动集中在周末进行，而双十二期间集中精力做好促销让用户购买冲动充分释放。

分析一天内每小时的用户行为：选取11.30日的24个小时进行数据统计

![](https://github.com/sunandflowering/user_behavor_taobao_app/raw/master/pic/5.png)
 
**分析：**

一天中有三个高峰，8-10点  13-16点   20-22点（重点投放）

8-10点小高峰，用户一般在上班途中会进行浏览加购；13点-16点会迎来另外一波小高峰，用户会在期间休息；最高峰发生在20-22点，用户下班，上床休息的时候会有一段时间进行浏览加购以及购买，这是一天中最放松的时刻，同时流量和购买力也是最高的时刻，应该在这段时间内进行重点投放，提高流量，增加销售额。

**3.  不同商品种类的用户行为**

双12当天不同商品种类的用户行为，我们所要计算的指标如下：

1. 所有商品的购买次数，并从高到低进行排序
2. 购买次数或者浏览次数或者收藏次数，或者加入购物车最多的商品
这几个指标互相比较，对不同的商品进行分析；

![](https://github.com/sunandflowering/user_behavor_taobao_app/raw/master/pic/6.png)

没有出现购买用户数量非常集中的商品，购买一次的商品占到89.3%，说明商品售卖主要依靠长尾商品的累积效应，而非爆款商品的带动。

商品浏览次数排名

![](https://github.com/sunandflowering/user_behavor_taobao_app/raw/master/pic/7.png)

商品收藏次数排名

![](https://github.com/sunandflowering/user_behavor_taobao_app/raw/master/pic/8.png)

商品加购次数排名

![](https://github.com/sunandflowering/user_behavor_taobao_app/raw/master/pic/9.png)

商品购买次数排名（前20名）

![](https://github.com/sunandflowering/user_behavor_taobao_app/raw/master/pic/10.png)

**分析：**

1. 浏览次数最高的商品32585501，其购买次数没有占据前排，所以意味着浏览之后没有很好的转化为实际销量；
2. 加购高的商品，支付次数排名也较高，加入购物车和购买行为有一定相关关系；
3. 收藏次数较高的商品，浏览量也相对较高。

**4.RFM模型找出有价值的用户**

RFM模型找出有价值的用户，对用户群进行划分

- R 最近购买的时间
- F 购买的频次
- M 支付金额

取出所有购买的商品

R 购买时间在11.17-23号 为0   11.24-30 为1   12.1-12.5 为2  12.6-11 为3  12.12-17为4

首先计算付费用户中用户的消费频次
F 用户消费频次为1-791    1-157为0   158-316为1   317-474为2   475-633为3   634-791为4

可以使用R和F各自的平均值来衡量高低，区分出客户的类型

- 如果R>平均值且F>平均值      重要价值
- 如果R>平均值且F<=平均值     重要保持
- 如果R<=平均值且F>平均值     重要发展
- 如果R<=平均值且F<=平均值    一般价值

![](https://github.com/sunandflowering/user_behavor_taobao_app/raw/master/pic/11.png)
 
对于重要价值用户，他们是最优质的用户，需要重点关注并保持， 应该提高满意度，增加留存；

对于重要保持用户，他们最近有购买，但购买频率不高，可以通过活动等提高其购买频率；

对于重要发展用户，他们虽然最近没有购买，但以往购买频率高，可以做触达，以防止流失；

对于一般价值用户，他们最近没有购买，以往购买频率也不高，特别容易流失，所以应该赠送优惠券或推送活动信息，唤醒购买意愿。

**结论与建议：**

本文分析了淘宝APP用户行为数据共11867590条，从四个不同角度提出业务问题，使用AARRR模型和RFM模型分析数据给出如下结论和建议。

**1.通过AARRR模型分析用户使用的各个环节**

1）获取用户

由于数据中没有给出用户的获取渠道，渠道转化率以及获取成本，我们暂且把浏览行为视为用户的获取。

2）激活用户

用户行为包括点击浏览、放进购物车、收藏以及购买。由于收藏和加入购物车都为浏览和购买阶段之间确定购买意向的用户行为，且不分先后顺序，因此将其算作一个阶段。
从浏览到有购买意向只有5%的转化率，当然有一部分用户是直接购买，但也说明大多数用户以浏览页面为主而购买转化较少，此处为转化漏斗中需要改善和提高的环节。

**针对这一环节改善转化率的建议有：**

1. 优化电商平台的搜索匹配度和推荐策略，主动根据用户喜好推荐相关的商品，优化商品搜索的准确度和聚合能力，对搜索结果排序优先级进行优化；
2. 在商品详情页的展示上突出用户关注的重点信息，精简信息流的呈现方式，减少用户寻找信息的成本；
3. 优化加入购物车和收藏按键的触达，用户在滑屏时也能方便触达，增加功能使用的次数。

3）提高留存

淘宝APP的留存相对而言较为稳定，让用户保提高持使用淘宝电商平台的频率相对而言更加重要。

4）增加收入

5）用户推荐

淘宝本身用户基数庞大，知名度高，个人认为在一二线城市的用户基本已经达到饱和，传播工作需要针对三四线城市的渠道下沉，在这些地区针对用户价格敏感度高的特性开展类似拼多多的拼团转发和打折促销活动，扩大这部分用户的使用率。

**2.研究用户在不同时间尺度下的行为规律，找到用户在不同时间周期下的活跃规律**

一个月中的消费活动在平时以一周为周期进行波动，而双十二促销期间各项指标达到高峰。一周中的高峰期在周末，符合上班族作息时间中的空闲时期。而平时一天中有两个高峰期，下午4点左右和晚十点左右，双十二期间由于活动时间的关系凌晨的销量最高。
针对高峰期进行营销活动收益最高，此时使用人数最多，活动容易触达用户，营销活动的形式可以通过促销、拼团等形式进行。

**3.找到用户对不同种类商品的偏好，找到针对不同商品的营销策略**

商品售卖主要依靠长尾商品的累积效应，而非爆款商品的带动。而浏览次数最高的商品甚至没有进入销量前20，说明这些吸引用户更多注意力的商品没有很好的转化为实际销量。
针对浏览量高而销量不高的这部分商品，需要提高的是用户从点击进入商品详情页到最终购买的体验。

**作为商家端可以从以下几个方面提高销售额：**

商品详情页的实际价格是否相比展示价格偏差过大，有的商家为了吸引用户点击在商品展示页投放的价格具有较强吸引力，但实际价格偏高，在用户心中反而引起反感；
详情页的信息流展示是否合理，是否将用户最想看到的部分置于容易看到的位置，便于信息的获取
优化商品展示的形式，利用视频等方式给用户更直观的感受，提高照片的美观程度；
评论区评价管理，尤其对于差评区的用户反馈进行认真对待，提高自身服务质量。

**4.通过RFM模型找出最具价值的核心付费用户群，对这部分用户的行为进行分析**

R和F评分都很高的用户是体系中的最有价值用户，需要重点关注。并且活动投放时需谨慎对待，不要引起用户反感；
对于R值为4而F值为0的用户，用户粘性不强而消费时间间隔较短，运营活动可以重点针对这部分用户，提高用户使用产品的频率，可以通过拼团打折、积分兑换等活动唤起用户注意力。

参考：<https://www.jianshu.com/p/e2b799c33aa8>
