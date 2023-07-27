# GitHub开发者编程能力评估指标

 

## 一、问题描述

GitHub并没有专门为招聘人员提供相关信息来推断软件开发者的技能水平，为了评估开发人员的质量，招聘人员必须手工检索相应软件开发者的个人信息及存储仓库。

在一个开源社区，如何评判一个开发者编程能力的强弱？

本指标通过开发者历史行为以及其他开发者与该开发者关联行为综合分析评判该开发者的编程能力，以此解决招聘者在招聘开发者时无法评价其价值的问题，为企业的高效招聘提供一种新的解决方案。

## 二、设计目标

- GitHub上的开发者可以根据此评估指标更加了解自己在编程能力上的长处和不足；
- GitHub上的所有开发者能更方便地向其他优秀开发者学习；
- 有招聘需求的企业可以参考此评估指标找寻优质开发者。

## 三、设计思路

开发者的编程能力可以在很多方面体现，通过分析GitHub上开发者的交互行为，得到以下一些客观事实和规律。

首先，创建项目的数量可以一定程度上反映开发者的编程能力。通常来说，编程能力越强的开发者往往能够创建更多的项目，这是因为他们具备了解决问题和实现创意的能力。

其次，一个项目被其他开发者关注（Watch）和Fork的数量也能反映其质量和受欢迎程度，从而间接体现了项目的优秀性以及对创建者编程能力的认可。当其他开发者对一个项目感兴趣并Watch、Fork时，这意味着他们对该项目的价值和贡献持肯定态度。

开发者参与他人项目的能力也是一个重要的指标。当开发者积极参与他人的项目开发，并提交多个Pull Request（PR）时，这表明他们具备一定的编程能力，并能与他人合作共同推进项目的发展。PR的数量可以作为开发者参与项目开发的活跃度的一个参考指标。

如果开发者提交的PR得到项目管理者的合并，这说明他们对项目的贡献被认可，具备较强的编程能力。合并PR意味着开发者的代码被认为对项目有价值，且满足了项目的标准和要求。

开发者对项目提交的PR进行审查和评审也是一项重要的能力，当开发者具备对他人代码进行审查的能力，并能提供有价值的反馈和建议时，这表明他们对项目有深入的了解和理解。

最后，开发者在GitHub上拥有许多关注者（followers）也是一个重要的指标。关注者数量的增加表明开发者引起了其他人的关注，其代码和项目受到广泛关注和认可。在GitHub这样的开源社区，能力强的开发者通常会得到更多人的关注和认可。

综上所述，在开发者的编程能力方面，创建项目数量、项目的关注和Fork数量、参与他人项目的能力（包括PR的数量和被合并的数量）、对项目PR的审查和评审能力以及拥有的关注者数量等因素，都能提供一定的参考，帮助评估开发者的技术水平和影响力。

对各方面进行考虑，确定了编程能力评估指标的影响因素后，下一步就要设计指标的计算公式。指标公式的确定首先需要计算出各影响因素的权重系数，然后确定该公式设计的合理性。对八个影响因素的数据进行分析之后，我们为每个影响因素不同数量分配相应的价值。通过统计开发者这八种交互行为的数量，可以计算出每个因素的分数。然后，我们根据重要程度为每个因素赋予相应的权重，将各影响因素的分数与相应权重相乘，最后将这八个乘积相加得到开发者的最终总分，即编程能力的评分。评分越高表示该开发者的编程能力越高。在评估指标模型确定下来之后，就是指标的呈现方式，最终在GitHub页面上以何种形式展示。

## 四、评估指标模型

### 1 指标计算模型

 <img src="https://github.com/OS-HUBU/ProgrammingAbility/blob/master/images/PA_xmind.png?raw=true" alt="PA_xmind" style="zoom: 50%;"  width="500px"/>

开发者编程能力评估指标的计算公式为：\
$$\
PA = W_{myItems}×S(C_{myItems}) + W_{watches}×S(C_{watches}) + W_{forks}×S(C_{forks}) +W_{parItems}×S(C_{parItems})+ W_{pr}×S(C_{pr})+W_{pr\_merged}×S(C_{pr\_merged}) + W_{pr\_review}×S(C_{pr\_review}) + W_{followers}×S(C_{followers})
$$
\
其中，PA表示开发者编程能力值，W<sub>factor</sub>表示影响因素factor的权重系数；C<sub>factor</sub> 表示该开发者在影响因素factor上的数量，比如某个开发者创建项目的数量；S(C<sub>factor</sub>) 表示影响因素factor的得分，是以影响因素factor为自变量的函数值，函数S即为相应影响因素的实际映射方式，例如某个开发者创建了多少个项目，依据创建的项目数量给予得分。

### 2 影响因素权重的确定

CRITIC权重法是一种基于数据波动性的客观赋权法。其思想在于两项指标，分别是波动性（对比强度）和冲突性（相关性）指标。对比强度使用标准差进行表示，如果数据标准差越大说明波动越大，权重会越高；冲突性使用相关系数进行表示，如果指标之间的相关系数值越大，说明冲突性越小，那么其权重也就越低。权重计算时，对比强度与冲突性指标相乘，并且进行归一化处理，即得到最终的权重。其核心思想是将决策问题转化为对一系列标准的评估，这些标准应该可以定量地描述问题的各个方面。选择最优决策方案的过程就是计算各个备选方案在每个标准下的得分，并根据标准的重要性加权计算总得分，最后选择综合得分最高的。

编程能力一共有八个影响因素，如下表所示，该表共有31万条开发者的数据，通过对已有的八个影响因素的数据进行分析，得出各个影响因素的权重。因此，可以来设计整个指标的公式，然后再进行对编程能力的整体综合评价。其中部分的数据表1展示如下：

<img src="https://github.com/OS-HUBU/ProgrammingAbility/blob/master/images/factor_partdata.png?raw=true" alt="factor_partdata" style="zoom: 50%;" width="800px"/>

<center>表1 开发者编程能力影响因素的部分数据</center>

本次研究利用从数据库提取出的2020年共计31万个开发者的数据指标进行CRITIC权重计算，根据CRITIC权重法计算出八个影响因素的权重如表2所示：

<img src="https://github.com/OS-HUBU/ProgrammingAbility/blob/master/images/critic_weight.png?raw=true" alt="critic_weight" style="zoom: 50%;"  width="500px"/>

<center>表2 CRITIC的计算结果</center>

$$
W_{myItem}=0.175 \ \ ,\ \ W_{watch}=0.106 \ \ ,\ \ W_{fork}=0.171 \ \ ,\ \ W_{watch}=0.087 \ \ \ \ 
\\ \
W_{pr}=0.071 \ \ ,\ \ W_{pr\_merged}=0.127 \ \ ,\ \ W_{pr\_review}=0.144 \ \ ,\ \ W_{follower}=0.120 \ \ \
$$

由此得出指标具体的计算公式为：\
$$
PA = 0.175×S(C_{myItem)} + 0.106×S(C_{watch}) + 0.171×S(C_{fork}) \ \
 + 0.087×S(C_{parItems})  + 0.071×S(C_{pr}) + 0.127×S(C_{pr\_merged}) \ \
 + 0.144×S(C_{pr\_review}) + 0.120×S(C_{followers})
$$

### 3 映射函数的设计

指标中各影响因素的评判采用分值的形式，先对数据进行如表3的方式统计，再对数据进行分段处理并给分。

<img src="https://github.com/OS-HUBU/ProgrammingAbility/blob/master/images/myItems_partdata.png?raw=true" alt="myItems_partdata" style="zoom:67%;" width="600px"/>

<center>表3 创建项目总数的统计数据</center>

上表是影响因素创建项目总数的统计数据，表中的第一列是开发者创建项目的数量，第二列是创建相应数量项目的开发者总人数，第三列是创建小于等于相应数量项目的开发者累计人数，第四列是创建相应数量项目的开发者总人数占所有开发者数量的比例，第五列是第三列累计人数占总开发者数量的比例。

通过创建相应数量项目的开发者人数占比对创建项目数量进行分段，如果创建相应数量项目的开发者占比比较高，则将这个项目创建数量单独分成一个段。比如上表中的创建项目总数的分段依据如下：

创建1个项目的人数占总人数的70%，它的占比相当之高，所以将这个项目创建数量单独分成一个段。同时，从表中也可以看出创建0个项目数的人数占比13%，比例也比较高，虽然这些开发者并没有创建项目，但是他们能注册参与到GitHub开源社区中来，也是值得肯定的，所以会以鼓励的形式给他们赋予10分的分值。从表中还可以看出，创建2个项目数的人数占比有10%，即创建0到2个项目数的人数总占比超过总人数的90%，从创建9个项目数开始，人数约低等于总人数的千分之一。因此，在分段时，创建项目数每增加一个，分值都相应的增加一段，将创建项目数大于等于9的开发者给予这个因素的满分。

在分段和分值里，采取的均是分十段的方式，第一段是10分，最后一段是满分100分，10分是每个因素所对应的值为0的分数，是作为鼓励参与到GitHub中的开发者；第二段设置为60分，因为60分是作为及格线；从第三段开始，每一段都增加5分，直到叠加为满分100分。同理，其他七个影响因素也是按照相同的原理进行分段和打分。

<img src="https://github.com/OS-HUBU/ProgrammingAbility/blob/master/images/myItems_histogram.png?raw=true" alt="myItems_histogram" style="zoom: 50%;" width="500px"/>

<center>图1 创建相应数量项目的开发者人数</center>

分析上述分段规律，将数据划分为如下区间：
$$
S_{myItems}=
\begin{cases}
10,&myItems=0\\60,&myItems=1\\65,&myItems=2\\70,&myItems=3\\75,&myItems=4\\80,&myItems=5\\85,&myItems=6\\90,&myItems=7\\95,&myItems=8\\100,&myItems≥9
\end{cases}
\ \ \ \ \ \ \ \ \ \ 
S_{watches}=
\begin{cases}
10,&watches=0\\60,&watches=1\\65,&watches=2\\70,&watches=3\\75,&4≤watches≤6\\80,&7≤watches≤11\\85,&12≤watches≤24\\90,&25≤watches≤67\\95,&68≤watches≤299\\100,&watches≥300
\end{cases}
\\\\
S_{froks}=
\begin{cases}
10,&forks=0\\60,&forks=1\\65,&2≤forks≤3\\70,&4≤forks≤9\\75,&10≤forks≤25\\80,&26≤forks≤70\\85,&71≤forks≤226\\90,&227≤forks≤1600\\95,&1601≤forks≤2999\\100,&forks≥3000
\end{cases}
\ \ \ \ \ \ 
S_{parItems}=
\begin{cases}
10,&parItems=0\\60,&parItems=1\\65,&parItems=2\\70,&parItems=3\\75,&parItems=4\\80,&parItems=5\\85,&parItems=6\\90,&parItems=7\\95,&parItems=8\\100,&parItems≥9
\end{cases}
\\\\
S_{pr}=
\begin{cases}
10,&pr=0\\60,&pr=1\\65,&pr=2\\70,&3≤pr≤6\\75,&7≤pr≤12\\80,&13≤pr≤26\\85,&27≤pr≤57\\90,&58≤pr≤149\\95,&150≤pr≤512\\100,&pr≥513
\end{cases}
\ \ \ \ \ \ \ \ \ \ 
S_{pr\_merged}=
\begin{cases}
10,&pr_merged=0\\60,&pr_merged=1\\65,&pr_merged=2\\70,&3≤pr_merged≤5\\75,&6≤pr_merged≤8\\80,&9≤pr_merged≤15\\85,&16≤pr_merged≤27\\90,&28≤pr_merged≤51\\95,&52≤pr_merged≤121\\100,&pr_merged≥122
\end{cases}
\\\\
S_{pr\_review}=
\begin{cases}
10,&pr_review=0\\60,&pr_review=1\\65,&2≤pr_review≤3\\70,&4≤pr_review≤5\\75,&6≤pr_review≤8\\80,&9≤pr_review≤14\\85,&15≤pr_review≤22\\90,&23≤pr_review≤38\\95,&39≤pr_review≤68\\100,&pr_review≥69
\end{cases}
\ \ \ \ \ \ \ \ \ \ 
S_{followers}=
\begin{cases}
10,&followers=0\\60,&followers=1\\65,&2≤followers≤3\\70,&4≤followers≤7\\75,&8≤followers≤14\\80,&15≤followers≤24\\85,&25≤followers≤43\\90,&44≤followers≤92\\95,&93≤followers≤351\\100,&followers≥352
\end{cases}
$$

## 五、 数据来源及处理

### 1 数据获取

获取GitHub中日志数据、所有开发者数据及所有仓库数据。从这三个数据表中筛选出我们所需八个因素的数据。

### 2 数据清洗

由于不同时间段，这八个因素的数据存在不一致的情况，我们需要对数据进行去重操作，去重操作我们采用的是筛选出最新时间段数据，对其他时间段的数据去重的方法。同时，开发者有些因素的值缺失，比如项目被fork总数或watch总数，统一补0处理。

### 3 数据分析

从获取的所有数据中，分别分析这八个因素的数据，可以看出每个因素值为0的用户是最多的，可以说明有绝大多数的用户仅仅只是注册了GitHub，但并没有创建项目或参与到其他项目中做出实际的贡献。从整体来看，大多数用户的各项因素值都不高，说明在GitHub中普通的开发者是占大部分的，优秀的开发者是占小部分的。

## 六、GitHub开发者编程能力分析与可视化

### 1 开发者编程能力计算与评估分析

根据数据库中获取到的全域开发者信息，我们对数据进行处理和分析，利用本文的评估指标模型计算公式，计算出每位开发者编程能力得分情况，如表4所示。

<img src="https://github.com/OS-HUBU/ProgrammingAbility/blob/master/images/PA_score_partdata.png?raw=true" alt="PA_score" style="zoom: 50%;" width="800px"/>

<center>表4 开发者编程能力得分部分数据</center>

表中第一列为开发者的用户名，第二列到第九列分别为该开发者八个影响因素的分值，第十列为根据指标计算公式计算出来的编程能力总得分。

分析所有开发者编程能力得分情况，我们发现共31万个开发者，约1.9万开发者编程能力分值在60以上，大部分开发者编程能力得分在60以下；得分在90及以上的开发者有87人，这些开发者编程能力的各项影响因素的分值都很高，在编程方面表现优秀。数据表明，真正参与到项目中、为GitHub社区做出重大贡献的优质开发者占少数，大部分开发者只是利用其学习，不曾真正参与到开源中来。

### 2 开发者编程能力可视化

（1）在GitHub社区中的开发者个人页面可以展示该开发者编程能力雷达图，雷达图展示编程能力八个影响因素的分值情况。

<img src="https://github.com/OS-HUBU/ProgrammingAbility/blob/master/images/chart1.png?raw=true" alt="chart1" style="zoom:67%;" width="400px"/>

<center>图3 开发者编程能力各影响因数的分值</center>

（2）在GitHub社区中的首页推荐部分可以展示开发者编程能力Top10排行榜。将开发者编程能力排行前十的开发者依据能力值以从高到低的形式排列出来。

<img src="https://github.com/OS-HUBU/ProgrammingAbility/blob/master/images/chart2.png?raw=true" alt="chart2" style="zoom: 50%;" width="400px"/>

<center>图4 GitHub中开发者编程能力TOP10</center>

## 提供指标权重的工具

-  [SPSSAU - 在线SPSS分析软件](https://spssau.com/analytical-services.html)https://spssau.com/analytical-services.html

## 参考资料

- 开源软件项目定性和定量分析指标———chaoss指标解析[https://chaoss.community/kb-metrics-and-metrics-models/](#_引用)

## 合作者

- 洪豆
- 王小丽
- 刘一

 
