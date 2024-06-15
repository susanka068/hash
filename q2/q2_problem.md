Firstly, reverse wtring S and solve this problem for prefixes. Calculate polynomial hashes on prefixes of string S. Then we need to compare all infinity extensions for each prefix with infinity extension of original string.

S[0...m) — prefix with length m and S[0...n) — prefix with length n, where n >= m. Consider an extension with length n*m. This will mean that we write a prefix S[0..m) n times in row, prefix S[0..n) — m times in row:

S[0...m)* = (1+p^m+p^(2m)+p^(3m)+p^((n-1)m))(S[0] + S[1] * p + S[2] * p^2 + ... + S[m-1] * p^(m-1)))

S[0...n)* = (1+p^n+p^(2n)+p^(3n)+p^((m-1)n))(S[0] + S[1] * p + S[2] * p^2 + ... + S[n-1] * p^(n-1)))

We can get hash on prefixes, but what we should do with sum of geometry progression?

sum(a, k) = 1+a+a^2+a^3+...+a^(k-1)

Answer sum(a, k) = (a^k-1) / (a - 1) = (a^k - 1) * inverse(a-1, mod) — wrong, because we can't calculate inverse element from even number in ring base 2^64.

Let k — even number, for example, k = 8. Then:

sum(a,8) = 1+a+a^2+a^3+a^4+a^5+a^6+a^7 = (1+a)*(1+a^2+a^4+a^6) = (1+a) * sum(a^2, 4)

Let k — odd number, for example, k = 7. Then:

sum(a,7) = 1+a+a^2+a^3+a^4+a^5+a^6 = 1 + a * (1+a+a^2+a^3+a^4+a^5) = 1 + a * sum(a, 6)

We obtained recurrence formulas that allow us to calculate the values sum(a, k) fro parameters a and k in O(log(k)) time:

sum(a, 2*k) = (1+a) * sum(a^2, k) and sum(a, 2*k+1) = 1 + a * sum(a, 2*k).

In the case of a prime module, the proposed method for calculating the sum of a geometric progression is twice as fast as sum(a, k) = (a^k - 1) * inverse(a-1, mod), since the second method calls the function of rapid exponentiation twice. In cases with hashes, the speed up in half can be critical.

Now we cam compare sum(p^m, n) * pref(m) with sum(p^n, m) * pref(n)