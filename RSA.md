# HOW TO SOLVE RSA PROBLEM

For those who didn't know how RSA works yet, here's the basic you need to know:

Determine _p_ and _q_, both must be a prime number.  
Calculate _n = pq_  
Calculate _phi_ =  (_p_-1)(_q_-1)  
Determine _e_, most of the time _e_ is between 3 to 65537. We'll discuss challenge with large _e_ and it's vulnerability later  
Determine _d_ as the modular multiplicative inverse of _e_ modulo _phi_, you don't really need to know how this actually works if  
you're a beginner, just find a script that can do it for you  
  
To encrypt a message _M_, do:  
_C_ = _M_^_e_ % _n_  

To decrypt a ciphertext _C_, do:  
_M_ = _C_^_d_ % _n_  

(_n_,_e_) is the public key while (_n_,_d_) is the private key. The rest of the value are kept secret since it can be used to calculate _d_


## THE CHALLENGE  
Most RSA Challenge gives you a public key and a ciphertext to decrypt, however we don't have the private key so we can't decrypt it right away.  
So, how to recover _d_ from only _n_ and _e_?  

If you remember how RSA works we only need _p_ and _q_ to calculate phi and use _phi_ and _e_ to recover _d_, so, the way we can find  
_p_ and _q_ is through factoring _n_. Unfortunately sometimes _n_ are just to big to factor by bruteforcing. so we have to find another alternative.  
There is a very nice [database](www.factordb.com) that stores factors of a number, the challenge author will intentionally store the factors of _n_ in the challenges so us player can find it.  
But what if factordb can't find the factor in the database? that means the challenge author need you to find another intentional flaw on the number.   
[alpetron](https://www.alpertron.com.ar/ECM.HTM) os a very nice website to help you find the flaw on the number.  

list of common flaw in factors of _n_:  
1. _p_ and _q_ are the same, the factor can be found by square rooting n.  
2. _p_ and _q_ values are too close, this makes _n_ = (p)(p+x), bruteforcing x would not took too much time.  
3. instead of using only 2 prime numbers, it uses a lot of small prime numbers, this could also factored easily.  
4. either _p_ or _q_ are too small, e.g _p_ = 3 or _q_ = 5.  

that was the very basic concept on decrypting a ciphertext using RSA by recovering the private key.

## WHAT IF FACTORING DIDN'T WORK  
the first thing you gonna do is to identify any values that you have, let's take a look at this example:  
```
n : 3398743732304737633847385920138274382130284237583 //not a legit modulo btw  
e : 3  
c : 903659261103750 //also not a legit ciphertext  
```  
as you can see at the example above c is much smaller than n, e is also very small, therefore when we try _c_ = m^e % n, there is a chance where m^e is very small that the _n_ is not even used. So we can recover the message by cube rooting _c_  
there is a lot of variation to this, for example the author can intentionally encrypt with a private key, and give you the public key. So you can do c^e % n and decrypt the ciphertext. You can identify this type of thing by reading the description of the challenge.  

## WHAT IF THE RSA LOOKS A LITTLE PECULIAR
- if _e_ is a large number, _d_ could be small or swapped with _e_,you can try to decrypt with _d_ = 65537, or perform [Wiener's attack](https://en.wikipedia.org/wiki/Wiener%27s_attack).  
- if there are _x_ same message but padded with small different padding. then [Coppersmith’s short-pad attack](https://en.wikipedia.org/wiki/Coppersmith%27s_attack#Coppersmith%E2%80%99s_short-pad_attack) is possible.  
- if there are _x_ same message encrypted with different _n_, if x > e, then [Håstad's broadcast attack](https://en.wikipedia.org/wiki/Coppersmith%27s_attack#H%C3%A5stad%27s_broadcast_attack) is possible.  
- if the challenge require you to factor _n_ given private exponent _d_ [This algorithm](https://www.di-mgt.com.au/rsa_factorize_n.html) can be used.  
- if there are 2 public key, you can do gcd(n1,n2) to find if they share the same factor  

## WHAT IF THE RSA LOOKS NORMAL
You maybe get a source code of the challenge generator, you need to find the vulnerability there, just try to look for any other thing you have if you've read everything in this page and non of them works.  




