---
title:  Project Checkpoint 2
author: Ittoop Shinu Shibu, Udaikaran Singh, Wesley Kwan
geometry: margin=3cm
---

- For Checkpoint 2:
	- All members - performed EDA
	- Udai: Performed EDA on smaller datasets & transformed in graphs.
	- Shinu: Cleaned and performed EDA on datasets
	- Wesley: Created Baselines for future model testing

\pagebreak

# Problem Statement

...**Wesley**

# Datasets

For this project, we are going to be using a large dataset from [Kaggle](https://www.kaggle.com/c/kdd-cup-2013-author-disambiguation/rules) and four smaller datasets from [Leipzig Universities Database Department](https://dbs.uni-leipzig.de/research/projects/object_matching/benchmark_datasets_for_entity_resolution). The goal is to analyze the smaller datasets from Leipzig university in order to verify the validity of our implementation, as well as resolve the glaring issues that may arise within the model.

# Author Disambiguation Dataset

The Author Disambiguation Dataset from [Kaggle](https://www.kaggle.com/c/kdd-cup-2013-author-disambiguation/rules) is a group of five datasets that were provided by Microsoft Corporation from their Microsoft Academic Search Database in order to try to find ways to solve disambiguation problems present in datasets.

The five datasets are:
1. Author dataset (Author.csv)
This dataset contains information on publication authors in the Academic Search Database. It contains Author ID, name and affiliation of the author.
Number of instances - 247,203
2. Paper dataset (Paper.csv)
The paper dataset contains information on the papers available in the Academic Search Database. It contains Paper ID, Title of the paper, Year published, Conference ID, Journal ID and keywords. The dataset is very noisy with the possibility of the same paper appearing multiple times in the dataset as it may have been obtained through different data sources.
Number of instances - 2,257,249
3. Paper-Author Dataset (PaperAuthor.csv):
The paper author dataset contains paper id and author id pairs. The dataset is very noisy as it contains possibly incorrect paper-author assignments due to author name ambiguity and variations of author names.
Number of instances - 12,775,821
4. Journal and Conference dataset (Journal.csv, Conference.csv):
These datasets contain additional metadata on the papers dataset. Each paper can be either a conference or journal and some of the paper may not be associated with either.
Number of instances in the journal dataset - 4,545
Number of instances in the conference dataset - 15,151

The paper, author and paper-author datasets, that are very important for our problem, varying degrees of missingness for each of its columns.

![author-dataset-null](./images/author-dataset-null.png)
As shown by the above bar chart, the authors dataset has the highest number of missing values(60%) in the affiliation column. The name column comes in second with only 7 percent of its data missing. The ID column has no missing values.

![paper-dataset-null](./images/paper-dataset-null.png)
The paper dataset has the highest number of missing values(76%) in the keyword column. The title column has the second highest number of missing values with 7% of its data missing. The other columns do not have any missing values.

![paper-author-dataset-null](./images/paper-author-dataset-null.png)
The affiliation column in the paper-author dataset has the highest number of missing values(65%). The only other column in the dataset that has any missing values is Name. Only 0.002 percent of the Name attribute is missing.

The missing values in the columns will play large part as more missing values result in it being harder to classify a link between records.


# Smaller Datasets

## Introduction

The four smaller dataset that we will be looking at are from [Leipzig Universities Database Department](https://dbs.uni-leipzig.de/research/projects/object_matching/benchmark_datasets_for_entity_resolution). Each dataset is sampled from real world examples and the noise in each one varies differently. These datasets are primarily used as benchmarks for testing different entity resolution techniques hence, they will be ideal to evaluate our model.

## Abt-Buy

This dataset is two seperate datasets of with 1081 and 1092 instances of purchasable items with names and description, but without a clear link between the two datasets. Most of the data within this dataset is textual, apart from a price column. However, the price column is the most dirty column in that it has NaN values. There is also an issue with the fact that the description column within the Buy dataset seems to also have many missing values. This intuitively gives means that the meaningful connections must be made between the description and the name columns.

| About Table NaN's      | Buy Table NaN's          |
| ------------- |:-------------:|
| ![About Table](./images/about_table_nan.png){ width=30% }     | ![Buy Table](./images/buy_table_nan.png){ width=30% }|

There is also some abiguity within the dataset. For example, there is a few examples within the dataset in which multiple entities are matched. Within this case, we want our recording linkage algorithm to be able to find that the entities are similar enough rather than being confused by the small differences. In other words, we want our record linkage algorithm to be invariant to such noise within the data.

![Example of Ambiguity within the dataset](./images/abt_buy_counter_example.png)

...**Wesley**

## DBLP-ACM

...**Wesley**

## DBLP-Scholars

...**Wesley**

## Amazon-Google Products

...**Wesley**

## Graph Descriptions

![Degree Frequency across 4 datasets](./images/node_frequency_all_four.png){ width=50% }

After creating the heterogenous graphs, we looked into describing the properties of the graph's nodes. We looked into finding descriptive statistics on each of the datasets seperately, as well as a unified analysis between the four datasets. The four datasets are considerably smaller than the large dataset we will work with. However, they are also very similar to the large dataset we will be using in that they are very heavy on textual data. This will allow us to test our model on a scaled down version of the problem, which will allow us to iterate quickly before testing on the larger dataset, which we anticipate to be computationally much more expensive.

\pagebreak

### Abt-Buy Dataset

|Graph Degree Frequency|Graph Edge Frequency  |
| :-------------: |:-------------:|
| ![About Table](./images/abt_buy_degree_dist.png){ width=50% }     | ![Buy Table](./images/abt_buy_edge_frequency.png){ width=50% }|

Within this dataset, we found that this seems to be relating electronic products with based on a name and description. This can be seem above, which shows that there is a high cardinality within the name and description embeddings. Also, as you can see below, there seems to be some overlap within the words for the columns. We can use this heuristic when refining our graphs to match overlapping words. For example, if the word 'black' occurs within the name of 1 entity, as well as the description of the a 2nd entity, we want our model to detect this link. In a probabilistic sense, this related term within the two entities should be detected as improving the chances of a matching entity. Lastly, it seems that this dataset will primary test the ability of the model to detect common words between columns using the graph embedding.

|Most common words in Description|Most Common Words in Manufacturer  |
| :-------------: |:-------------:|
| ![Some Title](./images/adt_buy_most_common_descriptions.png){ width=13% }    | ![Some Title](./images/adt_buy_most_common_manus.png){ width=20% }|


### DBLP-ACM & DBLP-Scholars

|DBLP-ACM Graph Degree Frequency|DBLP-ACM Graph Edge Frequency  |DBLP-Scholars Graph Edge Frequency|DBLP-Scholars Graph Degree Frequency |
| :-------------: |:-------------:|:-------------: |:-------------:|
| ![About Table](./images/dblp_acm_graph_degree_dist.png){ width=100% }     | ![Buy Table](./images/dblp_acm_graph_edge_frequency.png){ width=100% }| ![Buy Table](./images/dblp_scholar_edge_frequency.png){ width=100% }| ![About Table](./images/dblp_scholar_degree_dist.png){ width=100% }|

These are two related datasets with the task of relating an author with a paper. The primary difference within the two datasets is the importance placed on the decription compared to the other node types. As you can see from above, the DBLP-Scholars dataset has relatively more information stored within the venue and year columns compared to the DBLP-ACM dataset. These datasets are, in terms of content and statistics, the related to the large dataset we will work with. This means that this will likely act as the primary dataset we will like to test on before scaling up to the large datast.


### Amazon-Google Products

|Graph Degree Frequency|Graph Edge Frequency  |
| :-------------: |:-------------:|
| ![About Table](./images/amazon_google_degree_dist.png){ width=50% }     | ![Buy Table](./images/amazon_google_edge_frequency.png){ width=50% }|


This dataset is based on relating the product descriptions between the product descriptions within Google and Amazon. This looks similar to the Abt-Buy dataset, in that they are both heavily on textual data and are looking at a related topic of technology. As you can see below, our textual embedding has found nodes with common electronic terms. Also, this seems to have limited stop words, which means that our embedding looks to be meaningful in service of the task of characterizing an item within the dataset.

![Example Amazon Description](./images/amazon_descriptions.png){ width=50% }


\pagebreak

### Revision to the Proposal

We decided to change our approach to the record linkage problem as we were getting a really high accuracy from our baseline model for our donated dataset from Epidemiologisches Krebsregister NRW. The main problem with the datasets we used was that it was already cleaned, so we could not make our data more complex if we needed to, and it limited our ability to extract any additional information in order to improve our model. We decided to explore other datasets that have more noise and have features that need to undergo cleaning, thus, are more representative of the real world record linkage problem.

### Backlog:

- For Checkpoint 1:
	- All members - revised proposal
	- Udai: Create code for generating datasets with strong keys (and varying noise)
	- Shinu: Clean and perform EDA on our real-world datasets
	- Wesley: Finalize Structure of our Heterogenous Graphs
		- determine method for representing graphs (on disk)
		- determine method for using quantitative attributes.

- For Checkpoint 2:
	- Generate Heterogenous Graphs (Udai)
	- Train baselines Models (Wesley)
		- perform tests over a subset of generated datasets
	- Clean real dataset and transform in a usable format (Shinu)
	- Stretch Goal: Train/Evaluate Hindroid Model using our data (everyone)

- For checkpoint 3:
	- Generate Heterogenous graphs for Author Disambiguation dataset. (Shinu)
	- Train and evaluate Hindroid model on the datasets. (Everyone)
	- Finalize elevator pitch (Everyone)
	- Finalize visual presentation (Everyone)