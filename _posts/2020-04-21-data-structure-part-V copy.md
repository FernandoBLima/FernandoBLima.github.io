---
layout: post
title: Tree and Binary Search Tree - Data Structure & Algorithm Part V
tags: [Algorithm, Data Structure, Javascript]
author-id: fbl
excerpt_separator: <!--more-->
---
Finally, this series will talk about the tree structure and I am very excited because it is one of the most important structures and there is a lot to cover. 😁

<!--more-->

Of course, it will be impossible to cover everything in just one article. In this way, to keep things simple, we will have other articles to discuss this structure. However, this does not mean that what we are going to learn is not important! We will focus on the tree and binary search tree that are powerful concepts and that will help us to develop our knowledge to future articles! 👩‍💻👨‍💻

But wait for a second! Before winter comes, if you are unfamiliar or need to remember some concepts about data structures, I highly recommend reading the most recents posts in this series.

Now that you are ready for the winter, go ahead and may the Seven gods protect you in the game of thrones.

Because winter is coming!

<div style="text-align:center"><img src="https://media.giphy.com/media/3ohzdUi5U8LBb4GD4s/giphy.gif" /></div>

> 💭 "People have an enormous tendency to resist change. They love to say, 'We've always done it this way'. I try to fight that." - Grace Hopper


### Outline
- Basic concepts
- Terminology
- Types of Trees: Tree, Binary Tree and Binary Search Tree
- The main operations and properties.

#### ◼️ Tree 🌳


We can describe the simplest definition of tree structure by saying that it stores and manipulates elements hierarchically, and this is one of the biggest differences with other structures.

So, let's look at how this structure works using the following example:

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/5qwja9q07hm2irw12kys.jpg" /></div>

###### <center>Figure 1: representation.</center>

One of the first steps to understand the structure is to understand the main concepts. As we can see in the image above, each character in Stark House represents a node element in the tree structure. The node on the top is the Rickard Stark element and is called the root of the tree because it starts the structure and does not have a parent node. 

All elements that are under an element are represented as children, for instance, Eddard, Benjen, and Lyanna elements are related as children of the Rickard node and the link between a node to another, like Rickard and Eddard, is called an edge. 

Another thing to discuss in this picture is related to Robb, Sansa, Arya, Bran, Rickon, and Jon Snow (You know nothing!) elements, they represent leaves because they do not have children. 

Okay, the next step is to define the main class which is NodeTree, as you can see in the following code:

```javascript
class NodeTree {
    constructor(key) {
        this.key = key;
        this.descendents = [];
    }
}
```

Now we are going to create a simple example where we can add new values to the tree and then remove it. In this code, we create the Tree constructor that has the link to the root element and the number of nodes in the structure. 

Besides that, there is a function to insert a new value that we can specify where the value will be added. For instance, if the structure already has the root element, a new value will be added as a descendent node. However, we can specify the parent node of the new element. Another function is to remove a value from the structure that does a search on all child elements.

Take a look at the code below:

```javascript
class Tree {
    constructor() {
        this.root = null;
        this.length = 0;
    }

    add(value, root = null) {
        if(!this.root){
            this.root = new NodeTree(value);
        } else {
            if(!root){
                this.root.descendents.push(new NodeTree(value));
            } else {
                var currentRoot = this.getValue(root);
                currentRoot.descendents.push(new NodeTree(value));
            }
        }
    }

    remove(value) {
        var queue = [this.root];
        while(queue.length) {
            var node = queue.shift();
            for(var i = 0; i < node.descendents.length; i++) {
                if(node.descendents[i].key === value) {
                    node.descendents.splice(i, 1);
                } else {
                    queue.push(node.descendents[i]);
                }
            }
        }
    }

    ...

}
```

#### ◼️ Binnary Tree

As the name suggests, a binary tree it is a tree whose elements have at most 2 children, called left and right. Simple right?  We should keep in mind that every node is a representation of a subtree itself. That said, a node can have two subtrees.

#### ◼️ Binnary Search Tree (BST)

Binary Search Tree is a rooted binary tree and each node store a key and can have two children like the binary tree. But what is the difference between them? An important thing to remember is that the root element must satisfy the property to be greater than all keys stored in the left sub-tree, and not greater than all keys in the right sub-tree which provides the efficient way of data sorting, searching and retriving.

In general, the worst case of time complexity is O (h), where h is the height of the BST, because it depends on how many elements and the order we must go through.

To implement a binary search tree we have to update the NodeTree class, in order to support the binary search tree property.

```javascript
class NodeTree {
    constructor(key) {
        this.key = key;
        this.left = null;
        this.right = null;
    }
}
```

Let's take a look at the following image:

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/86vdv3m97ga3sbgmiafh.png" /></div>

###### <center>Figure 2: Binary Tree representation.</center>

#### Operations

Now we will learn some operation to build our structure.

#####  - Insert

As we discussed above, the root element must be greater than all left subtree elements and smaller than right subtree and this must occurs to all elements in the structure. In this way, when a new element is inserted must be verified the value. When a value is less than the node's key it must be added to the left sub-tree, otherwise it must be added to the right subtree. An important thing to get note is that duplicate nodes are not allowed in the tree. 

We implement a binary search tree using the class NodeTree. Here is how a binary search tree insertion might be:

```javascript
    insert(value){
        if(!this.root){
            this.root = new NodeTree(value);
            this.length++;
        }else{
            this._insertNode(this.root, value);
            this.length++;
        }
    }
    
    _insertNode(currentNode, value){
        if(currentNode.key){
            if(value < currentNode.key){
                if(!currentNode.left){
                    currentNode.left = new NodeTree(value);
                }else{
                    this._insertNode(currentNode.left, value);
                }
            } else {
                if(!currentNode.right){
                    currentNode.right = new NodeTree(value);
                }else{
                    this._insertNode(currentNode.right, value);
                }
            }
            return;
        }
    }
```

#####  - Search

When we want to search for an element, we have to follow the same logic as the previous function. Remember that an element is searched from the root node if the value is less than the root node, then we must traverse to the left subtree, otherwise, the search will be directed to the right subtree. Once you understand how value is inserted, it becomes easier to create other functions, right? 

One of the main differences between this structure to the others is the fact that we can search for an element more quickly than the Linked List, but it is slower compared to arrays. This behavior can occur in the same way to insert and delete functions.

```javascript
    search(value){
        if(!this.root){
            return null;
        }else{
            return this._search(this.root, value);
        }
    }

    _search(node, value){
        if(node){
            if(node.key != value){
                if(value < node.key){
                    return this._search(node.left, value);
                }else{
                    return this._search(node.right, value);
                }
            }else{
                return node.key;
            }
        }else{
            return null;
        }
    }
```

#####  - Delete

To remove an element in the binary search tree, three are some of the possibilities that must be followed, which are:

  - If the value to be deleted is a leaf, then we just need to remove it from the tree.
  - When a node has only one child, in this case, we need to remove the value and copy the child to the node.
  - If a node element to be deleted has two children, it is necessary to find the inorder successor of the node.

Below is an example:

```javascript
    delete(value){
        if(!this.findNode(value)){
            return false;
        }
        this._delete(this.root, value);
        return true;
    }

    _delete(node, value){
        if(node == null) return node;

        var parent = this.findParent(value);
        if(!parent && node.left == null && node.right == null){
            return this.root.key = null;
        }

        if(value < node.key){
            node.left = this._delete(node.left, value);
        }else if(value > node.key){
            node.right = this._delete(node.right, value);
        }else{
            if(node.left == null){
                return node.right;
            }else if(node.right == null){
                return node.left;
            }
            node.key = this._minValue(node.right); 
            node.right = this._delete(node.right, node.key);
        }
        return node;
    }
```

> ⚡️ The binary Search Tree is a extend topic to discuss and learn, if you would like to learn others functions that I implemented such as listLeafNodes, findParent, findMaximum, findMinimum and others, you can access all the code just clicking [here](https://github.com/FernandoBLima/data-structures). 

#### Types of binary trees

Okay, now that we already understand the main operations in a binary search tree, we can move on and discuss some other properties. We can classify the binary search tree into the following types of Binary Trees:

 - Full Binary Tree

It is considered a full binary tree if all nodes, except the leaves, have two children. The following image shows an example of a full binary tree. 


<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/oewh4m69vh1yglupbcaz.png" /></div>

###### <center>Figure 3: A full binary tree example </center>

```javascript
   isFull(){
        if(this.root == null || this.root.left == null && this.root.right == null ) 
            return true; 
        return this._isFull(this.root);
    }

    _isFull(root){
        if(root == null || root.left == null && root.right == null ) 
            return true; 

        if ((root.left == null && root.right != null) ||
            (root.left != null && root.right == null))
                return false; 

        if((root.left != null) && (root.right != null)) 
            return (this._isFull(root.left) && this._isFull(root.right));    
    }
```

 - Complete Binary Tree

Here, we can say that a Binary Tree is complete when all levels are full, the only exception being the last level.

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/oewh4m69vh1yglupbcaz.png" /></div>

###### <center>Figure 4: A complete binary tree example </center>

```javascript
    isComplete(){
        if (this.root == null)         
            return true; 
        let numberNodes = this.countNode();
        return this._isComplete(this.root, 0, numberNodes);
    }

    _isComplete(root, index, numberNodes) {
        if (root == null)         
            return true; 

        if (index >= numberNodes) 
            return false; 

        return (this._isComplete(root.left, 2 * index + 1, numberNodes) 
            && this._isComplete(root.right, 2 * index + 2, numberNodes));
    }
```

 - Perfect Binary Tree 

When a binary tree is complete and full at the same time, it is considered a Perfect Binary Tree, which means that all levels have elements and all leaf nodes are at the same level.

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/mmrzoc15yk7lfzkrjcan.png" /></div>

###### <center>Figure 5: A perfect binary tree example </center>

```javascript
   isPerfect() {  
        if (this.root == null)  
            return true;  
        let leftMostLeaf = this.leftMostLeaf(this.root);  
        return this._isPerfect(this.root, leftMostLeaf, 0);  
    }  

    leftMostLeaf(node) {  
        let depth = 0;  
        while (node != null)  
        {  
            depth++;  
            node = node.left;  
        }  
        return depth;  
    }  

    _isPerfect(root, d, level) {  
        if (root == null)  
            return true;  
    
        if (root.left == null && root.right == null)  
            return (d == level+1);  
    
        if (root.left == null || root.right == null)  
            return false;  
    
        return this._isPerfect(root.left, d, level+1) && this._isPerfect(root.right, d, level+1);  
    }  
```

#### Binary Tree Traversal

We can visit all nodes in a tree differently, generally, it starts at the root node to search or locate a particular tree, or to print all the values it contains. With this concept in mind, let's take a look at in the most commons ways to traverse a binary tree.

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/86vdv3m97ga3sbgmiafh.png" /></div>

###### <center>Figure 5: A binary search tree example </center>

 - Pre-order

In this function, the order that we must follow is to visit the root node first, and then go through all elements on the left subtree and the right subtree of the root. 

```
Root -> Left Tree -> Right Tree
```

As I said before, every node is a representation of a subtree itself. With that in mind, when a leaf node is located, which has no left and right subtree, we say that it has been completely traversed. As a consequence, the right node of the subtree will be visited. This process occurs until all elements are visited.

PreOrder traversal : [ 100, 50, 25, 75, 150, 125, 175 ].

```javascript
    preOrder(){ 
        if (this.root == null) 
            return null; 
        var listValues = [];
        return this._preOrder(this.root, listValues); 
    } 

    _preOrder(node, listValues){
        if(node.key != null) 
            listValues.push(node.key);
        if(node.left != null) 
            this._preOrder(node.left, listValues);
        if(node.right != null) 
            this._preOrder(node.right, listValues);
        return listValues;
    }
```

 - In-order

In this traversal method, its traverse to the left subtree first by recursively. At first, it visited all elements of the left subtree of the root, then the node root and all elements of the right subtree.

```
Left Tree -> Root -> Right Tree
```

Inorder traversal : [ 25, 50, 75, 100, 125, 150, 175 ].

```javascript
    inOrder(){ 
        if (this.root == null) 
            return null; 
        var listValues = [];
        return this._inOrder(this.root, listValues); 
    } 

    _inOrder(node, listValues){
        if(node.left != null) 
            this._inOrder(node.left, listValues);
        if(node.key != null) 
            listValues.push(node.key);
        if(node.right != null) 
            this._inOrder(node.right, listValues);
        return listValues;
    }
```

 - Post-order

In this method, we traverse the left subtree, the right subtree, and the root node.

```
Left Tree -> Right Tree -> Root
```

Post-order traversal: [ 25, 75, 50, 125, 175, 150, 100 ].

```javascript
    posOrder(){ 
        if (this.root == null) 
            return null; 
        var listValues = [];
        return this._posOrder(this.root, listValues); 
    } 

    _posOrder(node, listValues){
        if(node.left != null) this._posOrder(node.left, listValues);
        if(node.right != null) this._posOrder(node.right, listValues);
        if(node.key != null) listValues.push(node.key);
        return listValues;
    }
```

- Level order

Another important way to traverse in a tree is the level-order that visits every node on a level before going to a lower level. 

Level order: [ 100, 50, 150, 25, 75, 125, 175 ].

```javascript
   levelOrderQueue() {
        if (this.root == null)
            return null;
        
        var listOrderQueue = [];
        listOrderQueue.push(this.root);
        var listValues = []

        while (listOrderQueue.length > 0) {
            var n = listOrderQueue.shift();

            if (n.left != null)
                listOrderQueue.push(n.left);

            if (n.right != null)
                listOrderQueue.push(n.right);
            
            listValues.push(n.key)
        }
        return listValues;
    }
```

___

That's all, folks! I hope you are taking care of Yourself 😁

<div style="text-align:center"><img src="https://media.giphy.com/media/qNnQAESrblfDG/giphy.gif" /></div>

Code: https://github.com/FernandoBLima/data-structures

___

_So we finished our discussion about Tree and Binary Search Tree structure._ 🙌

_I hope you have a clear idea how to work. If you found this article helpful or if you find something I miss out or that you like it, feel free to let me know._ 😁


