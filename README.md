# covid-coauthorship-network

## Overview

This repository contains project work for UT Austin's Dr. Ying Ding. It uses from data from the [LitCovid](https://www.ncbi.nlm.nih.gov/research/coronavirus/) (Chen Q, Allot A, Lu Z. "Keep up with the latest coronavirus research". *Nature*. 2020;579(7798):193) and the [PubMed Knowledge Graph](http://er.tacc.utexas.edu/datasets/ped) (Xu J, Kim S, Song M, et al. "Building a PubMed knowledge graph." *Sci Data*. 2020 Jun 26;7(1):205). The manipulation of this data and the application of these algorithms was done entirely in Python using Jupyter Notebooks. 

### Contibutors

- Alexander Reese
- Redoan Rahman

## Problem Statement

Since the emergence of the COVID-19 coronavirus in late 2019, scientific research into and, accordingly, publication on COVID-19 (and other coronaviruses) has skyrocketed and taken center stage in many medical and scientific subfields and research communities. An analysis of this data offers computational social sciences a chance to observe the quick emergence and evolution of a scholarly network.

## Project Summary

This project compares the process and outputs of two different community detection methods on a newtwork graph: [Infomap](https://www.mapequation.org/infomap/) clustering and unsupervised [GaphSAGE](https://snap.stanford.edu/graphsage/) node embeddings using [t-SNE](https://lvdmaaten.github.io/tsne/) dimensionality reduction on a homogenous, undirected network graph of scientific authorship and collaboration. It is shown that the underlying algorithms produce qualitatively different results and insights into the organization of a system.

## Data Set: LitCovid

https://www.ncbi.nlm.nih.gov/research/coronavirus/

The LitCovid dataset contains profiles of the 623,973 authors who have written 173,635 articles in PubMed related to COVID-19 and other coronaviruses. Each author record includes author URI, names, emails, country (of residence presumably), concatenated publication codes for COVID-19 publicactions, codes for "other coronavirus" publications, and codes for all other publications.

At the time of this project, the LitCovid dataset represented 201,242 (and growing) international COVID-19 related articles in PubMed (Chen et al., 2020). However, this project uses an earlier instance of the dataset containing only 66,401 publications representing 300,644 authors.

## Approach

### Data wrangling

The LitCovid author data (for 300,644 authors), as described above, must be transformed into an edge list and network graph with viable node and edge attributes. 

The author data is first imported (from a CSV) to Pandas dataframe. The individual author records are then parsed to create a dictionary of publications   as keys with values containing nested dictionaries of author records witht their keyed identifiers, name, country, and “Total Publications on Coronavirus” attributes. Authors without COVID-19 specific publications (as opposed to papers on other coronaviruses) are eliminated in this process. The resulting dictionary of publications is then further parsed, comparing the author identifiers to one another within each publication’s internal dictionary of authors. This generates a new list of keyed author-to-coauthor connections while also recording the total number of authors involved in each paper – to be used as an edge attribute. The resulting list of nested dictionaries is stored as a dataframe and represents an edge list from which a graph object can be created. Each record contains an authorID and a coathorID, representing a collaborative node-to-node conection or "an edge" and as part of a publication object or event. 

The single edge attribute produced is AuthorNum: the total number of authors in the publication involved in the publication event. 

The node attributes retained for each author include Name, Country, and PubNum (the author's total Coronavirus publications). 

### Graph creation

Using the NetworkX Python Package, a graph was created from this coauthor edge list by identifying the source (author identifier) and target (coauthor identifier). This is a graph with 260,637 nodes (authors) and 2,024,546 edges. Infomap is applied using the iGraph package, but I started with a NetworkX graph for two reasons – the first being that the graph created with iGraph directly from the DataFrame edge list produced undesirable Infomap results and the second being that the use of a NetworkX graph allows us to visualize subgraphs of individual communities identified by Infomap analysis.
Because the Infomap algorithm does not take into account any node or edge attributes, they need not be added. However, adding node attributes does facilitate the labeling of NetworkX community subgraph visualizations. To add node attributes, dictionaries of author keys and node attribute values (name, country, publication number) need to be created form the author list and added to the graph one by one.

### Infomap community detection.

The NetworkX graph can then easily be converted to an iGraph graph object, and Infomap community detection can be run with a single line of Python code. Designating four (4) for the trials parameter yields 23,825 clusters. The modularity attribute of this clustering is around 0.871 – sufficiently high to indicate these clusters are meaningful and tightly knit. These clusters can be helpfully visualized with a simply matplotlib bar chart with community id on the x axis and number of nodes (authors) on the y, as seen below.

![Infomap community size plot](assets/infomap_community_size_plot.png)

One can then store the membership of each node, creating a dictionary with node keys and community id values. This dictionary will be used later to color the t-SNE plot of GraphSAGE embeddings to visually compare the clustering methods. This dictionary of Infomap community membership can also be applied back to the NetworkX graph object as a node attribute. A single community “subgraph” of a nodes can then be selected, extracted, and visualized using NetworkX, showing nodes with designated labels and their connections to one another, as seen in Figure 2. However, the nodes are positioned randomly; their proximity to one another is not informed by network structure or attributes.

### Future Work

Enhancing the dection of these scientific authorship communities would invole the consideration of more feautres - information about both papers and authors – i.e. language of publication, citation information, etc. 
