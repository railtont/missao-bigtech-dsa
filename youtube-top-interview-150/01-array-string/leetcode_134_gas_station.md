# LeetCode 134 — Gas Station

* **Problema:** [LeetCode 134](https://leetcode.com/problems/gas-station/)
* **Vídeo com a resolução:** [Episódio 14](https://youtu.be/Y2QzhSLwGfI) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Medium
* **Tópico:** Array / String

---

## Solução: Abordagem Gulosa (Menor Acumulado) [[00:01:08]](https://youtu.be/Y2QzhSLwGfI?t=68)

Se acompanharmos o saldo acumulado de combustível `acc += gas[i] - cost[i]` ao longo de todo o circuito a partir do índice `0`, o ponto de partida ótimo para completar a volta sem ficar negativo é **logo após o índice onde esse acumulado atinge o seu menor valor** (`(smaller_pos + 1) % n`).

A solução funciona em duas passagens lineares:

1. **Encontrar o ponto mínimo:** percorremos o circuito acumulando `gas[i] - cost[i]` e guardamos `smaller_pos`, a posição onde `acc` assume o menor valor (`acc <= smaller_value`).
2. **Validar a volta completa:** iniciando em `(smaller_pos + 1) % n`, simulamos a viagem pelos `n` postos. Se em algum momento o saldo `acc` ficar negativo, não existe solução e retornamos `-1`; caso contrário, retornamos o índice de partida encontrado.

* **Tempo:** $O(N)$, onde $N$ é o número de postos (duas passagens lineares pelo array).
* **Espaço:** $O(1)$, usando apenas variáveis auxiliares constantes.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def canCompleteCircuit(self, gas: list[int], cost: list[int]) -> int:
        n = len(gas)

        # Encontra a posição do menor valor do padrão
        acc = 0
        smaller_value = 0
        smaller_pos = -1
        for i in range(n):
            acc = acc + gas[i] - cost[i]
            if acc <= smaller_value:
                smaller_value = acc
                smaller_pos = i

        # Testa se partindo dessa posição é possível
        acc = 0
        smaller_pos = (smaller_pos + 1) % n
        for i in range(n):
            pos = (smaller_pos + i) % n
            acc = acc + gas[pos] - cost[pos]
            if acc < 0:
                return -1

        return smaller_pos


if __name__ == "__main__":
    solution = Solution()
    print(solution.canCompleteCircuit([1, 2, 3, 4, 5], [3, 4, 5, 1, 2]))  # 3
    print(solution.canCompleteCircuit([2, 3, 4], [3, 4, 3]))              # -1
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.canCompleteCircuit(
            new int[]{1, 2, 3, 4, 5}, new int[]{3, 4, 5, 1, 2}
        )); // 3
        System.out.println(solution.canCompleteCircuit(
            new int[]{2, 3, 4}, new int[]{3, 4, 3}
        )); // -1
    }
}

class Solution {
    public int canCompleteCircuit(int[] gas, int[] cost) {
        int n = gas.length;

        // Encontra a posição do menor valor do padrão
        int acc = 0;
        int smallerValue = 0;
        int smallerPos = -1;
        for (int i = 0; i < n; i++) {
            acc = acc + gas[i] - cost[i];
            if (acc <= smallerValue) {
                smallerValue = acc;
                smallerPos = i;
            }
        }

        // Testa se partindo dessa posição é possível
        acc = 0;
        smallerPos = (smallerPos + 1) % n;
        for (int i = 0; i < n; i++) {
            int pos = (smallerPos + i) % n;
            acc = acc + gas[pos] - cost[pos];
            if (acc < 0) {
                return -1;
            }
        }

        return smallerPos;
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <iostream>
#include <vector>
using namespace std;

class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = (int)gas.size();

        // Encontra a posição do menor valor do padrão
        int acc = 0;
        int smaller_value = 0;
        int smaller_pos = -1;
        for (int i = 0; i < n; i++) {
            acc = acc + gas[i] - cost[i];
            if (acc <= smaller_value) {
                smaller_value = acc;
                smaller_pos = i;
            }
        }

        // Testa se partindo dessa posição é possível
        acc = 0;
        smaller_pos = (smaller_pos + 1) % n;
        for (int i = 0; i < n; i++) {
            int pos = (smaller_pos + i) % n;
            acc = acc + gas[pos] - cost[pos];
            if (acc < 0) {
                return -1;
            }
        }

        return smaller_pos;
    }
};

int main() {
    Solution solution;
    vector<int> gas1 = {1, 2, 3, 4, 5}, cost1 = {3, 4, 5, 1, 2};
    vector<int> gas2 = {2, 3, 4}, cost2 = {3, 4, 3};
    cout << solution.canCompleteCircuit(gas1, cost1) << endl; // 3
    cout << solution.canCompleteCircuit(gas2, cost2) << endl; // -1
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
