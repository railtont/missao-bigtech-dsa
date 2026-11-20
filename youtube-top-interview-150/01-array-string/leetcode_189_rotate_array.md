# LeetCode 189 — Rotate Array

* **Problema:** [LeetCode 189](https://leetcode.com/problems/rotate-array/)
* **Vídeo com a resolução:** [Episódio 6](https://youtu.be/_DqwFFFyA3o) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Medium
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Solução Trick (Reversões)** | $O(N)$ | $O(1)$ | Mais curta e fácil de implementar sem erros em entrevistas |
| **2. Juggling Algorithm (Rotação por Ciclos)** | $O(N)$ | $O(1)$ | Move cada elemento exatamente uma vez direto para sua posição final |

---

## Solução 1: Solução Trick (Reversões) [[00:01:51]](https://youtu.be/_DqwFFFyA3o?t=111)

Primeiro reduzimos `k` com `k = k % n` (já que rotacionar `n` posições volta ao array original). Em seguida, obtemos a rotação à direita *in-place* aplicando **três inversões**:

1. Invertemos o array inteiro (`0` até `n - 1`).
2. Invertemos os primeiros `k` elementos (`0` até `k - 1`).
3. Invertemos os `n - k` elementos restantes (`k` até `n - 1`).

* **Tempo:** $O(N)$, onde $N$ é o tamanho de `nums` (cada elemento é trocado no máximo duas vezes).
* **Espaço:** $O(1)$, pois todas as trocas são feitas *in-place*.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def rotate(self, nums: list[int], k: int) -> None:
        n = len(nums)
        k = k % n

        for i in range(n // 2):
            nums[i], nums[n - 1 - i] = nums[n - 1 - i], nums[i]

        for i in range(k // 2):
            nums[i], nums[k - 1 - i] = nums[k - 1 - i], nums[i]

        for i in range((n - k) // 2):
            nums[k + i], nums[n - 1 - i] = nums[n - 1 - i], nums[k + i]


if __name__ == "__main__":
    solution = Solution()

    nums1 = [1, 2, 3, 4, 5, 6, 7]
    solution.rotate(nums1, 3)
    print(nums1)  # [5, 6, 7, 1, 2, 3, 4]

    nums2 = [-1, -100, 3, 99]
    solution.rotate(nums2, 2)
    print(nums2)  # [3, 99, -1, -100]
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();

        int[] nums1 = {1, 2, 3, 4, 5, 6, 7};
        solution.rotate(nums1, 3);
        System.out.println(Arrays.toString(nums1)); // [5, 6, 7, 1, 2, 3, 4]

        int[] nums2 = {-1, -100, 3, 99};
        solution.rotate(nums2, 2);
        System.out.println(Arrays.toString(nums2)); // [3, 99, -1, -100]
    }
}

class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;
        k = k % n;

        for (int i = 0; i < n / 2; i++) {
            int temp = nums[i];
            nums[i] = nums[n - 1 - i];
            nums[n - 1 - i] = temp;
        }

        for (int i = 0; i < k / 2; i++) {
            int temp = nums[i];
            nums[i] = nums[k - 1 - i];
            nums[k - 1 - i] = temp;
        }

        for (int i = 0; i < (n - k) / 2; i++) {
            int temp = nums[k + i];
            nums[k + i] = nums[n - 1 - i];
            nums[n - 1 - i] = temp;
        }
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <iostream>
#include <utility>
#include <vector>
using namespace std;

class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = (int)nums.size();
        k = k % n;

        for (int i = 0; i < n / 2; i++) {
            swap(nums[i], nums[n - 1 - i]);
        }

        for (int i = 0; i < k / 2; i++) {
            swap(nums[i], nums[k - 1 - i]);
        }

        for (int i = 0; i < (n - k) / 2; i++) {
            swap(nums[k + i], nums[n - 1 - i]);
        }
    }
};

int main() {
    Solution solution;

    vector<int> nums1 = {1, 2, 3, 4, 5, 6, 7};
    solution.rotate(nums1, 3);
    for (int x : nums1) cout << x << " "; // 5 6 7 1 2 3 4
    cout << endl;

    vector<int> nums2 = {-1, -100, 3, 99};
    solution.rotate(nums2, 2);
    for (int x : nums2) cout << x << " "; // 3 99 -1 -100
    cout << endl;

    return 0;
}
```

</details>

---

## Solução 2: Juggling Algorithm (Rotação por Ciclos) [[00:04:04]](https://youtu.be/_DqwFFFyA3o?t=244)

Cada elemento na posição `atual` precisa ir diretamente para `(atual + k) % n`. Seguindo esses saltos a partir do índice `0`, formamos um ciclo fechado de tamanho `tamanho_grupo`. Como todos os ciclos têm o mesmo tamanho, existem exatamente `quantidade_de_grupos = n / tamanho_grupo` ciclos independentes (equivalente a $\gcd(n, k)$), iniciando nos índices `0, 1, ..., quantidade_de_grupos - 1`.

Para cada grupo, salvamos o valor inicial e avançamos de `k` em `k` (módulo `n`) sobrescrevendo cada posição de destino até retornar ao índice de partida `i`.

* **Tempo:** $O(N)$, pois cada elemento é movido exatamente uma vez para sua posição final (além de até $N$ passos para medir o tamanho do primeiro ciclo).
* **Espaço:** $O(1)$, usando apenas variáveis auxiliares constantes.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def rotate(self, nums: list[int], k: int) -> None:
        n = len(nums)

        # Conta quantos elementos pertencem ao primeiro grupo (ciclo)
        tamanho_grupo = 0
        pos = 0
        while True:
            tamanho_grupo += 1
            pos = (pos + k) % n
            if pos == 0:  # Até voltar para a posição inicial 0 e fechar o ciclo
                break

        # Calcula quantos grupos (ciclos independentes) existem no total
        quantidade_de_grupos = n // tamanho_grupo

        # Rotaciona cada um dos grupos
        for i in range(quantidade_de_grupos):
            # Reinicializa as variáveis para começar o novo ciclo no índice 'i'
            atual = i
            valor_atual = nums[atual]

            # Enquanto não fechar o ciclo (voltar para a posição inicial i), continua rotacionando
            while True:
                proximo = (atual + k) % n

                # Maneira segura para evitar perder o valor da próxima posição
                valor_proximo = nums[proximo]
                nums[proximo] = valor_atual

                # Agora o próximo índice e valor tornam-se os atuais
                atual = proximo
                valor_atual = valor_proximo

                if atual == i:  # O ciclo termina quando retorna ao índice inicial 'i'
                    break


if __name__ == "__main__":
    solution = Solution()

    nums1 = [1, 2, 3, 4, 5, 6, 7]
    solution.rotate(nums1, 3)
    print(nums1)  # [5, 6, 7, 1, 2, 3, 4]

    nums2 = [-1, -100, 3, 99]
    solution.rotate(nums2, 2)
    print(nums2)  # [3, 99, -1, -100]
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();

        int[] nums1 = {1, 2, 3, 4, 5, 6, 7};
        solution.rotate(nums1, 3);
        System.out.println(Arrays.toString(nums1)); // [5, 6, 7, 1, 2, 3, 4]

        int[] nums2 = {-1, -100, 3, 99};
        solution.rotate(nums2, 2);
        System.out.println(Arrays.toString(nums2)); // [3, 99, -1, -100]
    }
}

class Solution {
    public void rotate(int[] nums, int k) {
        int n = nums.length;

        // Conta quantos elementos pertencem ao primeiro grupo (ciclo)
        int tamanhoGrupo = 0;
        int pos = 0;
        do {
            tamanhoGrupo++;
            pos = (pos + k) % n;
        } while (pos != 0); // Até voltar para a posição inicial 0 e fechar o ciclo

        // Calcula quantos grupos (ciclos independentes) existem no total
        int quantidadeDeGrupos = n / tamanhoGrupo;

        // Rotaciona cada um dos grupos
        for (int i = 0; i < quantidadeDeGrupos; i++) {
            // Reinicializa as variáveis para começar o novo ciclo no índice 'i'
            int atual = i;
            int valorAtual = nums[atual];

            // Enquanto não fechar o ciclo (voltar para a posição inicial i), continua rotacionando
            do {
                int proximo = (atual + k) % n;

                // Maneira segura para evitar perder o valor da próxima posição
                int valorProximo = nums[proximo];
                nums[proximo] = valorAtual;

                // Agora o próximo índice e valor tornam-se os atuais
                atual = proximo;
                valorAtual = valorProximo;
            } while (atual != i); // O ciclo termina quando retorna ao índice inicial 'i'
        }
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
    void rotate(vector<int>& nums, int k) {
        int n = (int)nums.size();

        // Conta quantos elementos pertencem ao primeiro grupo (ciclo)
        int tamanho_grupo = 0, pos = 0;
        do {
            tamanho_grupo++;
            pos = (pos + k) % n;
        } while (pos != 0); // Até voltar para a posição inicial 0 e fechar o ciclo

        // Calcula quantos grupos (ciclos independentes) existem no total
        int quantidade_de_grupos = n / tamanho_grupo;

        int atual, proximo;
        int valor_atual, valor_proximo;

        // Rotaciona cada um dos grupos
        for (int i = 0; i < quantidade_de_grupos; i++) {
            // Reinicializa as variáveis para começar o novo ciclo no índice 'i'
            atual = i;
            valor_atual = nums[atual];

            // Enquanto não fechar o ciclo (voltar para a posição inicial i), continua rotacionando
            do {
                proximo = (atual + k) % n;

                // Maneira segura para evitar perder o valor da próxima posição
                valor_proximo = nums[proximo];
                nums[proximo] = valor_atual;

                // Agora o próximo índice e valor tornam-se os atuais
                atual = proximo;
                valor_atual = valor_proximo;
            } while (atual != i); // O ciclo termina quando retorna ao índice inicial 'i'
        }
    }
};

int main() {
    Solution solution;

    vector<int> nums1 = {1, 2, 3, 4, 5, 6, 7};
    solution.rotate(nums1, 3);
    for (int x : nums1) cout << x << " "; // 5 6 7 1 2 3 4
    cout << endl;

    vector<int> nums2 = {-1, -100, 3, 99};
    solution.rotate(nums2, 2);
    for (int x : nums2) cout << x << " "; // 3 99 -1 -100
    cout << endl;

    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**