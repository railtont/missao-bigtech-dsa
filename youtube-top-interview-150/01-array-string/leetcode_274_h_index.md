# LeetCode 274 — H-Index

* **Problema:** [LeetCode 274](https://leetcode.com/problems/h-index/)
* **Vídeo com a resolução:** [Episódio 11](https://youtu.be/Bg9AxQfLABQ) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Medium
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Busca Binária na Resposta** | $O(N \log N)$ | $O(1)$ | Ótimo ponto de partida sem modificar o array e sem memória extra |
| **2. Counting Sort** | $O(N)$ | $O(N)$ | Solução ótima em tempo explorando o limite $0 \le h \le N$ |

---

## Solução 1: Busca Binária na Resposta [[00:02:07]](https://youtu.be/Bg9AxQfLABQ?t=127)

O H-Index sempre está no intervalo `[0, n]`, pois um pesquisador com `n` artigos não pode ter H-Index maior que `n`. Além disso, a condição é **monótona**: se é possível atingir um H-Index `h`, qualquer valor menor que `h` também é possível.

Aplicamos busca binária no espaço de respostas `[0, n]`:
1. Testamos o candidato `mid` contando quantos artigos possuem pelo menos `mid` citações.
2. Se a contagem for `>= mid`, `mid` é válido: guardamos `ans = mid` e tentamos um valor maior (`left = mid + 1`).
3. Caso contrário, `mid` é grande demais e reduzimos o limite superior (`right = mid - 1`).

* **Tempo:** $O(N \log N)$, pois fazemos $O(\log N)$ iterações da busca binária e cada checagem percorre os $N$ artigos.
* **Espaço:** $O(1)$, usando apenas variáveis auxiliares constantes e sem modificar `citations`.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def hIndex(self, citations: list[int]) -> int:
        left, right = 0, len(citations)
        ans = 0

        while left <= right:
            mid = left + (right - left) // 2
            count = sum(1 for c in citations if c >= mid)

            if count >= mid:
                ans = mid
                left = mid + 1
            else:
                right = mid - 1

        return ans


if __name__ == "__main__":
    solution = Solution()
    print(solution.hIndex([3, 0, 6, 1, 5]))  # 3
    print(solution.hIndex([1, 3, 1]))        # 1
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.hIndex(new int[]{3, 0, 6, 1, 5})); // 3
        System.out.println(solution.hIndex(new int[]{1, 3, 1}));       // 1
    }
}

class Solution {
    public int hIndex(int[] citations) {
        int left = 0;
        int right = citations.length;
        int ans = 0;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            int count = 0;
            for (int c : citations) {
                if (c >= mid) {
                    count++;
                }
            }

            if (count >= mid) {
                ans = mid;
                left = mid + 1;
            } else {
                right = mid - 1;
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
#include <vector>
using namespace std;

class Solution {
public:
    int hIndex(vector<int>& citations) {
        int left = 0;
        int right = (int)citations.size();
        int ans = 0;

        while (left <= right) {
            int mid = left + (right - left) / 2;
            int count = 0;
            for (int c : citations) {
                if (c >= mid) {
                    count++;
                }
            }

            if (count >= mid) {
                ans = mid;
                left = mid + 1;
            } else {
                right = mid - 1;
            }
        }

        return ans;
    }
};

int main() {
    Solution solution;
    vector<int> c1 = {3, 0, 6, 1, 5};
    vector<int> c2 = {1, 3, 1};
    cout << solution.hIndex(c1) << endl; // 3
    cout << solution.hIndex(c2) << endl; // 1
    return 0;
}
```

</details>

---

## Solução 2: Counting Sort [[00:07:40]](https://youtu.be/Bg9AxQfLABQ?t=460)

Como qualquer artigo com mais de `n` citações contribui exatamente da mesma forma que um artigo com `n` citações para o H-Index, limitamos cada valor a `min(x, n)` e ordenamos `citations` em ordem decrescente usando **Counting Sort** em tempo linear.

Com o array ordenado do maior para o menor, percorremos os índices `h` de `0` até `n - 1`: sempre que `citations[h] >= h + 1`, existem pelo menos `h + 1` artigos com pelo menos `h + 1` citações, então atualizamos `h_index = h + 1`.

* **Tempo:** $O(N)$, onde $N$ é o número de artigos (Counting Sort + varredura linear).
* **Espaço:** $O(N)$, para o array de contagem `cnt_sorting` de tamanho `n + 1`.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def hIndex(self, citations: list[int]) -> int:
        n = len(citations)
        cnt_sorting = [0] * (n + 1)

        # Counting Sort
        for x in citations:
            cnt_sorting[min(x, n)] += 1

        pos = 0
        for index in range(n, -1, -1):
            for _ in range(cnt_sorting[index]):
                citations[pos] = index
                pos += 1

        # H-Index
        h_index = 0
        for h in range(n):
            if citations[h] >= h + 1:
                h_index = h + 1

        return h_index


if __name__ == "__main__":
    solution = Solution()
    print(solution.hIndex([3, 0, 6, 1, 5]))  # 3
    print(solution.hIndex([1, 3, 1]))        # 1
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.hIndex(new int[]{3, 0, 6, 1, 5})); // 3
        System.out.println(solution.hIndex(new int[]{1, 3, 1}));       // 1
    }
}

class Solution {
    public int hIndex(int[] citations) {
        int n = citations.length;
        int[] cntSorting = new int[n + 1];

        // Counting Sort
        for (int x : citations) {
            cntSorting[Math.min(x, n)]++;
        }

        int pos = 0;
        for (int index = n; index >= 0; index--) {
            for (int k = 0; k < cntSorting[index]; k++) {
                citations[pos] = index;
                pos++;
            }
        }

        // H-Index
        int hIndex = 0;
        for (int h = 0; h < n; h++) {
            if (citations[h] >= h + 1) {
                hIndex = h + 1;
            }
        }

        return hIndex;
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
    int hIndex(vector<int>& citations) {
        int n = (int)citations.size();
        vector<int> cnt_sorting(n + 1, 0);

        // Counting Sort
        for (int x : citations) {
            cnt_sorting[min(x, n)]++;
        }

        int pos = 0;
        for (int index = n; index >= 0; index--) {
            for (int k = 0; k < cnt_sorting[index]; k++) {
                citations[pos] = index;
                pos++;
            }
        }

        // H-Index
        int h_index = 0;
        for (int h = 0; h < n; h++) {
            if (citations[h] >= h + 1) {
                h_index = h + 1;
            }
        }

        return h_index;
    }
};

int main() {
    Solution solution;
    vector<int> c1 = {3, 0, 6, 1, 5};
    vector<int> c2 = {1, 3, 1};
    cout << solution.hIndex(c1) << endl; // 3
    cout << solution.hIndex(c2) << endl; // 1
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
