# LeetCode 14 — Longest Common Prefix

* **Problema:** [LeetCode 14](https://leetcode.com/problems/longest-common-prefix/)
* **Vídeo com a resolução:** [Episódio 20](https://youtu.be/VCAfnaX5Qno) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Easy
* **Tópico:** Array / String

---

## Solução: Varredura Vertical por Posição [[00:01:26]](https://youtu.be/VCAfnaX5Qno?t=86)

Usamos a primeira string (`strs[0]`) como referência e percorremos coluna por coluna (`pos` de `0` até `len(strs[0]) - 1`). Para cada posição `pos`, verificamos se todas as strings do vetor possuem o mesmo caractere naquela coluna:

- Se alguma string terminar antes (`pos >= len(s)`) ou tiver um caractere diferente (`s[pos] != strs[0][pos]`), interrompemos a busca imediatamente.
- Caso todas coincidam, incrementamos `maior_prefixo`.

Ao final, retornamos o prefixo `strs[0][0:maior_prefixo]`.

* **Tempo:** $O(N \cdot M)$, onde $N$ é o número de strings em `strs` e $M$ é o comprimento da menor string (no pior caso, todas as strings são idênticas e percorremos todos os caracteres).
* **Espaço:** $O(1)$ de memória auxiliar (desconsiderando a string de saída).

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def longestCommonPrefix(self, strs: list[str]) -> str:
        maior_prefixo = 0

        for pos in range(len(strs[0])):
            tudo_igual = True

            for s in strs:
                if pos >= len(s) or s[pos] != strs[0][pos]:
                    tudo_igual = False
                    break

            if not tudo_igual:
                break

            maior_prefixo += 1

        return strs[0][0:maior_prefixo]


if __name__ == "__main__":
    print(Solution().longestCommonPrefix(["flower", "flow", "flight"]))  # "fl"
    print(Solution().longestCommonPrefix(["dog", "racecar", "car"]))     # ""
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.longestCommonPrefix(new String[]{"flower", "flow", "flight"})); // "fl"
        System.out.println(solution.longestCommonPrefix(new String[]{"dog", "racecar", "car"}));    // ""
    }
}

class Solution {
    public String longestCommonPrefix(String[] strs) {
        int maior_prefixo = 0;

        for (int pos = 0; pos < strs[0].length(); pos++) {
            boolean tudo_igual = true;

            for (String s : strs) {
                if (pos >= s.length() || s.charAt(pos) != strs[0].charAt(pos)) {
                    tudo_igual = false;
                    break;
                }
            }

            if (!tudo_igual) {
                break;
            }

            maior_prefixo += 1;
        }

        return strs[0].substring(0, maior_prefixo);
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
public:
    string longestCommonPrefix(vector<string>& strs) {
        int maior_prefixo = 0;

        for (int pos = 0; pos < (int)strs[0].size(); pos++) {
            bool tudo_igual = true;

            for (const string& s : strs) {
                if (pos >= (int)s.size() || s[pos] != strs[0][pos]) {
                    tudo_igual = false;
                    break;
                }
            }

            if (!tudo_igual) {
                break;
            }

            maior_prefixo += 1;
        }

        return strs[0].substr(0, maior_prefixo);
    }
};

int main() {
    Solution solution;
    vector<string> v1 = {"flower", "flow", "flight"};
    vector<string> v2 = {"dog", "racecar", "car"};
    cout << solution.longestCommonPrefix(v1) << endl; // "fl"
    cout << solution.longestCommonPrefix(v2) << endl; // ""
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
