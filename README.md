Overview
--------

Communities play an important role throughout the modern world and arise in numerous scenarios ranging from traffic monitoring to identifying criminal activity cells in social networks. Most interconnected datasets exhibit clusters of strongly interconnected data. Our parallel Louvain algorithm helps uncover accurate communities many times faster than the original sequential code. This makes it suitable for environments where data is distributed geographically besides centralized high performance clusters. 

Objectives
----------
The objectives of this algorithm are two folded: 
*	Quickly unfold highly accurate communities on large potentially distributed graphs 
*	Allow a hierarchical view of the communities which enables several levels of detail depending on the needed granularity, e.g., individual level, city level, country level

Benefits
--------

The algorithm is beneficial to data scientists seeking fast community discovery and analysis on geographically distributed datasets at various levels of detail.

Measures of effectiveness
-------------------------

The algorithm has been tested using MPI on an HPC cluster consisting of 16 nodes with 8 cores per node. Each node consisted of two Quad-core AMD Opteron 2376 2.3GHz processors. Speed-ups of up to 6x have been observed for synthetic community graphs consisting of up to 16M vertices and 60M edges.

Future enhancements
-------------------
Given the wide spread adoption of the cloud based frameworks a Map Reduce version of the proposed algorithm will be delivered.


Installation
------------

A quick start guide can be found [here](QuickStart.md) together with a precompiled VM to help you get started.

A detailed guide on how to install the software on a distributed setup can be found [here](/path/to/Louvain-install).
