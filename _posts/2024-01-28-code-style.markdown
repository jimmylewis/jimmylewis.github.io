---
layout: post
title:  "Why do I care so much about code style?"
date:   2024-01-28 00:00:00 -0800
categories: Coding
---

# It's a tale as old as time

"How do we enforce consistent style?  What style is best?"  These questions embroil every software team at some point (at least, in my experience).  We sometimes even argue about whether style really matters, as long as the code is functional.

Sharing intent is paramount to code maintainance, starting even from the initial code reviews.  Another developer reading the code should be reasonably able to understand at least these three things:
1. What is the code accomplishing?
2. Why is it doing it this way?
3. Why not do it another way? (Not every time, but this can and arguably should come up regularly)

The first question is the one that code style addresses most directly.  There are many ways to skin a cat, as the saying goes.  A developer reading the code (either for review, inspiration, or later maintenance) may have their own biases of what patterns they're familiar with reading.  These will stand out naturally, and the brain will be able to interpret the low level code into a higher level intent quicker.  Using consistent style is a way to train each brain on the team to recognize the same patterns; deviating from those patterns by submitting a bespoke or unconventional implementation will incur more cognitive load to understand intent.

The Why and Why Not Another answers can be difficult to express from syntactic code.  But again patterns aid easy recognition of the solution, in particular any well-known design patterns that are utilized.  This can simplify the shared understanding about Why vs. Why Not, as design patterns largely have well documented benefits and drawbacks.

# Consistency creates value

There's good reason to strive for a consistent code style: consistent patterns make the code more readable, and thereby easier to understand.  Code is all about communicating ideas, to two very different audiences: the compiler (which only understands a very constrained and simple set of commands) and our human collaborators (who must try to decode the abstract intent from the compiler's limited syntax).

The compiler doesn't care about style, and even for we humans, as far as functionality goes, correctness is the main importance.  But correctness needs to be validated, and it can change over time.  In either case, it's up to another human to ascertain what correctness means and to evaluate whether it's been achieved.

> note "On a personal note"
>
> In my own experience as someone diagnosed with OCD, patterned things create comfort and unpatterned things create a tiny but noticeable amount of discomfort.  One of my own struggles when reading code is when I see the same intent accomplished in different ways.  For example, in C#, there are many ways to check whether an object is set to `null`:
>
> ```csharp
> // check for null
> if (foo == null)
> if (foo is null)
> // or check the opposite
> if (foo != null)
> if (foo is not null)
> if (foo is {})
> if (foo is Widget widget)
> ```
>
> Seeing different patterns in close proximity makes me easily distracted - there's subtle differences to these, so I find myself slowing down to ask: why did the original author choose one and then choose a different one?  Are they unaware of the difference, or are they *emphasizing* the difference?
>
> As they say, ignorance is bliss.  If I didn't know of the differences, I might not thing anything of it.  But sometimes subtle semantics can break correctness, and I *am* aware of them, so my pedantic brain kicks in with lots of questions.

In my personal case, variety for the sake of personal preference distracts from understanding the higher level intentions.  It leads to time wasted, muddling about over a difference that most of the time doesn't make a difference.

# Code can't always be self-documenting

Often, we set an expectation that someone reading the code should be able to intuit the precise intentions of the author.  Such code doesn't require additional documentation, because it is so easily understood.  Often, the output of the code is easily conceptualized, which answers the question of What.

Fully answering the Why vs. Why Not questions sometimes need to be answered explicitly.  In these cases, a developer should leverage tools for explaining themselves in native language prose - such as code comments, or a detailed writeup in the commit log.  On occasion, even the What question requires some explanation, which typically merely indicates the complexity of the code; whether it merits complexity is sometimes another matter.

# In summary

Readable code leads to more productive understanding of what our code is attempting to accomplish - the intentions of the author. 

Consistent code style helps everyone working within that codebase to read and understand the code faster.  For some people, consistency accommodates better focus on higher level concepts, where inconsistency creates distractions that slow understanding.

Deviating from established patterns may draw attention to key differences, so a reader may naturally ponder more closely about what those represent.  Forcing the user to slow down is important in specific situations, but most of the time it hinders or confuses understanding of the code.
