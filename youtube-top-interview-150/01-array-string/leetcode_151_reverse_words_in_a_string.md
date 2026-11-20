# LeetCode 151 — Reverse Words in a String

* **Problema:** [LeetCode 151](https://leetcode.com/problems/reverse-words-in-a-string/)
* **Vídeo com a resolução:** [Episódio 21](https://youtu.be/MpuYAM_IBrM) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Medium
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Solução Base (`split` + inversão)** | $O(N)$ | $O(N)$ | Solução direta, limpa e rápida de escrever |
| **2. Follow-up In-Place (3 passos)** | $O(N)$ | $O(1)$* | Quando o entrevistador pede otimização de memória (*$O(1)$ em linguagens com strings mutáveis como C++; $O(N)$ em Python/Java devido à imutabilidade de strings*) |

*(Onde $N$ é o tamanho da string `s`.)*

---

## 1. Solução Base (`split` + Inversão) [[00:01:10]](https://youtu.be/MpuYAM_IBrM?t=70)

Dividimos a string nos espaços, filtramos as strings vazias geradas por espaços consecutivos ou nas extremidades, invertemos a lista de palavras e juntamos novamente com um único espaço simples.

* **Tempo:** $O(N)$, realizando quatro operações lineares sequenciais ($O(4N) = O(N)$).
* **Espaço:** $O(N)$, para armazenar a lista de palavras e a string final.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # O(N + N + N + N) = O(4*N) = O(N)

        # O(N) — divide a string por espaço simples
        palavras = s.split(" ")                    # ['the', 'sky', '', '', 'is', 'blue', '', '', '']

        # O(N) — remove strings vazias causadas por espaços extras
        palavras = [w for w in palavras if w != ""]  # ['the', 'sky', 'is', 'blue']

        # O(N) — inverte a ordem das palavras
        palavras = palavras[::-1]                  # ['blue', 'is', 'sky', 'the']

        # O(N) — junta as palavras com um único espaço
        return " ".join(palavras)                  # 'blue is sky the'


if __name__ == "__main__":
    print(Solution().reverseWords("the sky   is blue   "))
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.reverseWords("the sky   is blue   "));
    }
}

class Solution {
    public String reverseWords(String s) {
        // Divide a string por espaço e filtra partes vazias
        String[] partes = s.split(" ");
        List<String> palavras = new ArrayList<>();
        for (String w : partes) {
            if (!w.isEmpty()) {
                palavras.add(w);
            }
        }

        // Inverte a ordem das palavras e junta com um único espaço
        Collections.reverse(palavras);
        return String.join(" ", palavras);
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <algorithm>
#include <iostream>
#include <sstream>
#include <string>
#include <vector>
using namespace std;

class Solution {
public:
    string reverseWords(string s) {
        // Extrai todas as palavras ignorando espaços extras
        istringstream iss(s);
        vector<string> palavras;
        string palavra;
        while (iss >> palavra) {
            palavras.push_back(palavra);
        }

        // Inverte a ordem das palavras e junta com um único espaço
        reverse(palavras.begin(), palavras.end());

        string resultado;
        for (int i = 0; i < (int)palavras.size(); i++) {
            if (i > 0) resultado += ' ';
            resultado += palavras[i];
        }
        return resultado;
    }
};

int main() {
    Solution solution;
    cout << solution.reverseWords("the sky   is blue   ") << endl;
    return 0;
}
```

</details>

---

## 2. Follow-up In-Place (Espaço Extra $O(1)$) [[00:04:00]](https://youtu.be/MpuYAM_IBrM?t=240)

Para resolver o problema *in-place* (sem criar listas auxiliares de palavras), aplicamos três etapas diretamente sobre o array de caracteres:

1. **Limpar espaços desnecessários** (no início, no fim e múltiplos espaços entre palavras), compactando o texto válido no início do array.
2. **Inverter o texto inteiro** no intervalo válido `[0..pos-1]`.
3. **Inverter cada palavra isoladamente** para restaurar a ordem correta das letras de cada palavra.

*(Em Java e Python, como `String`/`str` são imutáveis, operamos sobre um array de caracteres mutável `char[]`/`list[str]`. Em C++, a `std::string` é mutável e a solução atinge espaço extra $O(1)$ real.)*

* **Tempo:** $O(N)$, pois cada caractere é lido, movido e invertido um número constante de vezes.
* **Espaço:** $O(1)$ em C++ (modificação *in-place* da string) e $O(N)$ em Python/Java (devido à conversão para array mutável).

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def _reverse(self, chars: list[str], left: int, right: int) -> None:
        while left < right:
            chars[left], chars[right] = chars[right], chars[left]
            left += 1
            right -= 1

    def reverseWords(self, s: str) -> str:
        # Strings em Python são imutáveis; convertemos para lista para simular in-place
        chars = list(s)
        n = len(chars)

        # (1) Remover espaços desnecessários
        accept_space = False
        pos = 0
        for i in range(n):
            if not accept_space and chars[i] == " ":
                continue

            chars[pos] = chars[i]
            accept_space = chars[pos] != " "
            pos += 1

        if pos > 0 and chars[pos - 1] == " ":
            pos -= 1

        # (2) Inverter o texto válido por completo
        self._reverse(chars, 0, pos - 1)

        # (3) Inverter cada palavra isoladamente
        inicio = 0
        for i in range(pos):
            if chars[i] == " ":
                self._reverse(chars, inicio, i - 1)
                inicio = i + 1
        self._reverse(chars, inicio, pos - 1)

        return "".join(chars[:pos])


if __name__ == "__main__":
    print(Solution().reverseWords("the sky   is blue   "))
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution sol = new Solution();
        String teste = "   Aprender   Java   no   Terminal   e   muito   legal   ";

        System.out.println("Texto original: '" + teste + "'");
        String resultado = sol.reverseWords(teste);
        System.out.println("Texto invertido: '" + resultado + "'");
    }
}

class Solution {
    public String reverseWords(String s) {
        if (s == null || s.length() == 0) return s;

        // Convertemos para array para fazer todas as operações in-place
        char[] a = s.toCharArray();
        int n = a.length;

        // 1. Inverte a string inteira primeiro
        reverse(a, 0, n - 1);

        // 2. Inverte cada palavra individualmente dentro do array invertido
        reverseWords(a, n);

        // 3. Remove os espaços extras (iniciais, finais e múltiplos espaços no meio)
        return cleanSpaces(a, n);
    }

    // Inverte os caracteres in-place entre os índices i e j
    private void reverse(char[] a, int i, int j) {
        while (i < j) {
            char t = a[i];
            a[i++] = a[j];
            a[j--] = t;
        }
    }

    // Varre o array invertendo os caracteres de cada palavra isolada
    private void reverseWords(char[] a, int n) {
        int i = 0, j = 0;
        while (i < n) {
            while (i < j || i < n && a[i] == ' ') i++; // pula espaços
            while (j < i || j < n && a[j] != ' ') j++; // vai até o fim da palavra
            reverse(a, i, j - 1);                      // inverte a palavra encontrada
        }
    }

    // Desloca os caracteres válidos para o início do array, eliminando espaços extras in-place
    private String cleanSpaces(char[] a, int n) {
        int i = 0, j = 0;
        while (j < n) {
            while (j < n && a[j] == ' ') j++;             // pula espaços iniciais/duplos
            while (j < n && a[j] != ' ') a[i++] = a[j++]; // copia a palavra válida para a posição 'i'
            while (j < n && a[j] == ' ') j++;             // pula os espaços após a palavra
            if (j < n) a[i++] = ' ';                      // se ainda houver conteúdo, adiciona apenas um espaço simples
        }
        // Retorna a string contendo apenas a região válida do array modificado
        return new String(a, 0, i);
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <algorithm>
#include <iostream>
#include <string>
using namespace std;

class Solution {
public:
    string reverseWords(string s) {
        // (1) Remover espaços desnecessários
        // Tempo: O(N) | Memória: O(1)
        bool accept_space = false;
        int pos = 0;
        int n = s.size();

        for (int i = 0; i < n; i++) {
            if (!accept_space && s[i] == ' ') {
                continue;
            }

            s[pos] = s[i];

            if (s[pos] == ' ') accept_space = false;
            else accept_space = true;

            pos++;
        }
        if (pos > 0 && s[pos - 1] == ' ') pos--;

        // (2) Reverse no texto por completo
        // Complexidade: Tempo O(N) | Memória O(1)
        reverse(s.begin(), s.begin() + pos);

        // (3) Reverse em cada palavra isoladamente
        // Complexidade: Tempo O(N) | Memória O(1)
        int inicio = 0;
        for (int i = 0; i < pos; i++) { // O(N)
            if (s[i] == ' ') {
                reverse(s.begin() + inicio, s.begin() + i); // Somatório total: O(N)
                inicio = i + 1;
            }
        }
        reverse(s.begin() + inicio, s.begin() + pos);

        return s.substr(0, pos);
    }
};

int main() {
    Solution solution;
    cout << solution.reverseWords("the sky   is blue   ") << endl;
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
