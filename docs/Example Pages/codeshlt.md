# Code Highlighting

```py title="ex1.py"
import tenserflow as tf
def function()
```

```py linenums="0"
def bubble_sort(items):
    for i in range(len(items)):
       for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

```py linenums="1"
def bubble_sort(items):
    for i in range(len(items)):
       for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

```py linenums="2"
def bubble_sort(items):
    for i in range(len(items)):
       for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```

```py hl_lines="2 3" title="highlight the lines"
def bubble_sort(items):
    for i in range(len(items)):
       for j in range(len(items) - 1 - i):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j + 1], items[j]
```