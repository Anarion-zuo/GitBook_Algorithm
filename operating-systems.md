# Operating Systems

## Basics of Information

For any kind of information, it can be described by a certain discrete random variable $X$.

| $x_1$ | $x_2$ | …    | $x_N$ |
| ----- | ----- | ---- | ----- |
| $p_1$ | $p_2$ | …    | $p_N$ |

Hereby, we define uncertainty to be 1 over probability, so that higher the probability, less the uncertainty is. Furthermore, in order to measure uncertainty in bits, we take a logarithm based 2.
$$
I(x_i)=-\log_2p_i
$$
When facing $N$ choices and receiving data to narrow down the choices into $M$, say by telling you the symbol of the card is the heart, we measure the magnitude of such data in the form of:
$$
I(data)=\log_2\frac{1}{M\cdot(1/N)}=\log_2\frac{N}{M}
$$
In an example of flipping coins, $N=2,M=1$ , the magnitude of information content is $2$, which in binary, $\log_22=1​$ bit.

Entropy is the average amount of information contained in each piece of data received about the value of $X$.
$$
H(X)=E(I(X))=-\sum_ip_i\log_2p_i
$$
Suppose we have a data sequence describing the values of the random variable $X$, the average number of bits used to transmit our choices/data had better be equal to the entropy of the variable $X$. A large difference between entropy and average number of bits may imply inconsistency between the coding mechanism and the real world and may cause low efficiency.