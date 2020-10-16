```python
def affine_gap_alignment(first, second, weight_match, weight_mismatch, open_gap_penalty, continue_gap_penalty):
    n = len(first)
    m = len(second)
    matrix = [[0 for x in range(n + 1)] for y in range(m + 1)]
    for x in range(1, m + 1):
        for y in range(1, n + 1):
            matching = (first[y - 1] == second[x - 1]) * weight_match \
                       + (first[y - 1] != second[x - 1]) * weight_mismatch
            # counting score if there was a long gap in second string
            mxSnd = 0
            for k in range(1, x):
                mxSnd = max(mxSnd, matrix[x - k][y] + (open_gap_penalty + k * continue_gap_penalty))
            # counting score if there was a long gap in first string
            mxFst = 0
            for k in range(1, y):
                mxFst = max(mxFst, matrix[x][y - k] + (open_gap_penalty + k * continue_gap_penalty))
            matrix[x][y] = max(matrix[x - 1][y - 1] + matching, mxFst, mxSnd, 0)

    maximum = 0
    i = 0
    j = 0
    for x in range(1, m + 1):
        for y in range(1, n + 1):
            if matrix[x][y] > maximum:
                maximum = matrix[i][j]
                i = x
                j = y
    #     tracing back
    ans1 = ans2 = ""
    x = m
    y = n
    while x != 0 and y != 0:
        while x > i:
            ans1 += "_"
            ans2 += second[x - 1]
            x -= 1
        while y > j:
            ans2 += "_"
            ans1 += first[y - 1]
            y -= 1
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
seq1 = "ABCAB"
seq2 = "BCBB"
affine_gap_alignment(seq1, seq2, 1, -1, 0, -1)
affine_gap_alignment(seq1, seq2, 1, -1, -50, -50)

```

    ABCAB_ _BC_BB
    ABCAB _BCBB



