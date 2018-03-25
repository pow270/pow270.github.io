---
layout: post
title:  "SecurinetsQuals2018 - Improve the quality"
author: "G0d3l"
tags: SecurinetsCTFQuals2018 crypto
use_math: true
---

>  Hello Every one,
We didn't know what to do, so we are asking for your help.

> A friend of us sent us the following text:

> I used an elliptic curve encrytion for the first time.
The only thing that i kown about elliptic curve is that a number K must always be hidden.
so i made multiple encryption to send some information.

> Here is all the informations about the elliptic curve that i used excep the K number.

> The elliptic curve is :
y^2 = x^3 + A*x + B
A = 658974
Sorry i forget the B :/ , I just remember that it's most significant number is  6

> As an order of a finite field must be a prime power, i used p = 962280654317 (FiniteField(p)).
as a starter point, i used the generator G for this elliptic curve: (518459267012 : 339109212996 : 1)
and each time i reuse it to encrypt again

> let my secret message be K .
for exemple I divided my K to 2 elements k1 and k2
then Q1 is k1*G
and Q2 is k2*G

> here are the Qi that i got:

> [(656055339629 : 670956206845 : 1),
(714432985374 : 30697818482 : 1),
(519532969453 : 833497145865 : 1),
(606806384185 : 353033449641 : 1),
(370553209582 : 211121736115 : 1),
(95617246846 : 666814491609 : 1),
(474872055371 : 795112698430 : 1),
(249845085299 : 222352033875 : 1),
(850954431245 : 810446463695 : 1),
(188731559428 : 877002121896 : 1),
(168665615402 : 464872506873 : 1),
(26722558561 : 269217869309 : 1),
(16403346294 : 478534963882 : 1),
(539749282946 : 332444159141 : 1),
(932295517649 : 23439478940 : 1),
(765194933041 : 920187938377 : 1),
(853124087439 : 845601917928 : 1),
(246454416048 : 212483699689 : 1),
(312547608490 : 688107262695 : 1),
(43261158649 : 439444472742 : 1),
(320785434805 : 477080449838 : 1),
(741706320740 : 672809544395 : 1),
(361762297756 : 858805805323 : 1),
(782235980044 : 600673464737 : 1),
(69196762074 : 327427680437 : 1),
(876001563166 : 573218279075 : 1),
(117946101727 : 954797129239 : 1),
(771781111553 : 314018907599 : 1),
(579549799021 : 322325160055 : 1),
(857081196493 : 464260539273 : 1),
(852938568103 : 429083796488 : 1),
(850954431245 : 810446463695 : 1),
(55203632714 : 255470537391 : 1),
(600464434215 : 605840305721 : 1),
(620532163623 : 575613893944 : 1),
(215810002861 : 481354983411 : 1),
(538481263994 : 666638294130 : 1),
(528666082457 : 895034116069 : 1),
(296218553972 : 899557390183 : 1),
(428618251485 : 445768511836 : 1),
(632412058600 : 685699421425 : 1),
(634041855232 : 495546745721 : 1),
(570481762204 : 252944477333 : 1),
(760959783781 : 435626456209 : 1)]


## Steps
- Finding b by simple deduction from the elliptic curve equation.
- Solve the Elliptic Curve Discrete Logarithm Problem (Qi=kiG).
- Split K and Convert it ASCII.
- Get the missing part of the flag from a picture.


## Details

#### Step 1

We have $$ y^2 = x^3 + A*x + B % p $$

So, $$ B = y^2 - x^3 - A*x % p $$

We used the generator point,

{% highlight python %}
G = (518459267012, 339109212996)
x, y, a, p = G[0], G[1], 658974, 962280654317
b = (y**2 - x**3 - a*x) % p
{% endhighlight %}

We got b = 618. Verified by the information that:
>  I just remember that it's most significant number is  6

#### Step 2

For the rest we use SageMath which is a free open-source mathematics software system on a cloud plateform called Cocalc.

We defined the curve over a finite field of integers modulo p and G and some other point from the points given in the description:

{% highlight python %}
Q = (656055339629, 670956206845)
G = (518459267012, 339109212996)
a, b, p = 658974, 618, 962280654317
Fp = FiniteField(p)
E = EllipticCurve(Fp,[a,b])
G = E.point(G)
Q = E.point(Q)
{% endhighlight %}

We searched for ways attacks against ECDLP. Then, by factoring the order of E we found small prime factors; an indicator that we can use the Pohlig-Hellman algorithm.

{% highlight python %}
factor(E.order())
{% endhighlight %}
> 19 * 23 * 31 * 71032757

We implemented the brute-force Algorithm:

{% highlight python %}
G = (518459267012, 339109212996)
a, b, p = 658974, 618, 962280654317
Fp = FiniteField(p)
E = EllipticCurve(Fp,[a,b])
G = E.point(G)
pts = [(656055339629, 670956206845),
(714432985374, 30697818482),
(519532969453, 833497145865),
(606806384185, 353033449641),
(370553209582, 211121736115),
(95617246846, 666814491609),
(474872055371, 795112698430),
(249845085299, 222352033875),
(850954431245, 810446463695),
(188731559428, 877002121896),
(168665615402, 464872506873),
(26722558561, 269217869309),
(16403346294, 478534963882),
(539749282946, 332444159141),
(932295517649, 23439478940),
(765194933041, 920187938377),
(853124087439, 845601917928),
(246454416048, 212483699689),
(312547608490, 688107262695),
(43261158649, 439444472742),
(320785434805, 477080449838),
(741706320740, 672809544395),
(361762297756, 858805805323),
(782235980044, 600673464737),
(69196762074, 327427680437),
(876001563166, 573218279075),
(117946101727, 954797129239),
(771781111553, 314018907599),
(579549799021, 322325160055),
(857081196493, 464260539273),
(852938568103, 429083796488),
(850954431245, 810446463695),
(55203632714, 255470537391),
(600464434215, 605840305721),
(620532163623, 575613893944),
(215810002861, 481354983411),
(538481263994, 666638294130),
(528666082457, 895034116069),
(296218553972, 899557390183),
(428618251485, 445768511836),
(632412058600, 685699421425),
(634041855232, 495546745721),
(570481762204, 252944477333),
(760959783781, 435626456209)]

s = ""
for Q in pts:
    Q = E.point(Q)
    factors, exponents = zip(\*factor(E.order()))
    primes = [factors[i] ^ exponents[i] for i in range(len(factors))]
    discretelogs = []
    for prime in primes:
        h = int(G.order()) / int(prime)
        discretelog = discrete_log(h*Q,h*G,operation="+")
        discretelogs += [discretelog]
    l = crt(discretelogs,primes)
    s = s+str(l)

print(s)
{% endhighlight %}

After few trials we got:
> 677978866982843284727383328479327679876982326765836932707382838432581084727383327377
657169326779788465737883328472693270766571443284828932847932716984327384108472693283
856677738484696832707665713277858384326669327378328472738332707982776584583210707665
714569679187726584328979853976763270737868327378328472693273776571699310737765716932
858276581072848480584747678289808479466784708369678582737869848346677977474947838469
71458065828446807871

#### Step 3

We checked for hints and we found:
> When you find the numbers just divide it to list of strings of length 2

So we added:
{% highlight python %}
print(" ".join([s[i:i + 2] for i in range(0, len(s), 2)]))
{% endhighlight %}

In ASCII,
> CONVERT THIS TO LOWER CASE FIRST :
THIS IMAGE CONTAINS THE FLAG, TRY TO GET IT
THE SUBMITTED FLAG MUST BE IN THIS FORMAT:
FLAG-EC[WHAT YOU'LL FIND IN THE IMAGE]
IMAGE URL:
HTTP://CRYPTO.CTFSECURINETS.COM/1/STEG-PART.PNG

#### Step4

Due to the high resolution, in my screen the flag was already clear xD. I guess that was a troll.

The flag is: flag-ec[EC_St!e-g1(a)no]
