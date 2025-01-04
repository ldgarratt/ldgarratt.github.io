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
see where your bitcoin is and where you send it. To state the obvious, you
definitely can't just transfer \\$4.5bn into your coinbase account; everyone will
see the stolen money moving directly there. Even if you could move it without
anyone seeing, \\$4.5bn is obviously far too suspicious an amount of money to just enter
your account (plus, think of the taxes!). So you've stolen the bitcoin, but how do you
actually spend it?

# What would I do?

Here I am going to enumerate the steps for what I think would maximise the
chances of actually getting away with stealing \\$4.5bn of bitcoin. To
clarify, the goal is to transfer this money into cash, and from there I will
assume there are normal ways to launder physical cash (for example, by starting
a business whose customers primarily pay in cash).

## 1. Lay low for awhile.
It is obviously tempting to try to launder and then spend this money quickly,
but a heist of several billion in bitcoin is obviously going to have a lot of people
investigating. The prudent move is lay low for a long time until interest in the
case dies down and resources are allocated elsewhere. This means not moving the
bitcoin at all for awhile, perhaps several years. The blockchain is all public:
damage is already done. If they can trace the coins back to you eventually based
on the bitcoin addresses it does not matter what you do with them in the future. You do not want to make any poorly thought out moves which make the situation any worse. Use time to lay low and educate yourself on the best possible next steps. Of course, since the blockchain is public then any move you eventually do make will cause a stir as someone is bound to notice, but perhaps time will still have eroded some interest in the case.

Consider moving to a country which does not cooperate with the authorities that
are looking for you.

## 2. Shut up.

This should pretty much go without saying, but it's actually surprisingly hard
for many people. The Grugq has an interesting talk on this. The
salient message is that paranoia is actually quite tedious, and if you want to get away with
crime you need to be boring, consistent and quiet.

<iframe width="560" height="315" src="https://www.youtube.com/embed/9XaYdCdwiWU" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Unfortunately, the Bonnie to our Clyde in this story has a lot of [rap videos](https://www.youtube.com/watch?v=R2wfjoHVexk) boasting about cashmoney, thug life, etc. online. While her rapping is not an admission of guilt or anything, it probably is not ideal and you really want to be boring and keep a low profile.

Following the theme of shutting up, if you are arrested you should absolutely
not say anything to the authorities without speaking to your lawyer first. This should be a compulsory training video:

<iframe width="560" height="315" src="https://www.youtube.com/embed/d-7o9xYp7eE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Or, to summarise humorously:
<iframe width="560" height="315" src="https://www.youtube.com/embed/6EI_RYIEtrg" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## 3. Remove the evidence.

The evidence taken from the hack is always going to be there. The authorities
already have it so there is not much we can do about that. But my partner and I do have access to the seed to the wallet containing the stolen bitcoin. This evidence must not ever be found and should definitely not be saved on some hard drive somewhere. So I would destroy this evidence. I would consider splitting this secret up into several, such that it does not exist in one place, and each individual share, on its own, is meaningless. We can do this with [Shamir Secret Sharing](https://en.wikipedia.org/wiki/Shamir%27s_Secret_Sharing). I've even written code for it [here](https://github.com/ldgarratt/shamir). We could even do something like [threshold cryptography](https://en.wikipedia.org/wiki/Threshold_cryptosystem), whereby the key to sign transactions no longer exists in one part, and both of us would have to combine to sign individual transactions. These shares will be stored on an encrypted USB.

Obviously the encrypted USBs would have nothing else pertaining to the crime and
everything would be encrypted with a passphrase to decrypt. These passphrases would be committed to memory only and would obviously have nothing to do with the seed to the bitcoins.

Destroy all technology which has been tainted with the crime. If you ever
discussed the crime over the phone, that phone is now gone. If you used a
particular computer to do the crime, that computer is now destroyed. Our new phones would not enable FaceID or thumbprints. We would never discuss the crime over any technology; only ever in person, and even then I would be incredibly paranoid in case the house is being bugged, so perhaps go on a walk to if you need to discuss. Perhaps agree on a codeword for signalling you need to talk about it, which is hopefully a very rare event.

## 4. Privacy coins

As mentioned earlier, bitcoin is not actually a privacy-preserving currency. But
some coins are. In particular, the best two candidates to my knowledge are two
coins called Monero and ZCash. Monero has a good developer community behind it
and is even the coin of choice for dark net drug dealers. ZCash on the other
hand has some renowned cryptographers behind it and is recommended by people
such as Edward Snowden among others. They use various different cool techniques to obscure transactions such as ring signatures and zk-SNARKs (zero-knowledge succinct non-interactive argument of knowledge). These concepts are actually pretty fascinating cryptographically, and deserve their own blog post. But approximately, ring signatures, for instance, look as follows:

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

So to conclude, after laying low for awhile, I would convert into monero or zcash, and wait, and do lots of transaction to myself to mask everything. Obviously I would use an anonymous swapping service to convert bitcoin to monero, and not just transfer on an exchange!

## 5. Tumblers and decoys

I would also use so-called bitcoin and other currency tumblers. But these aren't
nearly as good as coins like monero. So I would probably abandon a portion of
the \\$4.5bn and write a bot that simply tumbles it and sends to random address
after random address, simply to distract the Feds. It goes without saying that
everything will be over Tor.

## 6. Avoiding KYC
Now that we have monero, we need to convert it to cash. This is probably the
most dangerous part of the laundering operation (besides the actual laundering
of the cash in some Los Pollos Hermanos type of situation).

I would avoid exchanges that Know Your Customer altogether. Whether this means
using LocalMonero or finding some dodgier exchanges, I would do that. It would
be a slow and laborious process, but nothing will be more secure that literally
meeting someone to exchange Monero for cash in hand, small pieces at a time.
With new accounts every so often of course.
