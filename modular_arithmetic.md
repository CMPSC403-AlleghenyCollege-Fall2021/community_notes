# Activity 12

## For each function, add a formula to calculate `mod` and an example with numbers.

## Team 1 (front): Megan Munzek, Tyler Pham, Ramon Guzman, Nolan Thompson, Caden, Claire, Wes, Kiley, Chris

According to [Khan Academy](https://www.khanacademy.org/computing/computer-science/cryptography/modarithmetic/a/modular-addition-and-subtraction), "(A + B) mod C = (A mod C + B mod C) mod C". This way, if we want to calculate:

- addition

(3 + 4) mod 2 = 7 mod 2 = 1
(3 mod 2 + 4 mod 2) mod 2 = 1

- subtraction

(A-B) mod C = (A mod C-B mod C) mod C

- multiplication

(A * B) mod C = (A mod C * B mod C) mod C


## Team 2 (back window): Michael, Mai, Maddy, Rachael, Hannah

Division/inverse

The inverse of x (mod n) is asking what integer element of [1, n-1] maps “x” back to the identity element, 1. So, we need to find an integer y such that xy = 1 (mod n).  This only works if gcd(x, n) = 1.  Otherwise, x doesn’t have an inverse (mod n).

Solving the equation isn’t quite as easy as it looks, we can’t just divide 1 by x and get y -- we are working under a modulus.  Instead, we use the Extended Euclidean Algorithm to find y, which will be an integer in the closed interval [1, n-1].

Algorithm: ax+by=gcd(a,b)

## Team 3 (back wall): Bailey, Wonjoon, Adam, Adriana, Pedro

**Exponentiation**

Exponentiation is the concept of a base e, where 'e' is eular's number (aka 2.178....), that is to a variable a so that e^a.
In python, exponentiation can be utilized by using exp(a) where exp is eulars number and in the parenthesis is ^a
in general, exponentiation is basically a base number mutiplied by itself as many times as the raised number.
Base e, eulars number also relates

e is relation to the e in the log equation and the two ways to write log
algorithm

Solve g^a(mod d) using modular exponentiation

We start by building an algorithm where n is our exponent = a, y = d and u ≡ g mod d, shown as the modulus is the remainder piece after g is divided by a (shown below):

g^a mod d = g mod d

Example with numbers:

Assuming we have to calculate 3^17(mod 25):
  - Conovert 17 to binary: **(17)^2 = 10001**
  - Make the table:
| (17^2) | c_0 = 1                                      |
| ------ | -------------------------------------------- |
| 1      | c_1 = 1^2 * 3^1  = 1 * 3  = 3 mod 25         |
| 0      | c_2 = 3^2 * 3^0 = 9 * 1 = 9 mod 25           |
| 0      | c_3 = 9^2 * 3^0 = 81 * 1 = 81 = 6 mod 25     |
| 0      | c_4 = 6^2 * 3^0 = 36 * 1 = 36 = 11 mod 25    |
| 1      | c_5 = 11^2 * 3^1 = 121 * 3 = 363 = 13 mod 25 |

Ending result: 3^17 = 13(mod 25)
