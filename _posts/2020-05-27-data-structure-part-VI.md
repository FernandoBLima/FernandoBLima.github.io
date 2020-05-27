---
layout: post
title: Trie - Data Structure & Algorithm Part VI
tags: [Algorithm, Data Structure, Javascript]
author-id: fbl
excerpt_separator: <!--more-->
---
In this post our main goal is to understand the Trie Data Structure, learning the concepts, how it works, and how to implement it (a.k.a code!).

<!--more-->

It is important to understand the tree structure before diving into the trie. So, if you need to, you can read the last post about the [tree and binary search tree](https://fernandoblima.github.io/2020/04/21/data-structure-part-V.html).

Moving on, let’s discuss the data structure journey! 😁

> 💭 "Those hours of practice, and failure, are a necessary part of the learning process."- Gina Sipley

___

### Outline
The article is divided into the following parts:
- Understanding the Trie structure;
- The main operations


#### ◼️ Trie

- [x]  Pre-requisite : Tree

We can say that the trie structure stores a set of strings that can be visualized like a tree where each node is a character. This structure is stored in a top to the bottom manner and the order that appears is based on the prefix of a string that all the descendants' nodes have in common.

But what do I mean about prefix? 🧐

<div style="text-align:center"><img src="https://media.giphy.com/media/a5viI92PAF89q/giphy.gif" /></div>


Let's consider using the word 'Batman' for the set S of n strings to clarify our mind. 

```javascript
S1 = { B,a,t,m,a,n }
```

First all, the root of the structure is started with a node with value ```ε``` that represents the empty string. The next inserted node has the first value in the set S1 that is 'B'. Then, the next node to be used is the value 'a' and so on. 

As we can see, each node can have several child values ​​(or not). At most, the size of the alphabet that the children are connected to, in our case can have up to 26 children.

So, let’s see an example using the word that we are discussing.

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/3tn85tii8fieevlcrg05.png" /></div>


###### <center>Figure 1: Inserted a new word</center>

Great! Let's use this structure and add a new set that has the word 'Bat', using as the set S2 of n strings.

```javascript
S2 = { B,a,t}
```

Here, the first letter 'B' of the set S2 has already been inserted in the first node. Therefore, we do not have to create another node, and the same happens to the letters 'a' and 't'. As a consequence, just need to mark the letter 't' as the end of a word.

See the next figure below that shows a trie with the words “Batman” and “Bat”.

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/8jdjndvqyat26xc02ypy.png" /></div>

###### <center>Figure 2: Inserting a word that already have the as prefix in the structure</center>

What's happens if we add the word 'Batgirl'? 

```javascript
S3 = { B,a,t,g,i,r,l}
```

As we discussed earlier, the structure already have the letters 'B', 'a', and 't'. So, just needs to create the node for other words. See below:

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/f4zqms4k8ra4nmone688.png" /></div>


###### <center>Figure 3: Inserting a word that already have a prefix </center>

What if we add a word that starts with a different letter instead of 'B'? Don't worry, just need to insert a new node with a value. In this example, we will add the word 'Joker', in this way, the letter 'J' will be added after the node that represents the empty string. Keep in mind, don't forget to mark the last letter at the end of the word.

<div style="text-align:center"><img src="https://dev-to-uploads.s3.amazonaws.com/i/qkaasdw2wd93dpiolv82.png" /></div>

This happens with other words that can be added to our structure, such as Penguin, Ivy, Two-Face, and so on.

<div style="text-align:center"><img src="https://media.giphy.com/media/8fbeFbshnfyJW/giphy.gif" /></div>


###### <center>Figure 4: Inserting a word that start with a different first letter</center>

After all, why should we use this structure? Why not use the tree structure? Well, the trie structure is faster compared to the tree and hash table because we do not need to calculate any hash functions or worry about dealing with collisions.

Awesome! Now that we understand the behaviors and how we can add values, let's build our structure. At first, we need to create our main class. 

Talk is cheap. Let’s see the code. 😁

```javascript
class TrieNode {
    constructor(isEnd, value ) {
        this.children = {};
        this.isEndOfWord = isEnd;
        this.character = value;
    }

}

class Trie {
    constructor() {
        this.root = new TrieNode(true, '*');
        this.length = 0;
    }

    ...

}
```

Each TrieNode represents a letter in the structure and has the following parameters:
- children: As we discussed above, there can be more than one child.
- isEndOfWord: Represents if the letter is the end of the word.
- character: Is the node value.

Done! 😁

But not entirely! We need to create and add methods to our class. The implementation of insert, search and delete functions are a simpler way to implement these functions using Javascript, and all of these operations have the complexity of time O(L) where L is length of key. 

Let's check out:

- Insert

As mentioned earlier, this structure starts with a node that represents the empty string. We have to insert the first character of the set of strings, but if the value to be inserted has already been added, we just have to get down to the next level and continue to add the following values from the set.

However, if at some point there is no node, we will have to create and continue the process until the whole set is inserted, and of course, mark the last value of the set as the end of the word node. The space complexity of this structure in the worst case is when the word to be inserted is higher than the maximum number of nodes in the structure.
 
```javascript
    insert(key){
        var currentValue = this.root;
        
        for (let index = 0; index < key.length; index++) {
            const element = key[index];
            if (currentValue.children[element]) {
                currentValue = currentValue.children[element];
            } else {
                this.length++;
                const newNode = new TrieNode(false, element);
                currentValue.children[element] = newNode;
                currentValue = newNode;
            }
        }
        currentValue.isEndOfWord = true;
    }
```

- Search

Searching a string in this structure is a simple approach, we just have to iterate all characters of the set starting at the root and checking if the value matches and moving down to the next node. If the last letter used in the process is marked as the last node, then the set belongs to the searched word.

However, we can say that the set S is not present in the trie when:
- There is no transition for children nodes and there is still a letter in the set.
- If all the letters have been consumed and the last node in the process does not correspond to the string.
- Or all characters exist in the structure, but the last letter is not marked as the end of the word node.

```javascript
    searchWord(key){
        var currentValue = this.root;
        for (let index = 0; index < key.length; index++) {
            const element = key[index];
            if (currentValue.children[element]) {
                currentValue = currentValue.children[element];
            } else{
                return null;
            }
        }
        return currentValue;
    }
```

- Suggestion Word

The main goal of this function is to show all words that have a prefix in common. At the beginning, is searched if the set of string already has been inserted in the structure and returns a list that contains all words that contain the word as a prefix.


```javascript

    suggestionWord(key) {
        var word = this.searchWord(key);
        if(word){
            var suggestions = [];
            if(word.isEndOfWord){
                suggestions.push(key);
            }
            return this._suggestionWord(word, key, suggestions);
        }
        return [];
    }


    _suggestionWord(node, lastWord, suggestions){

        var letters = Object.keys(node.children); 
        for (let index = 0; index < letters.length; index++) {
            const element = letters[index];
            if(node.children[element].isEndOfWord){
                suggestions.push(lastWord + node.children[element].character);
                this._suggestionWord(node.children[element], lastWord + node.children[element].character, suggestions);
            }else{
                var rest = lastWord + node.children[element].character;
                this._suggestionWord(node.children[element], rest, suggestions);
            }
        }

        return suggestions;
    }

```

- Remove

In this function, the word is removed from the structure if contains the prefix and does not have any other words that use as a prefix.

```javascript
  remove(key) {
        if(this.search(key)){
            return this._removeNode(this.root ,key, key, 0);
        }else{
            return false;
        }
    }

    _removeNode(node, keySlice ,key, index) {
        var letter = key[index];
        var current = node.children[letter];
        if(current){
            keySlice = key.slice(index + 1, key.length);
            var shouldRemove = this._removeNode(current, keySlice, key, index + 1 );
            if(shouldRemove && !this.hasChild(node.children[letter].children)){
                this.length--;
                delete node.children[letter];
                key = keySlice;
                return true;
            }else{
                return false;
            }
        }
        return true;
    }

```
___

That's all folks! I hope you have fun learning. 😁

<div style="text-align:center"><img src="https://media.giphy.com/media/5DQdk5oZzNgGc/giphy.gif" /></div>


Code: https://github.com/FernandoBLima/data-structures

___

_So we finished our discussion about Trie structure._ 🙌

_I hope you have a clear idea how to work. If you found this article helpful or if you find something I miss out or that you like it, feel free to let me know._ 😁
