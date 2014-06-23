General Installation Guide for Parallel Louvain Community Detection method
=====================================================================================

Pre-Requisites
--------------

* We have tested our system with Linux – Ubuntu/Debian variant (although all windows equivalent s/w are available and may be used)

* MPI runtime (preferable Open MPI 1.6 or higher)

 * On a single machine (Make sure no MPI )

<code>sudo apt-get -y install build-essential g++ python-dev autotools-dev libicu-dev libbz2-dev</code>

<code>sudo apt-get install libcr-dev mpich2 mpich2-doc</code>

 * Set up a MPI Cluster (see http://techtinkering.com/2009/12/02/setting-up-a-beowulf-cluster-using-open-mpi-on-linux/)
 

* Metis Graph Partitioner

<code>sudo apt-get -y install metis</code>	

Compile
--------

Once the prerequisite are installed, download the code from the git repository mentioned earlier.

Following is the strucutrue of the source code

<code>
parallel-louvain-modularity/

├── conf

├── data

│   └── graphs

├── opt

│   └── python

└── src

    ├── benchmark

    ├── parallel   //parallel louvain algorithm

    ├── ref

    ├── sequential //Sequential louvain algorithm

    └── tools


</code>
go to src/parallel directory and run $make to complile the source code

Execution
---------

You need to have a graph in metis format and edge list format. 
Assume following are sample graphs

*4elt.txt - graph in edge list format.

*4elt.graph - graph in metis graph format.

* First the given graph (in METIS format) needs to be portioned into desired number of partitions. For example

<code>gpmetis ./4elt.graph 2</code>

This will create a partition file 4elt.graph.part.2

Psuedo Distributed (single node):

Then you need to convert the graph using the conversion tool to binery format. This will create .bin and .remote file for each partition. .bin file contains the local graph for each partition while .remote files contain mappings of cross partition edges.

Example:

<code>./convert -i 4elt.txt -o 4elt -p 4elt.graph.part.2 -n 2</code>

General format is : 

<code>./convert -i <graph in edge list format> -o <output graph name> -p <partition file> -n <number of partitions></code>


This will create .bin files with format 4elt_<partition id>.bin and .remote files with format 4elt_<partition id>.remote.

Use the Open MPI’s mpi-run command to start the algorithm using the desired number of processors (NOTE: This should be equal to the number of partitions)

example:

<code>mpirun -np 2 ./community 4elt -r 4elt -l 2 -v > level2.txt</code>

the general format is: 

<code>mpirun -np <number of processors> ./community <graph name> -r <graph name> -l <output level> -v verbose > output file </code>


*Number of Processors: This is equal to the number of partitions

*Graph Name: Name of the output graph given in conversion tool

*output level: displays the graph of level k rather than the hierachical structure.
	if k=-1 then displays the hierarchical structure rather than the graph at a given level 

*verbose : verbose mode: gives computation time, information about the hierarchy and modularity.


On MPI Cluster
--------------

Run the same command as before from the Cluster’s head node.


Data Format
------------


We use the standard usnweighted METIS graph data format for input graphs. See http://glaros.dtc.umn.edu/gkhome/fetch/sw/metis/manual.pdf for details.

To summarize, the metis format of the following

1. First line indicating the number of vertices and edges

2. All subsequent lines contain the data per vertex and its (undirected) edges in following format

e1 e2 …

For example: The following undirected graph is represented using:

<img src="http://losangeles.usc.edu/usc-cloud/goffish/bc_dataformat.png"/>

Metis File:

4 5 //4: num vertices, 5: num edges

2 3 4  // e1 w1 e2 w2 e3 w3 for vertex 1

1 3 4

1 2

1 2


Edge List file is just a standered undirected list of edges. Each line corresponds to an edge in metis formated graph. Same vertex ids must be used in both files.

Following is the edge list for above graph

1 2

1 3

1 4

2 1

2 3

2 4

3 1

3 2

4 1

4 2







