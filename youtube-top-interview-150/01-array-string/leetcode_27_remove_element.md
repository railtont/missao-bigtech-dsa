# LeetCode 27 — Remove Element

* **Problema:** [LeetCode 27](https://leetcode.com/problems/remove-element/)
* **Vídeo com a resolução:** [Episódio 2](https://youtu.be/7bG1gBWzbmA) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Easy
* **Tópico:** Array / String

---

## Solução: Two Pointers (Leitura e Escrita *In-Place*) [[00:06:17]](https://youtu.be/7bG1gBWzbmA?t=377)

Usamos dois ponteiros para compactar os elementos válidos no início de `nums` sem precisar de memória extra:

- `i` percorre todos os elementos do array;
- `last` indica a próxima posição livre onde o próximo elemento diferente de `val` deve ser escrito.

Sempre que `nums[i] != val`, copiamos `nums[i]` para `nums[last]` e incrementamos `last`. Ao final do laço, `last` é a quantidade `k` de elementos diferentes de `val`, e os `k` primeiros elementos de `nums` contêm exatamente esses valores.

* **Tempo:** $O(N)$, onde $N$ é o tamanho de `nums` (uma única passagem pelo array).
* **Espaço:** $O(1)$, modificando o array *in-place*.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def removeElement(self, nums: list[int], val: int) -> int:
        last = 0

        for i in range(len(nums)):
            if nums[i] != val:
                nums[last] = nums[i]
                last += 1

        return last


if __name__ == "__main__":
    solution = Solution()

    nums1 = [3, 2, 2, 3]
    k1 = solution.removeElement(nums1, 3)
    print(k1, nums1[:k1])  # 2 [2, 2]

    nums2 = [0, 1, 2, 2, 3, 0, 4, 2]
    k2 = solution.removeElement(nums2, 2)
    print(k2, nums2[:k2])  # 5 [0, 1, 3, 0, 4]
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();

        int[] nums1 = {3, 2, 2, 3};
        int k1 = solution.removeElement(nums1, 3);
        System.out.println(k1 + " " + Arrays.toString(Arrays.copyOf(nums1, k1))); // 2 [2, 2]

        int[] nums2 = {0, 1, 2, 2, 3, 0, 4, 2};
        int k2 = solution.removeElement(nums2, 2);
        System.out.println(k2 + " " + Arrays.toString(Arrays.copyOf(nums2, k2))); // 5 [0, 1, 3, 0, 4]
    }
}

class Solution {
    public int removeElement(int[] nums, int val) {
        int last = 0;

        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != val) {
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
    int removeElement(vector<int>& nums, int val) {
        int last = 0;

        for (int i = 0; i < (int)nums.size(); i++) {
            if (nums[i] != val) {
                nums[last] = nums[i];
                last++;
            }
        }

        return last;
    }
};

int main() {
    Solution solution;

    vector<int> nums1 = {3, 2, 2, 3};
    int k1 = solution.removeElement(nums1, 3);
    cout << k1 << ": ";
    for (int i = 0; i < k1; i++) cout << nums1[i] << " "; // 2: 2 2
    cout << endl;

    vector<int> nums2 = {0, 1, 2, 2, 3, 0, 4, 2};
    int k2 = solution.removeElement(nums2, 2);
    cout << k2 << ": ";
    for (int i = 0; i < k2; i++) cout << nums2[i] << " "; // 5: 0 1 3 0 4
    cout << endl;

    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
