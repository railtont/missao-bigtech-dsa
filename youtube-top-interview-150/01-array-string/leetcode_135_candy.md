# LeetCode 135 — Candy

* **Problema:** [LeetCode 135](https://leetcode.com/problems/candy/)
* **Vídeo com a resolução:** [Episódio 15](https://youtu.be/SOLagtZT_oU) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Hard
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Segmentos e Progressão Aritmética ("Fuja dessa")** | $O(N)$ | $O(N)$ | Evitar em entrevistas: excesso de casos de borda e código propenso a erros |
| **2. Ordenação Topológica (Partindo dos Vales)** | $O(N)$ | $O(N)$ | Muito intuitiva; propaga doces a partir dos mínimos locais |
| **3. Programação Dinâmica (Top-Down / Memoização)** | $O(N)$ | $O(N)$ | Mais enxuta e direta; excelente escolha para entrevistas |

---

## Solução 1: Segmentos e Progressão Aritmética ("Fuja dessa") [[00:06:27]](https://youtu.be/SOLagtZT_oU?t=387)

A primeira ideia costuma ser quebrar o array `ratings` em segmentos monótonos (crescentes, decrescentes e constantes) e somar os doces de cada segmento usando a fórmula da progressão aritmética $\frac{\text{tam} \cdot (\text{tam} + 1)}{2}$.

Apesar de atingir complexidade linear, essa abordagem exige tratar diversos casos de borda — como topos compartilhados entre uma subida e uma descida, platôs (vizinhos iguais) e qual dos lados domina a altura do pico — tornando o código longo e arriscado sob pressão de entrevista.

* **Tempo:** $O(N)$, onde $N$ é o tamanho de `ratings` (uma passagem para calcular os segmentos e outra para somar os doces).
* **Espaço:** $O(N)$, para armazenar a lista de segmentos `seg_sizes`.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def getSinal(self, ratings: list[int], index: int) -> int:
        if index == 0 or ratings[index] == ratings[index - 1]:
            return 0
        if ratings[index] < ratings[index - 1]:
            return -1
        return 1

    def calculaSegmentos(self, ratings: list[int]) -> list[tuple[int, int, bool]]:
        n = len(ratings)

        seg_sizes = []
        sinal_cur = sinal_prev = 0
        cur_start = 0
        for i in range(1, n):
            sinal_cur = self.getSinal(ratings, i)
            sinal_prev = self.getSinal(ratings, i - 1)

            if sinal_cur == 0:
                seg_size = i - cur_start
                isolado = True  # Corner case: subida e descida com topos iguais
                seg_sizes.append((seg_size, sinal_prev, isolado))
                cur_start = i
                continue

            if i > 1:
                if sinal_cur * sinal_prev == -1:  # Troca de inclinação
                    seg_size = i - cur_start
                    seg_sizes.append((seg_size, sinal_prev, False))
                    cur_start = i

        if cur_start < n:
            sinal = self.getSinal(ratings, n - 1)
            seg_size = n - cur_start
            seg_sizes.append((seg_size, sinal, False))

        return seg_sizes

    def progressaoAritimetica(self, tam: int) -> int:
        # Fórmula da Progressão Aritmética
        return (tam + 1) * tam // 2

    def candy(self, ratings: list[int]) -> int:
        segments = self.calculaSegmentos(ratings)
        candies = 0

        tam_prev = sinal_prev = 0
        prev_isolado = True
        for index, seg in enumerate(segments):
            tam, sinal, isolado = seg
            if index > 0:
                tam_prev, sinal_prev, prev_isolado = segments[index - 1]

            if sinal == 0:
                # Vizinhos iguais: não existe decrescente ou crescente
                candies += 1

            elif sinal == 1:
                if prev_isolado:
                    # A primeira criança começa em 1 doce, não em 2
                    candies += self.progressaoAritimetica(tam)
                else:
                    # Crescente que possui um decrescente antes:
                    # a criança inicial já foi contabilizada no segmento anterior
                    candies += self.progressaoAritimetica(tam) + tam
                    segments[index] = (tam + 1, sinal, isolado)  # Corrige o tamanho para as próximas iterações

            elif sinal == -1:
                # Decrescente: a última criança sempre recebe 1 doce
                if sinal_prev == 1 and tam >= tam_prev and not prev_isolado:
                    # Remove o pico do segmento anterior e recalcula incluindo o pico no atual
                    candies -= tam_prev
                    candies += self.progressaoAritimetica(tam + 1)
                else:
                    candies += self.progressaoAritimetica(tam)

        return candies


if __name__ == "__main__":
    solution = Solution()
    print(solution.candy([1, 0, 2]))        # 5
    print(solution.candy([1, 2, 2]))        # 4
    print(solution.candy([1, 3, 2, 2, 1]))  # 7
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.ArrayList;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.candy(new int[]{1, 0, 2}));       // 5
        System.out.println(solution.candy(new int[]{1, 2, 2}));       // 4
        System.out.println(solution.candy(new int[]{1, 3, 2, 2, 1})); // 7
    }
}

class Solution {
    private static class Segment {
        int tam;
        int sinal;
        boolean isolado;

        Segment(int tam, int sinal, boolean isolado) {
            this.tam = tam;
            this.sinal = sinal;
            this.isolado = isolado;
        }
    }

    private int getSinal(int[] ratings, int index) {
        if (index == 0 || ratings[index] == ratings[index - 1]) {
            return 0;
        }
        if (ratings[index] < ratings[index - 1]) {
            return -1;
        }
        return 1;
    }

    private List<Segment> calculaSegmentos(int[] ratings) {
        int n = ratings.length;
        List<Segment> segSizes = new ArrayList<>();
        int curStart = 0;

        for (int i = 1; i < n; i++) {
            int sinalCur = getSinal(ratings, i);
            int sinalPrev = getSinal(ratings, i - 1);

            if (sinalCur == 0) {
                int segSize = i - curStart;
                // Corner case: subida e descida com topos iguais
                segSizes.add(new Segment(segSize, sinalPrev, true));
                curStart = i;
                continue;
            }

            if (i > 1 && sinalCur * sinalPrev == -1) { // Troca de inclinação
                int segSize = i - curStart;
                segSizes.add(new Segment(segSize, sinalPrev, false));
                curStart = i;
            }
        }

        if (curStart < n) {
            int sinal = getSinal(ratings, n - 1);
            int segSize = n - curStart;
            segSizes.add(new Segment(segSize, sinal, false));
        }

        return segSizes;
    }

    private int progressaoAritimetica(int tam) {
        return (tam + 1) * tam / 2;
    }

    public int candy(int[] ratings) {
        List<Segment> segments = calculaSegmentos(ratings);
        int candies = 0;

        int tamPrev = 0;
        int sinalPrev = 0;
        boolean prevIsolado = true;

        for (int index = 0; index < segments.size(); index++) {
            Segment seg = segments.get(index);
            if (index > 0) {
                Segment prev = segments.get(index - 1);
                tamPrev = prev.tam;
                sinalPrev = prev.sinal;
                prevIsolado = prev.isolado;
            }

            if (seg.sinal == 0) {
                // Vizinhos iguais: não existe decrescente ou crescente
                candies += 1;
            } else if (seg.sinal == 1) {
                if (prevIsolado) {
                    // A primeira criança começa em 1 doce, não em 2
                    candies += progressaoAritimetica(seg.tam);
                } else {
                    // Crescente que possui um decrescente antes
                    candies += progressaoAritimetica(seg.tam) + seg.tam;
                    seg.tam += 1; // Corrige o tamanho para as próximas iterações
                }
            } else if (seg.sinal == -1) {
                // Decrescente: a última criança sempre recebe 1 doce
                if (sinalPrev == 1 && seg.tam >= tamPrev && !prevIsolado) {
                    candies -= tamPrev;
                    candies += progressaoAritimetica(seg.tam + 1);
                } else {
                    candies += progressaoAritimetica(seg.tam);
                }
            }
        }

        return candies;
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
    struct Segment {
        int tam;
        int sinal;
        bool isolado;
    };

    int getSinal(const vector<int>& ratings, int index) {
        if (index == 0 || ratings[index] == ratings[index - 1]) {
            return 0;
        }
        if (ratings[index] < ratings[index - 1]) {
            return -1;
        }
        return 1;
    }

    vector<Segment> calculaSegmentos(const vector<int>& ratings) {
        int n = (int)ratings.size();
        vector<Segment> seg_sizes;
        int cur_start = 0;

        for (int i = 1; i < n; i++) {
            int sinal_cur = getSinal(ratings, i);
            int sinal_prev = getSinal(ratings, i - 1);

            if (sinal_cur == 0) {
                int seg_size = i - cur_start;
                // Corner case: subida e descida com topos iguais
                seg_sizes.push_back({seg_size, sinal_prev, true});
                cur_start = i;
                continue;
            }

            if (i > 1 && sinal_cur * sinal_prev == -1) { // Troca de inclinação
                int seg_size = i - cur_start;
                seg_sizes.push_back({seg_size, sinal_prev, false});
                cur_start = i;
            }
        }

        if (cur_start < n) {
            int sinal = getSinal(ratings, n - 1);
            int seg_size = n - cur_start;
            seg_sizes.push_back({seg_size, sinal, false});
        }

        return seg_sizes;
    }

    int progressaoAritimetica(int tam) {
        return (tam + 1) * tam / 2;
    }

public:
    int candy(vector<int>& ratings) {
        vector<Segment> segments = calculaSegmentos(ratings);
        int candies = 0;

        int tam_prev = 0, sinal_prev = 0;
        bool prev_isolado = true;

        for (int index = 0; index < (int)segments.size(); index++) {
            auto& seg = segments[index];
            if (index > 0) {
                tam_prev = segments[index - 1].tam;
                sinal_prev = segments[index - 1].sinal;
                prev_isolado = segments[index - 1].isolado;
            }

            if (seg.sinal == 0) {
                // Vizinhos iguais: não existe decrescente ou crescente
                candies += 1;
            } else if (seg.sinal == 1) {
                if (prev_isolado) {
                    // A primeira criança começa em 1 doce, não em 2
                    candies += progressaoAritimetica(seg.tam);
                } else {
                    // Crescente que possui um decrescente antes
                    candies += progressaoAritimetica(seg.tam) + seg.tam;
                    seg.tam += 1; // Corrige o tamanho para as próximas iterações
                }
            } else if (seg.sinal == -1) {
                // Decrescente: a última criança sempre recebe 1 doce
                if (sinal_prev == 1 && seg.tam >= tam_prev && !prev_isolado) {
                    candies -= tam_prev;
                    candies += progressaoAritimetica(seg.tam + 1);
                } else {
                    candies += progressaoAritimetica(seg.tam);
                }
            }
        }

        return candies;
    }
};

int main() {
    Solution solution;
    vector<int> r1 = {1, 0, 2};
    vector<int> r2 = {1, 2, 2};
    vector<int> r3 = {1, 3, 2, 2, 1};
    cout << solution.candy(r1) << endl; // 5
    cout << solution.candy(r2) << endl; // 4
    cout << solution.candy(r3) << endl; // 7
    return 0;
}
```

</details>

---

## Solução 2: Ordenação Topológica (Partindo dos Vales) [[00:08:41]](https://youtu.be/SOLagtZT_oU?t=521)

Podemos enxergar as restrições de vizinhança como um grafo acíclico direcionado (DAG), onde uma aresta aponta da criança com menor `rating` para a vizinha com maior `rating`.

1. Toda criança que tem `rating` menor ou igual aos dois vizinhos (`ehMenorDosVizinhos`) é um **vale** (grau de entrada 0) e recebe exatamente `1` doce.
2. A partir de cada vale, expandimos para a **esquerda** e para a **direita** enquanto o `rating` for estritamente crescente, incrementando a quantidade de doces de `1` em `1` (`count = 2, 3, ...`).
3. Quando duas expansões se encontram em um **pico**, usamos `max(candies[pos], count)` para garantir que o pico satisfaça ambos os lados.

* **Tempo:** $O(N)$, pois cada criança é visitada no máximo uma vez no laço principal e no máximo duas vezes nas expansões (uma vindo da esquerda e outra da direita).
* **Espaço:** $O(N)$, para o array `candies`.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def ehMenorDosVizinhos(self, ratings: list[int], i: int) -> bool:
        if i > 0 and ratings[i] > ratings[i - 1]:  # rating é maior que o vizinho esquerdo
            return False
        if i + 1 < len(ratings) and ratings[i] > ratings[i + 1]:  # rating é maior que o vizinho direito
            return False
        return True

    def candy(self, ratings: list[int]) -> int:
        n = len(ratings)
        candies = [-1] * n

        for i in range(n):
            if self.ehMenorDosVizinhos(ratings, i):
                # Essa criança recebe 1 doce, pois não precisa ter mais doces que ninguém
                candies[i] = 1

                # Enquanto crianças da esquerda tiverem rating maior: distribuir doces aumentando de 1 em 1
                l = i - 1
                count = 2  # A próxima criança vai receber 2 doces
                while l >= 0 and ratings[l] > ratings[l + 1]:
                    # max é aplicado devido aos picos, que podem ser visitados 2 vezes (uma de cada lado)
                    candies[l] = max(candies[l], count)
                    l = l - 1
                    count = count + 1

                # Mesma lógica se aplica para a direita
                r = i + 1
                count = 2
                while r < n and ratings[r] > ratings[r - 1]:
                    candies[r] = max(candies[r], count)
                    r = r + 1
                    count = count + 1

        ans = 0
        for i in range(n):
            ans += candies[i]
        return ans


if __name__ == "__main__":
    solution = Solution()
    print(solution.candy([1, 0, 2]))        # 5
    print(solution.candy([1, 2, 2]))        # 4
    print(solution.candy([1, 3, 2, 2, 1]))  # 7
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.Arrays;

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.candy(new int[]{1, 0, 2}));       // 5
        System.out.println(solution.candy(new int[]{1, 2, 2}));       // 4
        System.out.println(solution.candy(new int[]{1, 3, 2, 2, 1})); // 7
    }
}

class Solution {
    private boolean ehMenorDosVizinhos(int[] ratings, int i) {
        if (i > 0 && ratings[i] > ratings[i - 1]) { // rating é maior que o vizinho esquerdo
            return false;
        }
        if (i + 1 < ratings.length && ratings[i] > ratings[i + 1]) { // rating é maior que o vizinho direito
            return false;
        }
        return true;
    }

    public int candy(int[] ratings) {
        int n = ratings.length;
        int[] candies = new int[n];
        Arrays.fill(candies, -1);

        for (int i = 0; i < n; i++) {
            if (ehMenorDosVizinhos(ratings, i)) {
                // Essa criança recebe 1 doce, pois não precisa ter mais doces que ninguém
                candies[i] = 1;

                // Expande para a esquerda enquanto o rating for maior
                int l = i - 1;
                int count = 2;
                while (l >= 0 && ratings[l] > ratings[l + 1]) {
                    // max é aplicado devido aos picos, que podem ser visitados 2 vezes
                    candies[l] = Math.max(candies[l], count);
                    l--;
                    count++;
                }

                // Expande para a direita enquanto o rating for maior
                int r = i + 1;
                count = 2;
                while (r < n && ratings[r] > ratings[r - 1]) {
                    candies[r] = Math.max(candies[r], count);
                    r++;
                    count++;
                }
            }
        }

        int ans = 0;
        for (int i = 0; i < n; i++) {
            ans += candies[i];
        }
        return ans;
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
    bool ehMenorDosVizinhos(const vector<int>& ratings, int i) {
        if (i > 0 && ratings[i] > ratings[i - 1]) { // rating é maior que o vizinho esquerdo
            return false;
        }
        if (i + 1 < (int)ratings.size() && ratings[i] > ratings[i + 1]) { // rating é maior que o vizinho direito
            return false;
        }
        return true;
    }

public:
    int candy(vector<int>& ratings) {
        int n = (int)ratings.size();
        vector<int> candies(n, -1);

        for (int i = 0; i < n; i++) {
            if (ehMenorDosVizinhos(ratings, i)) {
                // Essa criança recebe 1 doce, pois não precisa ter mais doces que ninguém
                candies[i] = 1;

                // Expande para a esquerda enquanto o rating for maior
                int l = i - 1;
                int count = 2;
                while (l >= 0 && ratings[l] > ratings[l + 1]) {
                    // max é aplicado devido aos picos, que podem ser visitados 2 vezes
                    candies[l] = max(candies[l], count);
                    l--;
                    count++;
                }

                // Expande para a direita enquanto o rating for maior
                int r = i + 1;
                count = 2;
                while (r < n && ratings[r] > ratings[r - 1]) {
                    candies[r] = max(candies[r], count);
                    r++;
                    count++;
                }
            }
        }

        int ans = 0;
        for (int i = 0; i < n; i++) {
            ans += candies[i];
        }
        return ans;
    }
};

int main() {
    Solution solution;
    vector<int> r1 = {1, 0, 2};
    vector<int> r2 = {1, 2, 2};
    vector<int> r3 = {1, 3, 2, 2, 1};
    cout << solution.candy(r1) << endl; // 5
    cout << solution.candy(r2) << endl; // 4
    cout << solution.candy(r3) << endl; // 7
    return 0;
}
```

</details>

---

## Solução 3: Programação Dinâmica (Top-Down com Memoização) [[00:10:32]](https://youtu.be/SOLagtZT_oU?t=632)

Definimos `solve(i)` como o número mínimo de doces que a criança `i` precisa receber:

- Toda criança recebe pelo menos `1` doce.
- Se `ratings[i - 1] < ratings[i]`, a criança `i` precisa de pelo menos `solve(i - 1) + 1` doces.
- Se `ratings[i + 1] < ratings[i]`, a criança `i` precisa de pelo menos `solve(i + 1) + 1` doces.

Como só recorremos para vizinhos com `rating` **estritamente menor**, não existem ciclos na recursão. Guardando os resultados em `dp[i]` (memoização), cada posição é calculada uma única vez.

* **Tempo:** $O(N)$, pois cada estado `dp[i]` é calculado apenas uma vez.
* **Espaço:** $O(N)$, para o array de memoização `dp` e a pilha de recursão.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def solve(self, ratings: list[int], i: int) -> int:
        if self.dp[i] != 0:
            return self.dp[i]

        ans = 1
        if i - 1 >= 0 and ratings[i - 1] < ratings[i]:
            ans = max(ans, self.solve(ratings, i - 1) + 1)
        if i + 1 < len(ratings) and ratings[i + 1] < ratings[i]:
            ans = max(ans, self.solve(ratings, i + 1) + 1)

        self.dp[i] = ans
        return self.dp[i]

    def candy(self, ratings: list[int]) -> int:
        n = len(ratings)
        self.dp = [0] * n

        candies = 0
        for i in range(n):
            candies += self.solve(ratings, i)
        return candies


if __name__ == "__main__":
    solution = Solution()
    print(solution.candy([1, 0, 2]))        # 5
    print(solution.candy([1, 2, 2]))        # 4
    print(solution.candy([1, 3, 2, 2, 1]))  # 7
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.candy(new int[]{1, 0, 2}));       // 5
        System.out.println(solution.candy(new int[]{1, 2, 2}));       // 4
        System.out.println(solution.candy(new int[]{1, 3, 2, 2, 1})); // 7
    }
}

class Solution {
    private int[] dp;

    private int solve(int[] ratings, int i) {
        if (dp[i] != 0) {
            return dp[i];
        }

        int ans = 1;
        if (i - 1 >= 0 && ratings[i - 1] < ratings[i]) {
            ans = Math.max(ans, solve(ratings, i - 1) + 1);
        }
        if (i + 1 < ratings.length && ratings[i + 1] < ratings[i]) {
            ans = Math.max(ans, solve(ratings, i + 1) + 1);
        }

        dp[i] = ans;
        return dp[i];
    }

    public int candy(int[] ratings) {
        int n = ratings.length;
        dp = new int[n];

        int candies = 0;
        for (int i = 0; i < n; i++) {
            candies += solve(ratings, i);
        }
        return candies;
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
    vector<int> dp;

    int solve(const vector<int>& ratings, int i) {
        if (dp[i] != 0) {
            return dp[i];
        }

        int ans = 1;
        if (i - 1 >= 0 && ratings[i - 1] < ratings[i]) {
            ans = max(ans, solve(ratings, i - 1) + 1);
        }
        if (i + 1 < (int)ratings.size() && ratings[i + 1] < ratings[i]) {
            ans = max(ans, solve(ratings, i + 1) + 1);
        }

        dp[i] = ans;
        return dp[i];
    }

public:
    int candy(vector<int>& ratings) {
        int n = (int)ratings.size();
        dp.assign(n, 0);

        int candies = 0;
        for (int i = 0; i < n; i++) {
            candies += solve(ratings, i);
        }
        return candies;
    }
};

int main() {
    Solution solution;
    vector<int> r1 = {1, 0, 2};
    vector<int> r2 = {1, 2, 2};
    vector<int> r3 = {1, 3, 2, 2, 1};
    cout << solution.candy(r1) << endl; // 5
    cout << solution.candy(r2) << endl; // 4
    cout << solution.candy(r3) << endl; // 7
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
