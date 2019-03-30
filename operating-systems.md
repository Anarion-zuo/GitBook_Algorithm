# Operating Systems

## Basics of Information

### Entropy

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
Suppose we have a data sequence describing the values of the random variable $X$, the average number of bits used to transmit our choices/data had better be equal to the entropy of the variable $X$. A large difference between entropy and average number of bits may imply inconsistency between the coding mechanism and the real world and may cause low efficiency or errors.

### Ambiguousity

Typically, when some way of coding may suggest ambiguous result, it is a bad way. For example, when we try to encode text with only 4 possibility of characters A, B, C, D, if use the following law, “0110” is ambiguous.

| A    | B    | C    | D    |
| ---- | ---- | ---- | ---- |
| 0    | 1    | 10   | 11   |

0110 may suggest “ABBA” or “ABC” or “ADA”.

To check whether the encoding is ambiguous, we draw a binary tree/decision tree to show the possible outcomes of decoding. 

![1553938967130](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1553938967130.png)

An unambiguous encoding is a binary tree with children with same depth. This is a typical ambiguous encoding.

### Variable-length Encodings

When encoding data we would like to match the length of the encoding to the information content of the data. For objects with higher probability of appearance, we give them shorter encodings, while longer for lower ones. This may save entropy and efficiency in the real world. Ideally, a perfect encoding would suggest encoding entropy equals real entropy, namely, the number of states of encoding equals the number of states of the real objects.

### Huffman’s Algorithm

The algorithm is for a given set of symbols and their probabilities, construct an optimal variable-lengths encoding.

- Build subtree using 2 symbols with lowest $p_i$.
- At each step, choose 2 symbols/subtrees with lowest $p_i$ combine to form new subtree.
- And it repeats.

The result is a optimal tree built from the bottom up.

![1553940340173](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1553940340173.png)

By the properties of probability, we can do better by encoding combinations of the symbols, instead of encoding single symbols.

### Hamming Distance

The factor describes the number of positions in which the corresponding digits differ in 2 encodings of the length.

### Parity Check to Detect Single(odd)-bit Errors

A parity bit can be added to any length message and is chosen to make the total number of 1 bits even (make a even parity). To check for a single-bit error/odd number of errors, count the number of 1s in the received message and if it is add, there has been an error.

To detect $E$ errors, we need a minimum Hamming Distance of $E+1$ between code words.