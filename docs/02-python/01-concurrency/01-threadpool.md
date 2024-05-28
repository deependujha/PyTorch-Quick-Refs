# ThreadPool python concurrency

if you use concurrent.futures.as_completed, you can handle the exception for each function.

```python
import concurrent.futures
iterable = [1,2,3,4,6,7,8,9,10]

def f(x):
    if x == 2:
        raise Exception('x')
    return x

with concurrent.futures.ThreadPoolExecutor(max_workers=4) as executor:
    result_futures = list(map(lambda x: executor.submit(f, x), iterable))
    # -> using `executor.submit()` **requires** calling
    #      `concurrent.futures.as_completed()` <-
    #
    for future in concurrent.futures.as_completed(result_futures):
        try:
            print('result is', future.result())
        except Exception as e:
            print('e is', e, type(e))
# result is 3
# result is 1
# result is 4
# e is x <class 'Exception'>
# result is 6
# result is 7
# result is 8
# result is 9
# result is 10
```

output:

```md
[6, 5, 4, 3, 2, 1]
start submit 1543473934.47
submitted 1543473934.47
(5, 1543473939.473743) 1543473939.47
(6, 1543473940.471591) 1543473940.47
(3, 1543473943.473639) 1543473943.47
(4, 1543473943.474192) 1543473943.47
(1, 1543473944.474617) 1543473944.47
(2, 1543473945.477609) 1543473945.48

start map 1543473945.48
mapped 1543473945.48
(6, 1543473951.483908) 1543473951.48
(5, 1543473950.484109) 1543473951.48
(4, 1543473954.48858) 1543473954.49
(3, 1543473954.488384) 1543473954.49
(2, 1543473956.493789) 1543473956.49
(1, 1543473955.493888) 1543473956.49
```
