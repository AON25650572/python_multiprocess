# Python並列処理はじめ
[Pythonの並列処理・並行処理をしっかり調べてみた](https://qiita.com/simonritchie/items/1ce3914eb5444d2157ac#%E4%B8%A6%E5%88%97%E5%87%A6%E7%90%86)


```python
from multiprocessing import Pool
```


```python
def sample_func(process_index):
    print('process index: %s started.' % process_index)
    num = 0

    for i in range(10000000):
        num += 1
    print('process index: %s ended.' % process_index)
```


```python
%%time
for i in range(10):
    sample_func(process_index=i)
```

    process index: 0 started.
    process index: 0 ended.
    process index: 1 started.
    process index: 1 ended.
    process index: 2 started.
    process index: 2 ended.
    process index: 3 started.
    process index: 3 ended.
    process index: 4 started.
    process index: 4 ended.
    process index: 5 started.
    process index: 5 ended.
    process index: 6 started.
    process index: 6 ended.
    process index: 7 started.
    process index: 7 ended.
    process index: 8 started.
    process index: 8 ended.
    process index: 9 started.
    process index: 9 ended.
    CPU times: user 2.62 s, sys: 0 ns, total: 2.62 s
    Wall time: 2.62 s



```python
%%time
def sample_func(initial_num):
    for i in range(1000000):
        initial_num += 1
    return initial_num
```

    CPU times: user 3 µs, sys: 0 ns, total: 3 µs
    Wall time: 3.58 µs



```python
initial_num_list = list(range(1000))
```


```python
%%time
with Pool(processes=4) as p:
    result_list = p.map(func=sample_func, iterable=initial_num_list)
print(result_list[:10])
```

    [1000000, 1000001, 1000002, 1000003, 1000004, 1000005, 1000006, 1000007, 1000008, 1000009]
    CPU times: user 3.55 ms, sys: 12.3 ms, total: 15.8 ms
    Wall time: 6.99 s



```python
%%time
result_list = []
for i in initial_num_list:
    result_list.append(sample_func(i))
print(result_list[:10])
```

    [1000000, 1000001, 1000002, 1000003, 1000004, 1000005, 1000006, 1000007, 1000008, 1000009]
    CPU times: user 26.5 s, sys: 1.13 ms, total: 26.5 s
    Wall time: 26.5 s


```python
import os
os.cpu_count()
```
