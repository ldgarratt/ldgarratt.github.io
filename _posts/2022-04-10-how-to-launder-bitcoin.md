---
layout: post
title: "How to launder $4.5 billion in bitcoin"
date: 2022-04-10
mathjax: true
---

# The situation

You and your partner have just got away with a hack for millions of dollars
worth of bitcoin. Bitcoin then surges in value and now your theft is worth $4.5 billion.
You live in New York. Nobody besides you and your partner know you've done it. What do you do? This is exactly the situation [Ilya Lichtenstein](https://www.justice.gov/opa/pr/two-arrested-alleged-conspiracy-launder-45-billion-stolen-cryptocurrency) (allegedly) found himself in. What would you do?

For additional context, the bitcoin blockchain is literally just a large, public database of transactions; everyone can
see where your bitcoin is and where you send it. To state the bleeding obvious, you
definitely can't just transfer \\$4.5bn into your coinbase account; everyone will
see the stolen money moving directly there. Even if you could move it without
anyone seeing, \\$4.5bn is obviously far too suspicious an amount of money to just enter
your account (plus, think of the taxes!). So you've stolen the bitcoin, but how do you
actually spend it?

As a caveat, obviously this is just a fun thought experiment. It's fun to think
about how to be paranoid about security in a high stakes situation. I of course
appreciate that a lot of people lost a lot of money and I'm glad the criminals
were brought to justice and some restitution can happen for the victims.

# What would I do?

Here I am going to enumerate the steps for what I think would maximise the
chances of actually getting away with stealing \\$4.5bn of bitcoin. To
clarify, the goal is to transfer this money into cash, and from there I will
assume there are normal ways to launder physical cash (for example, by starting
a business whose customers primarily pay in cash).

## 1. Shut up.

This should pretty much go without saying, but it's actually surprisingly hard
for many people. The Grugq has an interesting talk on this. The
salient message is that paranoia is actually quite tedious, and if you want to get away with
crime you need to be boring, consistent and shut the fuck up.

<iframe width="560" height="315" src="https://www.youtube.com/embed/9XaYdCdwiWU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Unfortunately, the Bonnie to our Clyde in this story has a lot of [rap videos](https://www.youtube.com/watch?v=R2wfjoHVexk) boasting about cashmoney, thug life, etc. online. While her rapping is not an admission of guilt or anything, it probably is not ideal and you really want to be boring and keep a low profile.

In addition to the shut up theme, this should be a compulsory training video:

<iframe width="560" height="315" src="https://www.youtube.com/embed/d-7o9xYp7eE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Or, to summarise humorously:
<iframe width="560" height="315" src="https://www.youtube.com/embed/sgWHrkDX35o" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Again, the salient point is: shut the fuck up.

## 2. Remove the evidence

The evidence taken from the hack is always going to be there. The authorities
already have it so there is not much we can do about that. But my partner and I do have access to the passphrase to the wallet containing the stolen bitcoin. This evidence must not ever be found. So I would destroy this evidence. I would consider splitting this secret up into several, such that it does not exist in one place, and each individual share, on its own, is meaningless. We can do this would [Shamir Secret Sharing](https://en.wikipedia.org/wiki/Shamir%27s_Secret_Sharing). I've even written code for it [here](https://github.com/ldgarratt/shamir). We could even do something like [threshold cryptography](https://en.wikipedia.org/wiki/Threshold_cryptosystem), whereby the key to sign transactions no longer exists in one part, and both of us would have to combine to sign individual transactions.

Obviously hardrives, USBs would have nothing else pertaining to the crime and
everything would be encrypted with the passphrases to decrypt committed to
memory only. Our phones would not enable FaceID or thumbprints. The only
evidence we have would be the shares of the secrets, which are encrypted.

## 3. Privacy coins

As mentioned earlier, bitcoin is not actually a privacy-preserving currency. But
some coins are. In particular, the best two candidates to my knowledge are two
coins called Monero and ZCash. Monero has a good developer community behind it
and is even the coin of choice for dark net drug dealers. ZCash on the other
hand has some renowned cryptographers behind it and is recommended by people
such as Edward Snowden among others. They use various different cool techniques to obscure transactions such as ring signatures and zk-SNARKs (zero-knowledge succinct non-interactive argument of knowledge). These concepts are actually pretty fascinating cryptographically,  and deserve their own blog post. But approximately, ring signatures, for instance, look as follows:

Suppose there are $n$ users where $\text{user}_i$ uses his private key $x_i$ to
sign message $m$. For a ring signature, we want the $n$ users to work together
in such a way that any of them could have made the signature on $m$. This will
hide the transaction from $\text{user}_i$, since it could have been from
anyone. Imagine how obscured transactions are when this happens again, and
again, and again, for all time. Monero has a ringsize of $11$, so there's $10$
decoys for every transaction. Every $\text{user}_j, j \neq i$ randomly samples a
$c_j$ and $e_j$ and computes $R_j = e_j G - c_j X_j$. Then $\text{user}_i$ actually making the signature samples $r_i$ and computes $R_i = r_i G$ (note the order is different) and computes $c = \text{hash}(m, R_1, \dots, R_n)$. The point is then that we can define the "commitment" as:

 $$c_i = \oplus_{j \neq i} c_j \oplus c$$

The key idea is that there is only one constraint while there are $n$ variables,
which correspinds to the fact that only one user signs the message with his real
key, while the others are free to cheat.

Then, of course, $e_i = r_i + c_ix_i$

There are some statements from agencies claiming that they can track monero, but
I am actually very sceptical of these claims. The proof is lacking. Why wouldn't
they track all the other nefarious uses of it in that case?

So to conclude, I would convert into monero and zcash, and wait, and do lots of
transaction to myself to mask everything.

## Tumblers and decoys

I would also use so-called bitcoin and other currency tumblers. But these aren't
nearly as good as coins like monero. So I would probably abandon a portion of
the //$4.5bn and write a bot that simply tumbles it and sends to random address
after random address, simply to distract the Feds. It goes without saying that
everything will be over Tor.

## Avoiding KYC

I would avoid exchanges that Know Your Customer altogether. Whether this means
using LocalMonero or finding some dodgier exchanges, I would do that. It would
be a slow and laborious process, but nothing will be more secure that literally
meeting someone to exchange Monero for cash in hand, small pieces at a time.
With new accounts every so often of course.

## Communication

All communication would be over Tor. Anything related to the crime would not be
on my phone unless absolutely necessarily, and then then I would leverage a
burner phone and Signal with exploding messages
