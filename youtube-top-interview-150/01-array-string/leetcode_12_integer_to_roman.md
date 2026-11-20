# LeetCode 12 — Integer to Roman

* **Problema:** [LeetCode 12](https://leetcode.com/problems/integer-to-roman/)
* **Vídeo com a resolução:** [Episódio 18](https://youtu.be/OCUJd3YVRZI) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Medium
* **Tópico:** Array / String

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Quando usar |
| :--- | :---: | :---: | :--- |
| **1. Abordagem Direta (casos `4` e `9` explícitos)** | $O(1)$ | $O(1)$ | Tradução literal das regras do enunciado; ótimo ponto de partida |
| **2. Abordagem Gulosa com Tabela Unificada** | $O(1)$ | $O(1)$ | Solução mais enxuta e recomendada para entrevistas |

---

## Solução 1: Abordagem Direta (Tratando Dígitos `4` e `9` Separadamente)

Essa solução traduz diretamente as regras do enunciado. Mantemos apenas os 7 símbolos básicos na tabela (`M`, `D`, `C`, `L`, `X`, `V`, `I`) e, a cada iteração, verificamos o primeiro dígito decimal de `num`:

- Se o primeiro dígito **não** for `'4'` nem `'9'`, escolhemos de forma gulosa o maior símbolo básico menor ou igual a `num` e subtraímos seu valor.
- Se o primeiro dígito for `'4'` ou `'9'`, tratamos explicitamente os 6 casos subtrativos (`CM`, `CD`, `XC`, `XL`, `IX`, `IV`).

* **Tempo:** $O(1)$, pois a tabela possui tamanho fixo (7 entradas) e o domínio do problema é limitado (`1 <= num <= 3999`), gerando no máximo 15 iterações.
* **Espaço:** $O(1)$, pois a tabela, a conversão temporária para string (no máximo 4 dígitos) e a string resultante têm tamanho máximo constante.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        roman_digits_map = [
            ("M", 1000), ("D", 500), ("C", 100), ("L", 50), ("X", 10), ("V", 5), ("I", 1)
        ]

        resposta = []
        while num > 0:
            primeiro_digito = str(num)[0]
            if primeiro_digito not in ("4", "9"):
                for digito_romano, valor_decimal in roman_digits_map:
                    if num >= valor_decimal:
                        num -= valor_decimal
                        resposta.append(digito_romano)
                        break
            else:  # primeiro_digito in ('4', '9')
                if num >= 900:
                    resposta.append("CM")
                    num -= 900
                elif num >= 400:
                    resposta.append("CD")
                    num -= 400
                elif num >= 90:
                    resposta.append("XC")
                    num -= 90
                elif num >= 40:
                    resposta.append("XL")
                    num -= 40
                elif num == 9:
                    resposta.append("IX")
                    num -= 9
                elif num == 4:
                    resposta.append("IV")
                    num -= 4

        return "".join(resposta)


if __name__ == "__main__":
    print(Solution().intToRoman(num=3749))  # MMMDCCXLIX
    print(Solution().intToRoman(num=58))    # LVIII
    print(Solution().intToRoman(num=1994))  # MCMXCIV
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.intToRoman(3749)); // MMMDCCXLIX
        System.out.println(solution.intToRoman(58));   // LVIII
        System.out.println(solution.intToRoman(1994)); // MCMXCIV
    }
}

class Solution {
    public String intToRoman(int num) {
        String[] symbols = {"M", "D", "C", "L", "X", "V", "I"};
        int[] values = {1000, 500, 100, 50, 10, 5, 1};

        StringBuilder resposta = new StringBuilder();
        while (num > 0) {
            char primeiroDigito = String.valueOf(num).charAt(0);
            if (primeiroDigito != '4' && primeiroDigito != '9') {
                for (int i = 0; i < values.length; i++) {
                    if (num >= values[i]) {
                        num -= values[i];
                        resposta.append(symbols[i]);
                        break;
                    }
                }
            } else {
                // Casos subtrativos (primeiro dígito é 4 ou 9)
                if (num >= 900) {
                    resposta.append("CM");
                    num -= 900;
                } else if (num >= 400) {
                    resposta.append("CD");
                    num -= 400;
                } else if (num >= 90) {
                    resposta.append("XC");
                    num -= 90;
                } else if (num >= 40) {
                    resposta.append("XL");
                    num -= 40;
                } else if (num == 9) {
                    resposta.append("IX");
                    num -= 9;
                } else if (num == 4) {
                    resposta.append("IV");
                    num -= 4;
                }
            }
        }

        return resposta.toString();
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <iostream>
#include <string>
#include <utility>
#include <vector>
using namespace std;

class Solution {
public:
    string intToRoman(int num) {
        vector<pair<string, int>> roman_digits_map = {
            {"M", 1000}, {"D", 500}, {"C", 100}, {"L", 50},
            {"X", 10},   {"V", 5},   {"I", 1}
        };

        string resposta;
        while (num > 0) {
            char primeiro_digito = to_string(num)[0];
            if (primeiro_digito != '4' && primeiro_digito != '9') {
                for (const auto& [digito_romano, valor_decimal] : roman_digits_map) {
                    if (num >= valor_decimal) {
                        num -= valor_decimal;
                        resposta += digito_romano;
                        break;
                    }
                }
            } else {
                // Casos subtrativos (primeiro dígito é 4 ou 9)
                if (num >= 900) {
                    resposta += "CM";
                    num -= 900;
                } else if (num >= 400) {
                    resposta += "CD";
                    num -= 400;
                } else if (num >= 90) {
                    resposta += "XC";
                    num -= 90;
                } else if (num >= 40) {
                    resposta += "XL";
                    num -= 40;
                } else if (num == 9) {
                    resposta += "IX";
                    num -= 9;
                } else if (num == 4) {
                    resposta += "IV";
                    num -= 4;
                }
            }
        }

        return resposta;
    }
};

int main() {
    Solution solution;
    cout << solution.intToRoman(3749) << endl; // MMMDCCXLIX
    cout << solution.intToRoman(58) << endl;   // LVIII
    cout << solution.intToRoman(1994) << endl; // MCMXCIV
    return 0;
}
```

</details>

---

## Solução 2: Abordagem Gulosa com Tabela Unificada [[00:03:48]](https://youtu.be/OCUJd3YVRZI?t=228)

Em vez de escrever condicionais separados para os dígitos `4` e `9` em cada casa decimal, incluímos diretamente os casos subtrativos (`CM`, `CD`, `XC`, `XL`, `IX`, `IV`) em uma única tabela ordenada do maior valor (`1000`) para o menor (`1`).

Enquanto `num > 0`, escolhemos de forma gulosa o maior valor da tabela que seja menor ou igual a `num`, subtraímos esse valor de `num` e adicionamos o símbolo correspondente à resposta.

* **Tempo:** $O(1)$, pois a tabela tem tamanho fixo (13 entradas) e o domínio do problema é limitado (`1 <= num <= 3999`), gerando no máximo 15 caracteres na saída.
* **Espaço:** $O(1)$, pois tanto a tabela quanto a string resultante têm tamanho máximo constante.

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def intToRoman(self, num: int) -> str:
        # Tabela unificada incluindo os casos de subtração (4 e 9 em cada casa decimal)
        roman_digits_map = [
            ("M", 1000),
            ("CM", 900),
            ("D", 500),
            ("CD", 400),
            ("C", 100),
            ("XC", 90),
            ("L", 50),
            ("XL", 40),
            ("X", 10),
            ("IX", 9),
            ("V", 5),
            ("IV", 4),
            ("I", 1),
        ]

        resposta = []
        while num > 0:
            for digito_romano, valor_decimal in roman_digits_map:
                if num >= valor_decimal:
                    num -= valor_decimal
                    resposta.append(digito_romano)
                    break

        return "".join(resposta)


if __name__ == "__main__":
    print(Solution().intToRoman(3749))  # MMMDCCXLIX
    print(Solution().intToRoman(58))    # LVIII
    print(Solution().intToRoman(1994))  # MCMXCIV
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
public class Main {
    public static void main(String[] args) {
        Solution solution = new Solution();
        System.out.println(solution.intToRoman(3749)); // MMMDCCXLIX
        System.out.println(solution.intToRoman(58));   // LVIII
        System.out.println(solution.intToRoman(1994)); // MCMXCIV
    }
}

class Solution {
    public String intToRoman(int num) {
        // Tabela unificada incluindo os casos de subtração (4 e 9 em cada casa decimal)
        String[] symbols = {
            "M", "CM", "D", "CD", "C", "XC", "L", "XL", "X", "IX", "V", "IV", "I"
        };
        int[] values = {
            1000, 900, 500, 400, 100, 90, 50, 40, 10, 9, 5, 4, 1
        };

        StringBuilder resposta = new StringBuilder();
        while (num > 0) {
            for (int i = 0; i < values.length; i++) {
                if (num >= values[i]) {
                    num -= values[i];
                    resposta.append(symbols[i]);
                    break;
                }
            }
        }

        return resposta.toString();
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <iostream>
#include <string>
#include <utility>
#include <vector>
using namespace std;

class Solution {
public:
    string intToRoman(int num) {
        // Tabela unificada incluindo os casos de subtração (4 e 9 em cada casa decimal)
        vector<pair<string, int>> roman_digits_map = {
            {"M", 1000}, {"CM", 900}, {"D", 500}, {"CD", 400},
            {"C", 100},  {"XC", 90},  {"L", 50},  {"XL", 40},
            {"X", 10},   {"IX", 9},   {"V", 5},   {"IV", 4},
            {"I", 1}
        };

        string resposta;
        while (num > 0) {
            for (const auto& [digito_romano, valor_decimal] : roman_digits_map) {
                if (num >= valor_decimal) {
                    num -= valor_decimal;
                    resposta += digito_romano;
                    break;
                }
            }
        }

        return resposta;
    }
};

int main() {
    Solution solution;
    cout << solution.intToRoman(3749) << endl; // MMMDCCXLIX
    cout << solution.intToRoman(58) << endl;   // LVIII
    cout << solution.intToRoman(1994) << endl; // MCMXCIV
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
