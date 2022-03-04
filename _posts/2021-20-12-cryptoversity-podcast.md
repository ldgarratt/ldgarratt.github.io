---
layout: post
title: "Cryptoversity: Bitcoin/blockchain podcast appearance"
date: 2021-12-20
mathjax: true
---

I made a guest appearance on the cryptoversity podcast to talk about how bitcoin
actually works and share my (somewhat controversial) thoughts about blockchain
technology in general.

<iframe style="border-radius:12px" src="https://open.spotify.com/embed/episode/5Lj883FSTFvOKFaXenOzGJ?utm_source=generator" width="100%" height="232" frameBorder="0" allowfullscreen="" allow="autoplay; clipboard-write; encrypted-media; fullscreen; picture-in-picture"></iframe>

It was a really fun experience. I got out a lot of what I wanted to say. I wish
Jack and Mike all the best on the podcast!

# One thing I would change

One regret I have is that I don't think my explanation of SHA256 and bitcoin mining
quite landed because they were still a little confused at the end. It is a
difficult challenge to explain a technical subject over a podcast. I've thought
about it, and perhaps I should have said something along the lines:

> We assume the majority of the network is honest. The ideal situation is every miner has a vote for what the next block is. But this would require a trusted authority to hold the election. Plus, it's hard to ensure one vote per person in a distributed network like bitcoin. So instead, we allow miners to dictate which signed transactions are added to the next block if they solve a difficult-to-solve problem. Then as a user we can trust the longest blockchain, with the most solved problems/blocks in it, is the honest one.

> In the case of bitcoin, the difficult-to-solve problem is SHA256. It is like an equation: in school you can solve an equation like $x^2 -5x +6 = 0$ the smart way like $x^2 -5x + 6 = (x-2)(x-3) = 0$, so $ x=2 $ or $x = 3$, or you can solve it the brute force way by continually trying $x = 0$, $x = 1$, $x = 2$, until you get a solution which works. SHA256 together with the current blockchain state produces a problem which nobody knows how to solve except by brute force. That is what the miners are doing. When they guess a solution, they get to add announce their solution and add the signed transactions they've seen to the next block.

After the podcast, I was looking around for the best layman's explanations for
how bitcoin actually works. It was very difficult because there's a lot of hype
from journalists or salesmen who don't actually understand it themselves. But a
fantastic video I found is the following:

<iframe width="560" height="315" src="https://www.youtube.com/embed/bBC-nXj3Ng4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

# The identity of Satoshi Nakamoto

Just for fun, I also briefly mentioned in the podcast Craig Wright, someone who once claimed to be Satoshi. There is a hilarious video of him being called out on his claim by an actual expert here:

<iframe width="560" height="315" src="https://www.youtube.com/embed/0thnCDgRJfM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Dr Courtois in this video is referring to the fact that with ECDSA signatures, if the randomness is bad on a single signature, that is sufficient to reveal the private key used in making the signature. Basically, ECDSA is a signature $(r, s)$ where $s = k^{-1}(z+rd)$ for private key $d$, random nonce $k$ and (hashed) message $z$. The values $z$, $r$ and $s$ are all public. So if you know $k$, then you can just rearrange that equation and solve for private key $d = (ks-z)/r$. I've even [written code](https://github.com/ldgarratt/ecdsa_tools) to do it. Nowadays there are plenty of bots scanning blockchains looking for weak ECDSA signatures that will compromise a private key within minutes if a transaction uses a weak nonce. Craig seems quite flustered by this basic fact.

There's also a very embarrassing video from the BBC where the only verification
the journalist does is asking Craig to promise "hand on heart" here. Craig signs
something with his own key, not Satoshi's, which doesn't prove he's Satoshi.

<iframe width="560" height="315" src="https://www.youtube.com/embed/5DCAC1j2HTY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


# Attack of the 50 foot blockchain

I also mentioned in the podcast my skepticism of blockchain tech in general,
especially for business. There's a cornucopia of useless applications of
blockchain in business, as well as some amusing personal anecdotes I can share, which I may save for a future post. I will say
I once started writing a book about it in late 2016, but I halfway through
writing it I found David Gerard
had already pretty much written everything I wanted to say here:
https://davidgerard.co.uk/blockchain/. I highly recommend his work!
