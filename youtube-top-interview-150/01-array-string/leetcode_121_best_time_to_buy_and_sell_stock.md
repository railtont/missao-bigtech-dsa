# LeetCode 121 — Best Time to Buy and Sell Stock

* **Problema:** [LeetCode 121](https://leetcode.com/problems/best-time-to-buy-and-sell-stock/)
* **Vídeo com a resolução:** [Episódio 7](https://youtu.be/jusKBMyJC0M) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Easy
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Abordagem Gulosa (Uma Passagem)** | $O(N)$ | $O(1)$ | Solução ótima para maximizar lucro em uma única transação |

---

## Solução 1: Abordagem Gulosa (Uma Passagem) [[00:01:56]](https://youtu.be/jusKBMyJC0M?t=116)

Para maximizar o lucro com **uma única compra e uma única venda** (comprando sempre antes de vender), consideramos cada dia `v` como um possível dia de venda e mantemos o menor preço de compra visto até então (`menor_valor_de_compra`).

Em uma única passagem pelo array `prices`:

1. Calculamos o lucro possível vendendo no dia `v` (`prices[v] - menor_valor_de_compra`) e atualizamos `lucro_maximo`.
2. Atualizamos `menor_valor_de_compra` caso `prices[v]` seja menor do que o mínimo anterior.

* **Tempo:** $O(N)$, onde $N$ é o tamanho do array `prices` (uma única varredura).
* **Espaço:** $O(1)$, usando apenas variáveis auxiliares constantes.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def maxProfit(self, prices: list[int]) -> int:
        if len(prices) == 0:
            return 0

        n = len(prices)
        lucro_maximo = 0
        menor_valor_de_compra = prices[0]

        for v in range(1, n):
            lucro_maximo = max(lucro_maximo, prices[v] - menor_valor_de_compra)
            menor_valor_de_compra = min(menor_valor_de_compra, prices[v])

        return lucro_maximo


if __name__ == "__main__":
    solution = Solution()
    print(solution.maxProfit([7, 1, 5, 3, 6, 4]))  # 5
    print(solution.maxProfit([7, 6, 4, 3, 1]))     # 0
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.maxProfit(new int[]{7, 1, 5, 3, 6, 4})); // 5
        System.out.println(solution.maxProfit(new int[]{7, 6, 4, 3, 1}));    // 0
    }
}

class Solution {
    public int maxProfit(int[] prices) {
        if (prices.length == 0) {
            return 0;
        }

        int n = prices.length;
        int lucroMaximo = 0;
        int menorValorDeCompra = prices[0];

        for (int v = 1; v < n; v++) {
            lucroMaximo = Math.max(lucroMaximo, prices[v] - menorValorDeCompra);
            menorValorDeCompra = Math.min(menorValorDeCompra, prices[v]);
        }

        return lucroMaximo;
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <algorithm>
#include <iostream>
#include <vector>
using namespace std;

class Solution {
public:
    int maxProfit(vector<int>& prices) {
        if (prices.empty()) {
            return 0;
        }

        int n = (int)prices.size();
        int lucro_maximo = 0;
        int menor_valor_de_compra = prices[0];

        for (int v = 1; v < n; v++) {
            lucro_maximo = max(lucro_maximo, prices[v] - menor_valor_de_compra);
            menor_valor_de_compra = min(menor_valor_de_compra, prices[v]);
        }

        return lucro_maximo;
    }
};

int main() {
    Solution solution;
    vector<int> prices1 = {7, 1, 5, 3, 6, 4};
    vector<int> prices2 = {7, 6, 4, 3, 1};
    cout << solution.maxProfit(prices1) << endl; // 5
    cout << solution.maxProfit(prices2) << endl; // 0
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
