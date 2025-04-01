---
# Documentation: https://docs.hugoblox.com/managing-content/

title: "Mixed Signals"
subtitle: "One Jeffrey Goldberg comments on another's Signal chat"
summary: "Jeffrey M Goldberg (not me) was included in a Signal chat group planning a military operation. Although I like and recommend Signal as a secure and private messaging app, I outline ways in which it was grossly inappropriate for that discussion."
authors: [jpgoldberg]
tags: []
categories: []
date: 2025-03-31T17:07:01-05:00
lastmod: 2025-04-01T06:30+00:00
featured: false
draft: false

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: "Signal profile for me, Jeffrey _Paul_ Goldberg, not to be confused with a different Jeffrey Goldberg. It does not contain a proof of my identity nor does it contain an indication security clearances."
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

As is now widely known and reported Jeffrey Mark Goldberg (not me),
editor of [the Atlantic](https://www.theatlantic.com),
was included on March 23, 2025
in a discussion of an impending strike on Houthi terrorists in Yemen.
The active participants included
United States Vice President JD Vance,
Secretary of Defense Pete Hegseth,
Director of National Intelligence Tulsi Gabbard,
Secretary of State Mark Rubio
among others, including the organizer Michael Waltz.
This was done in a chat group using [Signal].
I will be referring to that Jeffrey Goldberg as JMG throughout the remainder
of this article.[^middlename]

[^middlename]: It seems that neither of us are particularly keen to use our middle names or initials, but I started using my middle initial, P, some years back specifically to help avoid this kind of confusion.

The administration and participants are now trying to claim that no secrets pertaining to
US national security were included in that discussion, which included detailed plans
before the attack took place.
You should read the [transcript] of the chat and judge for yourself how sensitive
this information was at the time.

I naturally made a
[snarky Facebook posts](https://www.facebook.com/jeffrey.goldberg/posts/pfbid02CtenwwdBZzB8EZUaSSLXvh2TE8xjUxH2dZmxbvaEZ7c6LScVq7s6skkHzmC1GjSMl)
about this as the news broke.
In the discussion that followed I found myself attempting to explain
just how bad a screwup I thought it to be and what it says about the security
practices of those responsible for safeguarding the United States.
Some of what I say overlaps with what
[Steven Bellovin](https://www.cs.columbia.edu/~smb/bio.html) wrote
about in [Security Turtles All the Way Down][SMBTurtles].

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
could faithfully relay to each party what the other parties say.
That way, nobody would be able to detect during the conversation that
anything was amiss.

{{< figure
  src="./signal-safety-number.png"
  alt="Verify Safety Number example"
  link="https://signal.org/blog/safety-number-updates/"
  caption="Signal's Verify Safety Number screen, which is to be used for out of band verification."
  class="floatright"
>}}
With Signal there are (annoying) protocols that people can go through
to [verify other parties](https://support.signal.org/hc/en-us/articles/360007060632-What-is-a-safety-number-and-why-do-I-see-that-it-changed).
Performing that verification is optional,
and last I heard only a tiny portion
Signal users do so despite Signal's efforts to
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
But if they go it on their own by setting up their own chats
then they do need to understand these things to do things securely.

### Who's there?

Another thing we expect of the security of such discussions is to
make sure that lurkers have to identify themselves.
When you have conversion that nature,
it is important to know who you are speaking in front of,
even if some of those people will be silent.

In the [transcript] we see that many of the people who participated in the chat
announced their presence.
But announcing your presence doesn't solve the problem
unless there is some mechanism or protocol that will enforce that everyone does.
I do not know what those protocols are;
I can imagine a number of mechanisms that would prevent substantive conversation before
everyone is announced, but I have no specific knowledge of how that is handled
using the proper procedures.
I am, however, highly confident that there is such a system.

What I sense from the self-introductions in the transcripts
is that participants learned how to introduce themselves for such discussion,
perhaps through experience or training with the proper systems.
But they did not recognize that there is another half to the system
that enforces introductions.

Again, that is fine.
Not everyone needs to understand
that such safeguards are built into the proper systems.
Except that it isn't fine if you choose to ditch
the system run and developed by professionals
and opt to do things on your own.
Then you really do need to understand all of this and much more.


### Where are you?

Is the site each participant using secure from monitoring?
Are there windows enemies can look through?
Can adversaries listen to you type on your keyboard?
Is everything shielded from electro-magnetic monitoring?
Can enemies monitor through planted or compromised devices in that environment?

Signal itself has no ability to provide that kind of security.
It's design and implementation might be flawless,
but its security is only one component of an entire system.

{{< figure
  src="./not-a-scif.jpg"
  alt="Document boxes in Mar-a-Lago bathroom"
  caption="This is probably not a SCIF"
  class="floatleft"
>}}
The solution to addressing those sorts of very real threats to communication
at the levels of concern is for each participants to use a
[Sensitive Compartmented Information Facility ({{< abbr "SCIF" >}})](https://en.wikipedia.org/wiki/Sensitive_compartmented_information_facility).
Cabinet secretaries for departments that deal with issues of national security will have these installed at their homes.

I expect that some allowances[^allow] are made for when
it is not possible for all necessary participants to get to a {{< abbr "SCIF" >}}
for a conversation that cannot be delayed.
But I very strongly expect that such allowances need to be logged
and that each participant is informed that not everyone is is a secure location
for such a conversation.

It is clear from the transcripts that nobody made any attempt to ascertain if others
were in a secure location for such a conversation.
Nor did anyone volunteer such information about themselves.
Normally that wouldn't be the responsibility of the participants,
but as they were doing this on their own instead of through established procedures
in which a security officer of some sort would handle that, the responsibility fell
to the participants whether they knew it or not.

### What are you using?

Is the device you are using compromised?

Again this is the kind of things that would be handled by using the equipment within
the {{<abbr "SCIF" >}}.
Instead, participants used their own phones for which they they control
and are therefore responsible for securing.

Mike Waltz, who is now desperately trying to say pretend
that he never had any contact with
{{< abbr "JMG" "Jeffrey Mark Goldberg" >}},
has recently said in a [FOX News interview](https://www.foxnews.com/video/6370555739112)
that JMG's contact information was "sucked in" through another contact.
If that were actually the truth, it would be a perfect example of why
he shouldn't be using his own phone for managing such chat groups.
And if he is spouting nonsense, it also illustrates why he shouldn't
be the person maintaining the security of the device he is using.

## Who's to blame?

It would be easy to pin the blame for this on Mike Waltz who somehow added
JMG to the chat group.
But however he managed to do that, it is the kind of mistake that can be made
using such systems.
Signal does what it can within its limited power
to help people avoid making such mistakes,
but its power to do so is limited.

As Steven Bellovin [wrote][SMBTurtles]

> Adding a journalist to the group was the least of the problems and might have resulted from someone mistapping a name on a list (though “Jeffrey Goldberg” is not a rare name; I know someone else of that name) — but on a secure chat system, the wrong one probably wouldn’t have been listed at all.

There was a choice to not use the established procedures for
such highly sensitive discussions among the leaders of US national security.

Each and every participant who agreed to or choose to by-pass
the established standard procedures for such discussions is to blame.
And their individual responsibility is not diluted by the fact that their colleagues
are also responsible.

Had they not by-passed the procedures

- Each participant to the discussion, including the silent ones, would have been fully identified to the system along with their security clearances. JMG would have been identified as not belonging at that point.
  
- All participants would have been made aware of everyone who could see the conversation. This would have also shown JMG didn't belong.
- Each participant would have used specific government equipment, which JMG did not have access to.
- JMG's contact information could not be “sucked into” any of those devices.

### Born of a malicious contempt for expertise

What we see is a combination of “the rules don't apply to me” thinking along with
a malicious disregard for the experts and trained professionals who have developed
and manage the systems that the participants chose to by-pass.
The “deep state” is exactly what would have prevented this.

I am not claiming that I could set up and operate
the kinds of systems and protocols needed to secure
the kinds of planning and discussion that
took place in that chat by those participants.
I certainly think that I know better than those who participated in that chat,
and that means I would know to defer to the expertise and experience
of those who operate in a system that has been developed and refined over decades.
I might not understand or enjoy all of the operational aspects and contraints
of such a system,
but I would know better than to simply jettison everything I don't understand.

[Signal]: https://signal.org/ "Signal messaging system"

[SMBTurtles]: https://www.cs.columbia.edu/~smb/blog/2025-03/2025-03-24.html "Steven Bellovin: Security turtles"

[transcript]: https://www.theatlantic.com/politics/archive/2025/03/signal-group-chat-attack-plans-hegseth-goldberg/682176/ 

[^allow]: This is very much one of those “those who know don't say, and those who say don't know” situations. The precise rules about allowing exceptions should be kept secret so that an adversary will have a more difficult time trying to trigger those situations. I, as you see, am happy to speculate about this because I am among those who don't know.
