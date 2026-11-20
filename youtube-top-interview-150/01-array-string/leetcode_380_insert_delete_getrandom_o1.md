# LeetCode 380 — Insert Delete GetRandom O(1)

* **Problema:** [LeetCode 380](https://leetcode.com/problems/insert-delete-getrandom-o1/)
* **Vídeo com a resolução:** [Episódio 12](https://youtu.be/oZkkdtGNWeY) | [Playlists](https://www.youtube.com/@railtonc/playlists)
* **Dificuldade:** Medium
* **Tópico:** Array / String

---

## Solução: Array Dinâmico + Hash Map (*Swap-and-Pop*) [[00:02:30]](https://youtu.be/oZkkdtGNWeY?t=150)

Para suportar `insert`, `remove` e `getRandom` em tempo médio $O(1)$, combinamos duas estruturas:

- **`lista` (array dinâmico):** permite acesso aleatório por índice em $O(1)$ para `getRandom` e inserção/remoção no final em $O(1)$.
- **`mapa` (hash map):** mapeia cada valor para o seu índice atual em `lista`, permitindo verificar existência e localizar qualquer elemento em $O(1)$ médio.

O truque principal está no `remove(val)`: remover um elemento do meio de um array custaria $O(N)$ para deslocar os elementos seguintes. Em vez disso, **trocamos o elemento a ser removido com o último elemento da lista**, atualizamos o índice desse último elemento no `mapa` e removemos o final da lista em $O(1)$ (*swap-and-pop*).

* **Tempo:** $O(1)$ médio para `insert`, `remove` e `getRandom`.
* **Espaço:** $O(N)$, onde $N$ é a quantidade de elementos armazenados atualmente no conjunto (tanto na `lista` quanto no `mapa`).

<details open>
<summary><strong>Python</strong></summary>

```python
import random


class RandomizedSet:
    def __init__(self):
        self.lista = []  # Armazena valores únicos
        self.mapa = {}   # Mapeia valor para índice na lista

    def insert(self, val: int) -> bool:
        if val not in self.mapa:
            self.mapa[val] = len(self.lista)
            self.lista.append(val)
            return True
        return False

    def remove(self, val: int) -> bool:
        if val in self.mapa:
            index = self.mapa[val]
            last_index = len(self.lista) - 1
            last_val = self.lista[last_index]

            if index != last_index:
                # Se não for o último elemento, joga ele para o final antes de remover
                self.lista[index], self.lista[last_index] = (
                    self.lista[last_index],
                    self.lista[index],
                )
                # Atualiza o mapeamento do elemento movido
                self.mapa[last_val] = index

            # Remove o último elemento
            self.mapa.pop(val)
            self.lista.pop()
            return True
        return False

    def getRandom(self) -> int:
        if len(self.lista) == 0:
            return -1

        random_index = random.randint(1, len(self.lista)) - 1
        return self.lista[random_index]


if __name__ == "__main__":
    obj = RandomizedSet()
    print(obj.insert(1))   # True
    print(obj.remove(2))   # False
    print(obj.insert(2))   # True
    print(obj.getRandom()) # 1 ou 2
    print(obj.remove(1))   # True
    print(obj.insert(2))   # False
    print(obj.getRandom()) # 2
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;
import java.util.Random;

public class Main {
    public static void main(String[] args) {
        RandomizedSet obj = new RandomizedSet();
        System.out.println(obj.insert(1));   // true
        System.out.println(obj.remove(2));   // false
        System.out.println(obj.insert(2));   // true
        System.out.println(obj.getRandom()); // 1 ou 2
        System.out.println(obj.remove(1));   // true
        System.out.println(obj.insert(2));   // false
        System.out.println(obj.getRandom()); // 2
    }
}

class RandomizedSet {
    private final List<Integer> lista;          // Armazena valores únicos
    private final Map<Integer, Integer> mapa;   // Mapeia valor para índice na lista
    private final Random random;

    public RandomizedSet() {
        this.lista = new ArrayList<>();
        this.mapa = new HashMap<>();
        this.random = new Random();
    }

    public boolean insert(int val) {
        if (!mapa.containsKey(val)) {
            mapa.put(val, lista.size());
            lista.add(val);
            return true;
        }
        return false;
    }

    public boolean remove(int val) {
        if (mapa.containsKey(val)) {
            int index = mapa.get(val);
            int lastIndex = lista.size() - 1;
            int lastVal = lista.get(lastIndex);

            if (index != lastIndex) {
                // Move o último elemento para a posição do elemento removido
                lista.set(index, lastVal);
                mapa.put(lastVal, index);
            }

            // Remove o último elemento
            mapa.remove(val);
            lista.remove(lastIndex);
            return true;
        }
        return false;
    }

    public int getRandom() {
        if (lista.isEmpty()) {
            return -1;
        }
        int randomIndex = random.nextInt(lista.size());
        return lista.get(randomIndex);
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
#include <iostream>
#include <random>
#include <unordered_map>
#include <utility>
#include <vector>
using namespace std;

class RandomizedSet {
    vector<int> lista;              // Armazena valores únicos
    unordered_map<int, int> mapa;   // Mapeia valor para índice na lista
    mt19937 rng;

public:
    RandomizedSet() : rng(random_device{}()) {}

    bool insert(int val) {
        if (mapa.find(val) == mapa.end()) {
            mapa[val] = (int)lista.size();
            lista.push_back(val);
            return true;
        }
        return false;
    }

    bool remove(int val) {
        auto it = mapa.find(val);
        if (it != mapa.end()) {
            int index = it->second;
            int last_index = (int)lista.size() - 1;
            int last_val = lista[last_index];

            if (index != last_index) {
                // Move o último elemento para a posição do elemento removido
                swap(lista[index], lista[last_index]);
                mapa[last_val] = index;
            }

            // Remove o último elemento
            mapa.erase(val);
            lista.pop_back();
            return true;
        }
        return false;
    }

    int getRandom() {
        if (lista.empty()) {
            return -1;
        }
        uniform_int_distribution<int> dist(0, (int)lista.size() - 1);
        return lista[dist(rng)];
    }
};

int main() {
    RandomizedSet obj;
    cout << boolalpha;
    cout << obj.insert(1) << endl;   // true
    cout << obj.remove(2) << endl;   // false
    cout << obj.insert(2) << endl;   // true
    cout << obj.getRandom() << endl; // 1 ou 2
    cout << obj.remove(1) << endl;   // true
    cout << obj.insert(2) << endl;   // false
    cout << obj.getRandom() << endl; // 2
    return 0;
}
```

</details>

---

> Quer dominar os padrões por trás dessas questões com acompanhamento e trilhas estruturadas? Conheça a **[Missão BigTech](https://missaobigtech.com.br/)**
