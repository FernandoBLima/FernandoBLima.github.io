---
layout: post
title: Disjoint Set - Data Structure Part IV
tags: [Algorithm, Data Structure, Javascript]
author-id: fbl
excerpt_separator: <!--more-->
---
This is the fourth part of the Data Structure series. If you haven't read this series yet, I recommend you check it out first! In this series, we’ve already learned that there are different ways to organize data using variables, arrays, hashes and objects in data structures.

<!--more-->

We discussed linked list, hash and set structure, however, this is just the tip of the iceberg! There is much more to come and learn. Relax, take it easy, because we will learn step by step. So, you don't have to worry, even if it sounds hard to hear.

> 💭 "Those hours of practice, and failure, are a necessary part of the learning process."- Gina Sipley


### Outline
The article is divided into the following parts:

- Understanding what Disjoint Set is.
- How the union and merge function works?
- How to optimize the union function?
- Code implementation and complexity analysis.


#### ◼️ Disjoint Set

We will continue what we already had learned in the last post about sets. 
A disjoint-set data structure is also called a union-find or merge–find set. It's like every data structure has more than one name, right? 😂 So, I will refer only to the Disjoint Set, because it looks more sophisticated and scientific to me. 👨‍💻👩‍💻 This structure has several applications but the most known is in the Kruskal's algorithm.

But what is a Disjoint Set? 🧐

<div style="text-align:center"><img src="https://media.giphy.com/media/qfV0NxS4YhEf6/giphy.gif" /></div>

A good way to understand this structure is imagining that we have more than one element that belong to a set and is partitioned into further subsets. That is to say, in this structure, the elements can keep the track of elements of the set, as you can see on the following image, where each element can have a child and parent element.

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/mwgox6rjwnqzv764nild.jpg" /></div>

###### <center>Figure 1: Disjoint Set representation.</center>

We can use the same approach that we used in the last post where we learned that the linked list is not a good option because it does not perform well. That's a result because the efficiency of an algorithm most of the time is related to how the data is used in an efficient way in a data structure. So, how we can build the Disjoint Set?

Before diving into this structure, we need first to discuss our main class. That said, when a Disjoint Set is created it is necessary to initialize our structure using the _init_ function that creates all the elements, this function has O(n) of time complexity. But how exactly does this function work?

In this code, each element is a reference to the DisjointSetNode class and it is put as root at the beginning, which means that the parent property is mapped to itself. Furthermore, when an element has no child elements, it is called the root of a structure and is set to -1 for the parent property, as a consequence, all elements belong to a different set, pretty simple, right? 

Our main class would look something like this:

```javascript
class DisjointSetNode {
    constructor(value) {
        this.value = value,
        this.children = {};
        this.rank = 1;
        this.parent = -1;
    }
}

class DisjointSet {
    constructor() {
        this.list = {};
        this.size = 0;
    }

    init(size){
        this.size = size;
        for (var i = 0; i < this.size; i++) {
            var disjointSetNode = new DisjointSetNode(i);
            this.list[i] = disjointSetNode;
        }
    }

    ...

}
```

Okay, let’s move on and take more steps forward to continue the discussion now that we understand how to initialize the structure. We can summarize and define the Disjoint Set with just two primary operations: find and union.

* Find

As the name suggests, this operation follows the parent element until a root element is reached, in other words, finding the value whose parent is itself.

```javascript
    findRoot(x) {
        if (this.list[x] && this.list[x].parent !== -1) {
            return this.findRoot(this.list[x].parent);
        }else{
            return this.list[x];
        }
    }
```

* Union

The basic idea for this function is to merge two distinct roots and making one of the roots as a parent of the root of the other.

I provided a simple code implementation for this function, note that the number of roots never increases and this occurs when the elements are merged, instead, the number of roots decreases. As we can see in our example below:

```javascript
    union(x, y){
        var xRoot = this.findRoot(x);
        var yRoot = this.findRoot(y);

        yRoot.parent = -1;
        yRoot.children[xRoot.value] = xRoot;
        xRoot.parent = yRoot.value;
    }
```

Ok, let's see the example below that merges some values ​​to help us make the understanding of this structure clearer, let's use the following subset S = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9 } and merge some elements.

```javascript
disjoinSet.init(10);

disjoinSet.union(2,1)
disjoinSet.union(2,3)
disjoinSet.union(3,4)
disjoinSet.union(5,4)
disjoinSet.union(4,6)
```

The result will look something like this:

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/kuntkepvedmj0suctf0y.jpg" /></div>

###### <center>Figure 2: Example of union operation.</center>

After union operations, you can see that there are now 5 subsets. First there is the element {0}, then {6 4 3 1 2 5}, {7}, {8} and {9}. Another important function that we can use is _isConnected_, used to check whether the elements are in the same set or not. For instance, we can find out if the values ​​2 and 6 below in the same group if they have the same root, therefore, this will give us a true result. See the code below:

```javascript
isConnected(value1, value2){
     if(this.findRoot(value1).value == this.findRoot(value2).value) 
         return true;
     return false;
}
```

> ⚡️ If you would like to know others functions that I implemented such as getSetSize, extractSets, getChildrenByItem, and others, you can access all the code just clicking [here](https://github.com/FernandoBLima/data-structures). 

Can you see the problem that can occur if we continue to link one element as a child of another using the union function? To check if values 2 and 6 belong to the same group, you will need four hops in the example above. It is a consequence of the union function that makes the structure grow by 𝑂(𝑁). If we deal with a large data set, this approach may not be efficient, with that in mind, one way to optimize this problem and reduce the execution time is by using one of the following ways:

* Union By Size

In this function, we connect the sets by the size where the root of the smaller structure is linked to the root of the larger structure. Initially, each element is a subset, in other words, it has size 1. 

The code example:

```javascript
    unionBySize(x, y){
        var xRoot = this.list[x];
        var yRoot = this.list[y];
        
        if(this.getSetSize(xRoot.value) > this.getSetSize(yRoot.value)){
            yRoot.parent = xRoot.value;
            xRoot.children[yRoot.value] = yRoot;
        } else {
            xRoot.parent = yRoot.value;
            yRoot.children[xRoot.value] = xRoot;
        }
    }
```

The _getSetSize_ function is used to return the size of the structure, making the element that belongs to the smallest structure size points to the set that has the largest size. The following code is an example of this scenario.

```javascript
disjoinSet.unionBySize(2,1);
disjoinSet.unionBySize(2,3);

disjoinSet.unionBySize(0,4);
disjoinSet.unionBySize(5,4);
disjoinSet.unionBySize(4,6);

disjoinSet.unionBySize(3,6);
```

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/vgh4c5p2ij9kigcjd8tv.jpg" /></div>

###### <center>Figure 3: Example of Union By Size operation.</center>


* Union By Rank

We can use a different way to optimize the structure using the rank, which means that is used the height of the set instead of the size to link the root of a smaller rank to the root with a larger rank. Another key thing to remember is that each element initially has 0 of rank. However, when the roots have the same rank, only the rank of the new root increases by 1 otherwise, no change occurs. Let's create an example:

```javascript
disjoinSet.unionBySize(4,5);
disjoinSet.unionBySize(6,7);
disjoinSet.unionBySize(4,6);
disjoinSet.unionBySize(3,4);
```

Take a look at the code below:

```javascript
   unionByRank(x, y){
        var xRoot = this.findRoot(x);
        var yRoot = this.findRoot(y);

        if(xRoot.value == yRoot.value)
            return;

        if(xRoot.rank < yRoot.rank){
            xRoot.parent = yRoot.value;
            yRoot.children[xRoot.value] = xRoot;
        } else if (xRoot.rank > yRoot.rank) {
            yRoot.parent = xRoot.value;
            xRoot.children[yRoot.value] = yRoot;
        } else {
            xRoot.parent = yRoot.value;
            yRoot.children[xRoot.value] = xRoot;
            yRoot.rank = xRoot.rank + 1;
        }
    }
```
Using the union by rank function the worst-case running time per operation is 𝑂(log𝑛).

* Path Compression

We can use Path Compression to optimize the Union by size and that’s what makes this structure remarkable. The idea behind this function is to flatten the structure when the find() function is used. After finding the root of all the elements along the way, the elements point each one directly to the root. As a result, efficiency is increased compared to the basic union operation.

But before showing how this operation works, let's take a few steps back and compare it to the worst case scenario. Let's say there are 4 elements {0,1,2,3} and then we merge to understand how the find and join operation are important in this function.  As we can see:

```javascript
disjoinSet.union(0,1);
disjoinSet.union(1,2);
disjoinSet.union(3,0);
```

As we discussed earlier, in this situation the height of our structure can grow quickly, after each step you can observe that the height is growing which brings us a poor performance. If we perform theses operations above, then the result it will be:

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/sus6hh76igbtfesi8ce9.jpg" /></div>

###### <center>Figure 4: Example of the worst case scenario using the union operation.</center>

We can avoid this, merging the same elements that we used on last example but comparing the union function using the path compression technique, where each element along the path is compressed and point to the root in the structure.

```javascript
disjoinSet.unionByPathCompression(0,1);
disjoinSet.unionByPathCompression(1,2);
disjoinSet.unionByPathCompression(3,0);
```

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/xasqmj4d5mozjp6d8cpy.jpg" /></div>

###### <center>Figure 5: Example of union operation using the path compression technique.</center>

What if we use this path compression and union by rank? See the image below:

```javascript
disjoinSet.unionByRankByPathCompression(0,1);
disjoinSet.unionByRankByPathCompression(1,2);
disjoinSet.unionByRankByPathCompression(3,0);
```

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/ikheq6mbtt0c1qnz5tw7.jpg" /></div>

###### <center>Figure 6: Example of union by rank operation using the path compression technique.</center>

Great! We improved the performance and the time complexity of each operation becoming smaller than O(Logn), reducing the complexity of union. Let's see how is the code:

```javascript
    unionByRankByPathCompression(x, y){
        var xRoot = this.findByPathCompression(x);
        var yRoot = this.findByPathCompression(y);

        if(xRoot == yRoot)
            return;

        if(xRoot.rank < yRoot.rank){
            xRoot.parent = yRoot.value;
            yRoot.children[xRoot.value] = xRoot;
        } else if (xRoot.rank > yRoot.rank) {
            yRoot.parent = xRoot.value;
            xRoot.children[yRoot.value] = yRoot;
        } else {
            xRoot.parent =  yRoot.value;
            yRoot.children[xRoot.value] = xRoot;
            yRoot.rank = xRoot.rank + 1;
        }
    }

```

However, the bad news is that we can not use this approach using the union by rank because as we can see, this operation changes the heights of the structure.

___

That's all folks! I hope you have fun learning the disjoint set structure 😁

Code: https://github.com/FernandoBLima/data-structures

< [previous](https://dev.to/fernandoblima/set-and-multiset-data-structure-algorithm-part-iii-1ea8) | next ( coming soon) >

___

_So we finished our discussion about Disjoint Set structure._ 🙌

_I hope you have a clear idea how to work. If you found this article helpful or if you find something I miss out or that you like it, feel free to let me know._ 😁