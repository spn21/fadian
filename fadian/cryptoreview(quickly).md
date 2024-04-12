

### 1.整数

$Z,N,N^{+}$

$ +,-,×$

$Euclidean Division$ $(a = bq + r)$

$b | a$ 

### 2.质数

[Sieve of Eratosthenes](https://zh.wikipedia.org/wiki/%E5%9F%83%E6%8B%89%E6%89%98%E6%96%AF%E7%89%B9%E5%B0%BC%E7%AD%9B%E6%B3%95)

[Lucas–Lehmer primality test](https://zh.wikipedia.org/wiki/%E5%8D%A2%E5%8D%A1%E6%96%AF-%E8%8E%B1%E9%BB%98%E6%A3%80%E9%AA%8C%E6%B3%95)

[Cyclotomic AKS test](https://zh.wikipedia.org/wiki/AKS%E8%B3%AA%E6%95%B8%E6%B8%AC%E8%A9%A6)

[Miller–Rabin primality test](https://zh.wikipedia.org/wiki/%E7%B1%B3%E5%8B%92-%E6%8B%89%E5%AE%BE%E6%A3%80%E9%AA%8C)

### 3.Euclidean

$gcd$

```python
def euclidean_algorithm(a, b):
    if a < b:
        a, b = b, a
    while b:
        a, b = b, a % b
    return a
num1 = x
num2 = y
gcd_result = euclidean_algorithm(num1, num2)
print(f'{gcd_result}')
```

### 4.Extended Euclidean algorithm

$x_{i} a + y_{i} b = r_{i}$

$gcd(a,b) = ax + by$

### 5.modular arithmetic

##### 同余

1.反身性

2.对称性

3.传递性

##### 剩余类

$Z_{n}$模$n$的剩余类 $Z_n = 0,1,2,...,n -1$

##### 基础模运算

1.平移

2.缩放

3.加法

4.乘法

##### 二次剩余

$x^2 \equiv q (mod n)$

##### 除法

##### 模逆元

$ wy \equiv 1 (modn) $

$gcd(y,n) = 1$

### 6.Chinese remainder theorem

### 7.Fermat's little theorem

$a^p \equiv a (mod p)$

### 8.euler's toitient  function

1.单元集

2.$\Phi (n) = |\mathbb{Z^{*}_n}|$

$\phi(p) = p-1$

$\phi(p^k) = p^k -p^{k-1}$

$gcd(m,n) = 1,有 \phi(mn) = \phi(m)\phi(n)$

### 9.group

##### $(G,\cdot)$

1.封闭性(closure)

2.结合律(associativity)

3.存在单位元(identity Element)

4.存在逆元(inverse Element)

有限群	无限群

##### 子群 subgroup

单位元逆元交集

陪集

正规子群

商群

​	1.封闭性

​	2.结合律

​	3.单位元

​	4.逆元素

### 10.homomorphism

​	1.单位元保持

​	2.逆元的保持

​	3.子群的保持

​	4.子群保持逆定理(子群检验方法)

$lmf(n)=f(G)$	`***$ker(f)=f^{-1}(e_H)$***

$lmf(n)$为群$H$的子群	$ker(f)$是群$G$的子群+正规子群

单同态 $Ker(f) = {e_G}$

满同态 $lm(f) = H$

群同构（双射） 

### 11.Abel群

+满足交换律

循环群 阶$ord(g) = n$	($g^n = e$)  $ord(G) = |G|$

### 12.discrete logarithm

乘法阶	原根$g^k \equiv a$ $modn$ 

DLP

### 13.Diffie-Hellman

### 14.ElGamal

### 15.ring

### 16.field

$\mathbb{Q}$	$\mathbb{R}$ 

环的性质  + 

至少2个元素 加法单位元0和乘法单位元1	

域中无零因子（为整环）且元素$a,b \subseteq F$ 且$ab = 0$,则有 $a= 0$ 或$ b = 0$

$a,b \subseteq F$ 且 $a,b$均不是零元，则有$\tfrac{1}{a} \cdot \tfrac{1}{b} = \tfrac{1}{a\cdot{b}}$

一个交换环是域当且仅当它的理想只有自身和零理想

存在正整数n使得 $0=n⋅1=1+1+...+1（其中有n个1）$，那么这样的$n$中最小的一个称为这个域的**特征**。域的特征要么是一个素数 $p$，要么是$0$（表示这样的$n$不存在）。

### 17.polynomials

polynomial ring	$R|x|$

$P(x) = \sum^{n}_{j=0}a_{j}x^{j} = a_{n}x^{n} + a_{n-1}x^{n-1} + ...+ a_1x+a_0$	集合

field extension

### 18.galois field

任何包含$p$个元素的有限域都与素域$F_p$同构

```
有限域实现
```



### 19.quadratic residue

completeness

soundness

zero-knowledge

### 20.elliptic curve

标量乘法

Double-And-Add算法

### 21.ECDLP

ECC

ECDH

EC ElGamal

ECDSA

//Frobenius自同态

//双线性配对	1.双线性	2.非退化性	3.可计算性

//torsion points/group

//divisor

weil配对

Tate配对	-> Ate配对（eth based）

### 22.IBE

