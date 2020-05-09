---
layout: post
title: Linked List, Queue and Stack - Data Structure & Algorithms Part I
tags: [Algorithm, Data Structure, Javascript]
author-id: fbl
excerpt_separator: <!--more-->
---
<p>Learning the concepts and how to implement Linked List, Queue and Stack.</p>

Welcome to my first article where I going to talk about Data Structures. I'm so excited to be writing this series! It's my first article here and I have been postponed this for so a long time and many reasons, maybe I can write about it another time, but finally I decided to complete this goal.

<!--more-->

Here I will show how important this topic is and why you should understand all the concepts. In my point of view is important to know the concepts and how it works behind the scenes, although there are many frameworks that already have the complete implementation. But, trust me, it is essential for your career and maybe you may need it in the future to resolve some problem. 👨‍💻👩‍💻

Here we are going to have a brief discussion with Javascript examples and I will start from the beginning, gradually, because we do not have to be hurry! So, let’s diving in this fantastic world called data structure and algorithms together. 😀

💭 "Bad programmers worry about the code. Good programmers worry about data structures and their relationships." - Linus Torvalds

### Outline

- Discussion about Singly, Doubly and Circular Linked List.
- What is a Queue and Stack?
- Terminology.
- When and where is used?
- Code implementation and complexity analysis.


### What is a Linked List?

Before we start to discuss, we need to formulate a clear understanding of what a linked list is. A collection structure represents a sequence of nodes. But, wait! ✋ What does node mean? 🤔  An object that contains value and pointer with reference to stores the address for the next element into the sequence of the list, as you can see in the following figure:

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/mt5yv6qjod33bd37cr0o.jpg)
###### <center>Figure 1: Linked List representation.</center>

Actually, you can imagine a pointer, as a place where you can find and obtain the stored value in the node, is a reference to a location in memory. The first node in the list represent a head and has a pointer to the next element, and as you can imagine the last node is the tail because has a null pointer to the next node.

>  <Optional> You also can use a pointer to the previous object. As a result, a Doubly Linked List type is created.

Another important aspect to understand linked list is related to the efficient memory utilization. Is not necessary to pre-allocate memory, as a consequence you can add as much items you want in the list. However, some problems can show up if is required more memory than you can have, because each node has a pointer and other memory for itself.

### Terminology

As you can see in the image in the section above, we define two properties:
- value: Element that holds the data.
- next: Point to the next node.

> - prev (optional): Can be used to point to the previous node. You can see more about in Doubly Linked List Structure.

### Let's begin! 

Now that we are on the same page with the concepts, let’s start the discussion more deeply about Linked List methods, translate the concepts into to our code, and finally implement our data structure. At the beginning, we are going to focus in the Linked List, because it is the most common and simplest data structure linear collection of data elements. 
 
Let's start to work! 😃

#### ◼️ Singly Linked List

Is called as singly because a node only hold a reference to the next element of the sequence and you cannot access previous elements because it does not store any pointer or reference to the previous node, as you can see in the figure.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/3gdakj5md6oqx1gyuv9l.jpg)
###### <center>Figure 2: A singly linked list that contain an element and a pointer to the next node</center>

Before we describe the operations, we need to define the most important part in our code that will help us to build the linear list structure, the node class.

```javascript
class Node {
   constructor(value, next) {
      this.value = value;
      this.next = next;
   }
}
```

Our main class only has a reference to the value and the next node, pretty simple, right? So, let’s move on and define the Linked List class, which has the head property that point to the first element into the list, other property we have to declared is the size, which give to us the number of nodes that exist into our list.

```javascript
class LinkedList {
    constructor() {
       this.head = null;
       this.length = null;
    }
}
```

Okay, continuing the discussion we have to add methods to our class. Let’s check out:

- **insertAtHead**: Our first method is used to add a new element at the beginning of our data structure. This method has a constant running time (O(1)). But what does it mean? 🧐 It means that it takes the same amount of time to add a value in the list, is a constant time. In this case is necessary only to move one time to add a new element in the first position into the list. As result, we need to update only the current head that will be pointing to the new item that we are going to be creating. Here’s how it should be:

```javascript
addToHead(value){
   if(linkedList.head){
      var newNode = new Node(value, this.head );
      this.head = newNode;
   }else{
      var newNode = new Node(value, null);
      this.head = newNode;
   }
   this.length++;
}
```

- **removeAtHead**: If we want remove one element from the head all we have to do is replace the head by the following element. Like the method before the constant running time is O(1).

```javascript
removeAtHead(value){
    if(this.head){
       var newHead = this.head.next;
       this.head = newHead;
       this.length--;
    }else{
       return false;
    }
}
```

- **search**: If we are looking for a specific item? Do not be hurry; we only need iterate the list until the end to find the element in the list. But imagine the following scenario: We have a list with 1000 items and we are looking for the 999 item. Can you guess what can happen? If we want to get some specific value or node at position N then we have to moving the pointer throw the entire list to find it. This can cause a problem with the access time.

```javascript
    search(value){
        if(this.head){
            var node = this.head;
            var count = 0;
            while(node != null && node.value != value){
                if(count >= this.length && node.value != value){
                    return false;
                }
                node = node.next;
                count++;
            }
            if(node == null){
                return false;
            }else{
                return true;
            }
        }else{
            return false;
        }
    }
```

There are others functions like **getAtIndex**, **addAtIndex**, **removeAt** and **reverse** that I would like to discuss, but they have similar logic applies as the previous methods described before, so I’ll skip the explanation of them to not waste your time.


> ⚡️ But if you would like to know how I implemented, you can access all the code just clicking [here](https://github.com/FernandoBLima/data-structures). 
__

#### ◼️ Doubly Linked List

As I mentioned earlier, the Doubly Linked List is a structure that has capacity to pointer to the previous node, which is the biggest difference comparing with the Singly List. Now we gain the power to move traversed backward in the list. For instance, each node has a pointer to the previous element, allowing you to move through the list from the tail, as show in the picture below.

As Uncle Ben said to Peter Parker, “with great power comes great responsibility”. As consequence, is required more space to store the addresses of previous elements instead just one to the next element in list, so takes two more memory comparing with the singly structure.

Besides that, mostly all functions and behaviors are quite similar with the Singly List. With basic understanding of Linked List, it is so easy to build and extend functionality to make it a Double List. So easy, right? 😁 You can feeling that we are having progress. 💪

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/98yyqanvz90fabcn7ilc.jpg)

###### <center>Figure 3: A doubly linked list with pointer to the previous element</center>

Even though the behavior is similar, we need to update the Singly List functions such as **addAtHead**, **removeAtHead**, **search** and others to consider the previous property. Besides these functions, we have new weapons to use here, as you can see below:

- **insertAtTail**: We define a new element at the bottom of the list and point the last element as the tail. Can you imagine the constant running time?

```javascript
    addAtTail(value){
        var newNode = new Node(value, null, this.tail);
        if(this.tail){
            this.tail.next = newNode;
            this.tail = newNode;
        }else{
            this.head = newNode;
            this.tail = newNode;
        }
        this.length++;
    }
```

- **removeAtTail**: Here the last item from the list is set to the null value. As a result, the final element become the previous element of the last element.

```javascript
    removeAtTail(){
        if(this.length === 1){
            this.removeAtHead();
            this.tail = null;
            return;
        } else if (this.length > 1){
            this.tail = this.tail.prev;
            this.tail.next = null;
            this.length--;
            return;
        }
        return false;
    }
```

#### ◼️ Circular Linked List

The only difference between the doubly Linked List is the fact that the tail element is linked with the first element in the list. As a result, a loop was created and now we can move forward and back-forward into the entire list.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/hd8jlfhp09a7eijby075.jpg)
###### <center>Figure 4: Circular linked list that contain a link between the first and last element.</center>

Now we will use the entire acknowledgement that we learned to implement two new data structure.

#### ◼️ Queue

The First-In-First-Out (FIFO) is an example of a linear data structure where the first element added to the queue will be the first to be removed. For instance, you can visualize this behavior where you are in a queue in a store, bank or supermarket. 

🚶‍♂️🏦🚶‍♀️🚶‍♂️🚶‍♀️🚶‍♂️

A new element is added to the end of the list by the enqueuer (addFromTail) function and removed from the top of the list using the dequeue (removeFromTail) function. You can see other people or find in a book referencing the queue as removing or poling method, for me I prefer only dequeue. Other common operation in this structure is the peek that return the item at the top of the stack as peek.

However, when should I use these structure data? 🤔 It is suggested to use Queue when the order matter, like a queueing system for requests.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/fmgyixedyr4xnuy3vgjg.png)
###### <center>Figure 5: Representation of a Queue.</center>

#### ◼️ Stack

Known as LIFO (last in, first out) data structure, you can visualize understanding how it works making an analogy when a set of items is stacked on top of each other, creating a pile of books.

Like I said before, this structure has some similarities from Linked List and you can use addFromTail (Push) and removeFromTail (Pop) operations in your stack structure. Just like a queue, the operation that return an item at the top of the stack is called as peek.

You can find this structure in mechanisms in text editors, compiler syntax checking or also on a graph.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/l802rd6482odvyp7qvjw.png)
###### <center>Figure 6: A representation of a stack and the Push and Pop functions.</center>

___

#### ◼️ Time Complexity

You can see the time complexity in the image below, where n is the length of Linked List.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/99u1y27olrgqexnulq8m.png)
###### <center>Figure 7: The time complexity.</center>

Let’s create an example by adding some values in the head and then removing in a Linked List using addAtHead and removeAtHead functions. In addition, using the time() object in Javascript will allowed us to time and analyzes the performance of our code, as the follow figure:

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/xytlcgb5w0bswfern2br.png)
###### <center>Figure 8: Output after insert and remove some values in the Singly Linked List.</center>

As you can see, we add some values in the list that show us how faster it is. Seeing the values we can realize that the execution time become a constant. The image below show the plot using Python with the Panda DataFrame library.

![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/cm793kn3w5u72upq05fy.png)
###### <center>Figure 9:  The consume time between addAtHead and removeAtHead functions.</center>

We are done 🙌

#### ◼️ And that's it!

To sum up our brief discussion, we have learnt that the Linked List is a simplest and dynamic data structure that can be used to implement others structures such as Queue and Stack.

You can use these structures to perform a huge amount of insertion and deletion of items. It run fast by the fact that we need update only the next pointer in the node. However, if we want to get some specific value or node at position N, a problem with access time may occur if the size of the list is longer.

Other important factor is the efficient memory utilization, it not necessary to pre-allocate memory. Nevertheless, in case you need more space, a problem related to a contiguous block of memory can occur.

That's all folks!

Code: https://github.com/FernandoBLima/data-structures

| next ( coming soon) >

___

