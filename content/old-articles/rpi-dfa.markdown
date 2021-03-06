---
layout: post
title: "Realizing DFAs with Raspberry Pi"
subtitle: "There's a better way!!"
comments: true
off-topic: true
category: cs-theory
aliases:
  - /cs-theory/2014/01/10/rpi-dfa.html
---
I've been working on a [project][pisnort] recently for a friend's research,
  which needed a decently specialized piece of hardware.
As I was at the time in a course that more-or-less guided the class
  through several 'miniature' projects
  (they weren't really that miniature in the end),
  I eagerly agreed to help her out.
Turned out to be slightly more than I bargained for.

<!--more-->

She needed (and is still needing;
  I'm currently waiting on a motion sensor from Digikey)
  a device that must turn on and off,
  detect motion,
  and play a sound at some delay after the motion.
It might not sound like it, but even this is a
  a deterministic finite automaton (DFA).

## Theory ##

What's a DFA, you ask?
A DFA (often simply called a *state machine*) is
  a theoretical model of computation that consists of
  a set of states and a set of transitions between those states.
Additionally, it defines a state to begin in
  and states that make the machine happy (accept states).[^formal]
DFAs account for a large portion of the embedded systems around you.
They are everywhere.
Simple devices, such as three-state lamps, as an almost trivial example.
The hardware and menu systems of television sets are another good example,
  as is are most cars, traffic light systems, and every reasonable vending machine.
The little ghost-monsters in Pac-Man are floating, deadly, stress-inducing state machines.

So what does this all have to do with your next Raspberry Pi project?
When I came to the (recurring) realization that I was creating a real-life state machine,
  the project as a whole seemed much simpler to model.
It might not seem like much, but anyone who has done any decently serious electronics work
  will know that even the simplest behaviors are not trivial.

## Application ##

Take, for example, the requirement of turning on and off.
Doing this with a button[^hindsight] doesn't seem like much at all,
  but it implies quite a few things that
  most normal human beings just don't think about.
  (I didn't.)
The button will reliably be depressed for longer than a moment,
  and we should not just keep toggling.
This is engineering, not heads or tails.
Thus, the button cannot simply fire a `toggle on/off` event
  every time the button is depressed.
There must be something intelligent behind the curtain.

Enter the humble DFA.
Let's think about the problem and define the major states for our machine:

System Off
: The system is completely off.
  The indicater LED is `LOW` and the motion detector is not being monitored.

System On
: The system is on.
  The indicator LED is `HIGH` and the motion detector is being monitored.

<!-- [<img align="right" src="/images/rpi-dfa.png" height="350" width="350" alt="state machine"/>][tex-source] -->

Let's further define the inputs for our machine:

0
: The button is not depressed.

1
: The button is depressed.

We will constantly be getting inputs,
  so we'll need to have some sort of buffer area to capture excess input.
We define two more states as 'transition states' between on and off (and off and on)
  and end up with machine on the right.

> I also used to have a nice DFA diagram, but it is also lost.

## Implementation ##

Now that we've formally described our system,
  how do we take advantage of it?
There is a Python library for simulating state machines called `automata`
  and it might simplify things for you,
  but the install failed when I tried to run it and I have yet to coerce it to succeed.
It's not too hard to create your own basic implementation, so I will put one forward here.

> I have since lost the code that was associated with this post

## Practice ##

So how do we *use* what we've made?

> Again, it's lost to time `:(`

---

### Footnotes ###

[^formal]:
    Formally, DFAs (and other such models of computation) recognize a *language*.
	When DFAs receive input, these chunks of input are considered as letters in a word.
	If the DFA finished reading the word and is in an accept state,
	  the word is in the language that the machine was created to recognize.

[^hindsight]:
    In hindsight, it would have been better to implement
	  this particular functionality (on/off) with a lever switch.
	I believe the concept still stands, though.

[pisnort]: http://www.github.com/vermiculus/pisnort
[tex-source]: https://gist.github.com/vermiculus/8376562#file-diagram-tex
