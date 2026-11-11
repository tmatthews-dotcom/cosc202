# Lab 9

In this lab we will continue working on the sequence segmentation problem from the previous labs. Specifically, you will design a recursive brute force algorithm that is guaranteed to give you the **optimal** segmentation. 


## Problem description 

Like before, you are given a sequence and and a vocab, and your task is to break it down into segments, where each segment is a word from the vocab. Crucially, you want to return a segmentation that **maximizes the log probability of the entire sequence**. 

In the context of this lab, we will define the log probability of a sequence under a particulat segmentation as the sum of log probabilities of all the segments. 

For example, consider the following vocabulary where each word is associated with a log probability. 

```
vocab = {
	a 	: -1.3,
	an 	: -1.6,
	at 	: -2.0,
	ate 	: -2.7,
	not  	: -1.9,
	her  	: -2.2,
	other 	: -2.4,
	another	: -3.0,
	tea 	: -2.5,
	team 	: -2.6,
	eat 	: -2.5,
	eats 	: -2.7,
	meats 	: -4.2,
	spot 	: -3.3,
	pot 	: -3.3

}
```


Given this vocab, here are the correct input-ouput mappings: 

* `s = ateapot`. Output = `[a, tea, pot]` (the alternative `[ate, a, pot]` has a lower probability)

* `s = anotherteameatspot`. Output = `[another, team, eat, spot]` (all the other alternatives have a lower probability)

* `s = apotmeat`. Output = `null` (`meat` isn't a word in the vocabulary, so there is no valid segmentation)

Assume that the log probability is stored in the Trie in the node at the end of the word (i.e., the node where `node.isWord` is True). You can access the this with `node.logprob`


## Graph representation review

As a reminder, here is how we represented segmentation as a graph last lab.

Given a sequence with `n` letters, we build a graph with `n + 1` nodes, numbered` 0` to `n`. Node `i` represents the position just before letter `i` (with node n representing the end of the sequence), so each node corresponds to one entry of the DP table from the previous lab. There is a directed edge from node `i` to node `j` if `seq[i:j]` is a word in the vocab (using zero indexing, with j not included). We label each edge with its word.

# Step 0: Creating a shared collaborative document

**One** person in your team should create a copy of [the google doc template for this lab](https://docs.google.com/document/d/1L-hGWiVqJ1cKoqfVwjUc4UvL1LrRZDFgpxtlwpHL8wM/edit?usp=sharing), and share it with your group. 


## Step 1: Modifying the graph building algorithm

Start by answering the following questions: 

1. At an abstract level, how can you modify the graph representation to also include the log probabilities of words? 

2. How would you modify the data structure you used to store the graph to incorporate the modification you described? 

Now, modify the graph building algorithm from last lab to incorporate the change you described. **Make sure to use the more efficient algorithm!**

Finally answer the following question: 

3. Does the modification change the asymptotic time complexity to create the graph or the asymptotic space complexity to store the graph? Why or why not? 


## Step 2: Understanding and analyzing a greedy solution to the problem

Here is a greedy algorithm that attempts to solve this problem: 

```
greedy(seq, edges):
    n = seq.length
    segments = []
    i = 0

    while i < n:
        if edges[i] is empty:
            return null                       
        best_j = null
        best_w = -infinity
        for (j, w) in edges[i]:
            if w > best_w:
                best_j = j
                best_w = w
        segments = segments + [seq[i:best_j]] 
        i = best_j

    return segments
```


Answer the following questions: 

1. What is the greedy rule? How does the algorithm use the greedy rule to return a segmentation. 

2. What is the asymptotic upper bound time compleixty for this algorithm? What is the space complexity? (In your answers, assume that the graph is already built)

3. Using the vocab above, come up with an example sequence where the greedy rule returns the correct optimal segmentation. 

4. Using the vocab above, come up with an example sequence when the greedy algorithm returns `null` when there is a valid segmentation. Describe what the correct segmentation is, and why the greedy algorithm fails. 

5. Using the vocab above, come up with an example sequence when the greedy algorithm returns a non-optimal segmentation. Describe what the correct segmentation is, and why the greedy algorithm fails. 


## Step 3: Designing a brute force algorithm for a simpler problem 

In this step, you will design a brute force algorithm for a simpler problem: return the score of the best segmentation. If no segmentation exists, return -infinity (which is what log(0) is). 

Do the following: 

1. Write a recurrence relation that expresses the solution to the segmentation problem as the solution to sub-problems. 

2. Design a brute force algorithm that implements your reurrence relation. 

3. Analyze the time complexity of your algorithm by answering: 
	* What is the upper bound on the number of recursive calls in terms of `n` which is the total length of the sequence and you did not know anything about the vocab? (*Hint: think about what the worst case graph looks like, and how many paths from start to end need to be considered in this worst case graph*)
	* What is the time per recursive call (excluding the work of making the recursive call)? 
	* What is the overall time complexity? 

In answering question 3, remember the following: when recursive calls are made within a loop iteration, it is important to not double count the time for the iteration. If every loop iteration creates exactly one recursive call, then the cost of that iteration can be charged to the call it creates. In other words, instead of saying “each call runs a loop of up to d iterations,” reason about **the total number of loop iterations** across all recursive calls: that’s the same as the total number of calls (minus the first one). 

So if the loops run `m` times in total across all calls (making `m` recursive calls), and each iteration takes `x` time besides the call itself, and there is `y` work being done outside the iteration, then the total time complexity is `O(m · (x+y))`. 

## Step 4: Designing a brute force algorithm that operates over implicit graphs

In this step, you will design a brute force algorithm for the simpler problem -- return the score of the best segmentation -- but using the Trie as an implicit graph (instead of working with an existing graph)

Do the following: 

1. Write a recurrence relation that expresses the solution to the segmentation problem as the solution to sub-problems using the Trie as an implcit graph. 

2. Design a brute force algorithm that implements your reurrence relation. **Make each recursive call as efficient as possible by taking advantage of the Trie structure**

3. Analyze the time complexity of your algorithm by answering: 
	* What is the upper bound on the number of recursive calls in terms of `n` which is the total length of the sequence and you did not know anything about the vocab? 
	* What is the time per recursive call (excluding the work of making the recursive call)? 
	* What is the overall time complexity? 

4. Based on your answer to 3, is it worth storing the graph if you were going to implement the brute force algorithm? Why or why not? 

In answering question 3, consider the following: does every iteration make exactly one recursive call? 

## Step 5: Submit the write-up

* **One** person in your team should export the google doc as pdf and upload it to gradescope under Lab 9

* **Make sure to add all the group members to the submission**



