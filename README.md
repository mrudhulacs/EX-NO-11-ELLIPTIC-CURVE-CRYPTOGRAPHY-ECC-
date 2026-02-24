# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:

```
#include <stdio.h> 
 
typedef struct { 
    long long int x, y; 
} ECPoint; 
 
long long int modInv(long long int n, long long int m) { 
    long long int m0 = m, t, q; 
    long long int inv0 = 0, inv1 = 1; 
    if (m == 1) return 0; 
    while (n > 1) { 
        q = n / m; 
        t = m; 
        m = n % m, n = t; 
        t = inv0; 
        inv0 = inv1 - q * inv0; 
        inv1 = t; 
    } 
    if (inv1 < 0) inv1 += m0; 
    return inv1; 
} 
 
ECPoint ecAdd(ECPoint P, ECPoint Q, long long int a, long long int p) { 
    ECPoint R; 
    long long int slope; 
     
    if (P.x == Q.x && P.y == Q.y) { 
        slope = (3 * P.x * P.x + a) * modInv(2 * P.y, p) % p; 
    } else { 
        slope = (Q.y - P.y) * modInv(Q.x - P.x, p) % p; 
    } 
 
    R.x = (slope * slope - P.x - Q.x) % p; 
    R.y = (slope * (P.x - R.x) - P.y) % p; 
 
    R.x = (R.x + p) % p; 
    R.y = (R.y + p) % p; 
 
    return R; 
} 
 
ECPoint ecMul(ECPoint P, long long int k, long long int a, long long int p) { 
    ECPoint R = P; 
    k--; 
    while (k > 0) { 
        R = ecAdd(R, P, a, p); 
        k--; 
    } 
    return R; 
} 
 
int main() { 
    long long int p, a, b, priv1, priv2; 
    ECPoint base, pub1, pub2, shared1, shared2; 
 
    prin ("Enter the prime number (p): "); 
    scanf("%lld", &p); 
    prin ("Enter the curve parameters (a and b) for y^2 = x^3 + ax + b: "); 
    scanf("%lld %lld", &a, &b); 
    prin ("Enter the base point coordinates (x and y): "); 
    scanf("%lld %lld", &base.x, &base.y); 
 
    prin ("Enter first par cipant's private key: "); 
    scanf("%lld", &priv1); 
    prin ("Enter second par cipant's private key: "); 
    scanf("%lld", &priv2); 
 
    pub1 = ecMul(base, priv1, a, p); 
    pub2 = ecMul(base, priv2, a, p); 
 
    prin ("First par cipant's public key: (%lld, %lld)\n", pub1.x, pub1.y); 
    prin ("Second par cipant's public key: (%lld, %lld)\n", pub2.x, pub2.y); 
 
    shared1 = ecMul(pub2, priv1, a, p); 
    shared2 = ecMul(pub1, priv2, a, p); 
 
    prin ("Shared secret (First par cipant): (%lld, %lld)\n", shared1.x, 
shared1.y); 
    prin ("Shared secret (Second par cipant): (%lld, %lld)\n", shared2.x, 
shared2.y); 
 
    if (shared1.x == shared2.x && shared1.y == shared2.y) 
        prin ("Key exchange successful. Shared secrets match!\n"); 
    else 
        prin ("Key exchange failed. Shared secrets do not match.\n"); 
 
    return 0; 
} 
 

```



## Output:

<img width="1747" height="903" alt="image" src="https://github.com/user-attachments/assets/93dfaffe-2800-4f65-b25e-05ac5599bb8b" />


## Result:
The program is executed successfully

