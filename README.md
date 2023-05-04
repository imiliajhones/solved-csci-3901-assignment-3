Download Link: https://assignmentchef.com/product/solved-csci-3901-assignment-3
<br>
<h1>Problem 1</h1>




Goal

Work with graphs and minimum weight spanning trees.




<h2>Background</h2>

A common task in machine learning and big data is clustering data.  Clustering data means that we have many initial data points and we want to group the data points so that all of the points in the same cluster are similar in some way and points in different clusters are deemed sufficiently different from one another.  There are many ways to do clustering.  We are going to approximate one of them.




We are going to think about grouping documents around common topics.  Given a set of documents, we will compare each pair of documents and will create a number that reflects how similar the two documents are to one another.  If you’re interested in one example, see the cosine similarity measure ( <u>https://en.wikipedia.org/wiki/Cosine_similarity</u> ) where the number would be between -1 (dissimilar) and 1 (very similar).




The basic idea of the clustering is to start with each document as its own cluster and to repeatedly merge clusters until we reach a balance point.  How we merge the clusters is the point of this assignment.




<h2>Problem</h2>

You will have a method that lets us build an undirected, simple, weighted graph between vertices.  Each vertex has a string name/identifier (like a document name).  The weight on each edge is a similarity measure between the vertices.  For this assignment, that measure will be a positive integer, with a lower value meaning that the two vertices are very similar.




We will want to identify sets of vertices that are all similar.  We call these sets “clusters”.  For each cluster, we remember the weight of the heaviest edge that was used to create this cluster from previous ones (the maximum weight starts at 1 for clusters of a single vertex).




We begin with each vertex as its own cluster.  We then proceed with merges in a way similar to making a minimum weight spanning tree, using Kruskal’s algorithm (

<u>https://en.wikipedia.org/wiki/Kruskal%27s_algorithm</u> ).  Find the edge e with the smallest weight that connects two different clusters C and D.  If there are several edges of the same low weight then take the edge with the smallest-named vertex at the end of an edge.  Let the heavy edge that we’re tracking for clusters C and D be edges c and d.  Here is where we differ from Kruskal a bit:  if the ratio (weight of e) / min( weight of c, weight of d) is less than or equal to a given value then merge clusters C and D.  Otherwise, don’t consider edge e to merge clusters in the future.




Stop when none of the clusters can be merged.




Create a class called VertexCluster.  The class has at least 2 methods:

<ul>

 <li>boolean addEdge( String vertex1, String vertex2, int weight ) – record an edge going between the two vertices with the given weight. Return true if the edge was added.</li>

</ul>

Return false if some error prevented you from adding the edge.

<ul>

 <li>Set&lt;Set&lt;String&gt;&gt; clusterVertices ( float tolerance ) – return the  set of clusters that you get from the current graph, using the given tolerance parameter for deciding if an edge is good enough to merge two clusters (as described earlier).  Return null if some error happened while calculating the clusters.</li>

</ul>




<h3>Example</h3>

Consider the weighted graph of Figure 1, created with the following sequence of addEdge commands:

addEdge( “A”, “B”, 3) addEdge( “A”, “H”, 1) addEdge( “I”, “H”, 1) addEdge( “I”, “B”, 4) addEdge( “B”, “C”, 7) addEdge( “I”, “C”, 8) addEdge( “G”, “H”, 7) addEdge( “H”, “J”, 10) addEdge( “D”, “I”, 12) addEdge( “D”, “C”, 14) addEdge( “D”, “E”, 8)




addEdge( “J”, “F”, 3) addEdge( “E”, “F”, 2) addEdge( “F”, “G”, 7)

addEdge( “J”, “D”, 1)

<em> </em>




<em>Figure 1 Sample graph </em>




In the algorithm, we process the edges in the following order of increasing weight and then tiebreaker.  (Notice that I sorted the vertices of the edges to make it easier to know the order of the edges).  We use a maximum ratio of 3 for the calculation on whether or not to merge the components.

<table width="623">

 <tbody>

  <tr>

   <td width="156">Edge</td>

   <td width="156">Weight</td>

   <td width="156">Calculation</td>

   <td width="156">Outcome</td>

  </tr>

  <tr>

   <td width="156">(A, H)</td>

   <td width="156">1</td>

   <td width="156">1 / min(1,1) = 1</td>

   <td width="156">Merge components</td>

  </tr>

  <tr>

   <td width="156">(D, J)</td>

   <td width="156">1</td>

   <td width="156">1 / min(1,1) = 1</td>

   <td width="156">Merge components</td>

  </tr>

  <tr>

   <td width="156">(H, I)</td>

   <td width="156">1</td>

   <td width="156">1 / min(1,1) = 1</td>

   <td width="156">Merge components</td>

  </tr>

  <tr>

   <td width="156">(E, F)</td>

   <td width="156">2</td>

   <td width="156">2 / min(1,1) = 2</td>

   <td width="156">Merge components</td>

  </tr>

  <tr>

   <td width="156">(A, B)</td>

   <td width="156">3</td>

   <td width="156">3 / min(1,1) = 3</td>

   <td width="156">Merge components</td>

  </tr>

  <tr>

   <td width="156">(F, J)</td>

   <td width="156">3</td>

   <td width="156">3 / min(1,2) = 3</td>

   <td width="156">Merge components</td>

  </tr>

  <tr>

   <td width="156">(B, I)</td>

   <td width="156">4</td>

   <td width="156">Vertices in same component</td>

   <td width="156">Disregard edge</td>

  </tr>

  <tr>

   <td width="156">(B, C)</td>

   <td width="156">7</td>

   <td width="156">7 / min(3, 1) = 7</td>

   <td width="156">No merge</td>

  </tr>

  <tr>

   <td width="156">(F, G)</td>

   <td width="156">7</td>

   <td width="156">7 / min(3,1) = 7</td>

   <td width="156">No merge</td>

  </tr>

  <tr>

   <td width="156">(G, H)</td>

   <td width="156">7</td>

   <td width="156">7 / min(3,1) = 7</td>

   <td width="156">No merge</td>

  </tr>

  <tr>

   <td width="156">(C, I)</td>

   <td width="156">8</td>

   <td width="156">8 / min(3,1) = 8</td>

   <td width="156">No merge</td>

  </tr>

  <tr>

   <td width="156">(D, E)</td>

   <td width="156">8</td>

   <td width="156">Vertices in same component</td>

   <td width="156">Disregard edge</td>

  </tr>

  <tr>

   <td width="156">(H, J)</td>

   <td width="156">10</td>

   <td width="156">10 / min(3,3) = 3.3</td>

   <td width="156">No merge</td>

  </tr>

  <tr>

   <td width="156">(D, I)</td>

   <td width="156">12</td>

   <td width="156">12 / min(3,3) = 4</td>

   <td width="156">No merge</td>

  </tr>

  <tr>

   <td width="156">(C, D)</td>

   <td width="156">14</td>

   <td width="156">14 / min(3,1) = 14</td>

   <td width="156">No merge</td>

  </tr>

 </tbody>

</table>




We end with 4 components:

<ul>

 <li>A, B, H, I</li>

 <li>C</li>

 <li>D, E, F, J</li>

 <li>G</li>

</ul>

<h3>Assumptions</h3>

You may assume that

<ul>

 <li>You do not need to worry about round off error in the computations.</li>

 <li>If two vertex strings are different in any case-invariant way then they are different vertices.</li>

 <li>Not all vertices will necessarily be connected to one another in the graph.</li>

</ul>




<h3>Constraints</h3>

<ul>

 <li>You may use any data structures from the Java Collection Framework.</li>

 <li>You may not use a library package to for your graph or for the algorithm to do matchings on your graph.</li>

 <li>If in doubt for testing, I will be running your program on bluenose.cs.dal.ca. Correct operation of your program shouldn’t rely on any packages that aren’t available on that system.</li>

</ul>








