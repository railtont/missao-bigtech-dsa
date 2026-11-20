# LeetCode 80 — Remove Duplicates from Sorted Array II

* **Problema:** [LeetCode 80](https://leetcode.com/problems/remove-duplicates-from-sorted-array-ii/)
* **Vídeo com a resolução:** [Episódio 5](https://youtu.be/OymtFs5qpEU) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Medium
* **Tópico:** Array / String

---

## Solução: Two Pointers (Comparação com `last - 2`) [[00:02:24]](https://youtu.be/OymtFs5qpEU?t=144)

Como `nums` já está ordenado e cada valor pode aparecer **no máximo 2 vezes**, mantemos dois ponteiros:

- `i` percorre todos os elementos de `nums`;
- `last` marca a próxima posição válida para escrita no prefixo resultante.

Para cada `nums[i]`:
- Se `last < 2`, ainda temos menos de dois elementos gravados, então sempre podemos incluí-lo.
- Caso contrário, comparamos `nums[i]` com `nums[last - 2]`. Como o prefixo `[0..last - 1]` está ordenado, `nums[i] != nums[last - 2]` garante que `nums[i]` apareceu no máximo uma vez até agora e pode ser adicionado sem violar o limite de 2 ocorrências.

Note que comparar com `nums[last - 2]` (e não com `nums[i - 2]`) é fundamental porque posições anteriores de `nums` podem já ter sido sobrescritas *in-place*.

* **Tempo:** $O(N)$, onde $N$ é o tamanho de `nums` (uma única passagem pelo array).
* **Espaço:** $O(1)$, modificando o array *in-place*.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def removeDuplicates(self, nums: list[int]) -> int:
        last = 0

        for i in range(len(nums)):
            if last < 2 or nums[i] != nums[last - 2]:
                nums[last] = nums[i]
                last += 1

        return last


if __name__ == "__main__":
    solution = Solution()

    nums1 = [1, 1, 1, 2, 2, 3]
    k1 = solution.removeDuplicates(nums1)
    print(k1, nums1[:k1])  # 5 [1, 1, 2, 2, 3]

    nums2 = [0, 0, 1, 1, 1, 1, 2, 3, 3]
    k2 = solution.removeDuplicates(nums2)
    print(k2, nums2[:k2])  # 7 [0, 0, 1, 1, 2, 3, 3]
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();

        int[] nums1 = {1, 1, 1, 2, 2, 3};
        int k1 = solution.removeDuplicates(nums1);
        System.out.println(k1 + " " + Arrays.toString(Arrays.copyOf(nums1, k1))); // 5 [1, 1, 2, 2, 3]

        int[] nums2 = {0, 0, 1, 1, 1, 1, 2, 3, 3};
        int k2 = solution.removeDuplicates(nums2);
        System.out.println(k2 + " " + Arrays.toString(Arrays.copyOf(nums2, k2))); // 7 [0, 0, 1, 1, 2, 3, 3]
    }
}

class Solution {
    public int removeDuplicates(int[] nums) {
        int last = 0;

        for (int i = 0; i < nums.length; i++) {
            if (last < 2 || nums[i] != nums[last - 2]) {
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
        int last = 0;

        for (int i = 0; i < (int)nums.size(); i++) {
            if (last < 2 || nums[i] != nums[last - 2]) {
                nums[last] = nums[i];
                last++;
            }
        }

        return last;
    }
};

int main() {
    Solution solution;

    vector<int> nums1 = {1, 1, 1, 2, 2, 3};
    int k1 = solution.removeDuplicates(nums1);
    cout << k1 << ": ";
    for (int i = 0; i < k1; i++) cout << nums1[i] << " "; // 5: 1 1 2 2 3
    cout << endl;

    vector<int> nums2 = {0, 0, 1, 1, 1, 1, 2, 3, 3};
    int k2 = solution.removeDuplicates(nums2);
    cout << k2 << ": ";
    for (int i = 0; i < k2; i++) cout << nums2[i] << " "; // 7: 0 0 1 1 2 3 3
    cout << endl;

    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
