# Artificial intelligence, the near future of IT

## Theory evolution
* old theory
* new theory
  * more general
  * more precise
  * more simple

## Black box
* something we do not know or understand
* something we cannot see
* something we do not care about

Abstraction over abstraction over abstraction...
(e.g., machine code, assembler, C code, Java code, ...)

The more abstraction layers there are, the more blackboxes we rely on.

## Future
* test automation (weak AI)
* partial development automation (weak AI)
* full development automation (weak AI)
* strong AI

## Weak AI
Weak AI is all around us.

* emulates a single competence of humans (e.g., ability to recognize letters)
* emulates single sense (e.g., sight) 
* emulates a single problem (e.g., play chess, go, ...)

## Strong AI
Does not exist yet
* has all human competences
* is able to resolve any problem (e.g., turing test)
* has consciousness
* has feelings
* has self-awareness
* has creativity

## Neural networks
Give some data to it, let it learn: show it pairs of inputs and expected outputs.
After training, the neural network can make correct decisions.

Can be treated like a blackbox: we don't trace back the origin of an incorrect decision made by a neural network, we just make it learn better by feeding it with more data.

## Genetic algorithm
* Population
* Selection
* Crossover
* Mutation

We have a population, we select the best then for each element we analyze if good enough or not then we mix the best ones, we mutate and get a new population.

Evolutionary algorithm.

Again, can be treated like a blackbox, we don't analyze how the algorithm ended up with some result.

## Aquarium metaphor
Two people looking at a big aquarium and watching the fish swimming there.
The two people can discuss about the fish they like best.

For a computer, comparing and analyzing that kind of thing is very difficult:
* the data is not precise (no measurement tools, we just look)
* the environment is fuzzy and dynamic (the fishes move very fast)
* we have a relative perception (each of us has a difference experience, different color recognition and different position)

Our strength with this kind of problem, as humans, is that we are imprecise, a bit fuzzy.

As humans, there's just too much data for us to analyze, so we have to rely on approximations, feelings, experience, etc.

## Turing test
Recognize a machine from a human being.

Can we have a conversation with a machine and not be able to realize that it is a machine.

So far, no machine has passed the turing test.

## Machine classification of images
Over time, machines get better and better at recognizing images. In 2015, machines have surpassed humans.

As humans, we start being unable to verify the results obtained by machines!

## Machines vs Humains (IQ)
When machines will be able to learn by themselves, we'll reach the "singularity", the point in time where the IQ of machines will surprass ours.

At that point, either we start learning from machines, or we just ignore it and don't improve ourselves.

## IT evolution
Nowadays, many things are considered as black boxes and we don't care much anymore: mathematics, hardware, algorithms, complexity, ...
Over time, more and more things get abstracted away and receive less attention.

Now we are looking more and more and patterns and less and less about other things.

## Evolution
Went a long way since deep blue.

* deep blue 1997
  * brute force approach
  * 30 nodes x 120 MHz (11.38 GFLOPS)
  * 200 million chess positions per second
  * search to a depth of 6-8 to max 12 moves
  * base of 700.000 grandmaster games (memory)

Game of go has very simple rules, but the possibilities are huge: more than 10^1000 possible games.

Key to winning: imagination and global situation analysis (big picture)

* alpha go deepmind 2016
  * trained itself by playing against itself: deployed on the cloud
  * difficult to actually assess how strong it has become
  * we can understand what AlphaGo is doing but we don't know how

Next step:
* we don't know how it works but we know what it does
* at some point we will only understand the purpose (why it's done), but not how/what
* at some later point we won't even know why it's doing what it's doing