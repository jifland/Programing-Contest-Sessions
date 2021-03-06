# Graphs and their algorithms

A graph is a collection of points/vertices and a collection of connections between pairs of the vertices called edges.  

Graphs represent connections between groups of things. The edges can be annotated (often with a weight).  They could also be directed.  

Unless stated otherwise in a description, we will assume that graph edges are unweighted, and undirected.  Also that we do not have any edges that have the same endpoints, nor that connect to the same node on both ends.

### Trees
Trees are a special type of graph where there are no cycles in the graph, and the graph is connected.

Another way of saying this: There is exactly one sequence of edges to travel to get from any vertex to any other vertex in the graph.

## Searching

### BFS   [wikipedia](https://en.wikipedia.org/wiki/Breadth-first_search)
In this we will be searching from a specific vertex, and looking to find all the other vertices in the graph.  We will be doing so by looking at vertices close to the chosen vertex before looking at vertices further away.
* We will use a vertex "a" as our starting vertex.
* We will use "G" to represent our graph.
* We will say that any vertex that is at the other end of an edge from a given vertex 'z' is a neighbor of the vertex 'z'.
* We will be using a queue for this algorithm.
* We need to keep track of what vertices we have visited, we will be using a set to do this, but there are many ways.
~~~
Start with an empty queue.
Create an empty set for visited vertices "visited"
put "a" on the queue
while the queue is not empty
  pop a vertex off the queue "v"
  add "v" to the "visited"
  find the neighbors of vertex "v" in the graph "G"
  for each of these neighbors, 
    if it is not in "visited"
      add the vertex to the queue
~~~
Inside the while loop is where you would write code to do work per node.

EX: maybe you have a factory in each of these cities, and you need to travel to them to pick up the items produced, and you want to know how many items there are ini total.
* If the graph has a quantity value associated with each vertex, then you could have a variable that starts at 0, and each time through the while loop sums up the values as you go along.

### Prim's Algorithm   [wikipedia](https://en.wikipedia.org/wiki/Prim%27s_algorithm)

For this, we are taking a graph that is weighted (the edges have a value on them that indicates some sort of cost, thus lower is better), connected, and we want to create a [minimum spanning tree](https://en.wikipedia.org/wiki/Minimum_spanning_tree).

We are going to take the above BFS algorithm and modify it slightly.
* an edge has a property of weight, which we can get by using edge.weight()
* a vertex has a property of expense which we can get/set with vertex.expense()
* we are going to change our queue to a [priority queue](https://en.wikipedia.org/wiki/Priority_queue) which will give the oldest item of the lowest cost, before it will give newer items of the same cost, before it will give items of higher cost.
* we will calculate the total cost in a variable called totalCost
~~~
Start with an empty priority queue.
Create an empty set for visited vertices "visited"
set totalCost = 0;
set "a".expense(0)
put "a" on the queue
while the queue is not empty
  pop a vertex off the queue "v"
  add "v" to the "visited"
  totalCost += v.expense();
  find the neighbors of vertex "v" in the graph "G"
  for each of these neighbors, 
    if it are not in "visited"
      set it's expense to the cost of the edge from "v" to the vertex.
      add the vertex to the priority queue with the expense value
~~~

Note to create this from the BFS algorithm, we have minimal changes
1. Change the queue to a priority queue
2. Edges and vertices have a numeric property that can be worked with.

### Dijkstra's Algorithm [wikipedia](https://en.wikipedia.org/wiki/Dijkstra%27s_algorithm)
Unlike Prim's Algorithm, which finds a minimum spanning tree, Dijkstra's Algorithm finds a tree that has the shortest paths to each vertex in a graph from a given start vertex.

Starting with Prims algorithm, we are going to make a slight adjustment in the value that we save for the expense for a vertex. Note this value is passed into the priority queue.
~~~
Start with an empty priority queue.
Create an empty set for visited vertices "visited"
set "a".expense(0)
put "a" on the queue
while the queue is not empty
  pop a vertex off the queue "v"
  add "v" to the "visited"
  find the neighbors of vertex "v" in the graph "G"
  for each of these neighbors, 
    if it are not in "visited"
      set it's expense to the cost of the edge from "v" to the vertex + v.expense().
      add the vertex to the priority queue with the expense value
~~~
At the end of this, the graph will have values on each vertex that states what the minimum cost is to walk to it from the starting vertex "a".



### Kruskal's Algorithm   [wikipedia](https://en.wikipedia.org/wiki/Kruskal%27s_algorithm)
This is another algorithm to find a minimum spanning tree on a graph, like Prim's Algorithm. It is comparable to Prim's for run time.  However it approaches the solution differently.

We will start with the BFS algorithm.
* add in a priority queue
* add a property to vertices that will store and allow access to the "group" the vertex may be in.  Initially these will all be blank.  This will take the place of our visited set.
* We will be putting all the edges on the priority queue.
* We will track what the latest group is we have created so we can create a new group identifier.
* We will replace the neighbor logic with assigning groups to edges logic.
* We will need a function to change all the group indicators for a specific group to another specific group.  changeGroupFromTo(group, group)
~~~
Add all the edges by their cost to the priority queue.
set totalCost = 0;
set currentGroup = 1
while the queue is not empty
  pop an edge off the queue "e"
  totalCost += edge.cost()
  let the two verticies that e connects be called "v1" and "v2"
  if "v1" or "v2" has a group value
    if "v1" and "v2" have a group value
      if "v1".group() != "v2".group()
        changeGroupFromTo( "v1".group(), "v2".group() )
    else
      if "v1".group()
	    "v2".group() = "v1".group()
	  else
	    "v1".group() = "v2".group()
  else
    "vi".group() = "v2".group() = currentGroup
    currentGroup++
~~~
This approach picks edges from all around the graph, and while it is still doing a breath first search, it is doing it over the edges, and considering them near when their costs are close to each other.
