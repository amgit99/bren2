## <span style="color:#0b9348">Lecture 1 : : Counting and Probability</span>
Applications : Genetics, Econometrics, History(Bayes rules to figure out correctness of events), Physics(Quantum Mechanics), Finance, Gambling(origin of probability), Life.

> Statistics is the logic of uncertainty - Sir

<span style="color:#e1db3d">Sample Space</span> : The set of all possible outcomes of an experiment.
Event : A subset of the Sample Space, if any of the outcome is observed after the experiment, then the event is said to have occurred.
### Counting :
Multiplication Rule : N-stage task each stage can be done in Ai ways, the task can be done in Product A(1…N) ways.
![[Pasted image 20231208141104.png]]
## <span style="color:#0b9348">Lecture 2 : : Axioms of Probability</span>
An interesting observation : Number of ways to make 2 teams of 4 and 6 players out of 10 total players is 10 choose 4 or 10 choose 6 but if the team sizes are 5 then the number of ways become (10 choose 5)/2. think about it …

The difficult case out of the four above is the replace + order doesn't matter, in this case we relate the problem to the classic ice cream scope problem, we have to choose k scoops out of n flavors of infinite supply of ice cream. This further reduces to the sticks and stones problem interpreted as distributing the scoops over the flavors.

Interesting identities :

$$ n{n-1 \choose k-1} = k{n \choose k} $$

This one can be understood like this, i want to form a group of k people from n people and choose one of them as the leader. This can be done in two ways, choose a leader and then choose the rest n-1 people of the group. Or choose k people and then one of them becomes the group leader.

$$ {m+n \choose k}=\sum_{j=0}^{k}{m \choose j}{n \choose k-j} $$

The question behind this is, there are two groups of people with m and n people in them, then the number of ways to select k people out of them. So the obvious way is (m+n) choose k. But then considering the groups we pick j from the first group and k-j from the other group, and we do this for all js.

## <span style="color:#0b9348">Lecture 3 : : Birthday Problem, Properties of Probability</span>
The Problem : There are k people in a room, what is the probability that two people share a birthday.

if k>365 the answer is 1 (pigeonhole principle). The way to calculate for k<365 is using the counter case, the probability that k people have district birthdays which would be given by :

$$ {\dfrac{365_364_...*(365-k+1)}{365^k}} $$

this number falls quickly, when k = 23 probability is .5, k=75 probability is .97 and for k=100 the probability becomes .999994

Derangements: The number approaches 1/e.