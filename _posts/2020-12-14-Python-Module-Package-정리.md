---
title: "[Python] Module, Package 정리"
last_modified_at: 2020-12-14T02:06
categories: 
  - python
tags: 
  - 'Module' 
  - 'package' 
  - 'python'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# 🙆‍♂️ import 🙇‍♂️

[점프 투 파이썬](https://wikidocs.net/28)

[파이썬 - 기본을 갈고 닦자!](https://wikidocs.net/16075)

[]()

[]()

[]()

[]()

<br>

# Module

**`Module`이란 함수, 변수, Class를 모아 놓은 파일**이다.

**`Module`은 다른 Python 프로그램에서 불러와 사용할 수 있도록 만든 Python 파일** 이라 할 수 있다.

`Module`을 불러 올때 `import 불러올모듈.py파일명`으로 `Module` 전체를 불러 오거나, `from 불러올모듈.py파일명 import 불러올함수명`처럼 사용할 수도 있다.

**`import`는 현재 디렉터리에 있는 파일이나 Python Library가 저장된 디렉터리에 있는 Module만 불러올 수 있다.**


```python
# gillog.py
class Gil:
    name = "gillog"
    def gillog():
        print("gillog 작성")

def gil():
    print("gil")
    
#################
# ModuleTest.py

import gillog

gil = gillog.Gil

gilMethod = gillog.gil()

gil.gillog()

print(gil.name)

'''
gil
gillog 작성
gillog
'''
#################
# ModuleTestTwo.py

from gillog import gil

gilMethod = gil()

gilMethod

'''
gil
'''
```

## `__name__`

**`__name__`변수는 Python 내부적으로 사용하는 특별한 변수 이름**이다.

해당 파일을 직접 실행 한 경우 `__name__`에는 `__main__`이 저장되고, 해당 파일을 `import`등으로 불러와서 실행한 경우 `__name__`에는 `해당모듈파일명`이 저장된다.

<br>

`if __name__ == "__main__"`을 사용하면, 해당 `Moudle`파일을 실행 시킬 때만 실행되고, `import`해서 사용하는 다른 파일에서는 실행 되지 않는 문장을 만들 수 있다.

`if __name__ == "__main__"`은 직접 해당 `Module`을 실행 할 때는 `__name__ == "__main__"`이 참이 되어 실행 되지만, 대화형 인터프리터나 `import`로 해당 `Module`을 불러와서 실행 할때는 `__name__ == "__main__"`이 거짓이 되어 수행되지 않는다.




```python
# gillog.py
class Gil:
    name = "gillog"
    def gillog():
        print("gillog 작성")

def gil():
    print("gil")

if __name__ == "__main__":
    print("gillog 스스로 실행")

'''
gillog 스스로 실행
'''
#################
# ModuleTestTwo.py

from gillog import gil

gilMethod = gil()

gilMethod

'''
gil
'''

```


# Package

`Python`에서 `Package`는 **Python 3.3 아래 버전까지는 `__init__.py` 파일이 있어야 해당 디렉터리가 패키지의 일부로 인식** 되었다.

Python 3.3 버전 부터는 `__init__.py`파일이 없어도 문제 없지만, **하위 버전과의 호환을 위해서 `__init__.py`파일을 생성하는 것이 안전**하다.

**`__init__.py`안에 `__all__`를 사용**하면 해당 디렉터리에 대해 **import `*`기호를 사용할 경우, `__all__ = ['.py 명']`으로 선언된 `Module`만 `import`된다는 의미**이다.

## `..`, `.`
**`..`은 relative 패키지에서 부모 디렉터리를 뜻하고, `.`은 현재 디렉터리를 의미**한다.

relative한 접근자를 사용할 때는 해당 `Module`안에서만 사용해야 한다. 

인터프리터에서 사용할 경우 `SystemError: cannot perform relative import` 에러가 발생한다.