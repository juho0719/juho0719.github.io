---
layout: post
title:  "Python Package / Exception / inoutfunc"
date:   2017-06-23 22:00:00 +0900
categories: Python
---

패키지는 .으로 구분

```python
>>> import game.sound.echo
>>> game.sound.echo.echo_test()
echo

>>> from game.sound import echo
>>> echo.echo_test()
echo

# import game을 수행하면 game 디렉터리의 모듈 또는 game 디렉터리의 __init__.py에 정의된 것들만 참조할 수 있음 (3.3이상에서는 없어도 인식)
>>> import game
>>> game.sound.echo.echo_test()
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
AttributeError: 'module' object has no attribute 'sound'
```

3.3이하 버전에서는 __init__.py 이 있어야 패키지로 인식

import *은 패키지가 최하위모듈이어야 별다른 조치없이 가능
해당 패키지 하위의 모듈이 여러개라면 __init__.py에 __all__ = ['모듈명']의 형태로 지정해줘야 함

```python
>>> from game.sound import *
>>> echo.echo_test()
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
NameError: name 'echo' is not defined

# C:/Python/game/sound/__init__.py
__all__ = ['echo']
```

패키지를 상대경로로 지정 가능
.. 부모
. 자신

```python
from ..sound.echo import echo_test
```


예외처리
else : 예외처리가 일어나지 않은 경우 무조건 수행 (반드시 except뒤에 기술해야 함)
finally : 정상/예외 구분없이 무조건 수행
pass : 예외 발생시 아무처리 없이 그냥 넘김
raise : 강제 예외 발생
예외 as 변수명 : 예외처리내용을 변수에 담음

```python
class MyError(Exception):
    def __init__(self, msg):
        self.msg = msg

    def __str__(self):
        return self.msg


def say_nick(nick):
    if nick == '바보':
        raise MyError("허용되지 않는 별명입니다.")
    print(nick)

try:
    say_nick("천사")
    say_nick("바보")
except MyError as e:
    print(e)
else:
    print("성공")
finally:
    print("완료")
```


내장함수

dir : 객체 자체가 가지고있는 함수들을 보여줌

```python
>>> dir([1, 2, 3])
['append', 'count', 'extend', 'index', 'insert', 'pop',...]
>>> dir({'1':'a'})
['clear', 'copy', 'get', 'has_key', 'items', 'keys',...]
```

lambda : 간단한 함수를 생성할 때 사용하는 예약어. 

```python
>>> sum = lambda a, b: a+b
>>> sum(3,4)
7

>>> myList = [lambda a,b:a+b, lambda a,b:a*b]
>>> myList[0](3,4)
7

```

ord : 아스키 코드 값 리턴
chr : 문자 리턴

range : 범위 지정

```python
>>> list(range(5))
[0, 1, 2, 3, 4]

>>> list(range(5, 10))
[5, 6, 7, 8, 9]

>>> list(range(1, 10, 2))
[1, 3, 5, 7, 9]
>>> list(range(0, -10, -1))
[0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
```



외장 함수

sys
파이썬 인터프리터가 제공하는 변수들이나 함수들을 제공

sys.argv : 실행시 넣은 인자값들을 가져옴

```python
import sys
print(sys.argv)
```

sys.exit : 스크립트 강제 종료

```python
>>> sys.exit()
```

sys.path : 파이썬 모듈들이 저장되어 있는 위치

```python
>>> import sys
>>> sys.path
['', 'C:\\Windows\\SYSTEM32\\python35.zip', 'c:\\Python35\\DLLs', 
'c:\\Python35\\lib', 'c:\\Python35', 'c:\\Python35\\lib\\site-packages']
>>>

# 모듈 추가시
import sys
sys.path.append("C:/Python/Mymodules")
```



pickle
객체를 파일에 저장

```python
>>> import pickle
>>> f = open("test.txt", 'wb')
>>> data = {1: 'python', 2: 'you need'}
>>> pickle.dump(data, f)
>>> f.close()

>>> import pickle
>>> f = open("test.txt", 'rb')
>>> data = pickle.load(f)
>>> print(data)
{2:'you need', 1:'python'}
```




os모듈

os.environ : 내 시스템의 환경변수
os.chdir : 현재 디렉토리 위치 변경
os.getcwd : 자신의 디렉토리 위치 리턴
os.system : 시스템 명령어 호출
os.popen : 실행한 시스템 명령어의 리턴
os.mkdir : 디렉토리 생성
os.rmdir : 디렉토리 삭제
os.unlink : 파일 삭제
os.rename(src, dst) : 파일명 변경
os.walk : 하위 디렉토리까지 겁색

```python
>>> import os
>>> os.environ
environ({'PROGRAMFILES': 'C:\\Program Files', 'APPDATA': … 생략 …})
>>> os.environ['PATH']
'C:\\ProgramData\\Oracle\\Java\\javapath;...생략...'

>>> os.chdir("C:\WINDOWS")

>>> os.getcwd()
'C:\WINDOWS'

>>> os.system("dir")

>>> f = os.popen("dir")

import os

for (path, dir, files) in os.walk("c:/"):
    for filename in files:
        ext = os.path.splitext(filename)[-1]
        if ext == '.py':
            print("%s/%s" % (path, filename))
```



shutil
파일 복사 모듈

```python
>>> import shutil
>>> shutil.copy("src.txt", "dst.txt")
```



glob
특정 디렉토리에 있는 파일 목록

```python
>>> import glob
>>> glob.glob("C:/Python/q*")
['C:\Python\quiz.py', 'C:\Python\quiz.py.bak']
>>>
```



tempfile
임시 파일 생성

```python
>>> import tempfile
>>> filename = tempfile.mktemp()
>>> filename
'C:\WINDOWS\TEMP\~-275151-0'

# 객체로 생성
>>> import tempfile
>>> f = tempfile.TemporaryFile()
>>> f.close()
```


random
난수 생성

```python
import random
def random_pop(data):
    number = random.choice(data)
    data.remove(number)
    return number

if __name__ == "__main__":
    data = [1, 2, 3, 4, 5]
while data: print(random_pop(data))
결과값:
2 
3 
1 
5 
4

>>> import random
>>> data = [1, 2, 3, 4, 5]
>>> random.shuffle(data)
>>> data
[5, 1, 3, 4, 2]
>>>
```



threading
스레드 모듈

```python
import threading
import time

def say(msg):
    while True:
        time.sleep(1)
        print(msg)

for msg in ['you', 'need', 'python']:
    t = threading.Thread(target=say, args=(msg,))
    t.daemon = True
    t.start()

for i in range(100):
    time.sleep(0.1)
    print(i)


import threading
import time

class MyThread(threading.Thread):
    def __init__(self, msg):
        threading.Thread.__init__(self)
        self.msg = msg
        self.daemon = True

    def run(self):
        while True:
            time.sleep(1)
            print(self.msg)

for msg in ['you', 'need', 'python']:
    t = MyThread(msg)
    t.start()

for i in range(100):
    time.sleep(0.1)
    print(i)
```

