
> If X follows a Poisson distribution with parameter λ, then EX = DX.

Here:

- EX refers to the expected value (or mean) of X.
- DX refers to the variance of X.

For a Poisson distribution, both the mean and the variance are equal to λ. 

-> True

> The basis of the fundamental idea of hypothesis testing is the principle of rare events (or small probability events).

This refers to the idea in hypothesis testing that if, under a certain hypothesis (typically the null hypothesis), the observed data are extremely unlikely (or has a "small probability"), then we might reject that hypothesis.

-> True

> 20 employees from technical department are arranged in 4 rows, each with 5 people. We randomly select 4 people, and want to find the probability that all 4 selected people are from different rows.

P2 = C(20,4) = 20!/4!/16!
P1 = 5^4

-> p1/p2

> The original rule for the special prize of a certain lottery was to select 7 numbers out of 34. Now, it has been adjusted to select 7 numbers out of 34, and the last number must be a special number. After the rule adjustment, the winning rate of the special prize is approximately ( ) of the original.

-> 1/7

> Consider a soccer match between two teams: Team 0 and Team 1. It is assumed that in 65% of the matches, Team 0 wins, i.e., P(Y=0) = 0.65. In the remaining matches, Team 1 wins, i.e., P(Y=1) = 0.35. Of the matches that Team 0 wins, only 30% are on Team 1's home ground, i.e., P(X=1|Y=0) = 0.3. While for the matches that Team 1 wins, 75% are home victories, i.e., P(X=1|Y=1) = 0.75. Then, the probability that Team 1 wins at home is P(Y=1|X=1) is:

$P(X=1)=P(X=1|Y=0)*P(Y=0)+P(X=1|Y=1)*P(Y=1)=0.30*0.65+0.75*0.35$
$P(Y=1|X=1)P(X=1)=P(X=1|Y=1)P(Y=1)$

$P(Y=1|X=1)=P(X=1|Y=1)*P(Y=1)/P(X=1)=0.57$

-> 0.57


> In a certain product, the qualified rate is 85%. The probability that a qualified product is inspected as substandard is 10%. The probability that a substandard product is inspected as qualified is 5%. Question: What is the probability that a product inspected as qualified is indeed qualified?

A: event that product is qualified
B: even that product inspected as qualified

p(A|B)
= P(AB)/P(B)
= P(AB)/(P(BA)+P(B~A))
= (0.85 * 0.9）/（0.85 * 0.9+0.15 * 0.05）
= 0.99

