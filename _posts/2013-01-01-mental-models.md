---
layout: post
title: Mental Models in Computer Science
---

Isaac Asimov has a brilliant treatise on how science moves forward and
gets things wrong at the same time, which you can and should read
[here][1] (or [here][2]). Go read it, I'll wait.

This disucssion resonated with me on several levels. In computer
science, we heavily rely on abstractions to make computations
possible. Often, we develop mental models for how a particlar
situation will unravel in the underlying abstractions. So, the
accuracy of the mental models is critical for creating code that is
efficient.

The other thing that stuck with me from this discussion was how, from
one perspective, the changes were very small in terms of our
understanding, but, from another perspective, they represented large
breaks in the conceptual model of how the world works. In teaching an
undergraduate class in C/Unix programming, I am convinced a similar
thing happens to students learning computer science. Specifically,
they have more of a difficult time, because CS is all about
abstractions *that someone else has done for you*. Some other person
has gone to the trouble of abstracting away the details of the file
system so you can make simple calls to read and write files. The
operating system completely hides the fact that memory is in fact
cached at many different levels of memory. Java hides the fact that
objects are, in fact, pointers to objects and therefore everything in
Java is pass by value. At each layer of abstraction, you can function
90% of the time without understanding the details of the layer. In
fact, you can even function 90% of the time with having complex, but
wrong understanding of the details in the layer. But, eventually, you
run into a situation where your model of the abstraction breaks down
and it does something unexpected that you don't understand.

At this stage, one of two reactions seem to emerge. The first reaction
is to dive into the abstraction layer to correct the internal mental
model you were building. Sometimes, this is a quick fix, just patching
the model, so that you now cover 90.1% of the dynamics of the
model. But, other times, this involves a groundbreaking rework of your
mental model, in which the fundamental assumptions change about how
the world works. This is the model that science follows, where
incremental changes can often lead to complete mental model
changes. This "tipping" point has been discussed in detail in other
places. This is clearly the harder path, as it requires the student or
programmer to completely throw away a model that may have taken years
to acquire.

There is a second reaction that I found surprising. This second
reaction is analogous to "burying your head in the sand". Under this
mentality, the importance is placed on the mental model and not how
the world disagrees with it. So, instead of taking the time to
understand why your model disagrees with the world, you route around
the disagreement, completely ignoring the fact that the disagreement
occurred. How this manifests in CS students is the generation of
complex structures devoted solely to working around errors or
segmentation faults that they don't understand. If the compiler says
that you can't call `strcpy` in that manner, instead of updating the
mental model, a `strcpy` function is written from scratch. Instead of
investing the time and energy fixing the mental model, these
programmers create elaborate detours to ignore any inconsistencies
between reality and the model.

There is a sliding scale of intelligence and personality types that
are correlated with each group. In the group of detour makers, I worry
most about the highly intelligent detour makers. They are fairly
bright and can work very quickly to create increasingly complex
structures to route around problems they don't understand, because
they think the world has a bug in it and their model is correct. In
addition, because they can generate functioning code so quickly, the
flawed model can persist for a long time.


[1]: http://hermiene.net/essays-trans/relativity_of_wrong.html
[2]: http://chem.tufts.edu/answersinscience/relativityofwrong.htm


