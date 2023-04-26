---
layout: post
title: SQL Query Recommendation Systems
image: 
  path: /assets/img/weekly-review/sql_recommenders.jpg
categories: [papers, recommenders]
description: >
    An Extensive Overview of the Existing SQL Query Recommendation Systems
sitemap: false
hide_last_modified: true
---



<style>
dl {
    list-style-type: none;
    position: relative;
}

dl:before {
    content: ' ';
    background: #000000;
    display: inline-block;
    position: absolute;
    left: 29px;
    width: 2px;
    height: 100%;
    z-index: 400;
}

dl > dt:before {
    content: ' ';
    background: white;
    display: inline-block;
    position: absolute;
    border-radius: 50%;
    border: 3px solid #000000; 
    left: 20px;
    width: 20px;
    height: 20px;
    z-index: 400;
}

dl >dt, dl > dd {
    margin: 6pt 0; /* was 20px */
    padding-left: 60px;
}

.paperTitle {
    color: #aaa8a8;
    font-weight: bold;
    font-size: 20px;
}

</style>

<dl>
<dt>2009</dt>

<dd class="paperTitle">Recommending Join Queries via Query Log Analysis</dd>

<dd> In this pioneering paper, the authors designed a system for recommending join predicates&#8212;including join tables and join conditions. The sources of such recommendations are query logs and database schema. This system receives two inputs; input specification&#8212;the tables used in <tt>WHERE</tt>, and output specification&#8212;attributes in <tt>SELECT</tt> clauses. Then, given these two specifications as the system's input, the system generates a join query graph based on which it recommends join predicates. Therefore, the output of the system is a join query graph. The authors evaluated their work on a dataset named 'AT&T Proprietary Database,' which is not publicly available. The evaluation metric is the accuracy of the suggested join tables.</dd>

 <dd>However, this work has several critical limitations. First, the recommendations are based on a static join query graph that does not update by the addition of new data to the database. Second, it only recommends the join tables and conditions without focusing on the query structure or any other query parts. Third, it is neither session-aware nor sequence-aware.</dd>


<br>

<dd class="paperTitle">Interactive Query Refinement</dd>

<dd> The main focus of this paper is to address the <i>many/few answers</i> problem. This problem states that, while submitting queries to databases, for some queries, too many or very few tuples are returned. In such situations,  the proposed innovative model aids users in refining their queries to return a reasonable number of tuples. Specifically, this model receives a query as input and returns a refined set of recommendations for adjusting the ranges of <tt>WHERE</tt> attributes. The recommendations are according to the database schema and data. However, the challenge is that multiple ways exist for narrowing down or expanding the ranges of <tt>WHERE</tt> attributes. Thus, in this work, the policy used for choosing a proper way to refine the ranges is to interact with users. </dd>

<dd>The limitations of this work can be outlined as follows: first, it may require resubmitting queries, which can cause a heavy workload for most databases. Second, it only recommends the ranges of <tt>WHERE</tt> attributes without considering other aspects of the query structure. Third, the refinement process heavily relies on user involvement. Fourth, it does not take user sessions or the sequence of queries in each session into account for recommendations.</dd>


<br>

<dt>2010</dt>

<dd class="paperTitle">SnipSuggest: Context-aware autocompletion for SQL</dd>

<dd>SnipSuggest is a context-aware auto-completion system for SQL queries. In this case, context-aware denotes that the suggestions given by the system depend on the query written thus far. The main approach in this insightful work is as follows; first,  a Directed Acyclic Graph (DAG) is created based on the log of submitted SQL queries, which is called <i>workload DAG</i>. Subsequently, depending on what the user is typing, the nodes of the workload DAG are ranked according to the probability of their appearance in the continuation of the query. Finally, the most probable nodes are provided as suggested fragments to the user. The recommendations are generated based on query log files. This model is evaluated on Sloan Digital Sky Survey (SDSS) dataset. The evaluation metric is the accuracy of the recommended fragments.
</dd>
<dd>The limitations of this approach can be summarized as follows: first, the recommendations rely on a static DAG that is created only once and not updated as new data is added to the database. Second, the system does not consider user sessions or the sequence of queries within each session for generating recommendations.</dd>


<dt>2011</dt>

<dd class="paperTitle">Interactive SQL Query Suggestion: Making Databases User-friendly</dd>

<dd>In this paper, the authors proposed an SQL recommendation system, named SQLSugg, which takes the current partial query written thus far as input; then, as output, the system recommends the fragments for the same query. The suggestions are based on database schema and data. The approach has two steps: (a) an offline step, in which two sets of graphs&#8212;schema and data graphs, known as templates&#8212;are created and indexed. (b) an online step in which users type the query keywords. Then, the keywords are mapped to database attributes. Based on the matching between keywords and attributes, the most relevant schema graphs are selected. These selected graphs are ranked based on the number and relevance of the matched data graphs. The model is evaluated on two datasets: (1) DBLP, a dataset of publication records, and (2) DBLife, a dataset of activity information of top people in the database community. To evaluate their approach, they asked experts to score the relevance of their suggestions. 
</dd>
<dd>
However, this approach has several critical limitations, including its reliance on having a static database schema and data, failure to consider user sessions, and neglect of the sequence of queries in each session.
</dd>


<dt>2012</dt>

<dt>2013</dt>

<dd class="paperTitle">QueRIE: Collaborative Database Exploration</dd>

<dd> In this paper, the authors proposed a model, named QueRIE, which takes a full query from users as input. Then, as output, it selects the most probable query in the log file and returns it as the recommended next query. This work is evaluated on the SDSS dataset. Notably, this work considers the concept of <i>user sessions</i>; to do so, it leverages the queries within each session to create a vector representation for each session. Then, by comparing these vectors, it can identify similar user sessions. 
</dd>

<dd>
The limitation of this work is that it does not consider the sequence of queries in each user session for recommendations
</dd>

<dt>2014</dt>

<dt>2015</dt>

<dt>2016</dt>

<dd class="paperTitle">Cluster-driven Navigation of the Query Space</dd>

<dd> Blaeu is an interactive system that facilitates data exploration and refinement. As input, Blaeu takes the users' initial query and clusters the data based on the result set of the query. The system then presents interactive cluster maps to users, allowing them to navigate and zoom into areas of interest. Moreover, Blaeu provides users with a query that can be used for selecting that area of data. The authors proposed several algorithms for creating the data clusters and evaluated their accuracy on two datasets: (1) US Bureau of Transportation Statistics, which describes delays of US internal flights during January 2010. (2) Hollywood films, which describes a few economic indicators for 785 movies released between 2007 and 2012. 
</dd>

<dd>
The limitations of this work are: first, generating data maps requires resubmitting queries, which can be computationally expensive for large databases. Second, users need to have a high level of involvement in the process. Third, it does not consider user sessions and sequence of queries for recommendation.
</dd>

<dt>2017</dt>

<dt>2018</dt>

<dt>2019</dt>

<dd class="paperTitle">ExplIQuE: Interactive Databases Exploration with SQL</dd>

<dd> ExplIQuE is a framework for query refinement recommendations to assist users in improving their queries. The model takes a full query as input and produces clustered result sets, accompanied by the <tt>WHERE</tt> clauses needed to retrieve each cluster. The recommendations are based on data records rather than logs and database schema. The system is evaluated on a dataset for bacteria growth on solid plates. 
</dd>

<dd>
The limitations of this work can be summarized as follows: the approach presented is only applicable when the database schema and data are static and do not change. Furthermore, the recommendation system does not consider user sessions or the sequence of queries in each session for recommendations.
</dd>

<dt>2020</dt>

<dt>2021</dt>

<dd class="paperTitle">PyExplore: Query Recommendations for Data Exploration without Query Logs</dd>

<dd> PyExplore is a framework that takes users' initial query with <tt>WHERE</tt> clause. Then, as output, it recommends a query with refined <tt>WHERE</tt> clause based on the data. The method involves measuring the correlation between attributes in the database and dividing them into several groups. Each group is then represented by one attribute, and a decision tree is created per representative attribute. Each node in each of these decision trees defines the split point of the corresponding attribute at that level. Then, the data rows of the database are clustered based on the leaves of the created decision tree. Thus, when users type an input query, the input query is mapped to a node of the decision tree. By navigating from the mapped node to the root, sample data is selected for each node and recommended to the user. The framework is evaluated on several datasets: CORDIS, SDSS, Movies, Car Sales (IBM), Intel Lab Data. For measuring the performance, expert users are asked to score the recommendations. 
</dd>

<dd>The limitations of this work include the assumption that the database schema and data are static and never change, the absence of user session and sequence information for recommendation.
</dd>


<br>
<dd class="paperTitle">Scalable and Data-aware SQL Query Recommendations</dd>

<dd> 
In this inspiring paper, the authors provide a data-aware query recommendation system, called DASQR, capable of suggesting complete queries, query templates, and query fragments. The system is considered data-aware as it takes into account actual data values when recommending filtering conditions and predicates. In this work, the methods used for query representation are (1) feature-based, (2) tuple-based, and (3) access-area-based. The similarity between queries is evaluated based on the utilized representation approach.  In case of using the first and second representation approach, cosine similarity is used as the metric. In case of using the third representation approach, two metrics&#8212;named overlap and closeness&#8212;are proposed for evaluation.
</dd>

<dd> The limitation of this work is that it does not consider the sequence of queries in each user session for recommendations.
</dd>

<dt>2022</dt>

<dt>2023</dt>

<dd class="paperTitle">1) Sequence-Aware Query Recommendation Using Deep Learning</dd>
<dd class="paperTitle">2) Workload-Aware Query Recommendation Using Deep Learning</dd>

<dd> 
In these insightful papers, an innovative recommendation system is introduced, which takes a full query as input and recommends the templates and fragments for the next query as output. This system is capable of providing sequence-aware and session-based suggestions at both the query fragment and query template levels. For the query fragment level, they exploited several models based on the encoder-decoder architecture&#8212;such as Sequence-to-Sequence (Seq2Seq) CNN, Seq2Seq RNN, and transformers&#8212;to predict the next query; then, the predicted query is parsed to extract its fragments. Also, for the query template level, the same Seq2Seq models are used as a classification task. In both levels, one-hot encoding is leveraged as the way of query vectorization. They have evaluated their methods on two open-source datasets&#8212;SDSS and SQLShare. The evaluation metrics for fragment prediction are precision&#8212;which equals the number of correct fragment predictions over the number of total fragment predictions, and recall, which equals the number of correct fragment predictions over the number of total target fragments. The evaluation metric for template prediction is prediction accuracy. A noteworthy advantage of this system is that it is session-aware and sequence-aware.</dd>

<dd> However, a critical limitation of this work is the utilization of one-hot encoding for embedding queries, which does not consider word similarities in queries and consequently leads to reduced performance. </dd>

</dl>