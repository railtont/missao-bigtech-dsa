# LeetCode 6 — Zigzag Conversion

* **Problema:** [LeetCode 6](https://leetcode.com/problems/zigzag-conversion/)
* **Vídeo com a resolução:** [Episódio 22](https://youtu.be/VXIz6PYfChI) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Medium
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Simulação Linha a Linha** | $O(N)$ | $O(N)$ | Solução limpa e intuitiva evitando fórmulas de índices de ciclo |

---

## Solução 1: Simulação Linha a Linha

Em vez de construir uma matriz bidimensional com espaços vazios, criamos um vetor com `numRows` acumuladores de caracteres (um para cada linha) e simulamos o movimento em zigue-zague usando duas variáveis de controle:

- `curRow`: linha atual (começa em `0`).
- `goingDown`: direção do movimento (`true` ao descer, `false` ao subir).

A cada caractere de `s`, inserimos na linha `curRow`. Ao atingir a primeira linha (`curRow == 0`), mudamos a direção para descer (`goingDown = true`); ao atingir a última linha (`curRow == numRows - 1`), mudamos para subir (`goingDown = false`). Ao final, concatenamos todas as linhas em ordem.

* **Tempo:** $O(N)$, onde $N$ é o comprimento da string `s` (cada caractere é inserido em uma linha e depois concatenado uma única vez).
* **Espaço:** $O(N)$, para armazenar os caracteres distribuídos entre as `numRows` linhas e a string final.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows <= 1:
            return s

        rows = [[] for _ in range(numRows)]
        cur_row = 0
        going_down = True

        for ch in s:
            rows[cur_row].append(ch)

            # Inverte a direção ao atingir o topo ou a base do zigue-zague
            if cur_row == 0:
                going_down = True
            elif cur_row == numRows - 1:
                going_down = False

            if going_down:
                cur_row += 1
            else:
                cur_row -= 1

        return "".join("".join(row) for row in rows)


if __name__ == "__main__":
    solution = Solution()
    print(solution.convert("PAYPALISHIRING", 3))  # PAHNAPLSIIGYIR
    print(solution.convert("PAYPALISHIRING", 4))  # PINALSIGYAHRPI
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.convert("PAYPALISHIRING", 3)); // PAHNAPLSIIGYIR
        System.out.println(solution.convert("PAYPALISHIRING", 4)); // PINALSIGYAHRPI
    }
}

class Solution {
    public String convert(String s, int numRows) {
        if (numRows <= 1) return s;

        int n = s.length();

        StringBuilder[] rows = new StringBuilder[numRows];
        for (int i = 0; i < numRows; i++) {
            rows[i] = new StringBuilder();
        }

        int curRow = 0;
        boolean goingDown = true;

        for (int i = 0; i < n; i++) {
            rows[curRow].append(s.charAt(i));

            // Inverte a direção ao atingir o topo ou a base do zigue-zague
            if (curRow == 0) {
                goingDown = true;
            } else if (curRow == numRows - 1) {
                goingDown = false;
            }

            if (goingDown) {
                curRow += 1;
            } else {
                curRow -= 1;
            }
        }

        StringBuilder answer = new StringBuilder();
        for (int i = 0; i < numRows; i++) {
            answer.append(rows[i]);
        }
        return answer.toString();
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
    string convert(string s, int numRows) {
        if (numRows <= 1) return s;

        int n = s.size();
        vector<string> rows(numRows);

        int curRow = 0;
        bool goingDown = true;

        for (int i = 0; i < n; i++) {
            rows[curRow].push_back(s[i]);

            // Inverte a direção ao atingir o topo ou a base do zigue-zague
            if (curRow == 0) {
                goingDown = true;
            } else if (curRow == numRows - 1) {
                goingDown = false;
            }

            if (goingDown) {
                curRow += 1;
            } else {
                curRow -= 1;
            }
        }

        string answer;
        for (int i = 0; i < numRows; i++) {
            answer += rows[i];
        }
        return answer;
    }
};

int main() {
    Solution solution;
    cout << solution.convert("PAYPALISHIRING", 3) << endl; // PAHNAPLSIIGYIR
    cout << solution.convert("PAYPALISHIRING", 4) << endl; // PINALSIGYAHRPI
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
