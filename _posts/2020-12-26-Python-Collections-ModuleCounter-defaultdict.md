---
title: "[Python] Collections Module(Counter, defaultdict)"
last_modified_at: 2020-12-26T11:02
categories: 
  - python
tags: 
  - 'collections' 
  - 'counter' 
  - 'defaultdict' 
  - 'python'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---
# 🙆‍♂️ import 🙇‍♂️

[Practical Python Programming - 2.5 collections 모듈](https://wikidocs.net/84392)

[]()

[]()

[]()

[]()

[]()

<br>


# Counter

**`Counter` Module은** **종류별로 카운팅이 필요할 때 사용하는 Module**이다.

아래와 같은 정보에서 회사 별 주식 수를 카운팅 하고 싶다면 아래와 같이 `Counter` Module을 사용하면 된다.
```python

portfolio = [
    ('GOOG', 100, 490.1),
    ('IBM', 50, 91.1),
    ('CAT', 150, 83.44),
    ('IBM', 100, 45.23),
    ('GOOG', 75, 572.45),
    ('AA', 50, 23.15)
]

from collections import Counter
total_shares = Counter()
for name, shares, price in portfolio:
    total_shares[name] += shares

print(total_shares['IBM'])
# 150
```

# defaultdict

**`defaultdict` Module**은 **일대다(One-Many) 매핑**처럼 **하나의 키에 여러 개의 값을 매핑하고 싶을 때 사용하는 Module**이다.

아래와 같이 회사 별 보유하고 있는 총 주식 수를 알고 싶을 때 아래와 같이 `defaultdict` Module을 사용하면 하나의 키로 매핑 할 수 있다.

```python
portfolio = [
    ('GOOG', 100, 490.1),
    ('IBM', 50, 91.1),
    ('CAT', 150, 83.44),
    ('IBM', 100, 45.23),
    ('GOOG', 75, 572.45),
    ('AA', 50, 23.15)
]

from collections import defaultdict
holdings = defaultdict(list)
for name, shares, price in portfolio:
    holdings[name].append((shares, price))

print(holdings['IBM'])
# [ (50, 91.1), (100, 45.23) ]
```

