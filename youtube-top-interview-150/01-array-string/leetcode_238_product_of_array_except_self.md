# LeetCode 238 — Product of Array Except Self

* **Problema:** [LeetCode 238](https://leetcode.com/problems/product-of-array-except-self/)
* **Vídeo com a resolução:** [Episódio 13](https://youtu.be/VOZGDXsKAyM) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Medium
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Produto de Prefixos e Sufixos** | $O(N)$ | $O(1)$ | Solução ótima sem divisão, reaproveitando `nums` e `output` |

---

## Solução 1: Produto de Prefixos e Sufixos ($O(1)$ de Espaço Extra) [[00:01:14]](https://youtu.be/VOZGDXsKAyM?t=74)

O produto de todos os elementos exceto `nums[i]` é igual ao produto dos elementos **à esquerda** de `i` (prefixo) multiplicado pelo produto dos elementos **à direita** de `i` (sufixo), sem precisar usar divisão.

Para evitar alocar dois arrays auxiliares, reaproveitamos o próprio array `output` e o array de entrada `nums`:

1. **Produto de sufixos em `output`:** percorremos da direita para a esquerda armazenando em `output[i]` o produto de `nums[i..n-1]`.
2. **Produto de prefixos em `nums`:** percorremos da esquerda para a direita acumulando em `nums[i]` o produto de `nums[0..i]`.
3. **Montagem da resposta em `output`:** percorremos da esquerda para a direita calculando `output[i] = left * right`, onde `left = nums[i - 1]` (ou `1` se `i == 0`) e `right = output[i + 1]` (ou `1` se `i == n - 1`). Como só lemos `output[i + 1]`, sobrescrever `output[i]` durante a varredura é seguro.

* **Tempo:** $O(N)$, onde $N$ é o tamanho de `nums` (três passagens lineares).
* **Espaço:** $O(1)$ de espaço auxiliar (desconsiderando o array de saída `output`, conforme o enunciado).

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def productExceptSelf(self, nums: list[int]) -> list[int]:
        n = len(nums)
        output = [1] * n

        # Produto de sufixos
        for i in range(n - 1, -1, -1):
            output[i] = nums[i]
            if i + 1 < n:
                output[i] = output[i] * output[i + 1]

        # Produto de prefixos (in-place em nums)
        for i in range(1, n):
            nums[i] = nums[i] * nums[i - 1]

        # Resposta final
        for i in range(n):
            left = right = 1
            if i - 1 >= 0:
                left = nums[i - 1]
            if i + 1 < n:
                right = output[i + 1]

            output[i] = left * right

        return output


if __name__ == "__main__":
    solution = Solution()
    print(solution.productExceptSelf([1, 2, 3, 4]))       # [24, 12, 8, 6]
    print(solution.productExceptSelf([-1, 1, 0, -3, 3]))  # [0, 0, 9, 0, 0]
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(Arrays.toString(
            solution.productExceptSelf(new int[]{1, 2, 3, 4})
        )); // [24, 12, 8, 6]
        System.out.println(Arrays.toString(
            solution.productExceptSelf(new int[]{-1, 1, 0, -3, 3})
        )); // [0, 0, 9, 0, 0]
    }
}

class Solution {
    public int[] productExceptSelf(int[] nums) {
        int n = nums.length;
        int[] output = new int[n];

        // Produto de sufixos
        for (int i = n - 1; i >= 0; i--) {
            output[i] = nums[i];
            if (i + 1 < n) {
                output[i] = output[i] * output[i + 1];
            }
        }

        // Produto de prefixos (in-place em nums)
        for (int i = 1; i < n; i++) {
            nums[i] = nums[i] * nums[i - 1];
        }

        // Resposta final
        for (int i = 0; i < n; i++) {
            int left = 1;
            int right = 1;
            if (i - 1 >= 0) {
                left = nums[i - 1];
            }
            if (i + 1 < n) {
                right = output[i + 1];
            }
            output[i] = left * right;
        }

        return output;
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
    vector<int> productExceptSelf(vector<int>& nums) {
        int n = (int)nums.size();
        vector<int> output(n, 1);

        // Produto de sufixos
        for (int i = n - 1; i >= 0; i--) {
            output[i] = nums[i];
            if (i + 1 < n) {
                output[i] = output[i] * output[i + 1];
            }
        }

        // Produto de prefixos (in-place em nums)
        for (int i = 1; i < n; i++) {
            nums[i] = nums[i] * nums[i - 1];
        }

        // Resposta final
        for (int i = 0; i < n; i++) {
            int left = 1;
            int right = 1;
            if (i - 1 >= 0) {
                left = nums[i - 1];
            }
            if (i + 1 < n) {
                right = output[i + 1];
            }
            output[i] = left * right;
        }

        return output;
    }
};

int main() {
    Solution solution;

    vector<int> nums1 = {1, 2, 3, 4};
    for (int x : solution.productExceptSelf(nums1)) cout << x << " "; // 24 12 8 6
    cout << endl;

    vector<int> nums2 = {-1, 1, 0, -3, 3};
    for (int x : solution.productExceptSelf(nums2)) cout << x << " "; // 0 0 9 0 0
    cout << endl;

    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**