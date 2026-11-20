# LeetCode 58 — Length of Last Word

* **Problema:** [LeetCode 58](https://leetcode.com/problems/length-of-last-word/)
* **Vídeo com a resolução:** [Episódio 19](https://youtu.be/PO02pxnC508) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Easy
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Solução "Sem Graça" (`strip` + `split`)** | $O(N)$ | $O(N)$ | Rápida de escrever com funções prontas da linguagem, mas aloca memória extra |
| **2. Iteração de Trás para Frente (Ótima)** | $O(N)$ | $O(1)$ | Ideal para entrevistas: evita alocações extras e para assim que termina a última palavra |

---

## Solução 1: Solução "Sem Graça" (`strip` + `split`) [[00:01:49]](https://youtu.be/PO02pxnC508?t=109)

A forma mais direta de resolver o problema é usar funções prontas da linguagem para remover os espaços das extremidades, dividir a string em palavras e retornar o tamanho da última palavra. Apesar de concisa, essa abordagem percorre a string inteira e aloca memória extra para armazenar a cópia e a lista de substrings.

* **Tempo:** $O(N)$, onde $N$ é o tamanho da string `s` (percorre toda a string para remover espaços e fazer o `split`).
* **Espaço:** $O(N)$, para armazenar a string aparada e as substrings geradas.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        return len(s.strip().split(' ')[-1])


if __name__ == "__main__":
    print(Solution().lengthOfLastWord("Hello World"))                 # 5
    print(Solution().lengthOfLastWord("   fly me   to   the moon  ")) # 4
    print(Solution().lengthOfLastWord("luffy is still joyboy"))       # 6
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.lengthOfLastWord("Hello World"));                 // 5
        System.out.println(solution.lengthOfLastWord("   fly me   to   the moon  ")); // 4
        System.out.println(solution.lengthOfLastWord("luffy is still joyboy"));       // 6
    }
}

class Solution {
    public int lengthOfLastWord(String s) {
        String[] words = s.trim().split(" ");
        return words[words.length - 1].length();
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <iostream>
#include <sstream>
#include <string>
using namespace std;

class Solution {
public:
    int lengthOfLastWord(string s) {
        istringstream iss(s);
        string word, lastWord;
        while (iss >> word) {
            lastWord = word;
        }
        return (int)lastWord.size();
    }
};

int main() {
    Solution solution;
    cout << solution.lengthOfLastWord("Hello World") << endl;                 // 5
    cout << solution.lengthOfLastWord("   fly me   to   the moon  ") << endl; // 4
    cout << solution.lengthOfLastWord("luffy is still joyboy") << endl;       // 6
    return 0;
}
```

</details>

---

## Solução 2: Iteração de Trás para Frente [[00:04:02]](https://youtu.be/PO02pxnC508?t=242)

Em entrevistas técnicas, evita-se depender de funções prontas como `.strip()` e `.split()`, que alocam memória extra para copiar e dividir a string inteira. A solução ideal percorre a string diretamente de trás para frente com um ponteiro `pos`:

1. Ignoramos todos os espaços em branco no final da string (`s[pos] == ' '`).
2. Contamos os caracteres da última palavra enquanto `pos >= 0` e `s[pos] != ' '`.

* **Tempo:** $O(N)$ no pior caso, onde $N$ é o tamanho da string `s` (no melhor caso, percorre apenas a última palavra e os espaços finais).
* **Espaço:** $O(1)$, utilizando apenas duas variáveis inteiras (`pos` e `count`).

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def lengthOfLastWord(self, s: str) -> int:
        pos = len(s) - 1

        # Pula os espaços em branco no final da string
        while pos >= 0 and s[pos] == " ":
            pos -= 1

        # Conta o tamanho da última palavra
        count = 0
        while pos >= 0 and s[pos] != " ":
            count += 1
            pos -= 1

        return count


if __name__ == "__main__":
    print(Solution().lengthOfLastWord("Hello World"))                 # 5
    print(Solution().lengthOfLastWord("   fly me   to   the moon  ")) # 4
    print(Solution().lengthOfLastWord("luffy is still joyboy"))       # 6
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.lengthOfLastWord("Hello World"));                 // 5
        System.out.println(solution.lengthOfLastWord("   fly me   to   the moon  ")); // 4
        System.out.println(solution.lengthOfLastWord("luffy is still joyboy"));       // 6
    }
}

class Solution {
    public int lengthOfLastWord(String s) {
        int pos = s.length() - 1;

        // Pula os espaços em branco no final da string
        while (pos >= 0 && s.charAt(pos) == ' ') {
            pos -= 1;
        }

        // Conta o tamanho da última palavra
        int count = 0;
        while (pos >= 0 && s.charAt(pos) != ' ') {
            count += 1;
            pos -= 1;
        }

        return count;
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <iostream>
#include <string>
using namespace std;

class Solution {
public:
    int lengthOfLastWord(string s) {
        int pos = (int)s.size() - 1;

        // Pula os espaços em branco no final da string
        while (pos >= 0 && s[pos] == ' ') {
            pos -= 1;
        }

        // Conta o tamanho da última palavra
        int count = 0;
        while (pos >= 0 && s[pos] != ' ') {
            count += 1;
            pos -= 1;
        }

        return count;
    }
};

int main() {
    Solution solution;
    cout << solution.lengthOfLastWord("Hello World") << endl;                 // 5
    cout << solution.lengthOfLastWord("   fly me   to   the moon  ") << endl; // 4
    cout << solution.lengthOfLastWord("luffy is still joyboy") << endl;       // 6
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
