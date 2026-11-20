# LeetCode 13 — Roman to Integer

* **Problema:** [LeetCode 13](https://leetcode.com/problems/roman-to-integer/)
* **Vídeo com a resolução:** [Episódio 17](https://youtu.be/0txKBdiEORM) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Easy
* **Tópico:** Array / String

---

## Solução: Varredura Linear com Regra de Subtração [[00:01:17]](https://youtu.be/0txKBdiEORM?t=77)

Nos numerais romanos, os símbolos normalmente aparecem em ordem decrescente da esquerda para a direita (ex: `VI` = $5 + 1 = 6$). A exceção ocorre nos casos de subtração (`IV`, `IX`, `XL`, `XC`, `CD`, `CM`), em que um símbolo de valor menor aparece imediatamente antes de um símbolo de valor maior.

Portanto, percorremos a string comparando o símbolo atual `s[i]` com o próximo `s[i + 1]`:
- Se `valor(s[i]) < valor(s[i + 1])`, **subtraímos** `valor(s[i])` da resposta.
- Caso contrário, **somamos** `valor(s[i])` à resposta.

* **Tempo:** $O(N)$, onde $N$ é o tamanho da string `s` (no domínio do problema, `1 <= valor <= 3999`, o tamanho máximo é constante, logo também pode ser considerado $O(1)$).
* **Espaço:** $O(1)$, pois o dicionário armazena sempre os mesmos 7 símbolos romanos.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def romanToInt(self, s: str) -> int:
        roman_dict = {"I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000}

        n = len(s)
        ans = 0
        for i in range(n):
            # Se o símbolo atual for menor que o próximo, aplica a regra de subtração
            if i + 1 < n and roman_dict[s[i]] < roman_dict[s[i + 1]]:
                ans -= roman_dict[s[i]]
            else:
                ans += roman_dict[s[i]]

        return ans


if __name__ == "__main__":
    print(Solution().romanToInt("III"))      # 3
    print(Solution().romanToInt("LVIII"))    # 58
    print(Solution().romanToInt("MCMXCIV"))  # 1994
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.Map;

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.romanToInt("III"));     // 3
        System.out.println(solution.romanToInt("LVIII"));   // 58
        System.out.println(solution.romanToInt("MCMXCIV")); // 1994
    }
}

class Solution {
    public int romanToInt(String s) {
        Map<Character, Integer> romanDict = Map.of(
            'I', 1,
            'V', 5,
            'X', 10,
            'L', 50,
            'C', 100,
            'D', 500,
            'M', 1000
        );

        int n = s.length();
        int ans = 0;
        for (int i = 0; i < n; i++) {
            // Se o símbolo atual for menor que o próximo, aplica a regra de subtração
            if (i + 1 < n && romanDict.get(s.charAt(i)) < romanDict.get(s.charAt(i + 1))) {
                ans -= romanDict.get(s.charAt(i));
            } else {
                ans += romanDict.get(s.charAt(i));
            }
        }

        return ans;
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <iostream>
#include <string>
#include <unordered_map>
using namespace std;

class Solution {
public:
    int romanToInt(string s) {
        unordered_map<char, int> roman_dict = {
            {'I', 1}, {'V', 5}, {'X', 10}, {'L', 50},
            {'C', 100}, {'D', 500}, {'M', 1000}
        };

        int n = s.size();
        int ans = 0;
        for (int i = 0; i < n; i++) {
            // Se o símbolo atual for menor que o próximo, aplica a regra de subtração
            if (i + 1 < n && roman_dict[s[i]] < roman_dict[s[i + 1]]) {
                ans -= roman_dict[s[i]];
            } else {
                ans += roman_dict[s[i]];
            }
        }

        return ans;
    }
};

int main() {
    Solution solution;
    cout << solution.romanToInt("III") << endl;     // 3
    cout << solution.romanToInt("LVIII") << endl;   // 58
    cout << solution.romanToInt("MCMXCIV") << endl; // 1994
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
