---
layout: post
title: Dictionary and HashTable -  Data Structure & Algorithms Part II
tags: [Algorithm, Data Structure, Javascript]
author-id: fbl
# feature-img: "assets/img/thumbnails/postII.jpeg"
# thumbnail: "assets/img/thumbnails/postII.png"
# image: "assets/img/thumbnails/postII.png" #seo tag
excerpt_separator: <!--more-->
---
<p>Continuing our journey in the data structure and algorithms in a galaxy far, far away... </p>

In the previous post, we had learned about [linked list, queue and stack.](https://fernandoblima.github.io/2019/10/25/data-structure-part-I.html) Now we will continue our journey and move on to covering the Dictionary and HashMap data structure.

<!--more-->
In this post, I will try to help you to understand these data structure. Once again, I will use JavaScript code examples; however, the main goal is not to go deeper in the language, but to try to demonstrate what makes these structures unique. You can find this structure implemented in several languages such as Python, JavaScript and so on and also in various algorithms, so understanding the structure behind the code it’s important, because what is the point of just learning code?

> 💭 “You Can't Write Perfect Software. Did that hurt? It shouldn't. Accept it as an axiom of life. Embrace it. Celebrate it. Because perfect software doesn't exist. No one in the brief history of computing has ever written a piece of perfect software. It's unlikely that you'll be the first. And unless you accept this as a fact, you'll end up wasting time and energy chasing an impossible dream.” 
― Andrew Hunt, The Pragmatic Programmer: From Journeyman to Master


### Outline

The article is divided into the following parts:

- Understanding what is Dictionary and Hash table.
- How important is a hash function.
- Code implementation and complexity analysis.
- What is factor load.

◼️ Dictionary 

Dictionary, which some people prefer refer as map structure, is a collections of pairs _[key, value]_ of distinct elements that use a key to find a value. A little bit confusing, right? I will try to explain in a different way.


As the name suggest this structure is like a dictionary book, where we can use as an example of being applied to a real-world when you search and found a word followed by his definition. 📚 In our case, the word is the key and the description is the value stored.

At first, you might be wondering if there is some way we can use what we had learned in the last post and use the linked list to create this structure, right? Of course! We can use but we have to adapt the structure adding the key property because a linked list add a new element at the beginning of the list, resulting in an _O(1)_ complexity of time. If we want to delete some value, we need to search the key and as you can remember, is not so efficient. So how we can build this structure? Programming is a kind of magic and we can implement in different ways, let&#39;s discover together! 🧐


◼️ Bucket array

As we saw, the linked list couldn’t be used; on the other hand an array can solve our problem. However, do you know what an array is? It is a collection with _N_ elements where each position, called as bucket, in the array can have a value stored. I will try to illustrate in the following figure an array with an element at position 8.

<div style="text-align:center"><img src="https://thepracticaldev.s3.amazonaws.com/i/6ax97be32zt6k4qduzxv.jpg" /></div>

###### <center>Figure 1: An array illustration</center>

In a bucket array, we can use a key to identify any value stored, like a dictionary book. To get a better understanding of how it works why not create an example to store a key-value pairs. Suppose we have an array and we want to add some value let’s take a look at the example:

```javascript
var bucketArray = [];
key = 1;
value = 'Darth Vader';
bucketArray[key] = value;
```

Yeah! We got it! 🙌 It was added the value in to our array using a key. The element stored in the hash table is quickly retrieved using the key. We can add, delete and search the pair value _[key, value]_ with the _O(1)_ constant time. Great! All the problems were solved, right? No, unfortunately. ☹️🥺

Look at the following example assuming that both of our keys has the same value in this case 1.

```javascript
var bucketArray = [];

key = 1;
value = 'Darth Vader';
bucketArray[key] = value;

key = 1;
value = 'Obi Wan Kenobi';
bucketArray[key] = value;
```

Do you know what happens when the value 'Obi Wan Kenobi' is added using a key that already have being used? Collision! 💥 And bug! 🐞 We can't add the value because the key has to be unique.  With this in mind the bucket array didn't resolve all our problems. ☹️ 

<div style="text-align:center"><img src="https://media.giphy.com/media/xHwqspaBmfUMU/giphy.gif" /></div>

◼️ HashTable

We don’t need to be hurry about that! We can create a function to convert the key in an integer to resolve and handle our problem. Then using the hash value created we can use as an index in our array to avoid the collisions and that is what makes the hash table particularly useful. Is it confused? I will try to explain.

We need to keep in mind that the hash table is another approach to implement the dictionary data structure and the difference between them is by the fact how we can store and access data. Just remember that a hash table is composed with two parts, an array and hash function.

<div style="text-align:center"><img src="https://thepracticaldev.s3.amazonaws.com/i/iudr032c4fyl2zb0gsas.jpg" /></div>

###### <center> Figure 2: A example of hash table </center>

Talk is cheap. Show me the code! 😁 Our main hash table class would looks something like this:

```javascript
class DumpHashTable {
    constructor() {
        this.list = {};
        this.length = 0;
    }
}
```

 * Hash Function

In order to understand hash table we first need to know what the purpose of hash function is. As I said before, the main goal in a hash function is to convert a key in an integer and try to minimize the collision that can happen when we are adding a new value in the array. 

In this function, the key is the input parameter and has a range between 0 and infinite and we need to distribute the keys uniformly across an array. It is necessary to reduce the value of the key and compress in the map function to convert in a range between _0_ and _N - 1_, where N is the length of our array. Suppose we have an array of size _10_ and our key has the value _23_, it doesn’t fit because the value is larger than the size. Therefore, we need to compress the key into the size of the array.  

<center>hash(x) : x → {0, 1, 2, N − 1}</center>

There are many ways to achieve a good hashing mechanism, let’s take a look in the most common function, the modulo operation.

- Mod

Suppose our array has length N and we need to add a new value. Then is necessary to convert the key into the array size using the mod operation, which result in the hash value, right? 

<center> hash(x) = x mod N </center>

However, we cannot choose a random number to be used in the mod operation because we want to avoid clusters. If we choose a small number or a hash value that has many multiples we will get similar values, and as a result, the hash table will not be distributed. Let's consider a table of size 24 and assuming we have a set of keys between 0 and 100 in a uniformly random distribution.

<center> 𝐾 = {0,1,...,100} </center>

Every number in 𝐾 that has a common factor with the number 24 will be hashed as multiple of this factor; in this case, the factors of 24 are 1, 2, 3, 4, 6, 8, 12 and 24. That is to say, the values won’t be spread over all possible value between 0 and the array size. 

```
24 % 24 = 0
48 % 24 = 0
72 % 12 = 0
```

We can use a large prime number to avoid this problem, using a value we can spread more the hash values over all possible index between 0 and the array size, and as consequence, every value stored in the array will be within the range of prime number. 

To minimize collisions it is important to reduce the number of common factors and choosing a prime number is how we can deal with because are the only number that have two different dividers: 1 and itself. For instance, let’s take a closer look in the following image where 100000 values were generated between the range _{0,1,...,1000}_ in a normal distribution using 97 and 100 mod value. Can you notice which one is the best option?

|  ![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/cb9gvz5s4ggj91wz24nw.png)   | ![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/80ne5szx4rp43l3uvq9u.png)  |
| :------------: | :------------: |
|   |   |

###### <center> Table 1: Comparing a hash function using a normal distribution using 97 and 100 mod value</center>

We can have the same result using uniform, triangular and exponential distribution. 

|  ![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/37rwd3txbct4eqpeycqa.png)   | ![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/1yx9p6goa5ou1qucud7n.png)    |  ![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/143ahpwf9y3ql249n6kp.png)  |
| :------------: | :------------: | :------------: |
| ![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/y3jau61fsa6y70rwkyx3.png)  |  ![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/eshllfhuhsmhvmixtusn.png)  |  ![Alt Text](https://thepracticaldev.s3.amazonaws.com/i/7hd02dmv8ze31780ikil.png) |
|  UNIFORM  |  TRIANGULAR  |   EXPONENCIAL  |

###### <center> Table 2: Comparing others distributions using 97 and 100 mod value</center>

Allright, now that we understand how deal with hash function; we can see how our hash function would be considering the last example given:

```javascript
hash(data){
   return data % 97;
}
```

We also can use a string instead a number to use as the key, we just need to sum of the ASCII values of the characters in our string as we can see

```javascript
hash(data){
    var hashTable = 0;
    for(var i = 0; i < data.length; i++){
       hashTable += data.charCodeAt(i);
    }
    return (hashTable) % 97;
}
```

> Another common hashing mechanism is the MAD (Multiply, Add, Divide).


◼️ Collision Handling

Even though we use some hash function sometimes it is almost impossible to create a uniform random distribution to avoid collisions. Therefore are many ways to handling collisions, as we can see below.

- Separate chaining

We use this method when the key is already used, which means it is impossible to store a new value. We can handle this situation creating in the index a point to a linked list structure to store the new value into our array, in this way; the number of keys can exceed the size of the hash table. Nevertheless, is required more space to store the keys using the linked list, and at the same time, some buckets could be never used, which leads to wasted space.

In addition, using a linked list bring us to the disadvantage of searching and deleting values and to minimize this problem is common to limit the number of values that can be inserted in the linked list. The worst scenario of separate chaining is when all values are inserted at the same index and all the keys will be in only one linked list. To give an illustration of this approach, let’s look at the following image.

<div style="text-align:center"><img src="https://thepracticaldev.s3.amazonaws.com/i/agsibwixpb6asp3id0rq.jpg" /></div>

###### <center> Figure 3: Using separate chaining approach to handle collision in hash table. </center>

```javascript
    insert(value) {
        var key = this.hash(value); 
        let indexHash = new IndexHash(value, key);
        if(this.list[key]){
            indexHash.next = this.list[key];
        }
        this.list[key] = indexHash;
        this.length++;
    }
```
◼️ Open addressing

Another way to improve the hash function is using the open addressing approach. On contrast of separate chaining, all values are stored in the bucket array and the hash table can never exceed the size. There are different ways to implement and the most common approaches are:

- Linear Probing

Our hash function that we are working on it happens to have collision on the index; one way to resolve is increasing the index and check if the next element on the bucket array is available to insert the new value.

<center> hash(key) = (hash(key) + i) mod N </center>

The probing sequence for linear probing will be:

newIndex = (index + 0) % hashTableSize <br/>
newIndex = (index + 1) % hashTableSize <br/>
newIndex = (index + 2) % hashTableSize <br/>
newIndex = (index + 3) % hashTableSize <br/>
and so on…

We have to iterate the array to check if the index of hash value of the _'hash(key) + i'_ is available. We can see how it works:

```javascript
    insert(value) {
        try{
            var key = this.hash(value);
            key = this.proibingHash(key, value);
            let indexHash = new IndexHash(value, key);
            this.list[key] = indexHash;
            this.length++;
        }
        catch (error) {
            return error.message;
        }   
    }

    proibingHash(key){
        if(this.list[key] == null){
            return key;
        }else{
            let flag = false;
            let index = 1;
            do{
                if(index >= this.ARRAY_LENGTH || this.length == this.ARRAY_LENGTH){
                    throw new Error('Error! Array size exceeds');
                }else{
                    let indexTable = index;
                    indexTable = key + indexTable;
                    if(this.list[indexTable] == null){
                        flag = true;
                        index = indexTable;
                    }else{
                        index++;
                    }
                }
            }while(flag == false);
            return index;
        }
    }
```

In _proibingHash_ function, we iterate the array to check if the next index is available and if the size is exceed. It is important to say that the remove function has a similar logic of insert function, as we can see in the code bellow:


```javascript
    remove(value){
        if(value == null){
            return false;
        }
        var key = this._hash(value);
        key = this.removeProibingHash(key, value);
        if(this.list[key]){
            this.list[key].value = null;
            this.length--;
            return true;
        }else{
            return false;
        }
    }

    removeProibingHash(key, value){
        if(this.list[key] && this.list[key].value == value){
            return key;
        }else{
            if(this.list[key] == undefined){
                return null;
            }
            let flag = false;
            let index = 1;
            do{
                if(index >= this.ARRAY_LENGTH || this.length == this.ARRAY_LENGTH){
                    return false;
                }else{
                    let indexTable = index;
                    indexTable = key + indexTable;
                    if(this.list[indexTable] && this.list[indexTable].value == value){
                        flag = true;
                        index = indexTable;
                    }else{
                        index++;
                    }
                }
            }while(flag == false);
            return index;
        }
    }
```

> ⚡️ If you would like to know how I implemented, you can access the code just clicking [here](https://github.com/FernandoBLima/data-structures). 

- Quadratic Probing

Okay, we talked about how linear probing can be useful, but let’s spend a minute to talk about the disadvantages of this approach. The biggest problem is the fact that can occur clusters when many elements are in the consecutive array index. Just imagine the following scenario where our bucket list has more than 1 million of element and we need to add a new element which index already was stored.

Consequently, we have to go through many indexes to find an empty space in the array. Can you see that linear probing is not so efficient? It could take time to search an element or find an empty bucket. The biggest problem is when clustering of values in our array occur. We might want to solve this problem using a different probing approach, which lead us to the quadratic probing. Instead, add the index we have to add the power of the original index.

<center> hash(key) = (hash(key) + I^2 ) mod N </center>

The sequence will be:

newIndex = hash(key) % hashTableSize <br/>
newIndex = (hash(key) + 1^2 ) % hashTableSize <br/>
newIndex = (hash(key) + 2^2 ) % hashTableSize <br/>
newIndex = (hash(key) + 3^2 ) % hashTableSize <br/>
and so on…


On the other hand, depending on the size of the array an infinite loop may be created and not able to add the new element. 

- Double Hashing

Here we have a different approach comparing to linear and quadratic probing, because a secondary hash function is used as a fixed increment in the jump distance, an advantage is that we can use a unique jump value. 

<center> hash(key) = (hash1(key) + j hash2(key)) % hashTableSize </center>

Where _j_ is the index, the probing sequence will be:

newIndex = (hash1(key) + 1 * hash2(key)) % hashTableSize; <br/>
newIndex = (hash1(key) + 2 * hash2(key)) % hashTableSize; <br/>
and so on…

Furthermore, as we can see the open addressing, such as linear, quadratic and double hashing has almost the same drawback and we cannot exceed the number of spaces in the bucket array.  

◼️ Time complexity 

In general, we can say that the time complexity in big O notation is:


<div style="text-align:center">
   Algorithm    | Average                       |   Worst case                         
----------------|-------------------------------|----------------------|
 Search         | O(1)                          | O(n)                 |
 Insert         | O(1)                          | O(n)                 |
 Delete         | O(1)                          | O(n)                 |
</div>

###### <center> Table 3: The time complexity of Hash table</center>

◼️ Load Factor

Now we will discuss the relationship between the number of entries and buckets, the load factor, which is equal to the number of elements divided by the number of buckets.

$$
\lambda = \frac Nn
$$

It is expected to have emptier bucket to accommodate all the elements that we inserted in our bucket, resulting in a load factor less than 1. When is more than 1 is necessary to rehashing, which means to increase the number of buckets and change the hash function, otherwise, the element can't be add into our array.


#### ◼️ Conclusion 

That’s it! The Hash table is an extended topic and is almost impossible to cover everything in just only one article. However, as we can see, it is crucial to understand how and why the data structure is used, even though a linked list could be used to create a structure of collections of pairs _[key, value]_ of distinct elements, the result won't be so efficient.  

Which makes us to use the bucket array that have the speed advantage, where we can access a value in a constant time _O(1)_, however, many values can be added resulting in collisions. We have learnt that there are many ways to build a hash function to avoid this behavior but sometimes is almost impossible to create a perfect function, which can make this structure quite inefficient when many collisions occur. As consequence, some approaches were developed to try to solve or handle this issue but each one has advantage and drawbacks.

All of this points to the conclusion that by comparing the approaches we can see that we do not have a better one, because depend on context and other factor, such as where an extra space is needed or not, or even whether the number of keys to be stored cannot be exceeded, for instance.

___

That's all folks! Now that we had a chance to discuss this data structure I hope you keep coding and having fun. 🤓

Code: https://github.com/FernandoBLima/data-structures


< [previous](https://fernandoblima.github.io/2019/10/25/data-structure-part-I.html) | next ( coming soon) >

___

_So we finished our discussion about Dictionary and Hash Table data structure._ 🙌

_I hope you have a clear idea how to work. If you found this article helpful, if you find something I miss out or that you like it, feel free to let me know._ 😁

___