---
description: >-
  Martin, H., Wiedemann, N., Reck, D. J., & Raubal, M. (2023). Graph-based
  mobility profiling. Computers, Environment and Urban Systems, 100, 101910
---

# Graph-based Mobility Profiling

[Annotated Link](https://drive.google.com/file/d/1Ulad1fG3gNsp4yHa-iLuNGg9uOqXk6LL/view?usp=share\_link)

## Abstract

The decarbonization of the transport system requires a better understanding of human mobility behavior to optimally plan and evaluate sustainable transport options (such as Mobility as a Service). Current analysis frameworks often rely on specific datasets or data-specific assumptions and hence are difficult to generalize to other datasets or studies. In this work, we present a workflow to identify groups of users with similar mobility behavior that appear across several datasets. Our method does not depend on a specific [clustering algorithm](https://www.sciencedirect.com/topics/computer-science/clustering-algorithm), is robust against the choice of hyperparameters, does not require specific labels in the dataset, and is not limited to specific types of tracking data. This allows the extraction of stable mobility profiles based on several small and inhomogeneous tracking data sets. Our method consists of the following main steps: Representing individual mobility using location-based graphs; extraction of graph-based mobility features; clustering using different hyperparameter configurations; group identification using statistical testing. The method is applied to six tracking datasets (Geolife, Green Class 1 + 2, yumuv and two Foursquare datasets) with a total of 1070 users that visit about 3′000’000 different locations with a total tracking duration of over 200′000 days. We can identify and interpret five mobility profiles that appear in all datasets and show how these profiles can be used to analyze longitudinal and cross-sectional [tracking studies](https://www.sciencedirect.com/topics/earth-and-planetary-sciences/tracking-position).

## Methodology Summary&#x20;

### Step 1: Graph Representation&#x20;

* Use DBSCAN with 1 sample and 30m or existing label to identify locations&#x20;
* Locations are nodes, edge weight is the number of sequential visit from one node to another&#x20;

### Step 2: Graph-based Mobility Feature Extraction&#x20;

6 features are developed&#x20;

1. Node degree parameter: $$\hat{\delta}_i = i ^{-\beta}$$. Delta hat is the normalized node degree (division by largest), and i is the rank. The parameter is beta. High value means that user behavior is centralized to few base locations&#x20;
2. Transition count parameter: similar to node degree using power law distribution. High transition means that most of the trips happen between same set of locations&#x20;
3. Median distance: Distribution of haversine distance weighted by transition count&#x20;
4. 9th decile trip distance
5. Average journey length: Starting a random walk from home for 5000 step, journey length is number of steps between home visit.&#x20;
6. Hub size: The number of nodes required to account for at least 80% of total visit. Total visit is obtained through 5000 steps random walk. The number of nodes is than normalized by the square root of the total number of location (graph size). Similiar to PageRank.&#x20;

Mapping between theory and features

* Role of home base:&#x20;
  * Does a person have a single home base? (1, 6)
  * How home-centered is the person's behavior? Does he return home after each activity or rather move from place to place (1, 5)&#x20;
* Complexity&#x20;
  * Are the activities of the person focused on few locations and trips or distributed over many? (1, 2, 6)&#x20;
  * Are most trips of the user between the same locations (2)&#x20;
* Regularity and geometry&#x20;
  * Is the user flexible, or does he have a very regular mobility behavior? (5, 2)&#x20;
  * How far does a user usually travel? (3, 4)&#x20;

### &#x20;Step 3: Identifying User Groups&#x20;

1. Run KMeans ++ algorithm with $$k \in \{6, 7, 8, 9\}$$. Each $$k$$ is run 3 times with different initialization. So total of 12 runs.&#x20;
2. Each cluster in each run, the actual cluster is represented by a vector of length 6 (feature space). The values are whether that feature is lower, same, or higher than the rest of the cluster in the same run.&#x20;
3. Across all runs, clusters are grouped into the same user group if they feature value doe not contradict each other (i.e., lower vs. higher) and share at least two significant features. In practice this is done iteratively.&#x20;
4. Users are grouped into user group based on most frequent user group that the users' cluster result belong to.&#x20;

## Datasets&#x20;

In general those datasets involve hundreds of users tracked for months.&#x20;

1. Yumu
2. Green Class
3. Geolife&#x20;
4. Foursquare

## Result&#x20;

### User Group

By looking at mean feature values at group level and cross group standard deviation, 5 user groups are identified: commuter, traveler, flexible, local routine, centered.  Figure 5 gives feature distribution across groups.&#x20;

* Commuter: high median distance
* Traveler: high 9th decile&#x20;
* Flexible: high hub size and journey length, shows distributed activity&#x20;
* Local routine: few trips and few nodes, low radius&#x20;
* Centered: single or few nodes with high degree and low journey length&#x20;

### Validation against tracking time&#x20;

1. Most of the graph features are mean stable across tracking period (4 to 28 weeks)
2. Not the case for other graph features or traditional mobility features

