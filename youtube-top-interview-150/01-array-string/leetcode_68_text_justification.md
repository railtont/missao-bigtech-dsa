# LeetCode 68 — Text Justification

* **Problema:** [LeetCode 68](https://leetcode.com/problems/text-justification/)
* **Vídeo com a resolução:** [Episódio 24](https://youtu.be/yeJ5y2YsJ24) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Hard
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Construção Gulosa Linha a Linha** | $O(T)$ | $O(T)$ | Solução direta e modular isolando o empacotamento e a justificação por linha |

---

## Solução 1: Construção Gulosa Linha a Linha [[00:02:48]](https://youtu.be/yeJ5y2YsJ24?t=168)

Isolamos a responsabilidade de montar e justificar cada linha em uma função auxiliar `newLine`, dividindo o problema em duas etapas bem claras:

1. **Empacotamento guloso:** Enquanto houver espaço restante (`maxWidth`), adicionamos a próxima palavra à linha atual (separando palavras subsequentes por um espaço mínimo de `" "`).
2. **Justificação:**
   - **Casos de borda** (linha com apenas uma palavra ou última linha do texto): alinhamos à esquerda e completamos o restante à direita com espaços.
   - **Caso geral:** Calculamos o número de intervalos (`gaps = words_count - 1`), a divisão inteira dos espaços extras (`evenly = maxWidth // gaps`) e o resto (`remainder = maxWidth % gaps`). Em seguida, expandimos cada espaço da esquerda para a direita adicionando `evenly` e mais `1` espaço extra enquanto houver resto (`remainder > 0`).

* **Tempo:** $O(T)$, onde $T$ é o total de caracteres na saída (cada palavra é processada uma única vez e cada linha construída tem exatamente `maxWidth` caracteres).
* **Espaço:** $O(T)$ para armazenar todas as linhas justificadas na resposta ($O(\text{maxWidth})$ de memória auxiliar por linha).

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def newLine(self, cur_pos: int, words: list[str], maxWidth: int) -> tuple[str, int]:
        line = []
        words_count = 0

        # (1) Adiciona palavras enquanto couberem na linha atual
        while cur_pos < len(words):
            cur_word = words[cur_pos]
            if len(line) == 0 and len(cur_word) <= maxWidth:
                line.append(cur_word)
                maxWidth -= len(cur_word)
            elif len(line) > 0 and len(cur_word) + 1 <= maxWidth:
                line.append(" ")
                line.append(cur_word)
                maxWidth = maxWidth - len(cur_word) - 1
            else:
                break

            words_count += 1
            cur_pos += 1

        # (2) Justificação da linha
        # Casos de borda: palavra única ou última linha (alinhamento à esquerda)
        if len(line) == 1:
            return "".join(line) + " " * maxWidth, cur_pos

        if cur_pos >= len(words):
            return "".join(line) + " " * maxWidth, cur_pos

        # Caso principal: distribui espaços extras igualmente entre os intervalos
        gaps = words_count - 1
        evenly = maxWidth // gaps
        remainder = maxWidth % gaps

        for i in range(len(line)):
            if line[i] == " ":
                spaces = 1 + evenly
                if remainder > 0:
                    spaces += 1
                    remainder -= 1

                line[i] = " " * spaces

        return "".join(line), cur_pos

    def fullJustify(self, words: list[str], maxWidth: int) -> list[str]:
        ans = []
        cur_pos = 0

        while cur_pos < len(words):
            line, cur_pos = self.newLine(cur_pos, words, maxWidth)
            ans.append(line)

        return ans


if __name__ == "__main__":
    words1 = ["This", "is", "an", "example", "of", "text", "justification."]
    print(Solution().fullJustify(words1, 16))

    words2 = ["What", "must", "be", "acknowledgment", "shall", "be"]
    print(Solution().fullJustify(words2, 16))
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
        String[] words1 = {"This", "is", "an", "example", "of", "text", "justification."};
        System.out.println(solution.fullJustify(words1, 16));

        String[] words2 = {"What", "must", "be", "acknowledgment", "shall", "be"};
        System.out.println(solution.fullJustify(words2, 16));
    }
}

class Solution {
    private int newLine(int curPos, String[] words, int maxWidth, List<String> ans) {
        List<String> line = new ArrayList<>();
        int wordsCount = 0;

        // (1) Adiciona palavras enquanto couberem na linha atual
        while (curPos < words.length) {
            String curWord = words[curPos];
            if (line.isEmpty() && curWord.length() <= maxWidth) {
                line.add(curWord);
                maxWidth -= curWord.length();
            } else if (!line.isEmpty() && curWord.length() + 1 <= maxWidth) {
                line.add(" ");
                line.add(curWord);
                maxWidth = maxWidth - curWord.length() - 1;
            } else {
                break;
            }

            wordsCount += 1;
            curPos += 1;
        }

        // (2) Justificação da linha
        // Casos de borda: palavra única ou última linha (alinhamento à esquerda)
        if (line.size() == 1 || curPos >= words.length) {
            StringBuilder sb = new StringBuilder();
            for (String token : line) sb.append(token);
            sb.append(" ".repeat(maxWidth));
            ans.add(sb.toString());
            return curPos;
        }

        // Caso principal: distribui espaços extras igualmente entre os intervalos
        int gaps = wordsCount - 1;
        int evenly = maxWidth / gaps;
        int remainder = maxWidth % gaps;

        StringBuilder sb = new StringBuilder();
        for (String token : line) {
            if (token.equals(" ")) {
                int spaces = 1 + evenly;
                if (remainder > 0) {
                    spaces += 1;
                    remainder -= 1;
                }
                sb.append(" ".repeat(spaces));
            } else {
                sb.append(token);
            }
        }

        ans.add(sb.toString());
        return curPos;
    }

    public List<String> fullJustify(String[] words, int maxWidth) {
        List<String> ans = new ArrayList<>();
        int curPos = 0;

        while (curPos < words.length) {
            curPos = newLine(curPos, words, maxWidth, ans);
        }

        return ans;
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <iostream>
#include <string>
#include <vector>
using namespace std;

class Solution {
    string newLine(int& cur_pos, const vector<string>& words, int maxWidth) {
        vector<string> line;
        int words_count = 0;

        // (1) Adiciona palavras enquanto couberem na linha atual
        while (cur_pos < (int)words.size()) {
            const string& cur_word = words[cur_pos];
            if (line.empty() && (int)cur_word.size() <= maxWidth) {
                line.push_back(cur_word);
                maxWidth -= cur_word.size();
            } else if (!line.empty() && (int)cur_word.size() + 1 <= maxWidth) {
                line.push_back(" ");
                line.push_back(cur_word);
                maxWidth = maxWidth - (int)cur_word.size() - 1;
            } else {
                break;
            }

            words_count += 1;
            cur_pos += 1;
        }

        // (2) Justificação da linha
        // Casos de borda: palavra única ou última linha (alinhamento à esquerda)
        if (line.size() == 1 || cur_pos >= (int)words.size()) {
            string result;
            for (const string& token : line) result += token;
            result.append(maxWidth, ' ');
            return result;
        }

        // Caso principal: distribui espaços extras igualmente entre os intervalos
        int gaps = words_count - 1;
        int evenly = maxWidth / gaps;
        int remainder = maxWidth % gaps;

        string result;
        for (const string& token : line) {
            if (token == " ") {
                int spaces = 1 + evenly;
                if (remainder > 0) {
                    spaces += 1;
                    remainder -= 1;
                }
                result.append(spaces, ' ');
            } else {
                result += token;
            }
        }

        return result;
    }

public:
    vector<string> fullJustify(vector<string>& words, int maxWidth) {
        vector<string> ans;
        int cur_pos = 0;

        while (cur_pos < (int)words.size()) {
            ans.push_back(newLine(cur_pos, words, maxWidth));
        }

        return ans;
    }
};

int main() {
    Solution solution;
    vector<string> words = {"This", "is", "an", "example", "of", "text", "justification."};
    for (const string& line : solution.fullJustify(words, 16)) {
        cout << "\"" << line << "\"" << endl;
    }
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
