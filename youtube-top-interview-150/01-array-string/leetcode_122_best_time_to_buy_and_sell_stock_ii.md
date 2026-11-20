# LeetCode 122 — Best Time to Buy and Sell Stock II

* **Problema:** [LeetCode 122](https://leetcode.com/problems/best-time-to-buy-and-sell-stock-ii/)
* **Vídeo com a resolução:** [Episódio 8](https://youtu.be/SZMvYTdpJEs) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Medium
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Abordagem Gulosa (Soma das Subidas)** | $O(N)$ | $O(1)$ | Solução ótima quando múltiplas transações são permitidas |

---

## Solução 1: Abordagem Gulosa (Soma das Subidas) [[00:03:53]](https://youtu.be/SZMvYTdpJEs?t=233)

Como podemos comprar e vender quantas vezes quisermos (inclusive vendendo e recomprando no mesmo dia), o lucro total entre um vale e um pico é exatamente igual à soma de todos os aumentos diários consecutivos nesse intervalo.

Assim, percorremos o array `prices` a partir do índice `1`: sempre que o preço do dia atual for maior do que o do dia anterior (`prices[v] > prices[v - 1]`), somamos a diferença `prices[v] - prices[v - 1]` ao `lucro`.

* **Tempo:** $O(N)$, onde $N$ é o tamanho do array `prices` (uma única varredura).
* **Espaço:** $O(1)$, usando apenas variáveis auxiliares constantes.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def maxProfit(self, prices: list[int]) -> int:
        n = len(prices)
        lucro = 0

        for v in range(1, n):
            if prices[v] > prices[v - 1]:
                lucro = lucro + (prices[v] - prices[v - 1])

        return lucro


if __name__ == "__main__":
    solution = Solution()
    print(solution.maxProfit([7, 1, 5, 3, 6, 4]))  # 7
    print(solution.maxProfit([1, 2, 3, 4, 5]))     # 4
    print(solution.maxProfit([7, 6, 4, 3, 1]))     # 0
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.maxProfit(new int[]{7, 1, 5, 3, 6, 4})); // 7
        System.out.println(solution.maxProfit(new int[]{1, 2, 3, 4, 5}));    // 4
        System.out.println(solution.maxProfit(new int[]{7, 6, 4, 3, 1}));    // 0
    }
}

class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        int lucro = 0;

        for (int v = 1; v < n; v++) {
            if (prices[v] > prices[v - 1]) {
                lucro = lucro + (prices[v] - prices[v - 1]);
            }
        }

        return lucro;
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
    int maxProfit(vector<int>& prices) {
        int n = (int)prices.size();
        int lucro = 0;

        for (int v = 1; v < n; v++) {
            if (prices[v] > prices[v - 1]) {
                lucro = lucro + (prices[v] - prices[v - 1]);
            }
        }

        return lucro;
    }
};

int main() {
    Solution solution;
    vector<int> prices1 = {7, 1, 5, 3, 6, 4};
    vector<int> prices2 = {1, 2, 3, 4, 5};
    vector<int> prices3 = {7, 6, 4, 3, 1};
    cout << solution.maxProfit(prices1) << endl; // 7
    cout << solution.maxProfit(prices2) << endl; // 4
    cout << solution.maxProfit(prices3) << endl; // 0
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
