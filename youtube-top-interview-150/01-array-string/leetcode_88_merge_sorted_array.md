# LeetCode 88 — Merge Sorted Array

* **Problema:** [LeetCode 88](https://leetcode.com/problems/merge-sorted-array/)
* **Vídeo com a resolução:** [Episódio 1](https://youtu.be/W305-utklTs) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Easy
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Two Pointers de Trás para Frente (*In-Place*)** | $O(M + N)$ | $O(1)$ | Solução ótima aproveitando o espaço livre ao final de `nums1` |

---

## Solução 1: Two Pointers de Trás para Frente (*In-Place*) [[00:03:03]](https://youtu.be/W305-utklTs?t=183)

Como `nums1` possui espaço livre exatamente no final (nas últimas `n` posições), podemos preencher `nums1` **de trás para frente** (`i` indo de `m + n - 1` até `0`) sem nunca sobrescrever elementos ainda não processados de `nums1`.

Mantemos dois ponteiros para os maiores elementos restantes de cada array:
- `pos1 = m - 1` (fim dos elementos válidos de `nums1`);
- `pos2 = n - 1` (fim de `nums2`).

A cada posição `i`:
- Se `pos1 < 0` (já consumimos todo `nums1`) ou se ambos são válidos e `nums2[pos2] >= nums1[pos1]`, gravamos `nums2[pos2]` em `nums1[i]` e decrementamos `pos2`.
- Caso contrário, gravamos `nums1[pos1]` em `nums1[i]` e decrementamos `pos1`.

* **Tempo:** $O(M + N)$, pois cada posição de `nums1` (total `m + n`) é preenchida exatamente uma vez.
* **Espaço:** $O(1)$, fazendo a intercalação *in-place* em `nums1`.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def merge(self, nums1: list[int], m: int, nums2: list[int], n: int) -> None:
        pos1 = m - 1
        pos2 = n - 1

        for i in range(n + m - 1, -1, -1):
            if (pos1 >= 0 and pos2 >= 0 and nums2[pos2] >= nums1[pos1]) or pos1 < 0:
                nums1[i] = nums2[pos2]
                pos2 -= 1
            else:
                nums1[i] = nums1[pos1]
                pos1 -= 1


if __name__ == "__main__":
    solution = Solution()

    nums1 = [1, 2, 3, 0, 0, 0]
    solution.merge(nums1, 3, [2, 5, 6], 3)
    print(nums1)  # [1, 2, 2, 3, 5, 6]

    nums2 = [1]
    solution.merge(nums2, 1, [], 0)
    print(nums2)  # [1]

    nums3 = [0]
    solution.merge(nums3, 0, [1], 1)
    print(nums3)  # [1]
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();

        int[] nums1 = {1, 2, 3, 0, 0, 0};
        solution.merge(nums1, 3, new int[]{2, 5, 6}, 3);
        System.out.println(Arrays.toString(nums1)); // [1, 2, 2, 3, 5, 6]

        int[] nums2 = {1};
        solution.merge(nums2, 1, new int[]{}, 0);
        System.out.println(Arrays.toString(nums2)); // [1]

        int[] nums3 = {0};
        solution.merge(nums3, 0, new int[]{1}, 1);
        System.out.println(Arrays.toString(nums3)); // [1]
    }
}

class Solution {
    public void merge(int[] nums1, int m, int[] nums2, int n) {
        int pos1 = m - 1;
        int pos2 = n - 1;

        for (int i = n + m - 1; i >= 0; i--) {
            if ((pos1 >= 0 && pos2 >= 0 && nums2[pos2] >= nums1[pos1]) || pos1 < 0) {
                nums1[i] = nums2[pos2];
                pos2--;
            } else {
                nums1[i] = nums1[pos1];
                pos1--;
            }
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
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int pos1 = m - 1;
        int pos2 = n - 1;

        for (int i = n + m - 1; i >= 0; i--) {
            if ((pos1 >= 0 && pos2 >= 0 && nums2[pos2] >= nums1[pos1]) || pos1 < 0) {
                nums1[i] = nums2[pos2];
                pos2--;
            } else {
                nums1[i] = nums1[pos1];
                pos1--;
            }
        }
    }
};

int main() {
    Solution solution;

    vector<int> nums1 = {1, 2, 3, 0, 0, 0};
    vector<int> nums2 = {2, 5, 6};
    solution.merge(nums1, 3, nums2, 3);
    for (int x : nums1) cout << x << " "; // 1 2 2 3 5 6
    cout << endl;

    vector<int> a = {0};
    vector<int> b = {1};
    solution.merge(a, 0, b, 1);
    for (int x : a) cout << x << " "; // 1
    cout << endl;

    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
