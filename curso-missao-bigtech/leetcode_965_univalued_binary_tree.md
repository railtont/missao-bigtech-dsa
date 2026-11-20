# LeetCode 965 — Univalued Binary Tree

* **Problema:** [LeetCode 965](https://leetcode.com/problems/univalued-binary-tree/)
* **Referência:** Missão BigTech — Fundamentos de Árvores
* **Dificuldade:** Easy

---

## Visão Geral das Abordagens

| Abordagem | Tempo | Espaço | Característica |
| :--- | :---: | :---: | :--- |
| **1. DFS com Função Auxiliar (`target`)** | $O(N)$ | $O(H)$ | Separa o valor de referência (`root.val`) e mantém a verificação uniforme em todos os nós |
| **2. Recursão Direta na Própria Função** | $O(N)$ | $O(H)$ | Não precisa de função auxiliar; compara `root` diretamente com seus filhos antes de descer |

*(Onde $N$ é o número total de nós e $H$ é a altura da árvore.)*

---

## Solução 1: DFS com Função Auxiliar (`target`)

Como a raiz (`root`) sempre faz parte da árvore (pelas restrições do problema, há pelo menos 1 nó), passamos `root.val` como `target` para uma função auxiliar `dfs(node, target)`:

1. **Caso base:** Se `node` for nulo (`None` / `null` / `nullptr`), retornamos verdadeiro (`True`).
2. **Verificação:** Se `node.val != target`, retornamos `False` imediatamente.
3. **Passo recursivo:** Retornamos verdadeiro somente se a subárvore esquerda **e** a subárvore direita também satisfizerem a condição.

* **Tempo:** $O(N)$, onde $N$ é o número total de nós da árvore (cada nó é visitado no máximo uma vez).
* **Espaço:** $O(H)$, onde $H$ é a altura da árvore devido à pilha de recursão ($O(N)$ no pior caso em uma árvore degenerada e $O(\log N)$ em uma árvore balanceada).

<details open>
<summary><strong>Python</strong></summary>

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def dfs(self, node, target):
        if node is None:
            return True

        if node.val != target:
            return False 

        return self.dfs(node.left, target) and self.dfs(node.right, target)        

    def isUnivalTree(self, root: Optional[TreeNode]) -> bool:
        return self.dfs(root, root.val)
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    private boolean dfs(TreeNode node, int target) {
        if (node == null) {
            return true;
        }

        if (node.val != target) {
            return false;
        }

        return dfs(node.left, target) && dfs(node.right, target);
    }

    public boolean isUnivalTree(TreeNode root) {
        return dfs(root, root.val);
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool dfs(TreeNode* node, int target) {
        if (node == nullptr) {
            return true;
        }

        if (node->val != target) {
            return false;
        }

        return dfs(node->left, target) && dfs(node->right, target);
    }

    bool isUnivalTree(TreeNode* root) {
        return dfs(root, root->val);
    }
};
```

</details>

---

## Solução 2: Recursão Direta na Própria Função (Sem Função Auxiliar)

Em vez de comparar todos os nós com uma única referência fixa da raiz passada como parâmetro (`target`), também funciona comparar diretamente cada **pai com seus filhos**. Pela transitividade da igualdade, se cada nó tem o mesmo valor que seus filhos esquerdo e direito (quando existem), toda a árvore possui o mesmo valor — eliminando a necessidade de uma função auxiliar.

* **Tempo:** $O(N)$, visitando cada nó no máximo uma vez.
* **Espaço:** $O(H)$ devido à pilha de recursão ($O(N)$ no pior caso e $O(\log N)$ em árvores balanceadas).

<details open>
<summary><strong>Python</strong></summary>

```python
class Solution:
    def isUnivalTree(self, root: Optional[TreeNode]) -> bool:
        if root is None:
            return True

        if root.left != None and root.left.val != root.val:
            return False
        if root.right != None and root.right.val != root.val:
            return False

        return self.isUnivalTree(root.left) and self.isUnivalTree(root.right)
```

</details>

<details>
<summary><strong>Java</strong></summary>

```java
class Solution {
    public boolean isUnivalTree(TreeNode root) {
        if (root == null) {
            return true;
        }

        if (root.left != null && root.left.val != root.val) {
            return false;
        }
        if (root.right != null && root.right.val != root.val) {
            return false;
        }

        return isUnivalTree(root.left) && isUnivalTree(root.right);
    }
}
```

</details>

<details>
<summary><strong>C++</strong></summary>

```cpp
class Solution {
public:
    bool isUnivalTree(TreeNode* root) {
        if (root == nullptr) {
            return true;
        }

        if (root->left != nullptr && root->left->val != root->val) {
            return false;
        }
        if (root->right != nullptr && root->right->val != root->val) {
            return false;
        }

        return isUnivalTree(root->left) && isUnivalTree(root->right);
    }
};
```

</details>

---