# LeetCode 55 — Jump Game

* **Problema:** [LeetCode 55](https://leetcode.com/problems/jump-game/)
* **Vídeo com a resolução:** [Episódio 9](https://youtu.be/kusX9efDnig) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Medium
* **Tópico:** Array / String

---

## Solução: Abordagem Gulosa (Alcance Máximo) [[00:02:47]](https://youtu.be/kusX9efDnig?t=167)

Mantemos uma única variável `last` que representa o **maior índice alcançável** até o momento.

Percorremos cada posição `i` do array:
1. Se `i > last`, significa que não é possível alcançar a posição `i` a partir das posições anteriores — retornamos `False` imediatamente.
2. Caso contrário, atualizamos o alcance máximo com `last = max(last, i + nums[i])`.

Se conseguirmos atravessar todo o array sem que `i` ultrapasse `last`, é possível chegar ao último índice e retornamos `True`.

* **Tempo:** $O(N)$, onde $N$ é o tamanho de `nums` (uma única passagem pelo array).
* **Espaço:** $O(1)$, usando apenas a variável auxiliar `last`.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def canJump(self, nums: list[int]) -> bool:
        last = 0

        for i in range(len(nums)):
            if i > last:
                return False
            last = max(last, i + nums[i])

        return True


if __name__ == "__main__":
    solution = Solution()
    print(solution.canJump([2, 3, 1, 1, 4]))  # True
    print(solution.canJump([3, 2, 1, 0, 4]))  # False
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.canJump(new int[]{2, 3, 1, 1, 4})); // true
        System.out.println(solution.canJump(new int[]{3, 2, 1, 0, 4})); // false
    }
}

class Solution {
    public boolean canJump(int[] nums) {
        int last = 0;

        for (int i = 0; i < nums.length; i++) {
            if (i > last) {
                return false;
            }
            last = Math.max(last, i + nums[i]);
        }

        return true;
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
    bool canJump(vector<int>& nums) {
        int last = 0;

        for (int i = 0; i < (int)nums.size(); i++) {
            if (i > last) {
                return false;
            }
            last = max(last, i + nums[i]);
        }

        return true;
    }
};

int main() {
    Solution solution;
    vector<int> nums1 = {2, 3, 1, 1, 4};
    vector<int> nums2 = {3, 2, 1, 0, 4};
    cout << boolalpha;
    cout << solution.canJump(nums1) << endl; // true
    cout << solution.canJump(nums2) << endl; // false
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
