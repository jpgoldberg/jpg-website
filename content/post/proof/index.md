---
# Documentation: https://docs.hugoblox.com/managing-content/

title: "On evidence for God"
subtitle: "An answer to “what kind of evidence would convince you that God is real?"
summary: "I am occasionally what kind of evidence would help persuade me to believe that there is a god. Here I outline my answer to those asking. If you are not asking me such a question, there is little reason to read this."
authors: []
tags: []
categories: [religion]
date: 2026-05-06T22:42:29-05:00
lastmod: 2026-05-06T22:42:29-05:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

Over the years, I have been asked many time whether there is
any evidence that would help convince me that there is a god.
And my answer is yes. There are things a god could do that would go a long way to
convincing me that its exists and has god-like capabilities.
This article article is aimed at people who ask me that question,
and it is of little interest to anyone else.

{{% callout note %}}
Throughout this text I will I will use lowercase “god” as a common noun
that is not specific any particular god.
I will use uppercase “God” as a name for the Abrahamic god also known as
Yahweh or Jehovah.
{{% /callout %}}

## Why ask the question?

I suspect that many people who ask the question of me and other atheists feel
that there is no evidence that would persuade an atheist.
And so they are asking the question to check that feeling.
It also isn't surprising that they have such a feeling,
as many atheists seem dismissive of evidence for God,
often seeking any possible alternative explanation for
for such evidence.

The reason that many atheists, including me, treat reported evidence
with such skepticism is that we see the existence of a god to be no more plausible
than a real live Santa Claus
that flies around in a sled pulled by real flying reindeer.
And because we see the existence of such a thing as extraordinarily unlikely
we demand extraordinary evidence.
Whether or not we are justified in assigning just a low prior probability
to the existence of a god is a separate question for a separate day,
but a consequence of that feeling is that we find any alternative
explanation for some reported evidence as far more plausible than
explaining the evidence in terms of a god.

To a believer,
who doesn't assign such a low prior probability to the existence
of God or gods,
the same evidence that the atheist is seems to be explaining away
is genuinely taken as meaningful evidence.
Both the believer and the atheist are implicitly applying Bayes' Rule
when evaluating evidence,
but because of their different prior probabilities,
they are treating the evidence very differently.

As a result of those differences the kinds of evidence that believers find meaningful
(e.g., reports of miracles, mystical experiences, the fact that religiosity is so common
in humans, etc)
get dismissed by atheists as not credible evidence.
It then becomes reasonable for many believers to suspect that
there is no evidence that would persuade atheists.
The atheist cry of, “Prove it! Prove it!”
will feel like demanding the impossible.
And so, it is perfectly reasonable for believers to ask
atheists whether there is any evidence that would persuade us.

## Testing God

Every time I have answered the question and outlined the kind of evidence that would
help me believe that gods exist,
I have been met with a responses along the lines of
“you can't test God”
or “blessed is he who has not seen and yet believes.”

Most of my answers of the kinds of evidence that would help convince me
of the existence of a god very much involve testing god.
And so if you believe that one can't test God,
then there is no reason for you to read the details of such tests.
Of course it would also mean that you should be upfront about that
if asking an atheist what evidence they would find convincing.
Furthermore if you hold such a position then I would encourage you
to think about the role of evidence for God in belief.

No single test is going to get me all the way to believing in God.
After all, if I were to somehow become convinced
that a super-powerful and super-intelligent entity created the universe
that still wouldn't convince me it is still around much less care about my
moral choices.
But all such tests should meet the following criteria:

1. Demonstrate a god-like capability that is beyond human or physical capability;
2. Be witnessed by me;
3. Be verifiable by me;
4. Not be something I could write off as a hallucination or dream.

The test, below, of super-intelligence, clearly meets those.

## A test of super-intelligence

Anything worth calling a god should have a mind and be super-intelligent.
I am not demanding omniscience, nor would I know how to test that.
But testing super-intelligence is easy,
and any super-intelligent entity that wanted to reveal its existence to me could do so easily.

Factoring a carefully constructed very large numbers is currently beyond the capacity
of the combined efforts of all humanity and all of our computing equipment combined.
And so anything that can factor such number has a capability beyond all of humanity.

Anyone with access to a computer and the software for generating RSA keys can create one.

{{% spoiler text="Example challenge number generation" %}}
Here is how I created one such number using Python and [my own toy (not secure)](file:///Users/jeffrey/src/github.com/jpgoldberg/toy-crypto-math/docs/build/html/index.html)
RSA [key generation code](file:///Users/jeffrey/src/github.com/jpgoldberg/toy-crypto-math/docs/build/html/modules/rsa/keygen.html#toy_crypto.rsa.key_gen)

```python-repl { title="Creating a challenge number" }
>>> from toy_crypto import rsa, utils
>>> public_key, _ = rsa.key_gen(key_size = 2048)
>>> N = public_key.N
>>> utils.digit_count(N)
617
>>> print(N)
21688159958085883942561456689805736773277757384124302299463205165645290082754581450697829195994574100324591898541890384273852841431698506955208222505307009945730607757808099412878915273905384441811456806483349578975854638864042611210244602498718680196364839409843071924255373802312136281423798279735766563426868678733780712794092963121970425187910927409541816365796638582079608800051424772811220003170105296742676293637015241906131310939834833868378226034341754496127620035882293288172918834344995679233361328907062575828119310917824129456975268428082934405771174922639372272829121453957570211307897286065512740930241
```

The result will be different each time.
One could use any of the other RSA key generation tools out there
{{% /spoiler %}}

### The challenge number

The challenge number created in May 2026 is (here broken across several lines)

```txt
2168815995808588394256145668980573677327775738412430229946320516564529
0082754581450697829195994574100324591898541890384273852841431698506955
2082225053070099457306077578080994128789152739053844418114568064833495
7897585463886404261121024460249871868019636483940984307192425537380231
2136281423798279735766563426868678733780712794092963121970425187910927
4095418163657966385820796088000514247728112200031701052967426762936370
1524190613131093983483386837822603434175449612762003588229328817291883
4344995679233361328907062575828119310917824129456975268428082934405771
174922639372272829121453957570211307897286065512740930241
```

It is also listed in a separate files [n.txt](./n.txt) all on one line,
and in [n-multiline.txt](./n-multiline.txt), broken up into multiple lines.

## Cheating and how to prevent it

It is not humanly possible to cheat at factoring the number,
but there are a couple of ways to cheat that
might make it appear that the challenge has been met.
The discussion in this section is only of interest to anyone
who may wish to build on what I said here to conduct a more high profile test.
My purpose has been to illustrate
that a super-intellegent entity could reveal its existence to
me if it wished to.
I believe I have succeeded in that.

Here I just wish to sketch how people could appear to solve the challenge
and ways to make that more difficult.

One way to cheat is to simply hack into my systems
or the systems the challenge number is hosted on
and change the challenge number after the fact.
I have to assume that it is humanly possible for some group of people to have modified
what you are reading now to use a similar 617 digit number for which the factors
are retained.
The defenses against that inolved using time-stamped digital
signtures or other forms of noterazing the challange number, and getting
this challenge uniquely into the public record.

Given what I have stated, it would be possible for me to cheat in the other direction.
It is possible that I lied about not retaining the prime factors (I didn't lie, but
you don't know that) and then later falsely claim that the challenge has been solved
when it hasn't been.
There are more expensive and tedious ways to prevent that kind of cheating.
Those involve having the challange number created using an attested system.
Of course, if I were to cheat that way, it would only allow me to falsely assert that
the factors have been revealed.
It would not help persaude me that a super-intellegent entitity exists.
