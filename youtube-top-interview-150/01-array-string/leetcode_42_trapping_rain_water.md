# LeetCode 42 — Trapping Rain Water

* **Problema:** [LeetCode 42](https://leetcode.com/problems/trapping-rain-water/)
* **Vídeo com a resolução:** [Episódio 16](https://youtu.be/ln_dFs3KoMA) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Hard
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

A quantidade de água retida em uma posição `i` depende da menor entre as maiores paredes à sua esquerda e à sua direita, subtraída da altura atual (`height[i]`). Podemos evoluir a solução em três níveis:

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Força Bruta** | $O(N^2)$ | $O(1)$ | Ponto de partida intuitivo para validar a fórmula do problema |
| **2. Prefix Max e Suffix Max** | $O(N)$ | $O(N)$ | Solução ótima em tempo, simples e direta de implementar |
| **3. Two Pointers (Sem Memória Extra)** | $O(N)$ | $O(1)$ | Solução ótima em tempo e espaço usando o pico máximo como divisor |

*(Onde $N$ é o número de barras em `height`.)*

---

## Solução 1: Força Bruta [[00:02:33]](https://youtu.be/ln_dFs3KoMA?t=153)

Para cada posição `i`, percorremos todo o array buscando a maior barra à esquerda (`j < i`) e a maior barra à direita (`j > i`). O limite de escoamento é `min(maior_esquerda, maior_direita)`; se esse limite for maior que `height[i]`, somamos a diferença à resposta.

* **Tempo:** $O(N^2)$, pois para cada uma das $N$ posições percorremos todas as $N$ barras.
* **Espaço:** $O(1)$, usando apenas variáveis auxiliares.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    # Complexidade
    # Tempo: O(N^2)
    # Espaço: O(1)
    def trap(self, height: list[int]) -> int:
        n = len(height)

        resposta = 0
        for i in range(n):
            maior_esquerda = 0
            maior_direita = 0
            for j in range(n):
                if i != j and height[j] > height[i]:
                    if j < i:
                        maior_esquerda = max(maior_esquerda, height[j])
                    else:
                        maior_direita = max(maior_direita, height[j])

            limite_escoamento = min(maior_esquerda, maior_direita)
            if limite_escoamento > height[i]:
                resposta += limite_escoamento - height[i]

        return resposta


if __name__ == "__main__":
    print(Solution().trap([0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]))  # 6
    print(Solution().trap([4, 2, 0, 3, 2, 5]))  # 9
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.trap(new int[]{0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1})); // 6
        System.out.println(solution.trap(new int[]{4, 2, 0, 3, 2, 5})); // 9
    }
}

class Solution {
    // Complexidade
    // Tempo: O(N^2)
    // Espaço: O(1)
    public int trap(int[] height) {
        int n = height.length;

        int resposta = 0;
        for (int i = 0; i < n; i++) {
            int maior_esquerda = 0;
            int maior_direita = 0;
            for (int j = 0; j < n; j++) {
                if (i != j && height[j] > height[i]) {
                    if (j < i) {
                        maior_esquerda = Math.max(maior_esquerda, height[j]);
                    } else {
                        maior_direita = Math.max(maior_direita, height[j]);
                    }
                }
            }

            int limite_escoamento = Math.min(maior_esquerda, maior_direita);
            if (limite_escoamento > height[i]) {
                resposta += limite_escoamento - height[i];
            }
        }

        return resposta;
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
    // Complexidade
    // Tempo: O(N^2)
    // Espaço: O(1)
    int trap(vector<int>& height) {
        int n = height.size();

        int resposta = 0;
        for (int i = 0; i < n; i++) {
            int maior_esquerda = 0;
            int maior_direita = 0;
            for (int j = 0; j < n; j++) {
                if (i != j && height[j] > height[i]) {
                    if (j < i) {
                        maior_esquerda = max(maior_esquerda, height[j]);
                    } else {
                        maior_direita = max(maior_direita, height[j]);
                    }
                }
            }

            int limite_escoamento = min(maior_esquerda, maior_direita);
            if (limite_escoamento > height[i]) {
                resposta += limite_escoamento - height[i];
            }
        }

        return resposta;
    }
};

int main() {
    Solution solution;
    vector<int> h1 = {0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1};
    vector<int> h2 = {4, 2, 0, 3, 2, 5};
    cout << solution.trap(h1) << endl; // 6
    cout << solution.trap(h2) << endl; // 9
    return 0;
}
```

</details>

---

## Solução 2: Prefix Max e Suffix Max [[00:08:00]](https://youtu.be/ln_dFs3KoMA?t=480)

Para eliminar o laço interno, pré-computamos dois vetores auxiliares:
1. `maior_esquerda[i]`: a maior altura encontrada no prefixo `[0..i]`.
2. `maior_direita[i]`: a maior altura encontrada no sufixo `[i..n-1]`.

Com isso, para cada posição interna `i`, consultamos os limites à esquerda e à direita em $O(1)$.

* **Tempo:** $O(N)$, com três passadas lineares pelo vetor.
* **Espaço:** $O(N)$, para armazenar os vetores `maior_esquerda` e `maior_direita`.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    # Complexidade
    # Tempo: O(N)
    # Espaço: O(N)
    def trap(self, height: list[int]) -> int:
        n = len(height)
        maior_esquerda = [0] * n
        maior_direita = [0] * n

        maior_esquerda[0] = height[0]
        maior_direita[n - 1] = height[n - 1]
        for i in range(1, n):
            maior_esquerda[i] = max(maior_esquerda[i - 1], height[i])
        for i in range(n - 2, -1, -1):
            maior_direita[i] = max(maior_direita[i + 1], height[i])

        resposta = 0
        for i in range(1, n - 1):
            resposta += max(0, min(maior_esquerda[i - 1], maior_direita[i + 1]) - height[i])

        return resposta


if __name__ == "__main__":
    print(Solution().trap([0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]))  # 6
    print(Solution().trap([4, 2, 0, 3, 2, 5]))  # 9
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.trap(new int[]{0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1})); // 6
        System.out.println(solution.trap(new int[]{4, 2, 0, 3, 2, 5})); // 9
    }
}

class Solution {
    // Complexidade
    // Tempo: O(N)
    // Espaço: O(N)
    public int trap(int[] height) {
        int n = height.length;
        int[] maior_esquerda = new int[n];
        int[] maior_direita = new int[n];

        maior_esquerda[0] = height[0];
        maior_direita[n - 1] = height[n - 1];
        for (int i = 1; i < n; i++) {
            maior_esquerda[i] = Math.max(maior_esquerda[i - 1], height[i]);
        }
        for (int i = n - 2; i >= 0; i--) {
            maior_direita[i] = Math.max(maior_direita[i + 1], height[i]);
        }

        int resposta = 0;
        for (int i = 1; i < n - 1; i++) {
            resposta += Math.max(0, Math.min(maior_esquerda[i - 1], maior_direita[i + 1]) - height[i]);
        }

        return resposta;
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
    // Complexidade
    // Tempo: O(N)
    // Espaço: O(N)
    int trap(vector<int>& height) {
        int n = height.size();
        vector<int> maior_esquerda(n, 0);
        vector<int> maior_direita(n, 0);

        maior_esquerda[0] = height[0];
        maior_direita[n - 1] = height[n - 1];
        for (int i = 1; i < n; i++) {
            maior_esquerda[i] = max(maior_esquerda[i - 1], height[i]);
        }
        for (int i = n - 2; i >= 0; i--) {
            maior_direita[i] = max(maior_direita[i + 1], height[i]);
        }

        int resposta = 0;
        for (int i = 1; i < n - 1; i++) {
            resposta += max(0, min(maior_esquerda[i - 1], maior_direita[i + 1]) - height[i]);
        }

        return resposta;
    }
};

int main() {
    Solution solution;
    vector<int> h1 = {0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1};
    vector<int> h2 = {4, 2, 0, 3, 2, 5};
    cout << solution.trap(h1) << endl; // 6
    cout << solution.trap(h2) << endl; // 9
    return 0;
}
```

</details>

---

## Solução 3: Two Pointers (Sem Memória Extra) [[00:11:39]](https://youtu.be/ln_dFs3KoMA?t=699)

Encontramos primeiro a posição do pico máximo global (`maior_pos`). Como nenhuma barra ultrapassa esse pico, podemos calcular a água em duas varreduras independentes até ele:
1. Da esquerda até `maior_pos`, acumulando a água sempre que encontramos uma barra maior ou igual à parede atual (`cur_maior_pos`), descontando o somatório das barras intermediárias.
2. Invertendo o array e repetindo exatamente a mesma função até a posição correspondente (`n - maior_pos - 1`).

* **Tempo:** $O(N)$, realizando varreduras lineares sobre o vetor.
* **Espaço:** $O(1)$, sem alocar vetores auxiliares.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    # Complexidade
    # Tempo: O(N)
    # Espaço: O(1)
    def twoPointers(self, height: list[int], tam: int) -> int:
        cur_maior_pos = 0
        somatorio = 0
        qtd_agua = 0
        for i in range(1, tam + 1):
            if height[i] >= height[cur_maior_pos]:
                altura = min(height[i], height[cur_maior_pos])
                largura = i - cur_maior_pos - 1
                qtd_agua += altura * largura - somatorio

                # Reseta o acumulador para o próximo bloco de água
                somatorio = 0
                cur_maior_pos = i
            else:
                somatorio += height[i]

        return qtd_agua

    def trap(self, height: list[int]) -> int:
        n = len(height)

        maior_valor = 0
        maior_pos = 0
        for index, valor in enumerate(height):
            if valor > maior_valor:
                maior_valor = valor
                maior_pos = index

        resposta = self.twoPointers(height, maior_pos)
        height.reverse()
        resposta += self.twoPointers(height, n - maior_pos - 1)

        return resposta


if __name__ == "__main__":
    print(Solution().trap([0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1]))  # 6
    print(Solution().trap([4, 2, 0, 3, 2, 5]))  # 9
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.trap(new int[]{0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1})); // 6
        System.out.println(solution.trap(new int[]{4, 2, 0, 3, 2, 5})); // 9
    }
}

class Solution {
    // Complexidade
    // Tempo: O(N)
    // Espaço: O(1)
    private int twoPointers(int[] height, int tam) {
        int cur_maior_pos = 0;
        int somatorio = 0;
        int qtd_agua = 0;

        for (int i = 1; i <= tam; i++) {
            if (height[i] >= height[cur_maior_pos]) {
                int altura = Math.min(height[i], height[cur_maior_pos]);
                int largura = i - cur_maior_pos - 1;
                qtd_agua += altura * largura - somatorio;

                // Reseta o acumulador para o próximo bloco de água
                somatorio = 0;
                cur_maior_pos = i;
            } else {
                somatorio += height[i];
            }
        }

        return qtd_agua;
    }

    private void reverse(int[] arr) {
        int left = 0, right = arr.length - 1;
        while (left < right) {
            int temp = arr[left];
            arr[left++] = arr[right];
            arr[right--] = temp;
        }
    }

    public int trap(int[] height) {
        int n = height.length;

        int maior_valor = 0;
        int maior_pos = 0;
        for (int index = 0; index < n; index++) {
            if (height[index] > maior_valor) {
                maior_valor = height[index];
                maior_pos = index;
            }
        }

        int resposta = twoPointers(height, maior_pos);
        reverse(height);
        resposta += twoPointers(height, n - maior_pos - 1);

        return resposta;
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
    int twoPointers(const vector<int>& height, int tam) {
        int cur_maior_pos = 0;
        int somatorio = 0;
        int qtd_agua = 0;

        for (int i = 1; i <= tam; i++) {
            if (height[i] >= height[cur_maior_pos]) {
                int altura = min(height[i], height[cur_maior_pos]);
                int largura = i - cur_maior_pos - 1;
                qtd_agua += altura * largura - somatorio;

                // Reseta o acumulador para o próximo bloco de água
                somatorio = 0;
                cur_maior_pos = i;
            } else {
                somatorio += height[i];
            }
        }

        return qtd_agua;
    }

public:
    // Complexidade
    // Tempo: O(N)
    // Espaço: O(1)
    int trap(vector<int>& height) {
        int n = height.size();

        int maior_valor = 0;
        int maior_pos = 0;
        for (int index = 0; index < n; index++) {
            if (height[index] > maior_valor) {
                maior_valor = height[index];
                maior_pos = index;
            }
        }

        int resposta = twoPointers(height, maior_pos);
        reverse(height.begin(), height.end());
        resposta += twoPointers(height, n - maior_pos - 1);

        return resposta;
    }
};

int main() {
    Solution solution;
    vector<int> h1 = {0, 1, 0, 2, 1, 0, 1, 3, 2, 1, 2, 1};
    vector<int> h2 = {4, 2, 0, 3, 2, 5};
    cout << solution.trap(h1) << endl; // 6
    cout << solution.trap(h2) << endl; // 9
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
