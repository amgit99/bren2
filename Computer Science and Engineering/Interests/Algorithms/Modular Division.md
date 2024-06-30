The most intuitive way to imagine other operations under mod is to imagine a clock with m divisions, the hand moves around and lands on a division. Easy to see why this works for addition/subtraction/multiplication but does not really add up for division.
### The idea : 
Given a prime P, this holds : $$(\frac{a}{b}) modP \equiv  (a.b^{P-2})modP$$We can arrive at this from the Fermat's Little Theorem : 
$$
(a^{P}) \equiv a \hspace{10pt} modP
$$
$$(a^{P})modP = a$$$$(a^{P-1})modP = 1$$
$$(a^{P-1})modP = \frac{1}{a} = a^{-1}$$
From this we can easily find the value of $a^{-1}$ using binary exponentiation in $O(log \:p)$
An alternative way is as follows, this has a very tiny time complexity
```
int inv(int a, mod m) { 
	return a <= 1 ? a : m - (long long)(m/a) * inv(m % a) % m; 
}
```
