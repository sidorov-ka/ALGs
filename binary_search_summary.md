# Конспект по бинарному поиску и смежным темам

## 1. Линейный поиск

### Теория:
Линейный поиск — простейший способ поиска элемента в массиве. Мы просто перебираем каждый элемент и сравниваем его с искомым значением. Асимптотика:
- Худший случай: O(n)
- Лучший случай: O(1)

### Код:
```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i
    return -1
```

---

## 2. Бинарный поиск (базовая реализация)

### Теория:
Бинарный поиск работает только на отсортированных массивах. Идея — делить массив пополам и сужать область поиска в зависимости от сравнения. Асимптотика: **O(log n)**.

### Код:
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1
    while left <= right:
        mid = (left + right) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1
```

---

## 3. Левый и правый бинарный поиск

### Теория:
- **Левый** (lower_bound): минимальный индекс, при котором `arr[i] >= target`
- **Правый** (upper_bound): минимальный индекс, при котором `arr[i] > target`

### Код:
```python
def lower_bound(arr, target):
    left, right = 0, len(arr)
    while left < right:
        mid = (left + right) // 2
        if arr[mid] < target:
            left = mid + 1
        else:
            right = mid
    return left

def upper_bound(arr, target):
    left, right = 0, len(arr)
    while left < right:
        mid = (left + right) // 2
        if arr[mid] <= target:
            left = mid + 1
        else:
            right = mid
    return left
```

---

## 4. Модуль `bisect`

### Теория:
Модуль `bisect` предоставляет готовые реализации бинарного поиска:
- `bisect_left`: аналог lower_bound
- `bisect_right`: аналог upper_bound

### Код:
```python
import bisect

arr = [1, 2, 4, 4, 5]
target = 4

left_index = bisect.bisect_left(arr, target)
right_index = bisect.bisect_right(arr, target)
```

---

## 5. Бинарный поиск по ответу — задача про коров

### Теория:
Дана задача: расставить K коров в N стойлах так, чтобы минимальное расстояние между ними было как можно больше. Неизвестный ответ (расстояние) подбирается бинарным поиском.

### Код:
```python
def can_place_cows(stalls, k, min_dist):
    count, last = 1, stalls[0]
    for pos in stalls[1:]:
        if pos - last >= min_dist:
            count += 1
            last = pos
    return count >= k

def max_min_distance(stalls, k):
    stalls.sort()
    left, right = 1, stalls[-1] - stalls[0]
    best = 0
    while left <= right:
        mid = (left + right) // 2
        if can_place_cows(stalls, k, mid):
            best = mid
            left = mid + 1
        else:
            right = mid - 1
    return best
```
---

## 6. Бинарный поиск по ответу — размещение дипломов

### Теория:
Нужно найти минимальный размер доски `n x n`, в которую влезет `k` дипломов размером `w x h`. Решение — бинарный поиск по возможным размерам доски.

### Код:
```python
def min_board_size(w, h, n):
    left, right = 1, max(w, h) * n
    while left < right:
        mid = (left + right) // 2
        if (mid // w) * (mid // h) >= n:
            right = mid
        else:
            left = mid + 1
    return left
```

---

## 7. Вещественный бинарный поиск

### Теория:
Бинарный поиск может применяться к монотонным функциям на вещественных числах. Решение продолжается до заданной точности `eps`.

### Код:
```python
def f(x):
    return x * x

def binary_search_real(target, eps=1e-6):
    left, right = 0, max(1, target)
    while right - left > eps:
        mid = (left + right) / 2
        if f(mid) < target:
            left = mid
        else:
            right = mid
    return left
```

---

## 8. Проверка, является ли число полным квадратом (`isPerfectSquare`)

### Теория:
Задача: проверить, является ли заданное число `n` точным квадратом. Запрещено использовать `sqrt`. Решается бинарным поиском по возможным корням.

### Код:
```python
def isPerfectSquare(num):
    left, right = 1, num
    while left <= right:
        mid = (left + right) // 2
        if mid * mid == num:
            return True
        elif mid * mid < num:
            left = mid + 1
        else:
            right = mid - 1
    return False
```

---

## 9. Поиск минимума в сдвинутом отсортированном массиве

### Теория:
Массив был отсортирован, но сдвинут циклически. Нужно найти минимум. Решается бинарным поиском — сравниваем `mid` с `right`.

### Код:
```python
def find_min_in_rotated(arr):
    left, right = 0, len(arr) - 1
    while left < right:
        mid = (left + right) // 2
        if arr[mid] > arr[right]:
            left = mid + 1
        else:
            right = mid
    return arr[left]
```
