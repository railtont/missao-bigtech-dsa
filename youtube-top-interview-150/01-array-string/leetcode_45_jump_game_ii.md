# LeetCode 45 — Jump Game II

* **Problema:** [LeetCode 45](https://leetcode.com/problems/jump-game-ii/)
* **Vídeo com a resolução:** [Episódio 10](https://youtu.be/pT3fLg7fV3o) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Medium
* **Tópico:** Array / String

---

## Solução: Abordagem Gulosa por Níveis (Intervalos de Alcance)

Podemos enxergar o problema como uma busca em largura (BFS) implícita sobre intervalos do array: cada nível `[cur_start, cur_end]` representa todas as posições alcançáveis com exatamente `jumps` saltos.

1. Começamos com o intervalo inicial `[0, 0]` (0 saltos, já que começamos no índice `0`).
2. A cada iteração, percorremos todas as posições `i` dentro do intervalo atual `[cur_start, cur_end]` e calculamos a posição mais distante `last = max(last, i + nums[i])` alcançável com o próximo salto.
3. Avançamos para o próximo nível definindo `cur_start = cur_end + 1` e `cur_end = min(last, n - 1)`.

Como cada índice `i` pertence a exatamente um intervalo `[cur_start, cur_end]`, o algoritmo percorre cada posição do array apenas uma vez.

* **Tempo:** $O(N)$, onde $N$ é o tamanho de `nums` (cada índice é visitado exatamente uma vez).
* **Espaço:** $O(1)$, usando apenas variáveis auxiliares constantes.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def jump(self, nums: list[int]) -> int:
        n = len(nums)
        jumps = -1

        cur_start = 0
        cur_end = 0

        while cur_start < n:
            jumps = jumps + 1
            last = 0

            for i in range(cur_start, cur_end + 1):
                last = max(last, i + nums[i])

            cur_start = cur_end + 1
            cur_end = min(last, n - 1)

        return jumps


if __name__ == "__main__":
    solution = Solution()
    print(solution.jump([2, 3, 1, 1, 4]))  # 2
    print(solution.jump([2, 3, 0, 1, 4]))  # 2
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.jump(new int[]{2, 3, 1, 1, 4})); // 2
        System.out.println(solution.jump(new int[]{2, 3, 0, 1, 4})); // 2
    }
}

class Solution {
    public int jump(int[] nums) {
        int n = nums.length;
        int jumps = -1;

        int curStart = 0;
        int curEnd = 0;

        while (curStart < n) {
            jumps = jumps + 1;
            int last = 0;

            for (int i = curStart; i <= curEnd; i++) {
                last = Math.max(last, i + nums[i]);
            }

            curStart = curEnd + 1;
            curEnd = Math.min(last, n - 1);
        }

        return jumps;
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
    int jump(vector<int>& nums) {
        int n = (int)nums.size();
        int jumps = -1;

        int cur_start = 0;
        int cur_end = 0;

        while (cur_start < n) {
            jumps = jumps + 1;
            int last = 0;

            for (int i = cur_start; i <= cur_end; i++) {
                last = max(last, i + nums[i]);
            }

            cur_start = cur_end + 1;
            cur_end = min(last, n - 1);
        }

        return jumps;
    }
};

int main() {
    Solution solution;
    vector<int> nums1 = {2, 3, 1, 1, 4};
    vector<int> nums2 = {2, 3, 0, 1, 4};
    cout << solution.jump(nums1) << endl; // 2
    cout << solution.jump(nums2) << endl; // 2
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
