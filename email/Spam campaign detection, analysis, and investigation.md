# Spam campaign detection, analysis, and investigation

origin

https://www.sciencedirect.com/science/article/pii/S1742287615000079#aep-article-footnote-id9

Author links open overlay panel

[SonDinha](https://www.sciencedirect.com/science/article/pii/S1742287615000079#!)[TaherAzeba](https://www.sciencedirect.com/science/article/pii/S1742287615000079#!)[FrancisFortinb](https://www.sciencedirect.com/science/article/pii/S1742287615000079#!)[DjedjigaMouheba](https://www.sciencedirect.com/science/article/pii/S1742287615000079#!)[MouradDebbabia](https://www.sciencedirect.com/science/article/pii/S1742287615000079#!)

**Show more**

https://doi.org/10.1016/j.diin.2015.01.006[Get rights and content](https://s100.copyright.com/AppDispatchServlet?publisherName=ELS&contentID=S1742287615000079&orderBeanReset=true)

Under a Creative Commons [license](https://creativecommons.org/licenses/by-nc-nd/4.0/)

open access

---



## Abstract

Spam has been a major tool for criminals to conduct illegal activities on the Internet, such as stealing [sensitive information](https://www.sciencedirect.com/topics/computer-science/sensitive-informations), selling counterfeit goods, distributing malware, etc. The astronomical amount of spam data has rendered its manual analysis impractical. Moreover, most of the current techniques are either too complex to be applied on a large amount of data or miss the extraction of vital security insights for forensic purposes. In this paper, we elaborate a software framework for spam campaign detection, analysis and investigation. The proposed framework identifies spam campaigns on-the-fly. Additionally, it labels and scores the campaigns as well as gathers various information about them. The elaborated framework provides [law enforcement officials](https://www.sciencedirect.com/topics/computer-science/law-enforcement-official) with a powerful platform to conduct investigations on cyber-based criminal activities.

垃圾邮件已经成为违法犯罪分子在互联网上开展非法活动的主要手段之一，比如贩卖敏感信息，销售假货，散布恶意软件等。人工分析对于如此庞大数量级的垃圾信息已经杯水车薪。而且，现在的技术要么过于复杂，需要依赖大量的数据，要么无法提取证据供司法调查使用（miss the extraction of vital security insights for forensic purposes）。在本文中，我们介绍一种针对垃圾邮件行为的探测，分析，调查的软件架构。本框架可以做到及时标记垃圾邮件行为，并基于相关的多种信息对垃圾级邮件行为进行Labels(标记分类)和Scores(计分)。本框架为执法人员（https://www.sciencedirect.com/topics/computer-science/law-enforcement-official）提供了一个强大的平台，可以对网络在的犯罪活动进行调查。

## Keywords

Spam 

Spam campaign  

Spam analysis

Characterization

Frequent pattern tree

## Introduction

Electronic mail, or most commonly known as *email*, is ubiquitous and so is its abusive usage. *Spam emails* affect millions of users, waste invaluable resources and have been a burden to the email systems. For instance, according to Symantec Intelligence Report, the global ratio of spam in email traffic is 71.9% ([Symantec Intelligence Report, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib38)). Furthermore, adversaries have taken advantage of the ability to send countless emails anonymously, at the speed of light, to carry on vicious activities (e.g., advertising of fake goods or medications, scams causing financial losses) or even more severely, to commit cyber crimes (e.g., child pornography, identity theft, phishing and malware distribution). Consequently, spam emails contain priceless cyber security intelligence, which may unveil the world of cyber criminals.

电子邮件 无处不在，对电子邮件的滥用也无处不在。垃圾邮件几乎影响着我们每个人，大量占用资源，已然成为邮件系统的巨大负担。根据 Symantec Intelligence 的报告，全球邮件总量中，垃圾邮件占比 71.9%([Symantec Intelligence Report, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib38)).。此外，攻击者可以非常快速的发送无限量的匿名邮件，开展各种活动(宣传假货假药，商业诈骗)，甚至是实施网络犯罪（传播儿童色情，盗取个人信息，推送恶意软件）。因此，垃圾邮件蕴含着无价的网络安全情报，或许可以揭开网络罪犯世界的线索。

Spam data has been used extensively in various studies to detect and investigate cyber threats such as [botnets](https://www.sciencedirect.com/topics/computer-science/botnets) ([Zhuang et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib43), [Xie et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib42), [John et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib19), [Pathak et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib32), [Thonnard and Dacier, 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib39), [Stringhini et al., 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib37)) and [phishing attacks](https://www.sciencedirect.com/topics/computer-science/phishing-attack) ([Fette et al., 2007](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib11), [Bergholz et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib2), [Moore et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib29), [Bergholz et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib3)). Moreover, the accessibility of various data sources, which can be correlated to complement their incompleteness, has brought new opportunities and challenges to researchers. Unfortunately, most studies either use a single data source or work on a static spam data that is collected during a specific time frame ([Pitsillidis et al., 2012](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib34)). More importantly, [spammers](https://www.sciencedirect.com/topics/computer-science/spammer) have been constantly modernizing their arsenals to defeat the anti-spam efforts. Spamming techniques have evolved remarkably from simple programs to sophisticated spamming software, which disseminate template-generated spam through a network of compromised machines. Botnet-disseminated spam emails are usually orchestrated into large-scale campaigns and act as the pivotal instrument for several cyber-based criminal activities. As a consequence, it is critical to perform an analysis of spam data, especially *spam campaigns*, for the purpose of cyber-crime investigation. Nevertheless, given the stunning number of spam emails, it is implausible to analyze them manually. Therefore, cyber-crime investigators need automatic techniques and tools to accomplish this task.

In this research, we aim at elaborating methodologies for spam campaign detection, analysis and investigation. We also emphasize the importance of correlating different data sources to reveal spam campaign characteristics. More precisely, we propose a framework that: (1) Consolidates spam emails into campaigns. (2) Labels spam campaigns by generating related topics for each campaign from Wikipedia data. (3) Correlates different data sources, namely *passive DNS*, malware, WHOIS information and geo-location, to provide more insights into spam campaign characteristics. (4) Scores spam campaigns based on several customizable criteria.

The identification of spam campaigns is a crucial step for analyzing spammers' strategies for the following reasons. First, the amount of spam data is astronomical, and analyzing all spam messages is costly and almost impossible. Hence, clustering spam data into campaigns reduces significantly the amount of data to be analyzed, while maintaining their key characteristics. Second, because of the characteristics of spam, spam messages are usually sent in bulk with specific purposes. Hence, by clustering spam messages into campaigns, we can extract relevant insights that can help investigators understand how spammers obfuscate and disseminate their messages. Labeling spam campaigns and correlating different data sources reveal the characteristics of the campaigns and therefore, significantly increase the effectiveness of an investigation. Moreover, scoring spam campaigns helps investigators concentrate on campaigns that cause more damage (e.g., malware distribution or phishing).

The remainder of this paper is organized as follows. In Section [2](https://www.sciencedirect.com/science/article/pii/S1742287615000079#sec2), we present the related work. We discuss existing techniques for detecting spam campaigns in Section [3](https://www.sciencedirect.com/science/article/pii/S1742287615000079#sec3). In Section [4](https://www.sciencedirect.com/science/article/pii/S1742287615000079#sec4), we present our framework. Experimental results are presented in Section [5](https://www.sciencedirect.com/science/article/pii/S1742287615000079#sec5). Finally, we conclude the paper in Section [6](https://www.sciencedirect.com/science/article/pii/S1742287615000079#sec6).

## Related work

Several approaches for clustering spam emails into groups have been proposed in the literature:

参考文献中提出了几种将垃圾邮件分组的方法：

### URL-based spam email clustering

In this category, spam emails are clustered using features such as the embedded URLs or the source IP addresses of spam emails. F. Li et al. ([Li and Hsieh, 2006](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib27)) propose an approach for clustering spam emails based on identical URLs. The approach also uses the amount of money mentioned inside the content of the email as an extra feature of the cluster. [Xie et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib42) propose AutoRE, a framework that detects [botnet](https://www.sciencedirect.com/topics/computer-science/botnets) hosts by signatures that are generated from URLs embedded in [email bodies](https://www.sciencedirect.com/topics/computer-science/body-email). Both of these URL-based clustering approaches are very efficient in terms of performance and number of false positives. However, [spammers](https://www.sciencedirect.com/topics/computer-science/spammer) can easily evade such techniques using dynamic source IP addresses, URL shorten services or polymorphic URLs.

### 基于url的垃圾邮件聚类

垃圾邮件根据嵌套的URLs 和 源IP 进行分类。F. Li et al. ([Li and Hsieh, 2006](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib27)) 提出一种基于相同URLs进行垃圾邮件聚类的方法。该方法还将电子邮件内容中提到的金额用作聚类的辅助因素。 [Xie et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib42)提出了AutoRE，一种检测僵尸网络 [botnet](https://www.sciencedirect.com/topics/computer-science/botnets)的框架：通过统计邮件中嵌套的URLs 。这些基于URL的据类方法在性能上和误判上表现优秀，但是 spammer 可以通过 动态IP地址，短URL服务，多URL 等技术规避。

### Spam email clustering using text mining methods

[Zhuang et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib43) develop techniques to unveil botnets and their characteristics using spam traces. Spam emails are clustered together into campaigns using shingling algorithm ([Broder et al., 1997](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib4)). The relationship between IP addresses determines if the campaigns are originated from the same botnet. In [Qian et al. (2010)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib35), Qian et al. design an unsupervised, online spam campaign detection, namely *SpamCampaignAssassin* (SCA), and apply extracted campaign signatures for spam filtering. SCA utilizes a text-mining framework built on *[Latent Semantic Analysis](https://www.sciencedirect.com/topics/computer-science/latent-semantic-analysis)* (LSA) ([Landauer et al., 1998](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib24)) to detect campaigns. Nevertheless, template-generated spam and scalability render text mining ineffective.

### 文本发掘方法失效

[Zhuang et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib43)  开发了一种通过spam追踪揭露botnets和它们的特征 的技术，通过shingling algorithm ([Broder et al., 1997](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib4)) 对 Spam 进行聚类。通过IP地址之间的关系确定这些spam行为是否来自同一个 botnet。[Qian et al. (2010)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib35),Qian et al.  设计了一种 无监督的在线 spam行为检测，叫做 *SpamCampaignAssassin* (SCA)，并且根据提取的行为签名进行spam过滤。SCA 基于 潜在语义分析 (*[Latent Semantic Analysis](https://www.sciencedirect.com/topics/computer-science/latent-semantic-analysis)* (LSA) ([Landauer et al., 1998](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib24)) ) 来检测行为。但是模板生成和灵活调整的spam会使文本发掘方法失效。

### Spam email clustering using web pages retrieved from embedded URLs

[Anderson et al. (2007)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib1) propose a technique called *spamscatter* that follows embedded URLs inside spam emails, renders and clusters the websites into scams using *image shingling*. [Konte et al. (2009)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib22) analyze the scam hosting infrastructure. The authors extract *scam campaigns* by clustering web pages retrieved from URLs inside spam emails using different [heuristic methods](https://www.sciencedirect.com/topics/computer-science/heuristic-methods). Even though URL content provides sound results, scalability is also an issue. More importantly, actively crawling embedded URLs may alert spammers and expose the spamtraps used to collect data.

### 通过检索URL指向的页面进行spam聚类

[Anderson et al. (2007)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib1) 提出了*spamscatter* 技术，使用图像拼接，来判断和聚类诈骗类型的spam。[Konte et al. (2009)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib22)  分析了诈骗托管基础架构，作者通过不同的其他方法[heuristic methods](https://www.sciencedirect.com/topics/computer-science/heuristic-methods) 从url相应的web界面提取诈骗行为。即使根据web内容可以得到较好的结果，但是邮件内容和网页内容的灵活性仍然是一个挑战。更重要的，主动爬取URL页面，可能会提醒spammers 并暴露我方收集数据的垃圾邮件陷阱(spamtraps )。

### Spam email clustering based on email content

[Wei et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib40) propose an approach based on theju agglomerative [hierarchical clustering](https://www.sciencedirect.com/topics/computer-science/hierarchical-clustering) algorithm and the connected components with weighted edges model to cluster spam emails. Only spam emails used for advertising are tested by the authors. In [Calais et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib5), Calais et al. introduce a spam campaign identification technique based on [frequent-pattern](https://www.sciencedirect.com/topics/computer-science/frequent-patterns) tree (FP-Tree) ([Han et al., 2000](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib15), [Han et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib16)). Spam campaigns are identified by pointing out nodes that have a significant increase in the number of children. An advantage of this proposed technique is that it can detect obfuscated parts of spam emails naturally without prior knowledge. In another work ([Guerra et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib13)), Calais et al. briefly present *Spam Miner*, a platform for detecting and characterizing spam campaigns. [Kanich et al. (2009)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib21) analyze spam campaigns using a parasitic infiltration of an existing botnet's infrastructure to measure spam delivery, click-through and conversion. [Haider and Scheffer (2009)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib14) apply Bayesian hierarchical clustering ([Heller and Ghahramani, 2005](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib18)) to group spam messages into campaigns. The authors develop a [generative model](https://www.sciencedirect.com/topics/computer-science/generative-model) for clustering binary vectors based on a transformation of the input vectors.

### 基于 email 内容的 spam 聚类

[Wei et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib40)  提出一种spam聚类方法：基于层次聚类算法的，并将组件用加权的边连接起来。作者仅测试了用于广告的垃圾邮件。

 [Calais et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib5) , Calais et al.  提出了基于频率树的spam行为标识技术。通过挑选 子节点数据量显著增加的节点来识别垃圾邮件行为。这个技术的一个优点是：无需事先了解即可自然地检测垃圾邮件的混淆部分。

[Guerra et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib13)), Calais et al. 简要介绍了*Spam Miner*，一个检测和表征垃圾邮件活动的平台。

[Kanich et al. (2009)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib21) 通过对现有的僵尸网络设施的寄生渗透来分析垃圾邮件活动，以衡量垃圾邮件的投递，点击和转化。

 [Haider and Scheffer (2009)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib14) 应用贝叶斯层次聚类来聚合spam信息。作者开发了一个生成模型，基于输入矢量的转换对二进制矢量进行聚类。

## Spam campaign detection techniques

A straightforward approach for grouping spam emails into campaigns is to calculate the distances between spam emails and then apply a [clustering technique](https://www.sciencedirect.com/topics/computer-science/clustering-technique), which generates clusters of emails that are “close” to each other. The distance between two emails can be measured by computing the similarity of their textual contents, e.g., *w-shingling* and the [Jaccard coefficient](https://www.sciencedirect.com/topics/computer-science/jaccard-coefficient) ([Broder et al., 1997](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib4)), *Context Triggered Piecewise Hashing* (CTPH) ([Kornblum, 2006](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib23)) or *[Locality-Sensitive Hashing](https://www.sciencedirect.com/topics/computer-science/locality-sensitive-hashing)* (LSH) ([Damiani et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib9)). The first metric has been used in many studies ([Zhuang et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib43), [Anderson et al., 2007](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib1), [Gao et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib12)) while the second and the third metrics are originally utilized for spam mitigation ([spamsum, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib36), [Damiani et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib9)). However, one disadvantage of this approach is its non-scalability because of the enormous amount of spam emails to be analyzed. Additionally, the use of templates for content generation and numerous [obfuscation techniques](https://www.sciencedirect.com/topics/computer-science/obfuscation-technique) has remarkably increased the diversity of spam emails in one campaign. For these reasons, clustering techniques based on the textual similarity of spam emails have become ineffective.

一种聚类spam的前向方法是：计算spam emails 之间的距离，然后使用聚类技术，将靠近的放在同一个分组中。2个邮件的距离可以通过计算文本内容的相似性来衡量，比如 *w-shingling* 和 [Jaccard coefficient](https://www.sciencedirect.com/topics/computer-science/jaccard-coefficient) ([Broder et al., 1997](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib4))，上下文触发分段哈希 , 位置敏感哈希。第一种已经再多个研究中被使用，第二 第三种 最初是用于减少spam。但是，这种方法有一个缺点就是拓展性差，因为需要对大量spam进行分析。另外内容生成模板的使用和各种混淆技术可以在同一个spam活动中显著增量spam的多样性。由于这些原因，基于文本相似度的聚类技术已经变得收效甚问。

We approach the problem of identifying spam campaigns based on the premise that spam emails sent in one campaign are disseminated autonomously by the same mean. As a result, they must have the same goal and share common characteristics. However, a [spammer](https://www.sciencedirect.com/topics/computer-science/spammer) may employ many obfuscation techniques on the subject, the content and the embedded URL(s). For instance, we have observed that spammers may alter the email subjects in the same campaign, but these subjects still have the same meaning when interpreted manually. This behavior has also been witnessed in ([Pitsillidis et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib33)). Another observation is that a spam email may contain a random text from a book or a Wikipedia article ([Stringhini et al., 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib37)). The text is written by a human and therefore cannot be easily distinguished using statistical methods. Spammers also utilize fast-flux service networks and randomly generated subdomains ([Wei et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib41)) to obfuscate the embedded URLs. Furthermore, spammers usually insert random strings to the embedded URLs to confuse spam filters, or include tracking data to verify their target email lists and monitor their progress.

首先，我们假设一个前提，一个spam活动所发送的spam都是以相同的方式自动发送的，我们要做的是识别这样的活动，所以，这些邮件的目的是一致的，而且具有共同特征。但是，spammer 可能会在标题，内容和URL上使用各种混淆技术。spammer 可以在同一个活动中使用不同的标题，但是这些标题的含义对人类而言是一样的。

Because the obfuscation can be employed at any part of a spam email, our spam campaign detection technique must consider features from the whole spam email. We chose the features for our technique from the header, the textual content, the embedded URL(s) and the attachment(s) of spam emails. Our main spam campaign detection method is based on the *[frequent-pattern](https://www.sciencedirect.com/topics/computer-science/frequent-patterns)* tree (FP-Tree), which is the first step of the FP-Growth algorithm ([Han et al., 2000](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib15), [Han et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib16)). The FP-Tree technique was originally used by [Calais et al. (2008)](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib5). However, our approach is different in terms of the features used as well as the signals used to identify spam campaigns from the previously constructed FP-Tree. Furthermore, we enhance our method to detect spam campaigns on-the-fly by adopting a technique that incrementally builds the FP-Tree ([Cheung and Zaiane, 2003](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib6)). A detailed explanation of our methodology can be found in the next section.

## Our software framework

As depicted in [Fig. 1](https://www.sciencedirect.com/science/article/pii/S1742287615000079#fig1), the main components of our spam campaign detection, analysis and investigation framework are the following:

- •

  *Central Databas*e



![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr1.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr1.jpg)Fig. 1. Architecture of our software framework.

The database is carefully chosen so that it is scalable, flexible and able to store relationships between different entities. Section [4.1](https://www.sciencedirect.com/science/article/pii/S1742287615000079#sec4.1) elucidates our choice.

- •

  *Parsing and Features Extraction*



This component parses spam emails, extracts and stores their features in the central database for further analysis. More details about this component are given in Section [4.2](https://www.sciencedirect.com/science/article/pii/S1742287615000079#sec4.2).

- •

  *Spam Campaign Detection*



This module takes the features from the central database as its inputs, identifies spam campaigns and saves them back into the database. Section [4.3](https://www.sciencedirect.com/science/article/pii/S1742287615000079#sec4.3) explains this module thoroughly.

- •

  *Spam Campaign Characterization*



This component gives insights into spam campaigns to further reduce the analysis time and effort. We combine different data sources, label and score spam campaigns to help investigators quickly grasp the objective of a campaign and concentrate on what they are after. This component is described in detail in Section [4.4](https://www.sciencedirect.com/science/article/pii/S1742287615000079#sec4.4).

### Central database

Due to the tremendous number of spam emails, NoSQL databases such as document-based ones are considered instead of the traditional SQL-based RDBMS. Document-oriented databases are acknowledged for their flexibility and high scalability. However, most of them do not have a powerful [query language](https://www.sciencedirect.com/topics/computer-science/query-languages) like SQL. Moreover, the ability to represent relationships between spam emails, spam campaigns, IP addresses and domain names is essential. These relationships can be stored and processed efficiently in a [graph database](https://www.sciencedirect.com/topics/computer-science/graph-database), which is based on [graph theory](https://www.sciencedirect.com/topics/computer-science/graph-theory). In our system, we employ *OrientDB* ([OrientDB, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib31)), which is an open-source NoSQL [database management system](https://www.sciencedirect.com/topics/computer-science/database-management-systems). OrientDB has the flexibility of document-based databases as well as the capability of graph-based ones for managing relationships between records. It has the advantages of being fast, scalable and supports SQL-styled queries. Spam emails are stored in the database as documents with different fields, such as header fields, textual content, embedded URL(s), etc. Additionally, spam campaigns, IP addresses, domain names and attachments are also saved as different document types. For each document type, the database maintains a [hash table](https://www.sciencedirect.com/topics/computer-science/hash-table) of a specific field to eliminate duplications. The database also stores the relationships between different documents, such as between spam emails and spam campaigns, spam emails and attachments or spam campaigns and IP addresses.

### Parsing and feature extraction

Our parser takes spam emails, each of which consists of a header and a body, as its inputs. The header has many different fields, which are name/value pairs separated by “:”. The body may have only one part or multiple parts. Relevant features needed for the analysis are extracted. More precisely, we consider the following features:

- •

  *Content Type*: The first part of the Content-Type header field describes the MIME types of the email: text/plain, text/html, application/octet-stream, multipart/alternative, multipart/mixed, multipart/related and multipart/report.

- •

  *Character Set*: The encoding of a spam email can be extracted from the header fields or inferred from the email content. This encoding roughly represents the spam email language. This feature is rarely obfuscated by [spammers](https://www.sciencedirect.com/topics/computer-science/spammer) due to the fact that spam emails must be decoded successfully to be read.

- •

  *Subject*: The whole subject line is considered as a feature. If the subject is encoded using Q-encoding, it is decoded into Unicode.

- •

  *Email Layout*: In the case of text/plain, the layout is a sequence of characters “T”, “N” and “U”, which stand for text, newline and URL, respectively. For text/html, the layout is the top three levels of the DOM tree. For multipart emails, we use the tree structure of the [email body](https://www.sciencedirect.com/topics/computer-science/body-email) to form the layout.

- •

  *URL Tokens*: The embedded URLs of spam emails are split into tokens: the hostname, the path and the parameters. Each token is considered as a feature.

- •

  *Attachment Name(s)*: Each attachment name is considered as a feature.



### Spam campaign detection

Our spam campaign detection method is based on the *[Frequent-pattern](https://www.sciencedirect.com/topics/computer-science/frequent-patterns)* tree (FP-Tree) ([Han et al., 2000](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib15), [Han et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib16)). We employ the FP-Tree based on the premise that the more frequent an attribute is, the more it is shared among spam emails. Less frequent attributes usually correspond to the parts that are obfuscated by spammers. Thus, building an FP-Tree from email features allows spam emails to be grouped into campaigns. Furthermore, obfuscated features are discovered naturally within the FP-Tree and therefore exposing the strategies of spammers. Two scans of the dataset are needed to construct the FP-Tree. The cost of inserting a feature vector *fv* into the FP-Tree is O(*[Math Processing Error]|fv|*), where *[Math Processing Error]|fv|* indicates the number of features in *fv*. The FP-Tree usually has a smaller size than the dataset since feature vectors that share similar features are grouped together. Hence, this structure reduces the amount of data that needs to be processed. In the following, we present our FP-Tree-based spam campaign [detection algorithms](https://www.sciencedirect.com/topics/computer-science/detection-algorithm).

#### Frequent-pattern tree construction

We define two data structures, *FPTree* and *FPNode*:

- •

  The *FPTree* has one *root* and one method:

- –

  The *root* is an *FPNode* labeled *null*.

- –

  The *add(transaction)* method (Algorithm 1) accepts a feature vector as its only parameter.

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-fx1.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-fx1.jpg)

- •

  The *FPNode* has eight properties and two methods:

- –

  The *tree* property indicates the *FPTree* of this *FPNode*.

- –

  The *item* property represents the feature.

- –

  The *root* property is *True* if this *FPNode* is the root; *False* if otherwise.

- –

  The *leaf* property is *True* if this *FPNode* is a leaf; *False* if otherwise.

- –

  The *parent* property indicates the *FPNode* that is the parent of this *FPNode*.

- –

  The *children* property is a list of *FPNode*s that are the children of this *FPNode*.

- –

  The *siblings* property is a list of *FPNode*s that are the siblings of this *FPNode*.

- –

  The *add(node)* method adds the *FPNode* “node” as a child of this *FPNode* and adds this *FPNode* as the parent of the *FPNode* “node”.

- –

  The *search*(*item*) method checks to see if this *FPNode* has a child “item”.



We build the FP-Tree by creating the *null FPNode*, which is the root of the tree, and adding the feature vectors individually. The features in each feature vector are sorted in descending order based on their occurrences in the dataset. Algorithm 2 comprehensively demonstrates this process.

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-fx2.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-fx2.jpg)



At the end, we obtain the FP-Tree, in which each node represents a feature extracted from spam emails. Each path from one leaf to the root represents the feature vector of each spam message. From the FP-Tree, we can see that two messages that have some common features share a portion of the path to the root. We use this characteristic to cluster spam emails, which have several common features, into campaigns.

#### Spam campaign identification

From the constructed FP-Tree, we extract spam campaigns by traversing the tree using the depth-first search method. When visiting each node, we check for the following conditions:

- 1.

  The number of children of that node must be greater than the threshold min_num_children.

- 2.

  The average frequency of the children of that node must be greater than the threshold *freq_threshold*.

- 3.

  In the path from that node to the root, there must be one node that does not have its feature type in the *n_obf_features list*.

- 4.

  The number of leaves of the sub-tree starting from that node must be greater than a threshold *min_num_messages*.



If a node satisfies all of the above conditions, all the leaves of the sub-tree that have that node as root are the spam emails of a spam campaign. We store the campaign, delete the sub-tree and continue the process until all the remaining nodes in the tree are visited.

The FP-Tree technique is very efficient and naturally reveals the features that are common in the spam emails of a campaign as well as the features that are obfuscated by spammers ([Calais et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib5)). However, the disadvantage of this technique is that it only works with a static set of data. It has been demonstrated that spam campaigns may last for a long period of time ([Pathak et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib32)). Consequently, the ability to identify spam campaigns in their early stage gives investigators an upper hand in their pursuit of the spammers. We improve our FP-Tree-based spam campaign [identification algorithm](https://www.sciencedirect.com/topics/computer-science/identification-algorithm) by constructing the FP-Tree on-the-fly. We extract feature vectors from spam emails as soon as they arrive and insert those vectors into the tree. In the following, we provide more details about this process.

#### Incremental FP-Tree

Since the FP-Tree approach ([Han et al., 2000](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib15), [Han et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib16)) was proposed for [mining frequent patterns](https://www.sciencedirect.com/topics/computer-science/mining-frequent-pattern), many studies have been conducted to improve the functionality ([Kai-Sang Leung, 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib20)) as well as the performance ([Ong et al., 2003](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib30)) of this technique. Moreover, some FP-Tree-based incremental mining algorithms have been proposed ([Cheung and Zaiane, 2003](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib6), [Leung et al., 2007](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib26)). We adopt the ideas from ([Cheung and Zaiane, 2003](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib6)) to construct the incremental FP-Tree as follows:

- 1.

  Create a *null* root.

- 2.

  Each new transaction is added starting from the root level.

- 3.

  At each level:

- (a)

  The items of the new transaction are compared with the items of the children nodes.

- (b)

  If the new transaction and the children nodes have items in common, the node having the highest count is chosen to merge with the item in the transaction. Thus, the count of that node is increased by 1. The item of the merged node is then removed from the transaction. If the count of a [descendant node](https://www.sciencedirect.com/topics/computer-science/descendant-node) becomes higher than the count of its ancestor, the descendant node has to be moved in front of its ancestor and merged with the item in the transaction.

- (c)

  This process is repeated recursively with the remainder of the new transaction.

- 4.

  Any remaining items in the new transaction are added as a new branch to the last merged node.

- 5.

  Run the spam campaign detection process regularly to detect new campaigns or merge with the existing ones.



### Spam campaign characterization

Grouping spam emails into campaigns has greatly reduced the amount of data that needs to be analyzed. However, when examining a specific spam campaign, an investigator still needs to skim through a significant number of spam emails to grasp the essential information. Therefore, a summary of the spam content is valuable to the investigator. We employ techniques to automatically label spam campaigns to give the investigator an overall view of them. We also develop a scoring mechanism to rank spam campaigns so that the investigator can concentrate on the highly-ranked ones. Additionally, spam campaigns may reveal some characteristics that are hidden when analyzing spam emails separately. For example, the relationships between different domains and IP addresses or between spamming servers. Therefore, we utilize and correlate various kinds of information, such as WHOIS information, passive DNS data, geo-location and malware database, to assist investigators in tracing spammers. Moreover, we employ visualization tools to illustrate the relationships between spam emails, spam campaigns, domains and IP addresses. The visualization helps to emphasize critical information in the dataset, for instance, an aggressive spamming IP address. In the following, we provide more details about our characterization methods.

#### Correlation module

In this module, we utilize WHOIS information ([Harrenstien et al., 1985](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib17), [Daigle, 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib8)) and passive DNS data, received from a trusted third party, to gain more insights into the domain names found in spam emails. WHOIS information is associated with each second-level domain while passive DNS data gives the historical relationships between hostnames and IP addresses along with their time-stamps. In addition, we use geo-location and malware data to add valuable information to the IP addresses that we have found in spam campaigns. The IP addresses can be extracted from the Received header fields or from the passive DNS data.

#### Spam campaign labeling

We propose techniques for labeling spam campaigns to provide an overview of their content. First, we employ an automatic labeling technique that has been proposed by Lau et al. ([Lau et al., 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib25)). Even though this method works in some cases, it is not scalable because of the large number of queries to Wikipedia. Thus, we apply another method that utilizes *Wikipedia Miner toolkit* ([Milne and Witten, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib28)), which stores summarized versions of Wikipedia in offline databases. We create a *Spam Campaign Labeling Server*, which listens to a specific network port, using the libraries from Wikipedia Miner toolkit. We extract frequent words from the content of spam emails in a campaign and feed them to the *Spam Campaign Labeling Server*. The response, which contains a list of relevant topics of that spam campaign, is saved back into the central database.

#### Spam campaign scoring

In most cases, investigators only need to pursue a particular objective in their investigations. Therefore, we suggest a method that has been verified by a [law enforcement official](https://www.sciencedirect.com/topics/computer-science/law-enforcement-official) to assign each spam campaign a score. The score of each campaign is determined by the sum of various weighted elements. Each element is the count of an attribute, a characteristic or a signal that may appear in spam emails of a campaign. For example, an investigator who is interested in spammers targeting Canadians may want to put more effort into spam campaigns that have “.ca” domain names in the email addresses of the recipients. We implement the attributes both from our speculation and from the advice of a Canadian law enforcement official. Some of the signals are Canadian IP addresses, “.ca” top-level domain names, IP ranges of Canadian corporations, etc.

## Results

In this section, we present the results of our spam campaign analysis. We also show some visualizations that can provide investigators with a spectacular view of the spam campaigns to further understand the behaviors of [spammers](https://www.sciencedirect.com/topics/computer-science/spammer).

### The dataset

[Table 1](https://www.sciencedirect.com/science/article/pii/S1742287615000079#tbl1) shows some statistics of our spam dataset that we received from a trusted third party. [Table 2](https://www.sciencedirect.com/science/article/pii/S1742287615000079#tbl2) and [Table 3](https://www.sciencedirect.com/science/article/pii/S1742287615000079#tbl3) depict the distribution of the spam email MIME types. The MIME type and the character set are rarely obfuscated by spammers. However, spam messages may not follow the standard rules, which leads to the appearance of some non-standard MIME types. This valuable signal can be used to detect spam emails that are generated by the same automatic mean. The distribution of the correct MIME types is shown in [Table 4](https://www.sciencedirect.com/science/article/pii/S1742287615000079#tbl4).

Table 1. Statistics of the dataset.

|                       | 1 year      | 1 month |
| --------------------- | ----------- | ------- |
| (Apr. 2012–Mar. 3013) | (Apr. 2013) |         |
| Messages              | 678,095     | 100,339 |
| Unique IP addresses   | 91,370      | 16,055  |
| Unique hostnames      | 321,549     | 74,833  |
| Unique attachments    | 5537        | 393     |

Table 2. Distribution of ‘raw’ MIME types (1 month).

| MIME type                             | Count  | Percentage |
| ------------------------------------- | ------ | ---------- |
| text/html                             | 52,596 | 50.2345%   |
| text/plain                            | 32,977 | 31.49.54%  |
| multipart/alternative                 | 16,270 | 15.5395%   |
| multipart/mixed                       | 2465   | 2.3543%    |
| multipart/related                     | 264    | 0.2521%    |
| multipart/report                      | 60     | 0.0573%    |
| text/html∖n∖tcharset = “windows-1250” | 19     | 0.0181%    |
| text/html∖n∖tcharset = “us-ascii”     | 15     | 0.0143%    |
| text/html∖n∖tcharset = “iso-8859-1”   | 14     | 0.0134%    |
| text/html∖n∖tcharset = “windows-1252” | 13     | 0.0124%    |
| text/html∖n∖tcharset = “iso-8859-2”   | 8      | 0.0076%    |
| application/octet-stream              | 1      | 0.0010%    |

Table 3. Distribution of ’raw’ MIME types (1 year).

| MIME type                              | Count   | Percentage |
| -------------------------------------- | ------- | ---------- |
| text/plain                             | 353,774 | 52.1431%   |
| text/html                              | 213,578 | 31.4795%   |
| multipart/alternative                  | 73,436  | 10.8238%   |
| multipart/mixed                        | 32,797  | 4.8340%    |
| multipart/related                      | 2,134   | 0.3145%    |
| multipart/report                       | 1,878   | 0.2768%    |
| text/html∖n∖tcharset = “windows-1252”  | 131     | 0.0193%    |
| text/html∖n∖tcharset = “windows-1250”  | 127     | 0.0187%    |
| text/html∖n∖tcharset = “iso-8859-2”    | 126     | 0.0186%    |
| text/html∖n∖tcharset = “us-ascii”      | 125     | 0.0184%    |
| text/html∖n∖tcharset = “iso-8859-1”    | 108     | 0.0159%    |
| application/octet-stream               | 75      | 0.0111%    |
| text/plain charset = utf-8             | 35      | 0.0052%    |
| text/plain charset = us-ascii          | 29      | 0.0043%    |
| text/plain charset = “us-ascii”        | 28      | 0.0041%    |
| text/plain charset = “utf-8”           | 26      | 0.0038%    |
| text/plain charset = “iso-8859-1”      | 25      | 0.0037%    |
| text/plain charset = iso-8859-1        | 25      | 0.0037%    |
| text/plain∖n∖tcharset = “windows-1252” | 4       | 0.0006%    |
| text/plain∖n∖tcharset = “us-ascii”     | 3       | 0.0004%    |
| text/plain∖n∖tcharset = “iso-8859-2”   | 1       | 0.0001%    |
| text/plain∖n∖tcharset = “iso-8859-1”   | 1       | 0.0001%    |
| text/plain∖n∖tcharset = “windows-1250” | 1       | 0.0001%    |

Table 4. Distribution of the correct MIME types.

| MIME type                | 1 year  | 1 month  |        |          |
| ------------------------ | ------- | -------- | ------ | -------- |
| text/html                | 353,952 | 52.1694% | 52,665 | 50.3004% |
| text/plain               | 214,195 | 31.5704% | 32,977 | 31.4954% |
| multipart/alternative    | 73,436  | 10.8238% | 16,270 | 15.5395% |
| multipart/mixed          | 32,797  | 4.8340%  | 2465   | 2.3543%  |
| multipart/related        | 2134    | 0.3145%  | 264    | 0.2521%  |
| multipart/report         | 1878    | 0.2768%  | 60     | 0.0573%  |
| application/octet-stream | 75      | 0.0111%  | 1      | 0.0010%  |

### Spam campaign detection

[Fig. 2](https://www.sciencedirect.com/science/article/pii/S1742287615000079#fig2) depicts a full FP-Tree that is built using a *one-day* spam data ([Data-Driven Documents](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib10)) ([CodeFlower Source code visualization](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib7)). This figure clearly shows groups of nodes inside the FP-Tree, proving that the spam emails can be grouped into spam campaigns by using the FP-Tree structure. [Fig. 3](https://www.sciencedirect.com/science/article/pii/S1742287615000079#fig3) illustrates a part of the FP-Tree that we have built ([Data-Driven Documents](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib10)). The node labeled “*TTTTTNN*” represents the layout of the emails. We can clearly see that all the spam emails of the campaign share the same MIME type (“*text/plain*”), the same character set (“*windows-1250*”) and the same layout (“*TTTTTNN*”). However, they have different subject lines although those subjects have the same meaning (about stock markets).

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr2.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr2.jpg)Fig. 2. Visualization of the FP-Tree constructed from a one-day spam data.

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr3.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr3.jpg)Fig. 3. Visualization of one part of the [frequent-pattern](https://www.sciencedirect.com/topics/computer-science/frequent-patterns) tree.

[Table 5](https://www.sciencedirect.com/science/article/pii/S1742287615000079#tbl5) shows the time needed to extract spam campaigns from our dataset. The program runs on a server that has the Intel® Xeon® X5660 (2.80 GHz) CPU and 32 GB of memory. The most time-consuming task is the parsing of spam email files to insert into the main database (around 3 h for one-year data and 40 min for one-month data). The remaining steps only take around 5 min for one-year data and 1 min 30 s for one-month data to complete. We use one-year data just to demonstrate the efficiency of the FP-Tree method. The following sections present the results using one-month data.

Table 5. Processing time.

|                         | 1 year    | 1 month  |
| ----------------------- | --------- | -------- |
| Parsing time            | 10885.2 s | 2467.3 s |
| Feature extracting time | 204.6 s   | 78.0 s   |
| FP-Tree building time   | 36.7 s    | 7.8 s    |
| Campaign detecting time | 35.5 s    | 5.4 s    |

#### FP-tree features

In [Table 6](https://www.sciencedirect.com/science/article/pii/S1742287615000079#tbl6), we show the critical features that are used to identify spam campaigns. Moreover, we perform a more in-depth analysis of the relation between the decisive features and the parameters of our spam campaign [detection algorithm](https://www.sciencedirect.com/topics/computer-science/detection-algorithm). The process is as follows:

- •

  First, the *min_num_messages* parameter is fixed to 5.

- •

  For each step, the *min_num_children* parameter is increased by 1 (from 2 to 20). The number of detected spam campaigns corresponding to the decisive features are recorded 8 times (when the *freq_threshold* parameter is set to 1.5, 2.0, 2.5, 3.0, 3.5, 4.0, 4.5 and 5.0, respectively). The *freq_threshold* of 1.5 means that the average frequency of the children nodes is larger or equal to 1.5 times the frequency of the current node.



Table 6. Distribution of the decisive features used to identify spam campaigns.

| Feature       | 1 year | 1 month |
| ------------- | ------ | ------- |
| Layout        | 2,778  | 641     |
| Hostname      | 1,036  | 190     |
| Subject       | 1,160  | 134     |
| URL           | 602    | 106     |
| Attachment    | 108    | 15      |
| Character set | 3      | 2       |

The results of this analysis are shown in [Fig. 4](https://www.sciencedirect.com/science/article/pii/S1742287615000079#fig4).

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr4.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr4.jpg)Fig. 4. Relation between the FP-Tree features and the algorithm's parameters.

#### Text similarity

After spam campaigns have been identified using the FP-Tree, we calculate the text similarity scores of each campaign. We apply three measurements to generate three similarity scores of all the email pairs in each campaign: *w-shingling* and the [Jaccard coefficient](https://www.sciencedirect.com/topics/computer-science/jaccard-coefficient) ([Broder et al., 1997](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib4)), *Context Triggered Piecewise Hashing* (CTPH) ([Kornblum, 2006](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib23)) and *[Locality-Sensitive Hashing](https://www.sciencedirect.com/topics/computer-science/locality-sensitive-hashing)* (LSH) ([Damiani et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bib9)). A spam campaign has three similarity scores, each of which is the average score of each method. [Fig. 5](https://www.sciencedirect.com/science/article/pii/S1742287615000079#fig5) shows the results of this process. The vertical axis represents the score of each metric while the horizontal axis is the campaigns sorted by their scores. [Table 7](https://www.sciencedirect.com/science/article/pii/S1742287615000079#tbl7) summarizes the results.

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr5.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr5.jpg)Fig. 5. Spam campaign text similarity scores.

Table 7. Summary of campaign text similarity scores.

|         | w-s  | CTPH   | LSH  |        |      |        |
| ------- | ---- | ------ | ---- | ------ | ---- | ------ |
| >=50%   | 598  | 54.96% | 595  | 54.69% | 859  | 78.95% |
| 100%    | 265  | 24.36% | 206  | 18.93% | 260  | 23.90% |
| 0%      | 20   | 1.84%  | 46   | 4.23%  | 0    | 0.00%  |
| No text | 10   | 0.92%  | 10   | 0.92%  | 10   | 0.92%  |

Calculating text similarity between messages in each spam campaign is a very time-consuming process. It takes 13,339.6 s (around 3 h 42 min), 3942.5 s (around 65 min) and 584,486 s (nearly one week) to complete the calculation of the w-shingling method, the Context Triggered Piecewise Hashing method and the Locality-Sensitive Hashing method, respectively.

### Spam campaign characterization

In this subsection, we present some characteristics of the spam campaigns identified from a one-month data. In [Fig. 6](https://www.sciencedirect.com/science/article/pii/S1742287615000079#fig6), we show the spam campaigns along with the spam messages, the related IP addresses and the related hostnames. We select top 5 spam campaigns (with big numbers in the figure) that have the highest scores and have less than 1000 messages for the purpose of this illustration. The scores are computed based on some criteria that have been mentioned in our spam campaign scoring method. In [Fig. 7](https://www.sciencedirect.com/science/article/pii/S1742287615000079#fig7), a word cloud, which illustrates the topics of the spam campaigns, is presented.

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr6.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr6.jpg)Fig. 6. Five spam campaigns that have the highest scores and less than 1000 emails

![img](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr7.jpg)[Download : Download full-size image](https://ars.els-cdn.com/content/image/1-s2.0-S1742287615000079-gr7.jpg)Fig. 7. Topics of spam campaigns.

## Conclusion

Spam campaigns act as the pivotal instrument for several cyber-based criminal activities. Consequently, the analysis of spam campaigns is a critical task for cyber security officers. In this paper, we propose a software framework for spam campaign detection, analysis and investigation. The framework provides [law enforcement officials](https://www.sciencedirect.com/topics/computer-science/law-enforcement-official) with a platform to conduct investigations on cyber crimes. Our system greatly reduces investigation efforts by consolidating spam emails into campaigns. Moreover, it aids investigators by providing various information about spam campaigns. Spam campaign characteristics are also unveiled by labeling spam campaigns and correlating different data sources. Additionally, we employ a feature-rich and scalable database to handle a large number of spam emails, a scoring mechanism to highlight severe spam campaigns and a visualization tool to further assist investigators. Our system has been adopted by a governmental organization and used by law enforcement officials to pursuit [spammers](https://www.sciencedirect.com/topics/computer-science/spammer), take down spamming servers and reduce spam volume.

## References

- [Anderson et al., 2007](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib1)

  D.S. Anderson, C. Fleizach, S. Savage, G.M. Voelker**Spamscatter: characterizing internet scam hosting infrastructure**Proceedings of 16th USENIX security symposium, SS'07 (2007)pp. 10:1–10:14[Google Scholar](https://scholar.google.com/scholar_lookup?title=Spamscatter%3A characterizing internet scam hosting infrastructure&publication_year=2007&author=D.S. Anderson&author=C. Fleizach&author=S. Savage&author=G.M. Voelker)

- [Bergholz et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib2)

  A. Bergholz, J.H. Chang, G. Paaß, F. Reichartz, S. Strobel**Improved phishing detection using model-based features**CEAS (2008)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Improved phishing detection using model-based features&publication_year=2008&author=A. Bergholz&author=J.H. Chang&author=G. Paaß&author=F. Reichartz&author=S. Strobel)

- [Bergholz et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib3)

  A. Bergholz, J. De Beer, S. Glahn, M.-F. Moens, G. Paaß, S. Strobel**New filtering approaches for phishing email**J Comput Secur, 18 (1) (2010), pp. 7-35[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-71749098791&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=New filtering approaches for phishing email&publication_year=2010&author=A. Bergholz&author=J. De Beer&author=S. Glahn&author=M.-F. Moens&author=G. Paaß&author=S. Strobel)

- [Broder et al., 1997](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib4)

  A.Z. Broder, S.C. Glassman, M.S. Manasse, G. Zweig**Syntactic clustering of the web**Comput Netw ISDN Syst, 29 (8) (1997), pp. 1157-1166[Article](https://www.sciencedirect.com/science/article/pii/S0169755297000317)[Download PDF](https://www.sciencedirect.com/science/article/pii/S0169755297000317/pdf?md5=0a24c3c880cb996321cdfad936d3195b&pid=1-s2.0-S0169755297000317-main.pdf)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-0010362121&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Syntactic clustering of the web&publication_year=1997&author=A.Z. Broder&author=S.C. Glassman&author=M.S. Manasse&author=G. Zweig)

- [Calais et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib5)

  P. Calais, D.E. Pires, D.O.G. Neto, W. Meira Jr., C. Hoepers, K. Steding-Jessen**A campaign-based characterization of spamming strategies**CEAS (2008)[Google Scholar](https://scholar.google.com/scholar_lookup?title=A campaign-based characterization of spamming strategies&publication_year=2008&author=P. Calais&author=D.E. Pires&author=D.O.G. Neto&author=W. Meira Jr.&author=C. Hoepers&author=K. Steding-Jessen)

- [Cheung and Zaiane, 2003](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib6)

  W. Cheung, O.R. Zaiane**Incremental mining of frequent patterns without candidate generation or support constraint**Database engineering and applications symposium, 2003. Proceedings. Seventh international, IEEE (2003), pp. 111-116[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-84879079572&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Incremental mining of frequent patterns without candidate generation or support constraint&publication_year=2003&author=W. Cheung&author=O.R. Zaiane)

- [CodeFlower,](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib7)

  CodeFlower Source code visualizationhttp://redotheweb.com/CodeFlower/

- [Daigle, 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib8)

  L. Daigle**WHOIS protocol specification**Internet RFC, 2070-1721, 3912 (2004)[Google Scholar](https://scholar.google.com/scholar_lookup?title=WHOIS protocol specification&publication_year=2004&author=L. Daigle)

- [Damiani et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib9)

  E. Damiani, S.D.C. di Vimercati, S. Paraboschi, P. Samarati**An open digest-based technique for spam detection**ISCA PDCS (2004), pp. 559-564[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-33845952356&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=An open digest-based technique for spam detection&publication_year=2004&author=E. Damiani&author=S.D.C. di Vimercati&author=S. Paraboschi&author=P. Samarati)

- [Data-Driven Documents](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib10)

  Data-Driven Documents, http://d3js.org/.[Google Scholar](https://scholar.google.com/scholar?q=Data-Driven Documents, http:d3js.org.)

- [Fette et al., 2007](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib11)

  I. Fette, N. Sadeh, A. Tomasic**Learning to detect phishing emails**Proceedings of the 16th international conference on world wide Web, ACM (2007), pp. 649-656[CrossRef](https://doi.org/10.1145/1242572.1242660)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-35348913799&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Learning to detect phishing emails&publication_year=2007&author=I. Fette&author=N. Sadeh&author=A. Tomasic)

- [Gao et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib12)

  H. Gao, J. Hu, C. Wilson, Z. Li, Y. Chen, B.Y. Zhao**Detecting and characterizing social spam campaigns**Proceedings of the 10th ACM SIGCOMM conference on internet measurement, ACM (2010), pp. 35-47[CrossRef](https://doi.org/10.1145/1879141.1879147)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-78650880229&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Detecting and characterizing social spam campaigns&publication_year=2010&author=H. Gao&author=J. Hu&author=C. Wilson&author=Z. Li&author=Y. Chen&author=B.Y. Zhao)

- [Guerra et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib13)

  P. Guerra, D. Pires, D. Guedes, W. Meira Jr., C. Hoepers, K. Steding-Jessen**Spam miner: a platform for detecting and characterizing spam campaigns**Proc. 6th Conf. Email Anti-Spam (2008)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Spam miner%3A a platform for detecting and characterizing spam campaigns&publication_year=2008&author=P. Guerra&author=D. Pires&author=D. Guedes&author=W. Meira Jr.&author=C. Hoepers&author=K. Steding-Jessen)

- [Haider and Scheffer, 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib14)

  P. Haider, T. Scheffer**Bayesian clustering for email campaign detection**Proceedings of the 26th annual international conference on machine learning, ACM (2009), pp. 385-392[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-71149085753&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Bayesian clustering for email campaign detection&publication_year=2009&author=P. Haider&author=T. Scheffer)

- [Han et al., 2000](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib15)

  J. Han, J. Pei, Y. Yin**Mining frequent patterns without candidate generation**ACM SIGMOD Rec, 29 (2) (2000), pp. 1-12[CrossRef](https://doi.org/10.1145/335191.335372)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-0005378038&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Mining frequent patterns without candidate generation&publication_year=2000&author=J. Han&author=J. Pei&author=Y. Yin)

- [Han et al., 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib16)

  J. Han, J. Pei, Y. Yin, R. Mao**Mining frequent patterns without candidate generation: a frequent-pattern tree approach**Data Min Knowl Discov, 8 (1) (2004), pp. 53-87[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-2442449952&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Mining frequent patterns without candidate generation%3A a frequent-pattern tree approach&publication_year=2004&author=J. Han&author=J. Pei&author=Y. Yin&author=R. Mao)

- [Harrenstien et al., 1985](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib17)

  K. Harrenstien, M. Stahl, E. Feinler**WHOIS protocol specification**Internet RFC, 2070-1721, 954 (1985)[Google Scholar](https://scholar.google.com/scholar_lookup?title=WHOIS protocol specification&publication_year=1985&author=K. Harrenstien&author=M. Stahl&author=E. Feinler)

- [Heller and Ghahramani, 2005](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib18)

  K.A. Heller, Z. Ghahramani**Bayesian hierarchical clustering**Proceedings of the 22nd international conference on machine learning, ACM (2005), pp. 297-304[CrossRef](https://doi.org/10.1145/1102351.1102389)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-31844448530&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Bayesian hierarchical clustering&publication_year=2005&author=K.A. Heller&author=Z. Ghahramani)

- [John et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib19)

  J.P. John, A. Moshchuk, S.D. Gribble, A. Krishnamurthy**Studying spamming botnets using botlab**NSDI, vol. 9 (2009), pp. 291-306[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-78649292639&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Studying spamming botnets using botlab&publication_year=2009&author=J.P. John&author=A. Moshchuk&author=S.D. Gribble&author=A. Krishnamurthy)

- [Kai-Sang Leung, 2004](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib20)

  C. Kai-Sang Leung**Interactive constrained frequent-pattern mining system**Database engineering and applications symposium, 2004. IDEAS'04. Proceedings. International, IEEE (2004), pp. 49-58[Google Scholar](https://scholar.google.com/scholar_lookup?title=Interactive constrained frequent-pattern mining system&publication_year=2004&author=C. Kai-Sang Leung)

- [Kanich et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib21)

  C. Kanich, C. Kreibich, K. Levchenko, B. Enright, G.M. Voelker, V. Paxson, *et al.***Spamalytics: an empirical analysis of spam marketing conversion**Commun ACM, 52 (9) (2009), pp. 99-107[CrossRef](https://doi.org/10.1145/1562164.1562190)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-70149106104&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Spamalytics%3A an empirical analysis of spam marketing conversion&publication_year=2009&author=C. Kanich&author=C. Kreibich&author=K. Levchenko&author=B. Enright&author=G.M. Voelker&author=V. Paxson)

- [Konte et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib22)

  M. Konte, N. Feamster, J. Jung**Dynamics of online scam hosting infrastructure**Passive and active network measurement, Springer (2009), pp. 219-228[CrossRef](https://doi.org/10.1007/978-3-642-00975-4_22)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-67649946711&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Dynamics of online scam hosting infrastructure&publication_year=2009&author=M. Konte&author=N. Feamster&author=J. Jung)

- [Kornblum, 2006](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib23)

  J. Kornblum**Identifying almost identical files using context triggered piecewise hashing**Digit Investig, 3 (2006), pp. 91-97[Article](https://www.sciencedirect.com/science/article/pii/S1742287606000764)[Download PDF](https://www.sciencedirect.com/science/article/pii/S1742287606000764/pdfft?md5=6dbc5f184b66b60c3e245952108b03a7&pid=1-s2.0-S1742287606000764-main.pdf)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-33746191665&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Identifying almost identical files using context triggered piecewise hashing&publication_year=2006&author=J. Kornblum)

- [Landauer et al., 1998](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib24)

  T.K. Landauer, P.W. Foltz, D. Laham**An introduction to latent semantic analysis**Discourse Process, 25 (2–3) (1998), pp. 259-284[CrossRef](https://doi.org/10.1080/01638539809545028)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-35048842682&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=An introduction to latent semantic analysis&publication_year=1998&author=T.K. Landauer&author=P.W. Foltz&author=D. Laham)

- [Lau et al., 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib25)

  J.H. Lau, K. Grieser, D. Newman, T. Baldwin**Automatic labelling of topic models**ACL, 2011 (2011), pp. 1536-1545[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-84859022927&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Automatic labelling of topic models&publication_year=2011&author=J.H. Lau&author=K. Grieser&author=D. Newman&author=T. Baldwin)

- [Leung et al., 2007](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib26)

  C.K.-S. Leung, Q.I. Khan, Z. Li, T. Hoque**Cantree: a canonical-order tree for incremental frequent-pattern mining**Knowl Inform. Syst, 11 (3) (2007), pp. 287-311[CrossRef](https://doi.org/10.1007/s10115-006-0032-8)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-34147173382&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Cantree%3A a canonical-order tree for incremental frequent-pattern mining&publication_year=2007&author=C.K.-S. Leung&author=Q.I. Khan&author=Z. Li&author=T. Hoque)

- [Li and Hsieh, 2006](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib27)

  F. Li, M.-H. Hsieh**An empirical study of clustering behavior of spammers and group-based anti-spam strategies**CEAS (2006)[Google Scholar](https://scholar.google.com/scholar_lookup?title=An empirical study of clustering behavior of spammers and group-based anti-spam strategies&publication_year=2006&author=F. Li&author=M.-H. Hsieh)

- [Milne and Witten, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib28)

  D. Milne, I.H. Witten**An open-source toolkit for mining wikipedia**Artificial Intelligence (2013)[Google Scholar](https://scholar.google.com/scholar_lookup?title=An open-source toolkit for mining wikipedia&publication_year=2013&author=D. Milne&author=I.H. Witten)

- [Moore et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib29)

  T. Moore, R. Clayton, H. Stern**Temporal correlations between spam and phishing websites**Proc. of 2nd USENIX LEET (2009)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Temporal correlations between spam and phishing websites&publication_year=2009&author=T. Moore&author=R. Clayton&author=H. Stern)

- [Ong et al., 2003](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib30)

  K.-L. Ong, W.-K. Ng, E.-P. Lim**Fssm: fast construction of the optimized segment support map**Data warehousing and knowledge discovery, Springer (2003), pp. 257-266[CrossRef](https://doi.org/10.1007/978-3-540-45228-7_26)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-35248817442&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Fssm%3A fast construction of the optimized segment support map&publication_year=2003&author=K.-L. Ong&author=W.-K. Ng&author=E.-P. Lim)

- [OrientDB, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib31)

  OrientDB, http://www.orientdb.org/, last accessed in August 2013.[Google Scholar](https://scholar.google.com/scholar?q=OrientDB, http:www.orientdb.org, last accessed in August 2013.)

- [Pathak et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib32)

  A. Pathak, F. Qian, Y.C. Hu, Z.M. Mao, S. Ranjan**Botnet spam campaigns can be long lasting: evidence, implications, and analysis**Proceedings of the eleventh international joint conference on measurement and modeling of computer systems, ACM (2009), pp. 13-24[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-70449652587&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Botnet spam campaigns can be long lasting%3A evidence%2C implications%2C and analysis&publication_year=2009&author=A. Pathak&author=F. Qian&author=Y.C. Hu&author=Z.M. Mao&author=S. Ranjan)

- [Pitsillidis et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib33)

  A. Pitsillidis, K. Levchenko, C. Kreibich, C. Kanich, G.M. Voelker, V. Paxson, *et al.***Botnet judo: fighting spam with itself**NDSS (2010)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Botnet judo%3A fighting spam with itself&publication_year=2010&author=A. Pitsillidis&author=K. Levchenko&author=C. Kreibich&author=C. Kanich&author=G.M. Voelker&author=V. Paxson)

- [Pitsillidis et al., 2012](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib34)

  A. Pitsillidis, C. Kanich, G.M. Voelker, K. Levchenko, S. Savage**Taster's choice: a comparative analysis of spam feeds**Proceedings of the 2012 ACM conference on internet measurement conference, IMC'12 (2012), pp. 427-440[CrossRef](https://doi.org/10.1145/2398776.2398821)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-84870882181&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar?q=Tasters choice: a comparative analysis of spam feeds)

- [Qian et al., 2010](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib35)

  F. Qian, A. Pathak, Y.C. Hu, Z.M. Mao, Y. Xie**A case for unsupervised-learning-based spam filtering**ACM SIGMETRICS Perform Eval Rev, 38 (1) (2010), pp. 367-368[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-77954913096&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=A case for unsupervised-learning-based spam filtering&publication_year=2010&author=F. Qian&author=A. Pathak&author=Y.C. Hu&author=Z.M. Mao&author=Y. Xie)

- [spamsum, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib36)

  spamsum, http://www.samba.org/ftp/unpacked/junkcode/spamsum/README, (last accessed in August 2013).[Google Scholar](https://scholar.google.com/scholar?q=spamsum, http:www.samba.orgftpunpackedjunkcodespamsumREADME, .)

- [Stringhini et al., 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib37)

  G. Stringhini, T. Holz, B. Stone-Gross, C. Kruegel, G. Vigna**Botmagnifier: locating spambots on the internet**USENIX security symposium (2011)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Botmagnifier%3A locating spambots on the internet&publication_year=2011&author=G. Stringhini&author=T. Holz&author=B. Stone-Gross&author=C. Kruegel&author=G. Vigna)

- [Symantec Intelligence Report, 2013](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib38)

  Symantec Intelligence Report**Report**(May 2013)http://www.symantec.com/content/en/us/enterprise/other_resources/b-intelligence_report_05-2013.en-us.pdf[Google Scholar](https://scholar.google.com/scholar?q=Report)

- [Thonnard and Dacier, 2011](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib39)

  O. Thonnard, M. Dacier**A strategic analysis of spam botnets operations, in: proceedings of the 8th annual Collaboration**Electronic messaging, anti-abuse and spam conference, ACM (2011), pp. 162-171[CrossRef](https://doi.org/10.1145/2030376.2030395)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-80053642264&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=A strategic analysis of spam botnets operations%2C in%3A proceedings of the 8th annual Collaboration&publication_year=2011&author=O. Thonnard&author=M. Dacier)

- [Wei et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib40)

  C. Wei, A. Sprague, G. Warner, A. Skjellum**Mining spam email to identify common origins for forensic application**Proceedings of the 2008 ACM symposium on applied computing, ACM (2008), pp. 1433-1437[CrossRef](https://doi.org/10.1145/1363686.1364019)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-56749156896&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Mining spam email to identify common origins for forensic application&publication_year=2008&author=C. Wei&author=A. Sprague&author=G. Warner&author=A. Skjellum)

- [Wei et al., 2009](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib41)

  C. Wei, A. Sprague, G. Warner, A. Skjellum**Characterization of spam advertised website hosting strategy**Sixth conference on email and anti-spam, Mountain View, CA (2009)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Characterization of spam advertised website hosting strategy&publication_year=2009&author=C. Wei&author=A. Sprague&author=G. Warner&author=A. Skjellum)

- [Xie et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib42)

  Y. Xie, F. Yu, K. Achan, R. Panigrahy, G. Hulten, I. Osipkov**Spamming botnets: signatures and characteristics**ACM SIGCOMM Comput Commun Rev, 38 (4) (2008), pp. 171-182[CrossRef](https://doi.org/10.1145/1402946.1402979)[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-58449122201&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Spamming botnets%3A signatures and characteristics&publication_year=2008&author=Y. Xie&author=F. Yu&author=K. Achan&author=R. Panigrahy&author=G. Hulten&author=I. Osipkov)

- [Zhuang et al., 2008](https://www.sciencedirect.com/science/article/pii/S1742287615000079#bbib43)

  L. Zhuang, J. Dunagan, D.R. Simon, H.J. Wang, I. Osipkov, J.D. Tygar**Characterizing botnets from email spam records**LEET, 8 (2008), pp. 1-9[View Record in Scopus](https://www.scopus.com/inward/record.url?eid=2-s2.0-70349699173&partnerID=10&rel=R3.0.0)[Google Scholar](https://scholar.google.com/scholar_lookup?title=Characterizing botnets from email spam records&publication_year=2008&author=L. Zhuang&author=J. Dunagan&author=D.R. Simon&author=H.J. Wang&author=I. Osipkov&author=J.D. Tygar)

- [☆](https://www.sciencedirect.com/science/article/pii/S1742287615000079#baep-article-footnote-id9)

  The authors gratefully acknowledge support from the Canadian Radio-television and Telecommunications Commission (CRTC).

[View Abstract](https://www.sciencedirect.com/science/article/abs/pii/S1742287615000079)

Copyright © 2015 The Authors. Published by Elsevier Ltd.