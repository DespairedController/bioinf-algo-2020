```python
def needleman_wunsch(first, second, weights):
    n = len(first)
    m = len(second)

    matrix = [[0 for x in range(n + 5)] for y in range(m + 5)]

    for x in range(1, n + 1):
        matrix[0][x] = weights[first[x-1]]["_"] + matrix[0][ x - 1]

    for x in range(1, m + 1):
        matrix[x][0] = weights["_"][second[x - 1]] + matrix[x - 1][0]

    for x in range(1, m + 1):
        for y in range(1, n + 1):
            matrix[x][y] = max(matrix[x - 1][y - 1] + weights[first[y - 1]][second[x - 1]],
                               matrix[x - 1][y] + weights[first[y - 1]]["_"],
                               matrix[x][y - 1] + weights["_"][second[x - 1]])
    x = m
    y = n
    ans1 = ans2 = ""
    while x != 0 and y != 0:
        mx = matrix[x - 1][y - 1]
        cur1 = first[y - 1]
        cur2 = second[x - 1]
        new_x = x - 1
        new_y = y - 1
        if matrix[x - 1][y] > mx:
            cur1 = "_"
            mx = matrix[x - 1][y]
            new_y = y
        if matrix[x][y - 1] > mx:
            cur1 = first[y - 1]
            cur2 = "_"
            new_x = x
            new_y = y - 1
        ans1 += cur1
        ans2 += cur2
        x = new_x
        y = new_y
    while x > 0:
        ans1 += "_"
        ans2 += second[x - 1]
        x -= 1
    while y > 0:
        ans2 += "_"
        ans1 += first[y - 1]
        y -= 1
    print((ans1[::-1]) + " " + (ans2[::-1]))

```


```python
example = {"A": {"A": 5, "B": -5, "C": -2, "_": -50}, "B": {"A": -5, "B": 50, "C": -5, "_": -5},
           "C": {"A": -5, "B": -5, "C": 1, "_": -1}, "_": {"A": -5, "B": -5, "C": -1}}
first = "ABCAB"
second = "BC"
needleman_wunsch(first, second, example)
```

    ABCAB B_C__



```python
example["A"]["_"] = -5
needleman_wunsch(first, second, example)
```

    ABCAB _BC__


Уменьшение штрафа за gap вместо "A" повлекло за собой изменение выравнивания
