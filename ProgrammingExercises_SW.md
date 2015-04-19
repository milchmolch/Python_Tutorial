# Introduction to Python

## Python programming exercises

Stefan Wyder
 

**(Advanced) Birthday problem**   
(data structures, functions, looping)   
Let's do a simulation on the Birthday problem: What is the probability that students of a class have the same birthday?

As always write first a simple script and test it. Fix it if necessary. Once it works, add more and more functionality.

For simplicity, we don't take into account leap years.

1. First calculate the probability for a class with 20 students.  
Tip: Use function `randrange()` from the built-in module `random` to get random integer numbers.

2. Once you have a working script, get a better estimate by repeating the sampling many times, e.g. 1000 or 10000 times.

3. Calculate the probabilites for class sizes 5,10,15,..,50 and plot a graph.

4. Once you are confident about the results of your script check the [wikipedia entry](http://en.wikipedia.org/wiki/Birthday_problem).

5. Take into account leap years.

6. What is the minimal group size necessary for a 50% chance to for 3 people having a common birthday?