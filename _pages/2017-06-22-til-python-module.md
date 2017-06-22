모듈 만들고 부르기

```python
# mod1.py
def sum(a, b):
    return a + b

>>> import mod1
>>> print(mod1.sum(3,4))
7
```

모듈명.함수명으로 쓰지 않고 함수명만 쓰고 싶은 경우

```python
>>> from mod1 import sum
>>> sum(3, 4)
7

from mod1 import sum, safe_sum    #함수여러개
from mod1 import *                #모든함수
```

if __name__ == "__main__": 는 해당 파일을 모듈로 참조하는 게 아닌 직접 실행할 때만 수행하고 싶을 때 사용

```python
# mod1.py 
def sum(a, b): 
    return a+b

def safe_sum(a, b): 
    if type(a) != type(b): 
        print("더할수 있는 것이 아닙니다.")
        return 
    else: 
        result = sum(a, b) 
    return result 

if __name__ == "__main__":
    print(safe_sum('a', 1))
    print(safe_sum(1, 4))
    print(sum(10, 10.4))
```

모듈을 실행하는 또 다른 방법 1

```python
>>> import sys
>>> sys.path.append("C:/Python/Mymodules")
>>> sys.path
['', 'C:\\Windows\\SYSTEM32\\python35.zip', 'c:\\Python35\\DLLs', 
'c:\\Python35\\lib', 'c:\\Python35', 'c:\\Python35\\lib\\site-packages', 
'C:/Python/Mymodules']
>>>
```

모듈을 실행하는 또 다른 방법 2

```python
C:\Users\home>set PYTHONPATH=C:\Python\Mymodules
C:\Users\home>python
Python 3.5.1 (v3.5.1:37a07cee5969, Dec 6 2015, 01:54:25) [MSC v.1900 64 bit (AM...
Type "help", "copyright", "credits" or "license" for more information.
>>> import mod2
>>> print(mod2.sum(3,4))
7

```