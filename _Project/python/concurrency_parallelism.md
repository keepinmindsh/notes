# Synchronous Execution 
- Must maintain sequential execution flow. execution must do the next work after finished the previous step.

# Asynchronous Execution 
- Execution for proceeding to the next step even if the previous step is not completed
  - To perform asynchronous execution in Python, you must use a built-in library called asynsio, which utilizes coroutines.

## Code Sample

```python
import aiohttp 
import asyncio 
import time
import os 
import threading 

async def fetch(session, url): 
  print(f"PID: {os.getpid()} | Thread: {threading.get_ident()} | url: {url}")
  async with session.get(url) as response: 
      return await response.text() 

async def main():
  urls = ["https://www.naver.com", "https://www.google.com", "https://daum.net" ] * 10 

  async with aiohttp.ClientSession() as session"
    _ = await asyncio.gather(*[fetch(session, url) for url in urls])

if __name__ == "__main__":
  start = time.time()
  asyncio.run(main())
  print("elapsed-time:", time.time() - start)
```

- This task of sending a request to an external URL is called a type of I/O task, and if most of these I/O tasks are I/O Bound, they are also called I/O Bound.
- Asynchronous execution in Python provides great performance in I/O-bound tasks like this.

# Concurrency 

- Concurrency means handling multiple tasks not at the same time, but this 'handling concurrently' means handling multiple tasks not at the same time while switching between them.
  - usually use for I/O Bound 
  - code sample of concurrency is same with [Asynchronous Execution's code samples](#code-sample) 

# Parallelism 

- The ability to perform multiple tasks at the sample time
  - usually use for CPU Bound 

```python
import os 
import threading 
import time 
import concurrent.futures import ThreadPoolExecutor 

def calculate(n):
  print(f"{os.getpid()} process | {threading.get_ident()} thread | n: {n}")
  total = 0 
  for i in range(n):
    for j in range(n):
      for k in range(n):
        total += i * j * k 
  return total

def main(nums):
  with ThreadPoolExecutor(max_workers=10) as executor:
    results = list(executor.map(calculate, nums))
    print(results)

if __name__ == "__main__":
  start = time.time() 
  main([300] * 10)
  print("elased-time:", time.time() - start) 
```

# 동시성과 병렬성에 대한 나의 이해 

- 사람은 커피를 마시면서 음악을 듣고, 신문을 동시에 볼 수 있지만 이 행동이 모두 동시에 발생하는 것이 아닌 내부적으로 뇌가 각각의 상황을 받아들이는 시점이 존재한다. 이때 발생하는 것이 "동시성" 이다.
  - 동시성이 발생할 때는 처리 작업이 오래 걸리는 일을 혼자하면서 다른 추가 업무를 받게 된다면 처리시간에 지연이 발생할 수 밖에 없다. 완료시간이 늦어진다. 
  - 동시성의 관점에서 한사람이 여러업무를 다른 사람에게 각각 시켜서 각각이 결과를 비순차적으로 받더라도 받은 결과를 순차적으로 처리하게 되고, 각각의 업무는 개별 인원이 처리하므로 최종적인 업무 처리 결과는 당연히 빨라진다.
- 사람이 일을 할 때 문서 작업을 동시에 2명이서 진행한다면 동일한 시간에 문서의 수가 2배가 될 수 있다. 이때 발생하는 것이 "병렬성" 이다
  - 병렬성이 발생할 때는 2명이 업무를 동시에 처리하기 때문에 다른 추가 업무에 대한 처리 완료시간이 빨라진다. 
  - 병렬성은 동시성에 의해서 한 사람이 각각 다른 사람에거 업무를 하달했을때 각각의 사람이 처리하는 업무는 병렬적으로 처리하게 된다. 


