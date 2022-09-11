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

[https://www.ncbi.nlm.nih.gov/research/coronavirus/]

The LitCovid dataset contains profiles of the 623973 authors who have written 173635 articles in PubMed related to COVID-19 and other coronaviruses. Each author record includes author URI, names, emails, country (of residence presumably), concatenated publication codes for COVID-19 publicactions, codes for "other coronavirus" publications, and codes for all other publications.

At the time of this project, the LitCovid dataset represented 201,242 (and growing) international COVID-19 related articles in PubMed (Chen et al., 2020). However, this project uses an earlier instance of the dataset containing only 66,401 publications representing 300,644 authors.

## Approach

### Data wrangling

The LitCovid author data (for 300,644 authors), as described above, must be transformed into an edge list and network graph with viable node and edge attributes. 

The author data is firs imported (from a CSV) to Pandas dataframe. The individual author records are then parsed to create a dictionary of publications   as keys with values containing nested dictionaries of author records witht their keyed identifirs, name, country, and “Total Publications on Coronavirus” attributes. Authors without COVID-19 specific publications (as opposed to papers on other coronaviruses) are eliminated in this process. The resulting dictionary of publications is then further parsed, comparing the author identifiers to one another within each publication’s internal dictionary of authors – generating a new list of keyed author-to-coauthor connections while also recording the total number of authors involved in each paper (to be used as an edge attribute). The resulting list of nested dictionaries is stored as a dataframe and represents an edge list from which a graph object can be created. Each record representing an edge or collaborative node-to-node conection and essentially a publication. 

The single edge attribute produced is AuthorNum: the total number of authors in the publication involved in this collaboration. 

The node attributes retained for each author are Name, Country, and PubNum (the author's total Coronavirus publications). 

### Future Work

Enhancing the dection of these scientific authorship communities would invole the consideration of more feautres - information about both papers and authors – i.e. language of publication, citation information, etc. 
