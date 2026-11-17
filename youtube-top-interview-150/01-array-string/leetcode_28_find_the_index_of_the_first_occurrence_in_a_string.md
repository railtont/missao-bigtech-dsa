# LeetCode 28 — Find the Index of the First Occurrence in a String

* **Problema:** [LeetCode 28](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)
* **Vídeo com a resolução:** [Episódio 23](https://youtu.be/GitJwW78Hho) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Easy *(na prática, exige técnicas avançadas para atingir complexidade ótima)*
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

Embora seja rotulado como *Easy*, este problema possui três níveis distintos de solução:

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Força Bruta (Janela Deslizante)** | $O(N \cdot M)$ | $O(1)$ | Solução base / ponto de partida na entrevista |
| **2. Rolling Hash Polinomial (Rabin-Karp)** | $O(N + M)$ | $O(1)$ | Solução ótima versátil (reaproveitável em vários problemas de string) |
| **3. KMP (Knuth-Morris-Pratt)** | $O(N + M)$ | $O(M)$ | Solução ótima determinística (sem risco de colisão de hash) |

*(Onde $N$ é o tamanho de `haystack` e $M$ é o tamanho de `needle`.)*

---

## 1. Força Bruta (Janela Deslizante) [[00:00:51]](https://youtu.be/GitJwW78Hho?t=51)

Testamos cada posição inicial `i` de `haystack` (de `0` até `n - m`) e comparamos caractere por caractere com `needle`. No pior caso (ex: `haystack = "aaaaab"` e `needle = "aaab"`), refazemos quase todas as comparações a cada deslocamento.

* **Tempo:** $O(N \cdot M)$
* **Espaço:** $O(1)$

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        n = len(haystack) # text
        m = len(needle) # word

        for i in range(0, n - m + 1):
            equal = True
            for j in range(m):
                if haystack[i + j] != needle[j]:
                    equal = False

            if equal:
                return i

        return -1


if __name__ == "__main__":
    solution = Solution()
    print(solution.strStr("xadbutsad", "sad"))
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.strStr("xadbutsad", "sad"));
    }
}

class Solution {
    public int strStr(String haystack, String needle) {
        int n = haystack.length(); // text
        int m = needle.length();   // word

        for (int i = 0; i < n - m + 1; i++) {
            boolean equal = true;
            for (int j = 0; j < m; j++) {
                if (haystack.charAt(i + j) != needle.charAt(j)) {
                    equal = false;
                }
            }

            if (equal) {
                return i;
            }
        }

        return -1;
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

class Solution {
public:
    int strStr(string haystack, string needle) {
        int n = haystack.size(); // text
        int m = needle.size();   // word

        for (int i = 0; i < n - m + 1; i++) {
            bool equal = true;
            for (int j = 0; j < m; j++) {
                if (haystack[i + j] != needle[j]) {
                    equal = false;
                }
            }

            if (equal) {
                return i;
            }
        }

        return -1;
    }
};

int main() {
    Solution solution;
    cout << solution.strStr("xadbutsad", "sad") << endl;
    return 0;
}
```

</details>

---

## 2. Rolling Hash Polinomial (Rabin-Karp) [[00:03:30]](https://youtu.be/GitJwW78Hho?t=210)

Em vez de comparar todas as letras novamente a cada deslocamento, representamos cada janela como um número usando uma **hash polinomial** com base prima $P = 31$ e módulo $MOD = 10^9 + 7$. Ao deslizar a janela para a direita, atualizamos a hash em **$O(1)$** em três passos:

1. Multiplicamos a hash atual por $P$ (deslocando as potências para a esquerda).
2. Subtraímos o caractere que saiu pela esquerda multiplicado pela potência máxima ($P^M$).
3. Somamos o novo caractere que entrou pela direita.

* **Tempo:** $O(N + M)$
* **Espaço:** $O(1)$

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def strStr(self, haystack: str, needle: str) -> int:
        # O(N+M)
        n = len(haystack)
        m = len(needle)
        if n < m:
            return -1

        needle_hash = 0
        mod = 10**9 + 7
        p = 7
        max_pow = 1

        for i in range(m):  # O(M)
            c = ord(needle[i]) - ord("a")
            needle_hash = (needle_hash * p + c) % mod

            max_pow = (max_pow * p) % mod

        cur_hash = 0
        for i in range(m):  # O(M)
            c = ord(haystack[i]) - ord("a")
            cur_hash = (cur_hash * p + c) % mod

        if cur_hash == needle_hash:
            return 0

        for i in range(1, n - m + 1):  # O(N-M)
            # shift pow
            cur_hash = (cur_hash * p) % mod

            # remove left
            c = ord(haystack[i - 1]) - ord("a")
            cur_hash = (cur_hash - (c * max_pow) % mod + mod) % mod

            # add right
            c = ord(haystack[i + m - 1]) - ord("a")
            cur_hash = (cur_hash + c) % mod

            if cur_hash == needle_hash:
                return i

        return -1


if __name__ == "__main__":
    solution = Solution()
    print(solution.strStr("xadbutsad", "sad"))
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.strStr("xadbutsad", "sad"));
    }
}

class Solution {
    public int strStr(String haystack, String needle) {
        // O(N+M)
        int n = haystack.length();
        int m = needle.length();
        if(n<m) return -1;

        long needle_hash = 0;
        long mod = (int)1e9+7;
        long pow = 7;
        long max_pow = 1;

        for(int i=0; i<m; i++){ // O(M)
            long c = needle.charAt(i) - 'a';
            needle_hash = (needle_hash*pow + c)%mod;

            max_pow = (max_pow*pow)%mod;
        }

        long cur_hash = 0;
        for(int i=0; i<m; i++){ // O(M)
            long c = haystack.charAt(i) - 'a';
            cur_hash = (cur_hash*pow + c)%mod;
        }
        if(cur_hash == needle_hash)
            return 0;

        for(int i=1; i<n-m+1; i++){ //O(N-M)
            //shift pow
            cur_hash = (cur_hash*pow)%mod;

            //remove left
            long c = haystack.charAt(i-1) - 'a';
            cur_hash = (cur_hash - (c*max_pow)%mod + mod)%mod;

            //add right
            c = haystack.charAt(i+m-1) - 'a';
            cur_hash = (cur_hash + c)%mod;

            if(cur_hash == needle_hash)
                return i;
        }
        return -1;
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

class Solution {
public:
    int strStr(string haystack, string needle) {
        // O(N+M)
        int n = haystack.size();
        int m = needle.size();
        if (n < m) return -1;

        long long needle_hash = 0;
        long long mod = 1e9 + 7;
        long long pow = 7;
        long long max_pow = 1;

        for (int i = 0; i < m; i++) { // O(M)
            long long c = needle[i] - 'a';
            needle_hash = (needle_hash * pow + c) % mod;

            max_pow = (max_pow * pow) % mod;
        }

        long long cur_hash = 0;
        for (int i = 0; i < m; i++) { // O(M)
            long long c = haystack[i] - 'a';
            cur_hash = (cur_hash * pow + c) % mod;
        }
        if (cur_hash == needle_hash)
            return 0;

        for (int i = 1; i < n - m + 1; i++) { // O(N-M)
            // shift pow
            cur_hash = (cur_hash * pow) % mod;

            // remove left
            long long c = haystack[i - 1] - 'a';
            cur_hash = (cur_hash - (c * max_pow) % mod + mod) % mod;

            // add right
            c = haystack[i + m - 1] - 'a';
            cur_hash = (cur_hash + c) % mod;

            if (cur_hash == needle_hash)
                return i;
        }
        return -1;
    }
};

int main() {
    Solution solution;
    cout << solution.strStr("xadbutsad", "sad") << endl;
    return 0;
}
```

</details>

---

## 3. KMP — Knuth-Morris-Pratt [[00:14:08]](https://youtu.be/GitJwW78Hho?t=848)

O algoritmo **KMP** pré-processa `needle` construindo o vetor **LPS** (*Longest Prefix Suffix*), que indica para onde voltar caso uma comparação falhe, evitando retroceder o ponteiro de `haystack`. Assim, percorremos o texto em uma única passada linear sem risco de colisões de hash.

* **Tempo:** $O(N + M)$
* **Espaço:** $O(M)$ para armazenar a tabela de prefixos (`lps`)

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    next_try_prefix = []

    def getNextPrefix(self, i, word):
        if self.next_try_prefix[i] != -2:  # Memoization
            return self.next_try_prefix[i]

        if i == 0:  # Base case
            self.next_try_prefix[i] = -1
            return -1

        # DP Recurssion
        try_next = self.getNextPrefix(i - 1, word)
        while try_next != -1 and word[try_next + 1] != word[i]:
            try_next = self.getNextPrefix(try_next, word)

        # Save memorization and return
        if word[try_next + 1] == word[i]:
            self.next_try_prefix[i] = try_next + 1
        else:
            self.next_try_prefix[i] = try_next

        return self.next_try_prefix[i]

    def strStr(self, haystack: str, needle: str) -> int:
        n = len(haystack)
        m = len(needle)
        self.next_try_prefix = [-2] * m

        # Compute KMP
        for i in range(m):
            self.getNextPrefix(i, needle)

        # Itera no texto (haystack) usando os sufixos da palavra (needle)
        cur_prefix = -1
        for i in range(n):
            while cur_prefix != -1 and haystack[i] != needle[cur_prefix + 1]:
                cur_prefix = self.next_try_prefix[cur_prefix]

            if needle[cur_prefix + 1] == haystack[i]:
                cur_prefix += 1

            if cur_prefix == m - 1:
                return i - m + 1

        return -1


if __name__ == "__main__":
    solution = Solution()
    print(solution.strStr("xadbutsad", "sad"))
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.strStr("xadbutsad", "sad"));
    }
}

class Solution {
    private int[] next_try_prefix;

    private int getNextPrefix(int i, String word) {
        if (next_try_prefix[i] != -2) { // Memoization
            return next_try_prefix[i];
        }

        if (i == 0) { // Base case
            next_try_prefix[i] = -1;
            return -1;
        }

        // DP Recursion
        int try_next = getNextPrefix(i - 1, word);
        while (try_next != -1 && word.charAt(try_next + 1) != word.charAt(i)) {
            try_next = getNextPrefix(try_next, word);
        }

        // Save memoization and return
        if (word.charAt(try_next + 1) == word.charAt(i)) {
            next_try_prefix[i] = try_next + 1;
        } else {
            next_try_prefix[i] = try_next;
        }

        return next_try_prefix[i];
    }

    public int strStr(String haystack, String needle) {
        int n = haystack.length();
        int m = needle.length();
        next_try_prefix = new int[m];
        Arrays.fill(next_try_prefix, -2);

        // Computes KMP
        for (int i = 0; i < m; i++) {
            getNextPrefix(i, needle);
        }

        // Itera no texto (haystack) usando os sufixos da palavra (needle)
        int cur_prefix = -1;
        for (int i = 0; i < n; i++) {
            while (cur_prefix != -1 && haystack.charAt(i) != needle.charAt(cur_prefix + 1)) {
                cur_prefix = next_try_prefix[cur_prefix];
            }

            if (needle.charAt(cur_prefix + 1) == haystack.charAt(i)) {
                cur_prefix += 1;
            }

            if (cur_prefix == m - 1) {
                return i - m + 1;
            }
        }

        return -1;
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

class Solution {
    vector<int> next_try_prefix;

    int getNextPrefix(int i, const string& word) {
        if (next_try_prefix[i] != -2) { // Memoization
            return next_try_prefix[i];
        }

        if (i == 0) { // Base case
            next_try_prefix[i] = -1;
            return -1;
        }

        // DP Recursion
        int try_next = getNextPrefix(i - 1, word);
        while (try_next != -1 && word[try_next + 1] != word[i]) {
            try_next = getNextPrefix(try_next, word);
        }

        // Save memoization and return
        if (word[try_next + 1] == word[i]) {
            next_try_prefix[i] = try_next + 1;
        } else {
            next_try_prefix[i] = try_next;
        }

        return next_try_prefix[i];
    }

public:
    int strStr(string haystack, string needle) {
        int n = haystack.size();
        int m = needle.size();
        next_try_prefix.assign(m, -2);

        // Computes KMP
        for (int i = 0; i < m; i++) {
            getNextPrefix(i, needle);
        }

        // Itera no texto (haystack) usando os sufixos da palavra (needle)
        int cur_prefix = -1;
        for (int i = 0; i < n; i++) {
            while (cur_prefix != -1 && haystack[i] != needle[cur_prefix + 1]) {
                cur_prefix = next_try_prefix[cur_prefix];
            }

            if (needle[cur_prefix + 1] == haystack[i]) {
                cur_prefix += 1;
            }

            if (cur_prefix == m - 1) {
                return i - m + 1;
            }
        }

        return -1;
    }
};

int main() {
    Solution solution;
    cout << solution.strStr("xadbutsad", "sad") << endl;
    return 0;
}
```

</details>

---
