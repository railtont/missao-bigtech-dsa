# LeetCode 26 — Remove Duplicates from Sorted Array

* **Problema:** [LeetCode 26](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)
* **Vídeo com a resolução:** [Episódio 3](https://youtu.be/F5-vVueNysg) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Easy
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Two Pointers (Leitura e Escrita *In-Place*)** | $O(N)$ | $O(1)$ | Solução ótima para remover duplicatas em array ordenado sem memória extra |

---

## Solução 1: Two Pointers (Leitura e Escrita *In-Place*) [[00:02:17]](https://youtu.be/F5-vVueNysg?t=137)

Como o array `nums` já está ordenado em ordem não decrescente, todos os elementos duplicados aparecem em posições adjacentes.

Usamos dois ponteiros:
- `i` percorre o array a partir do índice `1` buscando novos valores únicos;
- `last` marca a próxima posição onde o próximo elemento único deve ser gravado (começa em `1`, pois `nums[0]` é sempre mantido).

Sempre que `nums[i] != nums[i - 1]`, encontramos um novo valor distinto: copiamos `nums[i]` para `nums[last]` e incrementamos `last`. Ao final, `last` representa a quantidade `k` de elementos únicos no início do array.

* **Tempo:** $O(N)$, onde $N$ é o tamanho de `nums` (uma única passagem pelo array).
* **Espaço:** $O(1)$, modificando o array *in-place*.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def removeDuplicates(self, nums: list[int]) -> int:
        last = 1

        for i in range(1, len(nums)):
            if nums[i] != nums[i - 1]:
                nums[last] = nums[i]
                last += 1

        return last


if __name__ == "__main__":
    solution = Solution()

    nums1 = [1, 1, 2]
    k1 = solution.removeDuplicates(nums1)
    print(k1, nums1[:k1])  # 2 [1, 2]

    nums2 = [0, 0, 1, 1, 1, 2, 2, 3, 3, 4]
    k2 = solution.removeDuplicates(nums2)
    print(k2, nums2[:k2])  # 5 [0, 1, 2, 3, 4]
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();

        int[] nums1 = {1, 1, 2};
        int k1 = solution.removeDuplicates(nums1);
        System.out.println(k1 + " " + Arrays.toString(Arrays.copyOf(nums1, k1))); // 2 [1, 2]

        int[] nums2 = {0, 0, 1, 1, 1, 2, 2, 3, 3, 4};
        int k2 = solution.removeDuplicates(nums2);
        System.out.println(k2 + " " + Arrays.toString(Arrays.copyOf(nums2, k2))); // 5 [0, 1, 2, 3, 4]
    }
}

class Solution {
    public int removeDuplicates(int[] nums) {
        int last = 1;

        for (int i = 1; i < nums.length; i++) {
            if (nums[i] != nums[i - 1]) {
                nums[last] = nums[i];
                last++;
            }
        }

        return last;
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
    int removeDuplicates(vector<int>& nums) {
        int last = 1;

        for (int i = 1; i < (int)nums.size(); i++) {
            if (nums[i] != nums[i - 1]) {
                nums[last] = nums[i];
                last++;
            }
        }

        return last;
    }
};

int main() {
    Solution solution;

    vector<int> nums1 = {1, 1, 2};
    int k1 = solution.removeDuplicates(nums1);
    cout << k1 << ": ";
    for (int i = 0; i < k1; i++) cout << nums1[i] << " "; // 2: 1 2
    cout << endl;

    vector<int> nums2 = {0, 0, 1, 1, 1, 2, 2, 3, 3, 4};
    int k2 = solution.removeDuplicates(nums2);
    cout << k2 << ": ";
    for (int i = 0; i < k2; i++) cout << nums2[i] << " "; // 5: 0 1 2 3 4
    cout << endl;

    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
