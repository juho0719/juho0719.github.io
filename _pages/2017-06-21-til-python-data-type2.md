리스트 연산자 가능

```python
>>> a = [1, 2, 3]
>>> b = [4, 5, 6]
>>> a + b
[1, 2, 3, 4, 5, 6]
>>> a * 3
[1, 2, 3, 1, 2, 3, 1, 2, 3]
>>> a[1:2] = ['a', 'b', 'c']
>>> a
[1, 'a', 'b', 'c', 4]
>>> a[1:3] = [ ]
>>> a
[1, 'c', 4]
>>> a
[1, 'c', 4]
>>> del a[1]
>>> a
[1, 4]
```

타입이 다를경우 오류

```python
>>>a = [1, 2, 3]
>>>a[2] + "hi"
Traceback (innermost last):
File "", line 1, in ?
a[2] + "hi"
TypeError: number coercion failed
```

리스트 관련 함수들

```python
>>> a = [1, 2, 3]
>>> a.append(4)
>>> a
[1, 2, 3, 4]

>>> a = [1, 4, 3, 2]
>>> a.sort()
>>> a
[1, 2, 3, 4]

>>> a = ['a', 'c', 'b']
>>> a.reverse()
>>> a
['b', 'c', 'a']

>>> a = [1,2,3]
>>> a.index(3)
2
>>> a.index(1)
0

>>> a = [1, 2, 3]
>>> a.insert(0, 4)
[4, 1, 2, 3]

>>> a = [1, 2, 3, 1, 2, 3]
>>> a.remove(3)
[1, 2, 1, 2, 3]

>>> a = [1,2,3]
>>> a.pop()
3
>>> a
[1, 2]

>>> a = [1,2,3,1]
>>> a.count(1)
2

>>> a = [1,2,3]
>>> a.extend([4,5])
>>> a
[1, 2, 3, 4, 5]
```

튜플(tuple)은 리스트와 거의 비슷

리스트 : [ ]
튜플 :( )

튜플은 그 값을 바꿀 수 없다.

```python
>>> t1 = ()
>>> t2 = (1,)
>>> t3 = (1, 2, 3)
>>> t4 = 1, 2, 3
>>> t5 = ('a', 'b', ('ab', 'cd'))
```

딕셔너리 = 맵형태 (Key=Vaule)

```python
>>> dic = {'name':'pey', 'phone':'0119993323', 'birth': '1118'}

>>> a = {1: 'a'}
>>> a[2] = 'b'
>>> a
{2: 'b', 1: 'a'}
>>> a['name'] = 'pey'
{'name':'pey', 2: 'b', 1: 'a'}
>>> del a[1]
>>> a
{'name': 'pey', 3: [1, 2, 3], 2: 'b'}
```

중복된 키값을 넣으면 랜덤하게 하나만 제외하고 모두 무시
딕셔너리는 키값으로 리스트 사용x

딕셔너리 관련 함수들

```python
>>> a = {'name': 'pey', 'phone': '0119993323', 'birth': '1118'}
>>> a.keys()
dict_keys(['name', 'phone', 'birth'])

>>> for k in a.keys():
...    print(k)
...
phone
birth
name

>>> a.values()
dict_values(['pey', '0119993323', '1118'])

>>> a.items()
dict_items([('name', 'pey'), ('phone', '0119993323'), ('birth', '1118')])

>>> a.clear()
>>> a
{}

>>> a = {'name':'pey', 'phone':'0119993323', 'birth': '1118'}
>>> a.get('name')
'pey'
>>> a.get('phone')
'0119993323'

a['nokey'] 과 a.get('nokey')의 차이는
a['nokey'] : None
a.get('nokey') : 에러

>>> a = {'name':'pey', 'phone':'0119993323', 'birth': '1118'}
>>> 'name' in a
True
>>> 'email' in a
False
```

집합자료형 : 순서x, 중복x
따라서 인덱싱 불가능

```python
>>> s2 = set("Hello")
>>> s2
{'e', 'l', 'o', 'H'}
```

인덱싱하려면 list나 튜플로 변환해서 사용

```python
>>> s1 = set([1,2,3])
>>> l1 = list(s1)
>>> l1
[1, 2, 3]
>>> l1[0]
1
>>> t1 = tuple(s1)
>>> t1
(1, 2, 3)
>>> t1[0]
1
```

집합은 아래와 같이 표현한다.
교집합 : & (intersaction)
합집합 : | (union)
차집합 : - (difference)

값 1개 추가 : add
값 2개 추가 : update
값 제거 : remove

```python
>>> s1 = set([1, 2, 3, 4, 5, 6])
>>> s2 = set([4, 5, 6, 7, 8, 9])
>>> s1 & s2
{4, 5, 6}
>>> s1.intersection(s2)
{4, 5, 6}
>>> s1 | s2
{1, 2, 3, 4, 5, 6, 7, 8, 9}
>>> s1.union(s2)
{1, 2, 3, 4, 5, 6, 7, 8, 9}
>>> s1 - s2
{1, 2, 3}
>>> s2 - s1
{8, 9, 7}
>>> s1.difference(s2)
{1, 2, 3}
>>> s2.difference(s1)
{8, 9, 7}

>>> s1 = set([1, 2, 3])
>>> s1.add(4)
>>> s1
{1, 2, 3, 4}

>>> s1 = set([1, 2, 3])
>>> s1.update([4, 5, 6])
>>> s1
{1, 2, 3, 4, 5, 6}

>>> s1 = set([1, 2, 3])
>>> s1.remove(2)
>>> s1
{1, 3}
```

변수매핑시 =의 경우 상수매핑이 없음. 상수 객체를 매핑 (call by reference)

```python
>>> a = 3
>>> b = 3
>>> a is b
True

>>> import sys
>>> sys.getrefcount(3)
30
>>> a = 3
>>> sys.getrefcount(3)
31
>>> b = 3
>>> sys.getrefcount(3)
32
```

call by value를 하려면 :나 copy() 사용

```python
>>> a = [1, 2, 3]
>>> b = a[:]
>>> a[1] = 4
>>> a
[1, 4, 3]
>>> b
[1, 2, 3]

>>> from copy import copy
>>> b = copy(a)
```

