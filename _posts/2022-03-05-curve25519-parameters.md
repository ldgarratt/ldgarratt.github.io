---
layout: post
title: "Curve25519 parameters"
date: 2022-03-05
mathjax: true
---

The parameters of curve25519 were carefully chosen, which I find quite
interesting. Let me summarise here.

* Montgomery curve $M_A, B : By^2 = x^3 + Ax^2 + x$

The Montgonery ladder arithmetic is very fast, also allowing for compressed
elliptic curve point (only the $x$ coordinate).

* $A = 486662$

$A$ should be as small as possible but still provide an order for the elliptic
curve which is greater than every 256-bit private key.

* $p = 2^{256} - 19$

The prime being close to a power of 2 makes the modulo multiplication very fast.

* Public keys $\{q \in \text{ \{0, 1, 2, $\dots ,2^{256} - 1$\}}\}$

Every 32 byte value is a possible key, which means you don't have to waste time
validating them because of the clamping (see next).

* Clamping of the private key.

The lowest 3 bits of the private key are set to 0, ensuring it is a
multiple of 8. Since every subgroup of Curve25519 has order 1, 2, 4, 8 or $r$
for large prime $r$, the fact our private key is a multiple of 8 means that if
the adversary were to send us a Diffie–Hellman share belonging to one of the small
subgroups, then the resulting DH shared key will be the identity element, leaking
nothing at all about the private key (these are called subgroup attacks). The reason for this is that private key is
essentially $8k$, and the factor of 8 is enough to ensure that the resulting
shared secret, contained in the small subgroup (because it's just a bunch of
additions of a point in the small subgroup), is ultimately some element in
the small subgroup multiplied by 8, and thus the identity element by Lagrange's
theorem. Of course, if your communication partner sends you a proper DH point in the large subgroup, then the shared secret will just end up being some multiple of $8 * 8 64$ in that large
subgroup, as intended. It won't be the identity element because 8 (or $4$, or $2$ or $1$)
is not the order of the subgroup, $r$ is.

Clamping also sets the highest bit to be the 255th bit. The reason for this is
that some implementations use a variable-time implementation of the Montgomery
ladder, where the variation in time depends on the highest bit. If the highest
bit is always the same, then even without a constant-time implementation,
the operations will still be constant time.

* Base point $P = (9, y)$

This is a nothing-up-my-sleeve number since it is just the first $x$ satisfying
the curve equation that is in the large prime order subgroup.


# Other curves

Of course, other curves are designed in similar ways, with careful justification
for the constants. For example, Curve448 chose the Solinas trinomial prime
because its form defines the golden ratio and is useful for Karatsuba
multiplication.

Other curves are a little unusual, such as Dual_EC obviously, or even secp256k1,
whith $a = FFFFFFFF00000001000000000000000000000000FFFFFFFFFFFFFFFFFFFFFFFC$ and
$b = 5AC635D8AA3A93E7B3EBBD55769886BC651D06B0CC53B0F63BCE3C3E27D2604B$.
Meanwhile bitcoin's secp256k1 just uses $a = 0$, $b = 7$.

