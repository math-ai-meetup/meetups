This text is based on [https://probml.github.io/pml-book/book2.html](https://probml.github.io/pml-book/book2.html)

**Scenario:**

Imagine you are observing a casino dealer who alternates between rolling a fair die (which we'll call _Die A_) and an unfair die (_Die B_). The problem is that you, as the observer, don't know which die is being used at any given time; you only get to see the outcomes of the rolls. Your task is to figure out when the dealer is switching from the fair die to the unfair one based on the sequence of observed numbers.

**Key Concepts:**

1.  **Hidden States:**

- In this example, the hidden states represent whether the dealer is using the fair die (_Die A_) or the unfair die (_Die B_). You can’t directly see which die is being used, hence it’s “hidden.”
- There are two hidden states: "Fair" and "Unfair."

2.  **Observations:**

- The observations are the outcomes of the dice rolls, which are numbers from 1 to 6.

3.  **Transition Probabilities:**

- These probabilities represent how likely the dealer is to switch between the fair and unfair dice after each roll.
- For example, there might be a 95% chance that the dealer stays with the current die and a 5% chance of switching to the other one. This captures the notion that the dealer doesn’t frequently switch dice.

4.  **Emission Probabilities:**

- These probabilities describe the likelihood of rolling a particular number given the type of die being used.
- For the fair die (_Die A_), each of the six outcomes (1 to 6) has an equal probability of 16\frac{1}{6}61​.
- For the unfair die (_Die B_), it might be biased, say rolling a '6' has a much higher probability (e.g., 50%), while other outcomes have lower probabilities.

In graph notation:

z(t-1) ---> z(t)
| |
v v
y(t-1) y(t)

Where z is the “hidden state” (which die was used), and y is the observed outcome.

Using the full probability formula:

P(A,B)=P(A)⋅P(B∣A)

P(A, B, C)=P(A)⋅P(B∣A)⋅P(C∣A,B)

The chain’s probability is:

P(y(1:T), z(1:T)) = p(z(1))\*p(z(2)|z(1))\*p(z(3)|z(2))\*…\*p(z(T)|z(T-1)) \* p(y(1)|z(1))\*(y(2)|z(2))\*…\*p(y(T)|z(T))

Our goal is to find the probability of a given roll coming from an unfair die.

There are two main ways to approach it:

- Using information only up to and including a given roll

- Using information up to and including a given roll, but also subsequent rolls.

The second case allows us to have more precise estimates. It is also referred to as “smoothing”.

Let’s start with smoothing distribution:

P(z(t)=unfair | y(1:t), y(t+1:T)) =

    = p(z(t)​=unfair, y(1:T)​) / p(y(1:T​))

    ∝ p(z(t)​=unfair, y(1:T)​)

    = p(y(t+1:T) ​,z(t)​=unfair, y(1:t​))

    = p(y(t+1:T​) ∣ z(t)​=unfair, y(1:t​)) * p(z(t)​=unfair, y(1:t​))

Recall that y(t) only depends on z(t), so y(t+1:T​) does not depend on the past values of y, if we condition on z(t), so

    = p(y(t+1:T​) ∣ z(t)​=unfair) * p(z(t)​=unfair, y(1:t​))

To calculate this, we first compute the filtering distribution p(z(t)=unfair | y(1:t)) by working forwards in time. Then we compute p(y(t+1:T) | z(t)=unfair) by working backwards in time, and then we finally combine the two.

Refer to the jupyter notebook.
