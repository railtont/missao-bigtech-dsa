# LeetCode 125 — Valid Palindrome

* **Problema:** [LeetCode 125](https://leetcode.com/problems/valid-palindrome/)
* **Vídeo com a resolução:** [Episódio 25](https://youtu.be/gums6WNKYEU) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Easy
* **Tópico:** Two Pointers

---

## Solução: Two Pointers (Ponteiros Opostos) [[00:08:50]](https://youtu.be/gums6WNKYEU?t=530)

Em vez de construir uma nova string filtrada e invertê-la (o que gastaria memória $O(N)$), usamos dois ponteiros que começam nas extremidades da string (`left = 0` e `right = len(s) - 1`) e caminham em direção ao centro:

1. Convertemos os caracteres em `left` e `right` para minúsculo.
2. Se o caractere de `left` não for alfanumérico (nem letra nem dígito), avançamos `left` e continuamos.
3. Se o caractere de `right` não for alfanumérico, recuamos `right` e continuamos.
4. Comparamos os dois caracteres: se forem diferentes, retornamos `false`; caso contrário, movemos ambos os ponteiros (`left++`, `right--`).

Se os ponteiros se encontrarem sem nenhuma divergência, a string é um palíndromo válido.

* **Tempo:** $O(N)$, onde $N$ é o tamanho da string `s` (cada caractere é visitado no máximo uma vez).
* **Espaço:** $O(1)$, usando apenas os dois ponteiros e variáveis auxiliares constantes.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left = 0
        right = len(s) - 1

        while left < right:
            a = s[left].lower()
            b = s[right].lower()

            # Ignora caracteres que não são letras nem dígitos
            if not a.isalnum():
                left += 1
                continue
            if not b.isalnum():
                right -= 1
                continue

            if a != b:
                return False

            left += 1
            right -= 1

        return True


if __name__ == "__main__":
    solution = Solution()
    print(solution.isPalindrome("A man, a plan, a canal: Panama"))  # True
    print(solution.isPalindrome("race a car"))                      # False
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.isPalindrome("A man, a plan, a canal: Panama")); // true
        System.out.println(solution.isPalindrome("race a car"));                     // false
    }
}

class Solution {
    public boolean isPalindrome(String s) {
        int left = 0;
        int right = s.length() - 1;

        char a, b;
        while (left < right) {
            a = Character.toLowerCase(s.charAt(left));
            b = Character.toLowerCase(s.charAt(right));

            // Ignora caracteres que não são letras nem dígitos
            if (!Character.isLetter(a) && !Character.isDigit(a)) {
                left += 1;
                continue;
            }
            if (!Character.isLetter(b) && !Character.isDigit(b)) {
                right -= 1;
                continue;
            }

            if (a != b) {
                return false;
            }

            left += 1;
            right -= 1;
        }
        return true;
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <cctype>
#include <iostream>
#include <string>
using namespace std;

class Solution {
public:
    bool isPalindrome(string s) {
        int left = 0;
        int right = (int)s.size() - 1;

        while (left < right) {
            unsigned char a = tolower(static_cast<unsigned char>(s[left]));
            unsigned char b = tolower(static_cast<unsigned char>(s[right]));

            // Ignora caracteres que não são letras nem dígitos
            if (!isalnum(a)) {
                left += 1;
                continue;
            }
            if (!isalnum(b)) {
                right -= 1;
                continue;
            }

            if (a != b) {
                return false;
            }

            left += 1;
            right -= 1;
        }
        return true;
    }
};

int main() {
    Solution solution;
    cout << boolalpha;
    cout << solution.isPalindrome("A man, a plan, a canal: Panama") << endl; // true
    cout << solution.isPalindrome("race a car") << endl;                     // false
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
