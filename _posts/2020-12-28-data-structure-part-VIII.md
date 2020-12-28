---
layout: post
title: Graph - Data Structure & Algorithm Part VIII
tags: [Algorithm, Data Structure, Javascript]
author-id: fbl
excerpt_separator: <!--more-->
---
Hello everyone, today I intend to continue this series that we discussed data structure and for this post we will learn graphs. This incredible structure you can apply to many problems in the real world, so it is one of the most important structures and also very common in interview questions.

<!--more-->
So, let's dive into this new adventure!
  
> 💭 _"The trick is to fix the problem you have, rather than the problem you want."_ - Bram Cohen

### Outline
- What is a Graph?
- Basic concepts.
- The main operations and properties.
- Learning BFS and DFS functions.

### Graph

Many people often confuse a graph with a tree structure, and that's happened because a tree is a type of graph!

<div style="text-align:center"><img src="https://media.giphy.com/media/wPb0Er6MG6d9K/giphy.gif" /></div>


Basically, a graph is a non-linear structure of a set of vertices _V_ connected by edges _E_ that can be represented as ordered pair of vertices _G(V,E)_ .

More precisely, a graph is composed of paths that contain adjacency vertices connected by edges. Usually, you can find many books and articles using different terms to refer to vertices and edges, the most common of which are:

- Vertex: Nodes or points;
- Edges: Lines, links or arcs;

### ▪️ Graph visualization

One of the most interesting things that make graphs a powerful structure is how they can represent a lot of information for some applications. There are many examples that we can use and the most common are a network of cities, streets, flights, ferries, railway maps, social network connections, and so on...

From theses examples, a graph structure can obtain a lot of information, such as how many cities are close to another or which is the sort path between two cities, for example. Can you see how powerful this structure can be?

Even though a graph is just a collection of nodes and edges, there are two ways to represent it, which are:

- Adjacency Matrices

As the name suggests, this representation uses a square matrix where rows and columns mean that there is a relationship from one vertex to another. We can see how it works in the image below.

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/1hp9ytum2igfskbxkzlk.png" /></div>

###### <center>Figure 1: Adjacency Matrice visualization.</center>

As we can see, the matrix represents which vertices are connected by edges, we can simply find out if there is a relationship between the vertices looking at the matrix.

- Adjacency List

Is the most common and efficient way to represent a graph, because creates an array that can store all vertices in a list for each vertex of the graph. Using the same set used in the adjacency matrix above:

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/m6jx17usaco7pqlo3h0k.png" /></div>

###### <center>Figure 2: Adjacency list visualization.</center>

### ▪️ Graph Representations

After talking about visualizations, the next step is to learn how many types of graph exists. Here we will see how the vertices are organized and connected.

### Directed or undirected

- Directed

In this type of graph, the edges are directed from one vertex to another. As we can see, the edge between _0_ and _1_ vertices is directed, right? 

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/xaxa0aw8qtcitrjjzhuf.png" /></div>

###### <center>Figure 3: Directed graph representation.</center>

- Undirected

Unlike the directed graph, this type of graph has all vertices pointing towards each other, that is, all edges are bidirectional.

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/wzce2q0l81bkz7na1zil.png" /></div>

###### <center>Figure 4: Undirected graph representation.</center>

* Cyclic or acyclic

A cycle graph means if the graph contains a path that begins at a given vertex and after few vertices ends at the same starting vertex. The example below contains the following cycle: 5 -> 2 -> 1 -> 4.

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/ehhhicqajs0zgbnl0juz.png" /></div>

###### <center>Figure 5: Cyclic graph representation.</center>

### ▪️ Basic operations

Okay, now that we already understand what a graph is, let’s see how to implement it. First thing first, we need to create our main class and, as we have seen, there are two ways to build this structure and will use the adjacency list representation, where a key and all its connections are associated.

Let's see how below:

```javascript
class Graph {
    constructor() {
        this.adjacencyList = {}
    }
    ...

}
```

Simple right? 😁 We just need to initialize the _adjacencyList_ variable that will be used as a dictionary to add key-value pairs. The next step is to know how to insert a vertex in our graph using the dictionary that was created.

When a new vertex is added to the structure, it takes only a constant time, the time complexity of O(1). This is because we just need to add the new element to the array. 

Great! Moving forward, we need to use a real-world example to facilitate our understanding and we will use a social network to exemplify operations.

```javascript
addVertex(vertex){
    this.adjacencyList[vertex] = [];
} 
```

A social network needs some users, right? So, let's fill it out by adding some people from middle-earth using the following code:
 
```javascript
const middle_earth = new Graph();

middle_earth.addVertex('Gandalf');
middle_earth.addVertex('Saruman')
middle_earth.addVertex('Frodo')
middle_earth.addVertex('Billy')
middle_earth.addVertex('Sean')
middle_earth.addVertex('Merry')
middle_earth.addVertex('Sméagol')
```

Well, something is missing from our social network. We need interactions between the users, the next step is to create some connections between the vertices created. 

As discussed earlier, the main differences between these types of graphs are in the fact that only the _undirected_ function creates connections on both sides.

The code below shows how we can create connections using directed and undirected approaches. 

```javascript
addEdgeDirected(vertex1, vertex2) { 
    if(!this.adjacencyList[vertex1]){
       this.addVertex(vertex1)
    }
    if(!this.adjacencyList[vertex2]){
       this.addVertex(vertex2)
    }

    if(!this.adjacencyList[vertex1].includes(vertex2))
        this.adjacencyList[vertex1].push(vertex2);   
}

addEdgeUndirected(vertex1, vertex2) { 
    if(!this.adjacencyList[vertex1]){
        this.addVertex(vertex1)
    }
    if(!this.adjacencyList[vertex2]){
        this.addVertex(vertex2)
    }
    
    if(!this.adjacencyList[vertex1].includes(vertex2))
        this.adjacencyList[vertex1].push(vertex2);    

    if(!this.adjacencyList[vertex2].includes(vertex1))
        this.adjacencyList[vertex2].push(vertex1); 
}
```

In this example of social networking, we will use the undirected approach, however, the directed type graph also can be used. Moving on, let's now imagine that Gandalf added some hobbits and a wizard to his social network.

```javascript
middle_earth.addEdgeUndirected('Gandalf', 'Billy');
middle_earth.addEdgeUndirected('Gandalf', 'Merry')
middle_earth.addEdgeUndirected('Gandalf', 'Sean')
middle_earth.addEdgeUndirected('Gandalf', 'Frodo')
middle_earth.addEdgeUndirected('Gandalf', 'Saruman')
```

After that, our graph looks something like this:

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/kpvbr8tu1c7clj93j1lr.png" /></div>

###### <center>Figure 6: Middle-earth network representation.</center>

Awesome! 😁 

Okay, moving forward with the discussion, let's imagine the scenario where the Sméagol user had a bad behaviour and it is necessary to removed it, how we can do this?
 
 <div style="text-align:center"><img src="https://media.giphy.com/media/MmFVEhDzF0UUw/giphy.gif" /></div>

For now, we have no way to remove any vertex. So, we need to fix this, right? To delete a vertex from the graph, it is necessary to iterate through the list of each vertex and validate whether an edge exists or not. If it exists, then we have to remove the vertex. Let’s take a look:

```javascript
removeVertex(vertex) { 
    if(vertex in this.adjacencyList){
        delete this.adjacencyList[vertex];
        var vertexList = Object.keys(this.adjacencyList);
        vertexList.forEach(element => {
            if(this.adjacencyList[element].includes(vertex) == true){
                var index = this.adjacencyList[element].indexOf(vertex);
                this.adjacencyList[element].splice(index,1);
            }
        });
    }
}
```
As you may be thinking, this function is O(n) because we need to iterate over the vertices and then remove the element.

And the edges? I mean, what if Gandalf had a big argument with Saruman and then decides to remove him from his social network? What do we have to do? 🧐 Well, to remove an edge, we need to check if the vertices are connected and then remove from the array.

```javascript
removeEdge(vertex1, vertex2) { 
    if(this.adjacencyList[vertex1].includes(vertex2)){
        var adjacents = this.adjacencyList[vertex1];
        var index = adjacents.indexOf(vertex2);
        this.adjacencyList[vertex1] = adjacents.splice(index,1);
    }
}
```

Great! Can you see the progress? 🚀

There are functions like _print_all_path_destination_, _print_adjacency_matrix_, _countPaths_, _isAdjacent_ and others that I would like to discuss, but I’ll skip the explanations for not taking too much of your time.

> ⚡️ But if you would like to learn or see others functions that I implemented you can access all the code just clicking [here](https://github.com/FernandoBLima/data-structures/graph). 

Now we are going to use all the acknowledgement that we learned to implement search function.

### ▪️ Graph Search

Let's dive into the most important topic related to this structure! We want to traverse all the vertices and edges of that graph. What does that mean? Well, we can put an example: Gandalf had a map and try to travel across middle-earth. 😁 But relax, we'll see all the steps of these functions using an example along the way. Let's create a graph to be used.

```javascript
var graph_example = new Graph();
graph_example.addEdgeDirected(0, 1)
graph_example.addEdgeDirected(0, 2)
graph_example.addEdgeDirected(1, 3)
graph_example.addEdgeDirected(1, 4)
graph_example.addEdgeDirected(2, 5)
graph_example.addEdgeDirected(2, 6)
```

After creating the vertices and edges, our graph will look something like this:

 <div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/9bpopckymfgad9klv60t.png" /></div>

###### <center>Figure 7: Graph example to be used on BFS and DFS functions.</center>

- Breadth-first search (BFS)

This approach is the most common and used. It starts by considering all vertices as unvisited and all edges undiscovered. With that in mind, we can choose an arbitrary vertex and then discover all vertices connected by an edge and visited. 

Every time that an adjacent vertex is visited, we must mark it and insert it in a queue. Since none of the edges that incident on the vertex are undiscovered, we can proceed and explore the next vertex.

Using the example above and considering vertex 0 as the current vertex, the result is:

```javascript
Visited Vertex: 0
Visited Vertex: 1
Visited Vertex: 2
Visited Vertex: 3
Visited Vertex: 4
Visited Vertex: 5
Visited Vertex: 6
```

We must repeat the process until no undiscovered and unvisited are left in the structure. When the queue is empty, it means that the algorithm covers all vertices and edges. With all that in mind, let's put everything in a code.

```javascript
breadthFirstSearch(current_vertice) {
    var vertices = Object.keys(this.adjacencyList);
    if(vertices.length === 0){
        return;
    }else {
        var discovered = {};
        vertices.forEach(function(item) {
            discovered[item] = false;
        })
        this._breadthFirstSearch(current_vertice, discovered);
    }
}

_breadthFirstSearch(vertex, discovered){
    var queue = [];
    discovered[vertex] = true;
    queue.push(vertex);

    while(queue.length > 0){
        var u = queue.shift();
        console.log('Visited Vertex: ' + u);

        var listAdjacents = this.adjacencyList[u].sort((a, b) => a - b)
        listAdjacents = listAdjacents.sort()

        for (let index = 0; index < listAdjacents.length; index++) {
            const element = listAdjacents[index];
            if(!discovered[element]){
                discovered[element] = true;
                queue.push(element);
            }
        }
    }
}
```

- Depth First Search (DFS)

Initially, this function has conditions similar to the BFS function, all vertices are unvisited and edges are not discovered. Then, we can choose an arbitrary vertex that will be our root element, which will be visited and called the current vertex.

Now is when the difference between DFS and BFS functions begins! The current vertex has to explore as far as possible along each vertex visited, moving to the next undiscovered adjacent edge and printing the path. 

We must continue this loop until there are no unvisited and undiscovered elements. Instead of queuing, the DFS function uses a stack to find the shortest path. After that, with no undiscovered edges left, we have to go back to the initial visited vertex and start again checking other unvisited vertices until cover all vertices and edges of the graph. 

Using vertex 0 as the current vertex, we will obtain the following result:

```javascript
Visited Vertex  0
Visited Vertex  1
Visited Vertex  3
Visited Vertex  4
Visited Vertex  2
Visited Vertex  5
Visited Vertex  6
```



```javascript 
depthFirstSearch(current_vertice) {
    var vertices = Object.keys(this.adjacencyList);
    if(vertices.length === 0){
        return;
    }
    var discovered = {};
    vertices.forEach(function(item) {
        discovered[item] = false;
    })
    this._depthFirstSearch(current_vertice, discovered);
}

_depthFirstSearch(current_vertice, discovered){
    discovered[current_vertice] = true;
    console.log('Visited Vertex ', current_vertice);
    
    var listAdjacents = this.dictAdj[current_vertice].sort((a, b) => a - b)
    for (let index = 0; index < listAdjacents.length; index++) {
        const element = listAdjacents[index];
        if(!discovered[element]){
            this._depthFirstSearch(element, discovered);
        }
    }
}
```

___

That's all folks! 

I hope you have fun learning. 😁

 <div style="text-align:center"><img src="https://media.giphy.com/media/zGnnFpOB1OjMQ/giphy.gif" /></div>

Code: [https://github.com/FernandoBLima/data-structures](https://github.com/FernandoBLima/data-structures)
___

_So we finished our discussion about Graph structure._ 🙌

_If you found something I miss out or find this article helpful, feel free to let me know._ 😁