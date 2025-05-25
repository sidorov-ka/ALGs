# Конспект по базовым алгоритмам сортировки

## Сортировка выбором (Selection Sort)

### Теория:
Сортировка выбором — базовый алгоритм сортировки. Ищем минимум на неотсортированной части массива и ставим его в начало. Повторяем n-1 раз.
Алгоритм можно реализовать устойчиво или неустойчиво в зависимости от реализации. Сложность — O(n^2) в худшем, лучшем и среднем случае.

### Код:
```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n - 1):
        min_idx = i
        for j in range(i + 1, n):
            if arr[j] < arr[min_idx]:
                min_idx = j
        arr[i], arr[min_idx] = arr[min_idx], arr[i]
    return arr
```

## Сортировка вставками (Insertion Sort)

### Теория:
Сортировка вставками — стабильная, онлайн-сортировка. Последовательно берём элементы и вставляем их на нужное место в отсортированной части массива. В худшем случае O(n^2), в лучшем (почти отсортированный массив) — O(n).

### Код:
```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
    return arr
```

## Сортировка пузырьком (Bubble Sort) — базовая

### Теория:
Сравниваем соседние элементы и меняем местами, если стоят неправильно. Самый тяжёлый элемент 'всплывает' в конец. Повторяем n-1 раз. Сложность: O(n^2).

### Код:
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n - 1):
        for j in range(n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
    return arr
```

## Сортировка пузырьком — с оптимизацией по флагу

### Теория:
Добавляется флаг `swapped`. Если за проход не было ни одного обмена — массив отсортирован, выходим. Это улучшает работу на почти отсортированных данных. В лучшем случае: O(n).

### Код:
```python
def bubble_sort_optimized(arr):
    n = len(arr)
    for i in range(n - 1):
        swapped = False
        for j in range(n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped:
            break
    return arr
```

## Сортировка пузырьком — с оптимизацией по индексу последнего обмена

### Теория:
Запоминаем индекс последнего обмена и сокращаем внутренний цикл. Это снижает количество сравнений, но не изменяет асимптотику — O(n^2).

### Код:
```python
def bubble_sort_advanced(arr):
    n = len(arr)
    while n > 1:
        new_n = 0
        for i in range(1, n):
            if arr[i - 1] > arr[i]:
                arr[i - 1], arr[i] = arr[i], arr[i - 1]
                new_n = i
        n = new_n
    return arr
```
