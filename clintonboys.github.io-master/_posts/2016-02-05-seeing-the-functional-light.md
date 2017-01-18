---
layout: post
title: Seeing the functional light
image:
  feature: sample-image-37.jpg
  credit: Cuneo, Italy, 2016
---

I learnt to program in Python in high school. I didn’t take computer science as a high school subject, but rather took a once-a-week extracurricular course with a mild-mannered teacher called Mr Huxley and a few other kids. I learnt the basics of control structures, solved basic programming problems, and entertained myself by “polluting the namespace” with variables whose names were offensive jibes against my teammates:

~~~~
while peter.is_an_asshole():
 	print 'Peter is an asshole!'
~~~~    

Over the next ten years I came to depend on Python as my only programming language, using it for scripting, data munging, scraping, and more sophisticated data analysis and machine learning through a bunch of great libraries. 

Recently I’ve been learning about functional programming. I have a very mathematical background, so I was instantly struck by how similar many formalisms in functional programming are to mathematics. You can see mathematical definitions and descriptions of sets, proofs by induction, formal constructions of objects, all translated very neatly into a programming paradigm. 

**An axiomatic approach**

For example, introductory courses on functional programming usually contain some sort of exercise which walks the student through constructing the integers, or the rational numbers. This is an important mathematical idea that is somewhat obscured by programming procedurally.

In an exercise I did in the early stages of learning functional programming in Scala, I had to define sets in a functional way. 

```
object FunSets {
  type Set = Int => Boolean
  def contains(s: Set, elem: Int): Boolean = s(elem)
    def singletonSet(elem: Int): Set = (x: Int) => if(x == elem) true else false

    def union(s: Set, t: Set): Set = (x: Int) => s(x) || t(x)
  
    def intersect(s: Set, t: Set): Set = (x: Int) => s(x) && t(x)
  
    def diff(s: Set, t: Set): Set = (x: Int) => s(x) && !t(x)
  
    def filter(s: Set, p: Int => Boolean): Set = (x: Int) => if (p(x)) s(x) else false
}
```

The above code will seem very strange for someone not used to functional program, but it perfectly encodes the mathematical idea of a set of integers as a map with domain the integers and a boolean range (i.e. the "set membership function" for a set S maps an integer to True if the integer is a member of S, and to False otherwise). This allows for very elegant descriptions of common set operations like union, intersection and difference by simple logical operators on the function values. 

**Modularity and readability**

Another moment of clarification when learning functional programming comes the first time you are able to distill a complicated “pipeline” formula into a single elegant line of code. Programming functionally teaches you to break all actions into their smallest possible pieces which build upon each other to produce more complicated actions and procedures mean the ultimate expressions of what your code is trying to do become very succinct and usually extremely readable. 

```
    def decode(tree: CodeTree, bits: List[Bit]): List[Char] = {

      def iterate(remaining: CodeTree, bits: List[Bit]): List[Char] = remaining match {
        case Leaf(c, _) if bits.isEmpty => List(c)
        case Leaf(c, _) => c :: iterate(tree, bits)
        case Fork(left, right, _, _) if bits.head == 0 => iterate(left, bits.tail)
        case Fork(left, right, _, _) => iterate(right, bits.tail)
      }
    iterate(tree, bits)
  }
```

The above code uses Huffman coding to decode the bit sequence `bits` using the code tree `tree` and returns the sequence of actual characters encoded by the bit sequence. Although good programmers will do this anyway, functional programming teaches the modularity evident in this example: in order to define `decode` we must first define the classes `CodeTree`, `Leaf` and `Fork`, as well as the function `iterate`, and the methods `isEmpty`, `head` and `tail` on a `List[Bit]` object. Changes made to any of these smaller pieces then propograte throughout the code. 

**Typing**

The strangest thing about functional languages for someone who is learning about them for the first time is that they are usually strongly typed, which represents a very alien departure from Python. In Python variables rarely need to have their type declared. This is a great thing for quick scripting, as it can make the translation from English to code much quicker and easier. 

What is difficult to understand (and I didn’t believe was possible at first) is that *nearly every single bug* in any program that causes it to fail at runtime is caused by an issue that could be caught by a strong typing system at compile time. 

**Conclusion**

Of course there is no “winning” paradigm here. People argue about the benefits of procedural versus functional programming, static versus strong typing, all the time, attempting to demonstrably show which is the better style. I think it’s great to know both. First of all, without knowing both paradigms it is impossible to be aware of the weaknesses and limitations of the one you use exclusively. And most importantly, being able to program fluently in both paradigms gives you the flexibility to choose the right tools for the task at hand. 



