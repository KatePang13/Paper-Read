# Clustering and classifification methods for spam analysis

origin :   https://pdfs.semanticscholar.org/90e2/5d15e1341d777cb140f06a5300c91143cceb.pdf

Author:  

Maksim Smirnov

Aalto University , School of Science ,Degree Programme in Computer Science and Engineering

## Abstraction



## Introduction and Scope

- 垃圾邮件危害巨大，垃圾邮件数量惊人，人工分析是不可行的。

- 垃圾邮件是海量的，本理论的主旨是使垃圾邮件的分析更简单，提供工具和方法来呈现和关联垃圾邮件，为分析海量垃圾邮件提供支持。

  - 机器学习和数据分析
    - 聚类
  - 可视化
    - 用图来表示聚类后的数据集

- 本文尝试回答一个问题：**聚类技术如何支撑对源源不断的垃圾邮件的分析**

  该问题可以划分成三个子问题：

  - What features can be used for clustering?  用哪些特征来聚类
  -  Does clustering spam emails provide better visibility over spam campaigns   对spam emails 聚类是否有利于发现  spam campaigns
  - How can the clustering results be visualized?  如何对聚类结果做可视化

## Background and related work

### Email

### spam

#### Collecting Spam

#### Spam Statistics

#### Spam Filtering Techniques

- SPF and DKIM
- Real Time Black Lists
- Grey Listing
- Razor and DCC
- spamassassin
  - https://spamassassin.apache.org/
- Bayesian filtering
  - http://dspam.sourceforge.net/

#### Filters Evasion Techniques

- Email forgery
- Hiding text in email body
- Generating subject and body text
- Generating URLs

### Clustering algorithms

- K-Means
  - https://en.wikipedia.org/wiki/K-means_clustering
  - https://blog.csdn.net/huangfei711/article/details/78480078
- DBSCAN
- Hierarchical Clustering

### Related work

- Hierarchical approaches
  - Frequent Pattern Tree  
    - [Spam campaign detection, analysis, and investigation](https://www.sciencedirect.com/science/article/pii/S1742287615000079#aep-article-footnote-id9)
  - Categorical Clustering Tree
    - Fast and Effective Clustering of Spam Emails based on Structural Similarity
  - hierarchical clustering and fixing false positives
    - Mining Spam Email to Identify Common Origins for Forensic Application
- Text based clustering approach
  - Clustering Spam Campaigns with Fuzzy Hashing

## Tool Requirements

- Functional Requirements
  - Clustering
  - Searching
- Non-functional Requirements
  - Performance
  - Visualization
  - Learning Curve

## Clustering Algorithms and Visualization

### 4.1 Feature selection for clustering

#### 4.1.1 Subjects



## Technical Implementation

## Evaluation

## Discussion

## Conclusions



 

