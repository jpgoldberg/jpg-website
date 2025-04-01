---
# Documentation: https://docs.hugoblox.com/managing-content/

title: "Mixed Signals"
subtitle: "One Jeffrey Goldberg comments on another's Signal chat"
summary: "Jeffrey M Goldberg (not me) was included in a Signal chat group planning a military operation. Although I like and recommend Signal as a secure and private messaging app, I outline ways in which it was grossly inappropriate for that discussion."
authors: [jpgoldberg]
tags: []
categories: []
date: 2025-03-31T17:07:01-05:00
lastmod: 2025-03-31T17:07:01-05:00
featured: false
draft: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: "Signal profile for me, Jeffrey _Paul_ Goldberg, not to be confused with a different Jeffrey Goldberg ="
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

As is now widely known and reported Jeffrey Mark Goldberg (henceforth JMG),
editor of the Atlantic,
was included on March 23, 2025
in a discussion of an impending strike on Houthi terrorists in Yemen.
The active participants included
United States Vice President JD Vance,
Secretary of Defense Pete Hegseth,
Director of National Intelligence Tulsi Gabbard,
Secretary of State Mark Rubio
among others, including the organizer Michael Waltz.
This was done in a chat group using [Signal].

I naturally made a
[snarky Facebook posts](https://www.facebook.com/jeffrey.goldberg/posts/pfbid02CtenwwdBZzB8EZUaSSLXvh2TE8xjUxH2dZmxbvaEZ7c6LScVq7s6skkHzmC1GjSMl)
about this as the news broke.
In the discussion that followed I found myself attempting to explain
just how bad a screwup I thought it to be and what it says about the security
Some of what I say overlaps with what
[Steve Bellovin](https://www.cs.columbia.edu/~smb/bio.html) wrote
about in [Security Turtles All the Way Down](https://www.cs.columbia.edu/~smb/blog/2025-03/2025-03-24.html).

## On Signal

[Signal] is a very secure consumer messaging system that I recommend,
but there are thing outside of the system’s control that also need to be done to meet the level of security for planning and sharing precise details
(targets, payloads) of a military operation.

### Who are you?

An important and tricky part of security communication is
verifying that the account you are
interacting with belongs to the person you think it does.
Without such verification an adversary could pretend to be, say,
JD Vance to the group
while pretending to be other members of the group to the vice president.
That [adversary in the middle](https://attack.mitre.org/techniques/T1557/)
could faithfully rely to each party what the other parties say.
That way, nobody would be able to detect during the conversation that
anything was amiss.

With Signal there are (annoying) protocols that people can go through
to [verify other parties](https://support.signal.org/hc/en-us/articles/360007060632-What-is-a-safety-number-and-why-do-I-see-that-it-changed).
Performing that verification is options, and last I heard only a tiny portion
Signal users do so
despite Signals efforts to
[make the process easier](https://signal.org/blog/safety-number-updates/)
with Signal.
It is a subtle concept, and it is easy for people to get wrong.
The systems that enforce doing it right are annoying to use,
which may be among the reasons that the participants
choose to not use the proper systems
and procedures.

While we don't know whether some pairs of members of that chat had previously
performed such verification with each other,
we do know that it was not standard practice among them.
Had it been standard practice,
all participants in the chat would have very explicitly
known that JMG was included, and JMG would never have doubted who
the other participants were.

There is very good reason to believe that the systems and procedures that people
were supposed to use (instead of Signal) do enforce some mechanism of that verification.
The participants don't need to know how to do all of that stuff if they use the right systems.
But if they go it on their own by setting up their own chats, then they do need to understand these things to do things securely.

### Who's lurking

Another thing we expect of the security of such discussions is to make sure that lurkers have to identify themselves.
When you have conversion that nature, it is important to know who you are speaking in front of, even if some of those people will be silent.

In the [transcript](https://www.theatlantic.com/politics/archive/2025/03/signal-group-chat-attack-plans-hegseth-goldberg/682176/)
we see that many of the people who participated in the chat
announced their presence.
But announcing your presence doesn't solve the problem
unless there is some mechanism or protocol that will enforce that everyone does.
I do not know what those protocols are;
I can imagine a number of mechanisms that would prevent substantive conversation before
everyone is announced, but I have no specific knowledge of how that is handled
using the proper procedures.
I am, however, highly confident that there is such a system.

What I sense from the self-introductions in the transcripts is that participants learned how to introduce themselves for such discussion, perhaps through experience or training with the proper systems.
But they did recognize that there is another half to the system that enforces introductions.
Again, that is fine.
Not everyone needs to understand that such are present in the proper
systems. 

you see that many people joining did identify themselves as they joined. But there needs to be a procedure to make sure that everyone knows about everyone. Signal does list the participants, but it takes addition, typically human, procedures to make sure that everyone knows who they are talking to.
OpSec of settings. Is the site each participant using secure from monitoring? Are there windows enemies can look through. Can they listen to you type on your keyboard? Is everything shielded from EM monitoring? Can enemies do any of that through planted or compromised devices in that environment. SCIFs are the general solution to that, but it does not appear that anyone tried to verify or even ask if everyone was in a SCIF.
So Signal might be sufficiently secure for the part of the communication that is its responsibility, but it is just wrong to use it the way it appears to have been used for such a discussion.

## Final cause

There was a choice to not use the established procedures for such highly sensitive
discussions among the leaders of US national security.

That is far more important then the fact that they made they subsequently made a mistake the secure system is designed to avoid. 



[Signal]: https://signal.org/ "Signal messaging system"
