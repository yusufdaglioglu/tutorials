############################

############################
# ALGORITHMS DATA STRUCTURES GRAPH
############################

############################

# çizge (or graph or tr:graf or çizit)
- Any node may have a connection to any node (even circular).
- For example: A node can be connected to 10 other nodes. In the same graph another node may connect only 1 node.

Some terms:
- __vertex (plural vertices) or node (or düğüm)__: vertex; "tepe noktası" anlamına gelir.
- __edge__: Each connection of a specific node.
- __Order__: The number of vertices in the graph.
- __Size__: The number of edges in the graph.
- __vertex degree (or düğümün derecesi)__: number of edges of a vertex.
- __isolated vertex__: a vertex which does not have any connections(edges) to other vertices.
- __self loop__: an egde from vertex to itself.
- __undirected graph__: In this type of graph there is no direction information for edges. In another word; if X node have connection to Y node, Y node also must have connection to X. The opposite graph type is __directed graph__ which has direction information.
- __komşuluk matrisi (or adjacency matrix)__: This takes more memory. But it is fast to find if the connections.

```
   A B C

A  0 1 0

B  1 1 1

C  0 1 1
```

- __komşuluk listesi (or adjacency list)__: Requires low memory. But it takes long time to finding the relations between nodes. Also it is more complex comparing "adjacency matrix".

```
A -> {B}

B -> {C, D}
```

The above "adjacency list" means:
- There is a connection from A to B.
- There is a connection from B to C and D.

# real use case examples of graph
- storing the pyhsical border connections between countries and cities.
- Storing the list of friend connections on social media web sites
- connections of between atoms/molecules (for simulation)
- connections between devices for electronic circuit (for simulation or on engineering design tools)
- connections of each part on puzzle game
- Google's PageRank algorithm. In this algorithm, Google stores which web page has linked by which web pages. If a web page has reference (link) inside other web pages, that means the web page reputation is high.
- network topology. It seems like, the network is based on tree structure. But it's not. Because of 2 reasons:
  - the child nodes (the computers) can send directly message to each other. They use the router (parent) only to detect the IP address of each other.
  - A computer (node) can has multiple routers (parents) if the computer use multiple network devices.