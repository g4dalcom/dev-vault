---
keyword : Python
class : Grammer, Algorithm
---

# sorted() 내장 함수

파이썬에서 정렬을 할 때 가장 부담없이 사용할 수 있는 방법은 내장된 `sorted()` 함수를 이용하는 것입니다.

`sorted()` 내장 함수는 파이썬에서 순회가 가능한(iterable) 객체를 인자로 받아 데이터를 정렬해줄 수 있습니다.

```python
>>> sorted([3, 5, 2, 1, 4])
[1, 2, 3, 4, 5]
>>> sorted(["D", "A", "C", "B", "E"])
['A', 'B', 'C', 'D', 'E']
```

`sorted()` 내장 함수는 인자로 넘어온 객체의 원래 순서를 건드리지 않고 정렬된 원소들을 새로운 객체에 담아서 반환해줍니다.

```python
>>> nums = [3, 5, 2, 1, 4]
>>> sorted_nums = sorted(nums)
>>> print(nums)
[3, 5, 2, 1, 4]
>>> print(sorted_nums)
[1, 2, 3, 4, 5]
```

파이썬에서는 리스트(list) 뿐만 아니라, 터플(tuple), 세트(set), 문자열(string)과 같은 다른 자료형도 순회가 가능하죠? 따라서 이러한 자료형의 인자와 함께 `sorted()` 함수를 호출할 수 있으며 이 때 주의할 점은 입력 자료형과 무방하게 항상 리스트가 반환된다는 것입니다.

```python
>>> sorted((3, 5, 2, 1, 4))
[1, 2, 3, 4, 5]
>>> sorted({3, 5, 2, 1, 4})
[1, 2, 3, 4, 5]
>>> sorted("35214")
['1', '2', '3', '4', '5']
```


문자열이 인자로 넘어왔을 때는 각 문자를 마치 리스트의 원소처럼 다루어 정렬 후에 새로운 리스트에 담아줍니다.

이렇게 리스트 대신에 다시 문자열의 형태로 결과를 얻고 싶다면 문자열의 `join()` 함수를 사용하면 됩니다.

```python
>>> ''.join(sorted("35214"))
'12345'
```

# 내림차순으로 정렬하기

`sorted()` 내장 함수는 기본적으로 작은 데이터에서 큰 데이터 순으로, 즉 오름차순으로 정렬해줍니다.

`sorted()` 함수를 호출할 때 `reverse` 옵션을 사용하면 정말 간단하게 내림차순으로 정렬을 할 수가 있는데요. `reverse` 옵션을 `True`로 설정해주면 큰 데이터에서 작은 데이터순으로, 즉 내림차순 정렬됩니다.
이 말인즉슨, `reverse` 옵션의 기본값은 `False`라는 얘기가 되겠네요.

```python
>>> sorted([3, 5, 2, 1, 4], reverse=True)
[5, 4, 3, 2, 1]

>>> sorted([3, 5, 2, 1, 4], reverse=False)
[1, 2, 3, 4, 5]
```


# 정렬 기준 지정하기

숫자나 문자가 아닌 사전이나 클래스의 객체와 같이 좀 더 복잡한 구조의 데이터를 정렬할 때는 특별한 기준이 필요합니다. 이러한 데이터는 이떤 방식으로 데이터 간에 대소비교를 해야할지를 정해주지 않으면 `sorted()` 함수는 어떻게 정렬해야 할지 모르기 때문입니다.

다음은 코드와 이름으로 이루어진 국가 데이터입니다.

```python
>>> countries = [
  {'code': 'KR', 'name': 'Korea'},
  {'code': 'CA', 'name': 'Canada'},
  {'code': 'US', 'name': 'United States'},
  {'code': 'GB', 'name': 'United Kingdom'},
  {'code': 'CN', 'name': 'China'}
]
```

이 배열을 그대로 `sorted()` 함수에 인자로 넘기면 논리적으로 사전 간에 대소 비교를 할 수가 없기 때문에 에러가 발생합니다.

```python
>>> sorted(countries)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: '<' not supported between instances of 'dict' and 'dict'
```

정렬 기준을 설정하려면 `sorted()` 함수의 `key` 옵션을 사용해야하는데요. `key` 옵션에 함수를 지정하면 각 원소에 이 함수를 호출한 결과를 기준으로 대소비교를 하게됩니다.

예를 들어, 국가 코드를 기준으로 정렬을 해볼까요? 이 기준으로 영국(`GB`)이 한국(`KR`) 앞에 오게 되네요.

```py
>>> sorted(countries, key=lambda country: country["code"])
[{'code': 'CA', 'name': 'Canada'}, {'code': 'CN', 'name': 'China'}, {'code': 'GB', 'name': 'United Kingdom'}, {'code': 'KR', 'name': 'Korea'}, {'code': 'US', 'name': 'United States'}]
```

이번에는 국가 이름을 기준으로 정렬을 해보겠습니다. 이 기준으로는 한국(`Korea`)이 영국(`United Kingdom`)보다 앞에 오는 것을 볼 수 있습니다.

```py
sorted(countries, key=lambda country: country["name"])
[{'code': 'CA', 'name': 'Canada'}, {'code': 'CN', 'name': 'China'}, {'code': 'KR', 'name': 'Korea'}, {'code': 'GB', 'name': 'United Kingdom'}, {'code': 'US', 'name': 'United States'}]
```


`key` 옵션은 서로 다른 자료형의 데이터를 정렬할 때도 활용할 수 있습니다. 위에서 숫자와 문자가 들어있는 배열과 함께 `sorted()` 함수를 호출했을 때 `<` 연산자가 지원되지 않아서 오류를 겪었었죠?

하지만 `key` 옵션에 `int` 함수를 넘기면 모든 데이터가 숫자로 취급되어 비교되기 때문에 정렬이 가능해질 것입니다.

```py
>>> sorted(["2", 1], key=int)
[1, '2']
```

여기서 주의깊게 볼 부분은 `key` 함수를 호출한 결과는 단지 원소 간에 대소비교에만 사용된다는 것입니다. `sorted()` 함수가 반환하는 결과 리스트에는 원소 원본이 그대로 들어있습니다.

# 리스트의 sort() 함수

파이썬의 리스트(list)는 `sorted()` 내장 함수와 이름이 엇비슷한 `sort()`라는 함수가 있는데요. 많은 분들이 이 두 함수가 비슷할 거라고 예상하시는데 사실 이 두 함수는 본질적으로 큰 차이가 있으면 잘못 사용하면 낭패를 볼 수가 있습니다.

리스트의 `sort()` 함수를 호출하면 새로운 리스트를 생성하지 않고 기존 리스트 내의 원소 간에 순서를 바꿔서, 소위 in-place 정렬해주는데요. 자연스럽게 `sorted()` 내장 함수처럼 새로운 리스트를 반환해줄 필요가 없으므로 `sort()` 함수는 항상 `None`을 반환합니다.

```python
>>> nums = [3, 5, 2, 1, 4]
>>> nums.sort()
>>> print(nums)
[1, 2, 3, 4, 5]
```

따라서 `sort()` 함수는 원본 데이터의 순서를 그대로 보존해야하는 상황에서 쓰면 데이터 순서가 틀어져서 난감해질 수 있습니다. 하지만 리스트의 `sort()` 함수는 `sorted()` 내장 함수 대비 메모리 사용량 측면에서 유리합니다. `sorted()` 함수처럼 입력 데이터와 동일한 크기의 리스트를 새로 만들 필요가 없기 때문입니다.

이러한 차이점을 제외하고는 리스트의 `sort()` 함수는 `sorted()` 내장 함수는 사용법이 거의 대동소이 한데요. `sort()` 함수를 호출할 때도 `reverse`와 `key` 옵션을 동일한 방법으로 사용할 수 있습니다.

```python
>>> nums = [-3, 5, -2, -1, 4]
>>> nums.sort(reverse=True, key=abs)
>>> print(nums)
[5, 4, -3, -2, -1]
```


[출처 : DaleSeo](https://www.daleseo.com/python-sorted/)