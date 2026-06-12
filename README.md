# Лабораторная работа №5: Бинарное дерево поиска (BST)
Выполнила: Матюнина Юлия, ИДБ-25-07


---

Функции:
1. `insert(root, value)` — рекурсивная вставка нового значения в дерево с сохранением свойств BST.
2. `search(root, target)` — эффективный поиск элемента в дереве (возвращает `True` / `False`).
3. `inorder(root)` — симметричный обход дерева в глубину (DFS). Автоматически выводит элементы в порядке возрастания.
4. `find_k_element(root, k)` — поиск $k$-го по величине минимального элемента в дереве.

---

## Сложность алгоритмов

| Операция | В среднем | В худшем случае | Смысл операции |
| :--- | :---: | :---: | :--- |
| **Вставка (`insert`)** | $O(\log n)$ | $O(n)$ | Спуск по нужным веткам до пустого места |
| **Поиск (`search`)** | $O(\log n)$ | $O(n)$ | Отсечение половины поддерева на каждом шаге |
| **Обход (`inorder`)** | $O(n)$ | $O(n)$ | Посещение абсолютно всех узлов дерева |
| **Поиск K-го элемента** | $O(n)$ | $O(n)$ | Построение отсортированного списка за $O(n)$ + доступ за $O(1)$ |

---

## Код
```python
class TreeNode:
    """Класс для описания одного узла дерева."""
    def __init__(self, value):
        self.value = value      
        self.left = None        
        self.right = None       

def insert(root, value):
    """Вставить значение в дерево поиска."""
    if root is None:
        return TreeNode(value)
    
    if value < root.value:
        root.left = insert(root.left, value)
    else:
        root.right = insert(root.right, value)
    return root

def search(root, target):
    """Найти, содержится ли значение в дереве."""
    if root is None:
        return False
    
    if root.value == target:
        return True

    if target < root.value:
        return search(root.left, target)
    else:
        return search(root.right, target)
        
def inorder(root, result=None):
    """Обход в глубину, симметричный (Inorder)."""
    if result is None:
        result = []
    if root:
        inorder(root.left, result)          # 1. левое поддерево (меньшие элементы)
        result.append(root.value)           # 2. текущий узел
        inorder(root.right, result)         # 3. правое поддерево (большие элементы)
    return result

def find_k_element(root, k):
    """Найти k-й по величине элемент (1-й минимум, 2-й минимум и т.д.)."""
    sorted_elements = inorder(root)
    
    if 1 <= k <= len(sorted_elements):
        return sorted_elements[k - 1]
    return None

# Пример дерева
if __name__ == "__main__":
    root = None
    elements = [10, 5, 15, 3, 7, 12, 18]
    for value in elements:
        root = insert(root, value)

    # Структура дерева:
    #         10
    #        /  \
    #       5    15
    #      / \   / \
    #     3   7 12  18

    print("--- Обход в глубину (Depth-First Search) ---")
    sorted_list = inorder(root)
    print(f"Элементы по возрастанию: {sorted_list}") 

    print("\n--- Функция поиска ---")
    print(f"Есть ли в дереве число 7? {search(root, 7)}")    
    print(f"Есть ли в дереве число 99? {search(root, 99)}")  

    print("\n--- Поиск k-го минимума ---")
    k = 3
    kth_min = find_k_element(root, k)
    print(f"{k}-й по величине элемент (минимум) в дереве — это: {kth_min}")
```
---
### Пример вывода
<img width="755" height="282" alt="image" src="https://github.com/user-attachments/assets/da506fc6-12b9-49e4-b923-115db1850703" />
