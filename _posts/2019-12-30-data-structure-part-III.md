---
layout: post
title: Set and MultiSet -  Data Structure & Algorithms Part III
tags: [Algorithm, Data Structure, Javascript]
author-id: fbl
excerpt_separator: <!--more-->
---
Welcome to the third part of the series, in this post I’ll tell you about the set and multiSet structure and continue our journey on data structure and algorithm. 👩‍💻👨‍💻 However, I recommend reading the hashtable post if you are not familiar with data structures.

<!--more-->
Because, unlike the [last post](https://fernandoblima.github.io/2019/11/25/data-structure-part-II.html) when we landed on a strange terrain, here we will be a place where we should already have seen it before. The focus of this post is to learn what a set is with examples of how it works, code implementation using javascript and, of course, answer any questions about it. This data structure is not difficult to learn at first, but it can be a bit complicated.

Let’s continuing our journey! 🌏 🚀

> 💭 "Those hours of practice, and failure, are a necessary part of the learning process."- Gina Sipley

### Outline

The article is divided into the following parts:

- Understanding what is Set and MultiSet.
- Code implementation and complexity analysis.


◼️ Set

#### What is it

As you can imagine you already have some basic understanding about what is the sets structure. Basically is a collection of unique elements that can be objects, numbers, strings, Pokémon's ... In other words, anything! 😁

We can also find in many language that support the set such as Java, C, Python, .NET Framework and so on. For instance, the ECMAScript 6 or ES2015 introduced the Set data structure to the JavaScript language. We can initialized by calling:

```javascript
const s = new Set()
```

Despite the fact that we can use this implementation, we will avoid and build our own because the goal here is to learn how this structure works behind the scenes.

With that in mind, what we need to create our structure is use an array and add some element. We can also use the list structure, but it is an inefficient and simple implementation where operations such as inserting or deleting do not perform well. Having been said that, there are better approaches to implemented using more efficient data structures such as trees, tries, or hash tables, however, in this post we will use the array and the hash table that we already have seen in this series. 

<div style="text-align:center"><img src="https://media.giphy.com/media/3ornk57KwDXf81rjWM/giphy.gif" /></div>

Great! We already have a basic understanding of how we can build the structure of the set, now let's discuss some properties. Every value you insert have to appear only once because this structure does not allow repeated values, see the example below:

```javascript
A = [1,2,3,4,5,6] // Allowed
B = [1,2,1,2,1,2] // Not Allowed
```

Moreover, other important factor about this structure is related by the fact that we do not need to order the elements, for instance:

```javascript
C = [1, 2, 3] 
D = [3, 2, 1] 
// The Set C is the same as set D.
``` 

We can say that this structure is an implementation of the mathematical concept of a finite set using the operations of the algebra of sets. Okay, let's put everything that we had learned into our code. But first things first, we will be creating the skeleton off our set class and, as you can notice we have two functions created.

```javascript
class SetStructure {
    constructor() {
        this.listValues = [];
        this.capacity = 0;
    }

    insert(value){
        if(this.listValues.indexOf(value)) {
            this.listValues.push(value);
            this.capacity++;
        }
    }

    remove(value){
        var index = this.listValues.indexOf(value);
        if(index >= 0) {
            if(this.capacity > 1){
                this.listValues.splice(index, 1);
            }
            this.capacity--;
        }
    }
}
```

But, wait a second! ✋ Before insert some values we must make sure if the value that we intend insert is not in our array. In the _insert()_ function use the _indexOf()_ propriety to verify if has any occurrence of a specified value in our structure. This method returns the position of element, however, if the array does not contain the data, the value -1 will be returned. We can use a similar logic in _remove()_ function.

As mentioned earlier, this structure is based on the mathematical concepts of sets; therefore, we can use its properties in our code to define some operations using set algebra, such as union and intersection. Let's have a brief discussion of the core set theoretical operations, so take a look below:

* Union

As the name suggest this operation will join two sets resulting in a new set structure that combine all members of either A or B set. We can use the math definition to define this operation:

<center>A U B = {x : x ∈ A or x ∈ B} </center>

Let's put in an example: 
```
{1, 2} ∪ {1, 2} = {1, 2}.
{1, 2, 3} ∪ {3, 4, 5} = {1, 2, 3, 4, 5}
```

To give an illustration of how the union operation is, take a look at the following image:

<div style="text-align:center"><img src="https://thepracticaldev.s3.amazonaws.com/i/jcfi7pepfou7aj8pk9x2.png" /></div>

###### <center> Figure 1: The union of A and B </center>

Now that we already have a clearly understanding, lets see how it works in our code.

```javascript
union(set) {
     var newSet = new SetStructure();
     set.listValues.forEach(function(value) {
         newSet.insert(value);
     });
     this.listValues.forEach(function(value) {
         newSet.insert(value);
     });
     return newSet;
};
``` 

* Intersect

In this operation a new set is created using all elements that the both sets have in common, that can be denoted by A ∩ B. In case of A ∩ B = ∅, then A and B are considered disjoint. The math concept about the intersect is defined as the following:

<center>A ∩ B = {x : x ∈ A and x ∈ B}</center>

<div style="text-align:center"><img src="https://thepracticaldev.s3.amazonaws.com/i/rszxuaxcrwu7gxu10o3t.png" /></div>

###### <center> Figure 2: The intersection of A and B </center>

And we can write the function that receives a set as parameter like this:

```javascript
    intersect(set) {
        var newSet = new SetStructure();
        this.listValues.forEach(function(value) {
            if(set.contains(value)) {
                newSet.insert(value);
            }
        });
        return newSet;
    };
``` 
 
* Difference

The difference operation, or complement if you prefer, is the difference between the set A and B. But what does it mean? 🧐 In other words, is the result of the values that contains in only one set and can be denoted by the following definition:

<center>A \ B or A − B where {x : x ∈ B, and x ∉ A} </center>

<div style="text-align:center"><img src="https://thepracticaldev.s3.amazonaws.com/i/dkytzz3o1mge5v9a5yz9.png" /></div>

###### <center> Figure 3: The difference of A and B </center>

Similar to union and intersect functions, we can iterate the list to get the difference between the sets:

```javascript
    difference(set) {
        var newSet = new SetStructure();
        this.listValues.forEach(function(value) {
            if(!set.contains(value)) {
                newSet.insert(value);
            }
        });
        return newSet;
    }
``` 

* Symmetric Difference

Another operation that we can create is the symmetric difference, also know as disjunctive union, which is the set where the elements not below in their intersection.

<div style="text-align:center"><img src="https://thepracticaldev.s3.amazonaws.com/i/eq8zrfkbs9jntiimwjk9.png" /></div>

###### <center> Figure 4: The symmetric difference of A and B </center>

```javascript
    symmetricDifference(set) {
        var newSet = new SetStructure();
        this.listValues.forEach(function(value) {
            if(!set.contains(value)) {
                newSet.insert(value);
            }
        });
        var setDifference = this;
        set.listValues.forEach(function(value) {
            if(!setDifference.contains(value)) {
                newSet.insert(value);
            }
        });
        return newSet;
    }
``` 

> ⚡️ If you would like to know others functions that I implemented such as removeAll, getAllSubsets, contains, and others, you can access all the code just clicking [here](https://github.com/FernandoBLima/data-structures). 


* Subset

The next operation define if every value of set A belongs to the set B and vice-versa. If they contain each other can denoted as A ⊆ B, which can be write as A is contained in B, is equivalent to A = B.

```javascript 
    isSubset(set) {
        return set.listValues.every(value => this.listValues.includes(value)); 
    }
```

 * Proper Subset

It is quite similar to subset operation, but two sets can be considered as proper subset if one set is not equal to another but has at least one element.

```javascript 
    isProperSubset(set){
        return set.listValues.some(value => this.listValues.includes(value));
    }
```

>  The set is mutable, however, we can create immutable frozenset objects initialized with elements of the given iterable. Which is a set that does not change after being build. In Python, we can use the frozenset () function to create this structure.

```python 
list = (1, 6, 7, 4, 9, 6, 2, 3, 5) 
frozenSet = frozenset(list) 
```

Very cool and easy to understand, right? 😁

◼️ MultiSet

The Multiset structure or Bag is quite similar to set structure that we have learned before, but the difference is due to the fact that unlike the set structure allows more than one instance of the element in the structure. 

An amazing thing that about programming it that there are many ways to develop the Multiset, we can continue using an array to store the values, or tuples if you are developing in Python.

This structure has the following properties:

- items: List of element that contains the data and key.
- multiplicity: Property which is a positive integer that gives how many elements has in the Multiset.
- cardinality: Summing the multiplicities of all its elements.

Since multiset is a type of set generalization, there are several ways to apply it to problem solving, Fuzzy multisets and Rough multisets, are some examples. 

Now that we already know what Multiset is, let's create the main operations, which are: insert and remove.

```javascript 
     insert(key, cardinality = 1){
        try{
            if(key == null || cardinality == null){
                throw new Error('Is not possible to insert a null value');
            }
            var flag = true;
            var listKeys = Object.keys(this.items);
            listKeys.forEach(item => {
                if(item == key){
                    this.items[key] = this.items[key] + cardinality;
                    flag = false;
                    this.cardinality = cardinality;
                }
            });
            if(flag){
                this.items[key] = cardinality;
                this.cardinality = cardinality;
            }
        }
        catch (error) {
            return error.message;
        }   
    }

    remove(chave, cardinality){
        if(this.items[chave]){
            var value = this.items[chave];
            if(cardinality > value){
                this.items[chave] = 0;
            }else{
                this.items[chave] = value - cardinality;
            }
        }
    }
``` 

We can use the hash table in our Multiset structure, that is to say, the time complexity is always a constant O(1) to add or search an element. As you can imagine this structure has the same functions as the set, however, there are some differences that we are going to learn together. 🤓

The algebra operations such as _union_, _sum_, _intersect_ and _difference_ have similar logic applies as the previous methods described before, so I’ll skip the code explanation of them to not waste our time and only discuss about the difference.

* Union

The main difference in the union of two multiset is that each element has the number of instances equal to the maximum of the multiplicity in A and B. 
```
{1, 2, 2} ∪ {2, 2, 3} = {1, 2, 2, 3}.
```

* Sum

In this operation the intersection of two multisets is equal to the sum  of the multiplicity of an element in A and B.
```
{1, 2, 2} + {2, 2, 3} = {1, 2, 2, 2, 2, 3}
```

* Intersect

The intersection of two multisets is equal to the minimum of the multiplicity of an element in A and B.
```
{1, 2, 2} + {2, 2, 3} = {2, 2}
```

* Difference

The difference of two multisets is equal to the multiplicity of the element in A minus the multiplicity of the element in B.
```
{1, 2, 2} + {2, 2, 3} = {3}
{2, 2, 3} − {1, 2, 2} = {1}
```

#### ◼️ Conclusion 

In conclusion, the most important factor that make the set structure special and unique comparing with the others is that use the core set-theoretical operations defined by the algebra of sets, which allows the use of properties and laws of sets using operation such as union and intersection. In this post we have a brief discussion about this operations.

We have learned that sets can be implemented using various data structures but the most common approach is using array or hash table. Even though the set structure looks like a simple structure there are many languages now include it can be applied in diverse scenarios and different generalizations, such as Fuzzy multisets, rough multisets and in relational databases. 

___

That's all folks! I hope you have fun learning the set structure 😁

Code: https://github.com/FernandoBLima/data-structures

< [previous](https://fernandoblima.github.io/2019/11/25/data-structure-part-II.html) | next ( coming soon) >

___

_So we finished our discussion about Set and Multiset data structure._ 🙌

_I hope you have a clear idea how to work. If you found this article helpful or if you find something I miss out or that you like it, feel free to let me know._ 😁



















