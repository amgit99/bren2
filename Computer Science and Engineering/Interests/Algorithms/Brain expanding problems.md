## Problem 1 :

There is a grid with the hero in a cell and monsters in other cells, the hero and the monsters can move up, down, left, right. First the hero takes a step, then the monsters, can the monsters ever be in the same cell as that of the hero. A move must be made by the hero and the monsters every time. The monsters simply need to catch the hero by being in the same cell.

<span style="color:#0b9348">Answer </span>: : treat the grid like a chess board, if in the start any of the monsters and the hero are on the same colour, the the hero can be caught, otherwise no.
## Problem 2 :

There are particles in a line with some value of charges, charges range from -1e9 to 1e9. We have a gun with which we can destroy a particle, this causes the surrounding particles to merge and their charges are added. If we destroy particle _i_then particles i+1 and i-1 merge to form a new particle with their charges summed. Shooting the first and the last will not do anything to the surroundings. The particle will simply be destroyed. Your goal is to shoot down particles till there is just one remaining, what is the maximum value particle that you can get.

<span style="color:#0b9348">Answer</span> : : If we notice the pattern in which the particles merge on being shot with the gun, for any order of shooting, when particles merge, if we note their original positions, the ones that have same parity will merge into one, this is because the particles i+1 and i-1 have same parity for the first shooting and after this the new particle is a mix of same parity particles. and odd particle is destroyed and an ever particles merges into another even particle. So a total of 1 for each parity and the order is preserved. So in the end the remaining particle will be a summation of either even positioned particles( a subset ) or odd positioned particles.

Further it can be proved that we can shoot down the particles in such a way that only positive ones survive, so the answer is the maximum of the positive particles in the odd or the even positions in the beginning of the array.
## Problem 3 :

You have to perform the following sum, given an array of size 1e5, so an N^2 solution is not acceptable.
![[Pasted image 20231211180410.png]]
<span style="color:#0b9348">Answer</span> : : The obvious answer is O(n^3). We need O(n), there is not way we divide this logarithmically so that we could get O(n*Log(n)). We can reduce the above formula, the Xj look can be considered to be the outermost loop, leaving us with calculating the sum over Xi and Xk. This we must do in constant time. Further the two nested inner summations can be simplified is a product of two summations. It is simply a product of two terms that are independent. Both having the constant Xj from the outermost loop.

For the remaining calculation we preprocess the array as follows, we calculate for every bit positions, how many numbers have the said bit set. Doing this we have the count of numbers that have the first second….sixty fourth bit set. This makes the constant time AND and OR operation possible. We can calculate the entire summation by knowing how many 1s will end up in the answer and acc to their position add their contribution up.
## Problem 4 : 1822D Super Permutations

The natural sum formula under mod is surprising and subtle, `𝑛⋅(𝑛+1)/2 mod 𝑛 `will be 0 if n is odd and non zero when n is even, so the answer of finding the super permutation is only for even values of n. Furthermore the super permutation follows a pattern. 
## Problem 5 : 1822D Super Permutations
