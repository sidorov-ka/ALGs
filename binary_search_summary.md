
# 📘 Конспект по теме: Алгоритмы на строках

---

## 1. 📚 Основы

**Строка** — это массив символов. В контексте алгоритмов мы рассматриваем строку как неизменяемую последовательность символов.

### Подстрока
- Это непрерывная последовательность символов в строке.
- Обозначение: `s[i:j]`.

### Префикс и суффикс
- **Префикс** — начало строки `s[:k]`.
- **Суффикс** — конец строки `s[-k:]`.
- **Собственный префикс/суффикс** — не равен всей строке и не пустой.

### Грань строки
- Грань — собственный префикс, совпадающий с суффиксом.

---

## 2. 📏 Количество подстрок

- Подстрок длины `k` — `n - k + 1`
- Всего подстрок:
\[
\sum_{k=1}^{n} (n - k + 1) = \frac{n(n + 1)}{2}
\]
- Асимптотика: **O(n²)**

---

## 3. 🔠 Сравнение строк

- Лексикографическое сравнение — посимвольное (как в словаре).
- Пример: `'100' < '21'` (потому что `'1' < '2'`)

---

## 4. 🔍 Поиск подстроки

### Наивный алгоритм

```python
for i in range(len(T) - len(P) + 1):
    if T[i:i+len(P)] == P:
        print(i)
```

- Сложность: **O(n·m)** — перебираем все возможные позиции

---

## 5. ⚡ Префикс-функция (π-функция)

- `π[i]` — длина максимального собственного префикса в `s[:i+1]`, совпадающего с суффиксом.
- Служит для быстрого определения повторяющихся паттернов.

### Построение π-функции

```python
def prefix_function(s):
    n = len(s)
    pi = [0] * n
    for i in range(1, n):
        j = pi[i - 1]
        while j > 0 and s[i] != s[j]:
            j = pi[j - 1]
        if s[i] == s[j]:
            j += 1
        pi[i] = j
    return pi
```

- Сложность: **O(n)**

---

## 6. 🧩 KMP-алгоритм (Knuth–Morris–Pratt)

- Использует π-функцию для ускоренного поиска подстроки в строке.

### Идея:
- Строим `π` для паттерна `P`
- Пробегаем по строке `T`, при несовпадении — сдвигаемся по `π`

### Поиск всех вхождений:

```python
def kmp_search(pattern, text):
    combined = pattern + '#' + text
    pi = prefix_function(combined)
    result = []
    for i in range(len(pattern)+1, len(combined)):
        if pi[i] == len(pattern):
            result.append(i - 2 * len(pattern))
    return result
```

- Сложность: **O(n + m)**

---

## 7. 🧠 Грани и их свойства

- Если `π[n-1] = k`, то `s[:k]` — граница.
- Все границы можно получить через переходы по `π[k-1]`.

---

## 8. ⏱️ Асимптотики

| Алгоритм            | Сложность     |
|---------------------|---------------|
| Перебор всех подстр | O(n²)         |
| Наивный поиск       | O(n·m)        |
| Построение π-функц. | O(n)          |
| KMP                 | O(n + m)      |

---

## 9. 📌 Выводы

- Префикс-функция — важнейший инструмент анализа строк.
- Алгоритм KMP позволяет искать подстроку за линейное время.
- Понимание границ и повторов в строках открывает путь к эффективным строковым алгоритмам.

---

## 🧮 Алгоритм Левенштейна (редакционное расстояние)

**Цель:** найти минимальное количество операций (вставка, удаление, замена), необходимых для преобразования строки `a` в строку `b`.

### 🔧 Динамическое программирование

Обозначим:
- `dp[i][j]` — минимальное расстояние между `a[:i]` и `b[:j]`.

Базовые случаи:
- `dp[0][j] = j` (вставить все символы `b`)
- `dp[i][0] = i` (удалить все символы `a`)

Рекуррентное соотношение:
```
dp[i][j] = min(
    dp[i-1][j] + 1,               # удаление
    dp[i][j-1] + 1,               # вставка
    dp[i-1][j-1] + cost           # замена (если a[i-1] != b[j-1])
)
```

### ⚡ Оптимизация по памяти

Для расчёта можно хранить только **две строки** `prev`, `curr`, переходя по ним, что даёт память `O(min(n, m))`.

### 🧪 Примеры

```python
levenshtein("sunday", "saturday")  # → 3
levenshtein("cat", "cats")         # → 1
levenshtein("kitten", "sitting")   # → 3
```

### 🧠 Сложность

- Время: `O(n * m)`
- Память: `O(min(n, m))` при оптимизации

### 💻 Реализация

```python
def levenshtein(a, b):
    if len(a) < len(b):
        a, b = b, a

    prev = list(range(len(b) + 1))
    curr = [0] * (len(b) + 1)

    for i in range(1, len(a) + 1):
        curr[0] = i
        for j in range(1, len(b) + 1):
            cost = 0 if a[i - 1] == b[j - 1] else 1
            curr[j] = min(
                curr[j - 1] + 1,      # вставка
                prev[j] + 1,          # удаление
                prev[j - 1] + cost    # замена
            )
        prev, curr = curr, prev

    return prev[len(b)]
```
