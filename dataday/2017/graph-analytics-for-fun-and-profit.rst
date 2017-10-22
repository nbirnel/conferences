Gene by Gene / FamilyTreeDNA

"OLTP and OLAP in Gremlin"
transactional, analytic

Apache TinkerPop Gremlin

JedCom
======
JedCom  - "industry standard data structure for family trees" from LDS

tree owns-> individual
individual member_of:Husband-> family
individual is_known_as-> name


oltp depth first, lazy eval, real-time, low memory
olap bread first, eager eval, long-running, high memory
  parallel, so it might be used on Hadoop or Spark

Centrality Analysis
==================
"What is most important in a graph"
But you must define what 'important' means.

Degree Centrality: how many edges?

Eigenvector Centrailty: relative importance matters
(could be weighted, but without weight...)
I have 6 edges, so connecting to 

PageRank: similar to EigenVector Centrality, but with scaling.

Other Centrality algorithms: closeness, betweenness, Katz, Freeman

Path Analysis
=============
gremlin path object: name, vertices, edges, side effects

simplePath: shortest path
cyclicPath: allowed to hit vertices twice. "usefull for naive cluster detection"

Pattern Detection
=================
What's hidden?

gremlin can run imperative or declarative.

