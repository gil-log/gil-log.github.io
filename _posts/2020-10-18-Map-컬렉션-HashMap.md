---
title: "Map 컬렉션 - HashMap, LinkedHashMap, Hashtable, TreeMap"
last_modified_at: 2020-10-18T02:09
categories: 
  - 자료구조
tags: 
  - 'HashMap' 
  - 'HashTable' 
  - 'LinkedHashMap' 
  - 'Map' 
  - 'TreeMap' 
  - '자료구조'
toc: true
toc_label: '목차'
toc_icon: 'sort'
toc_sticky: true
---

# Map 컬렉션이란?


Map 컬렉션은 Key와 Value로 구성된 Entry 객체를 저장하는 구조를 가지고 있다.
**키는 중복 저장될 수 없고, 값은 중복 저장될 수 있다.** 만약 **기존 키와 동일한 키로 값을 저장하면, 새로운 값으로 바뀌게**된다.


![](https://images.velog.io/images/gillog/post/e026405a-200a-4d02-b449-30b72d7a4a18/1212.png)

Map 컬렉션에는 `HashMap`, `Hashtable`, `LinkedHashMap`, `Properties`, `TreeMap`이 있다.

아래는 **Map 컬렉션에서 공통으로 사용 가능한 Map 인터페이스의 메소드**들이다.


|기능|메소드|설명|
| :--: | :-----------------------------------: | :------------------------------------------------------------: |
| 추가 | V put(K Key, V value)               | 주어진 key로 값을 저장한다.<br />새로운 key일경우 null을 return하고, 동일한 key가 있을경우 값을 대체하고 이전 값을 return 한다. |
| 검색 | boolean containsKey(Object key)     | 주어진 key가 있으면 true를 return한다.                       |
| 검색 | boolean containsValue(Object value) | 주어진 value이 있으면 true를 return한다.                     |
| 검색 | Set<Map.Entry<K,V>> entrySet()      | key와 value의 쌍으로 구성된 모든 Map.Entry 객체를 Set에 담아 return한다. |
| 검색 | V get(Object key)                   | 주어진 key가 있는 value를 return한다.                        |
| 검색 | boolean isEmpty()                   | 컬렉션이 비어있으면 true를 return한다.                       |
| 검색 | Set<K> keySet()                     | 모든 key를 Set객체에 담아서 return한다.                      |
| 검색 | int size()                          | 저장된 key의 개수를 return한다.                              |
| 검색 | Collection<V> values()              | 저장된 모든 value를 Collection에 담아 return한다.            |
| 삭제 | void clear()                        | 모든 Map.Entry(key와 vlaue)를 삭제한다.                      |
| 삭제 | V remove(Object key)                | 주어진 key와 일치하는 Map.Entry를 삭제하고 value를 return 한다. |
  
Map 컬렉션에서 객체를 추가할때, put(key, value)메소드를 사용하고, key를 이용해 value를 찾아올 때에는 get(key)메소드를 사용하고, 삭제에는 remove(key)메소드를 사용한다. 
  

<br>

만약 전체 객체를 대상으로 하나씩 얻고 싶을 경우 아래와 같이 **keySet()메소드로 모든 key를 Set 컬렉션으로 얻은 후, Iterator를 이용해 key를 하나씩 얻고 get(key)를 사용**해 주면 된다.

  
```java
// keySet(), Iterator를 이용한 Map 컬렉션 전체 객체 얻기
Map<String, Integer> hashMap = new HashMap<String, Integer>();
		
hashMap.put("a", 1);
hashMap.put("a", 4);
hashMap.put("b", 2);
hashMap.put("c", 3);
		
Set<String> keySet = hashMap.keySet();
Iterator<String> keyIterator = keySet.iterator();
		
// 출력 값 : a=4, b=2, c=3		
while(keyIterator.hasNext()) {
	String key = keyIterator.next();
	Integer value = hashMap.get(key);
	System.out.println(key + "의 값은 : "+value);
}
  
```

다른 방법으로는,** entrySet()메소드를 사용해서 Map.Entry를 Set컬렉션으로 얻고, Iterator로 Map.Entry를 하나씩 얻은 후 getKey()와 getValue()를 사용**하는 방법이다.
  
```java
// keySet(), Iterator를 이용한 Map 컬렉션 전체 객체 얻기
Map<String, Integer> hashMap = new HashMap<String, Integer>();
		
hashMap.put("a", 1);
hashMap.put("a", 4);
hashMap.put("b", 2);
hashMap.put("c", 3);
		
Set<Map.Entry<String,Integer>> entrySet = hashMap.entrySet();
Iterator<Map.Entry<String, Integer>> entryIterator = entrySet.iterator();
		
// 출력 값 : a=4, b=2, c=3		
while(entryIterator.hasNext()) {
	Map.Entry<String, Integer> entry = entryIterator.next();
	String key = entry.getKey();
	Integer value = entry.getValue();
			
	System.out.println(key + "의 값은 : "+value);
}
```
  
  
# HashMap
  
`HashMap`은 key로 사용할 객체를 **hashCode(), equals()메소드를 오버라이딩해서 동등 객체가 될 조건을 정해야 한다.**
이때,** 동등 객체 동일한 key가 될 조건**은 **hashCode()의 return 값이 같고 equals()가 true를 return** 해야 한다.

그래서 **대부분 Key의 타입으로 hashCode()와 equals()가 오버라이딩 되어있는, String을 많이 사용**한다.
HashMap을 사용하려면 아래와 같이 기본 생성자를 호출하면 된다.
  
  
```java
import java.util.Map;
import java.util.HashMap;
  
Map<K, V> hashMap = new HashMap<K, V>();
```
이때, **key와 value의 타입으로는** **기본 타입(byte, short, int, float, double, boolean, char)을 사용할 수없고,** **클래스 및 인터페이스 타입, 제너릭만 사용 가능**하다.

  
**`HashMap`의 특징**은 아래와 같다.
  
- key와 value를 하나의 쌍(entry)으로 저장하는 구조이며, 해싱(hashing)을 사용하기 때문에 많은 양의 데이터를 검색하는데 있어 뛰어난 성능을 보인다.
- 저장되는 key와 value는 null 값을 허용한다. 단, key는 중복 불가하다.(즉, null을 가지는 key 는 2개일 수 없다)
- key, value의 쌍으로 관리하므로, Iteration 객체를 사용하지 않고 해당 key에서 데이터의 값을 바로 추출할 수 있다.
- 동기화가 포함되지 않으므로, 멀티스레드 환경에서의 구현이 아니라면 HashTable에 비해 처리 속도가 빠르다.
- List와 달리, Map에는 순서가 없다.
  
## HashMap 메소드
  


| 메소드                                                       | 설명                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| boolean containsKey(Object key)                              | 지정된 key가 포함되어 있는지 여부를 반환한다.                |
| boolean containsValue(Object value)                          | 지정된 value가 포함되어 있는지 여부를 반환한다.              |
| Set entrySet()                                               | 저장된 키와 값을 엔트리(키와 값의 결합)의 형태로 Set에 저장하여 반환한다. |
| Set keySet()                                                 | 저장된 모든 key를 Set에 저장하여 반환한다.                   |
| void clear()                                                 | 저장된 모든 객체(key, value)를 제거한다.                     |
| Object remove(Object key)                                    | 지정된 key에 해당하는 value를 제거한다.                      |
| Object getOrDefault(Object key, Object defaultValue)         | 지정된 키의 값을 반환한다. 키가 없을 경우, default Value로 지정된 데이터를 반환한다. |
| void putAll(Map map)                                         | Map에 저장된 모든 요소를 HashMap에 저장한다.                 |
| Object replace(Object key, Object value)                     | 지정된 키의 값을 지정된 value로 대체한다.                    |
| boolean replace(Object key, Object oldValue, Object newValue) | 지정된 키와 값(oldValue)가 모두 일치하는 경우에만 새로운 값으로 대체하며, 일치 여부를 반환한다. |
| Object getOrDefault(Object key, Obejct defaultValue)         | key 값이 없다면 입력 시 설정한 default 값을 반환. 단, 해당 값이 map에 저장되지는 않는다. |
| Object putIfAbsent(Object key, Object value)                 | 해당 key가 있으나 없으나 get한다. (key 값이 없다면 입력 된 key와 value 를 입력, 해당 key가 존재하면 입력 되었던 값 반환). |
| V computeIfAbsent(Object key, Function<? super K, ? extends Object> mappingFunction) | 해당 key가 있으면 get 하고, 없으면 put 한다.                 |
| V computeIfPresent(Object key, BiFunction<? super K, ? super V> ? extends Object> remappingFunction) | 해당 key가 있으면 해당 메소드를 호출하여 (값을 변경, 변경된 value 값으로) 다시 put 한다. 키가 없으면 메소드를 호출하지 않는다. |
  



<br>

# LinkedHashMap

**`LinkedHashMap`은 입력된 순서대로 Key가 보장되는  FIFO(First In First Out, 선입선출) 방식**이다. 
**`HashMap`의 경우 객체를 넣을때 key의 순서가 지켜지지 않는데**,  **`LinkedHashMap`은 입력한 순서대로 출력되게 된다는 특징**이 있다. 나머지 특성은 HashMap과 동일하다. 생성방식은 아래와 같다.

```java
Map<K, V> linkedHashMap = new LinkedHashMap<K, V>();
```


<br>

# Hashtable

Hashtable은 HashMap과 동일한 내부 구조를 가지고 있다. Hashtable도 키로 사용할 객체를 hashCode()와 equals() 메소드를 재정의해서 동등 객체 조건을 정해야한다.

HashMap과 차이점은 **Hashtable은 동기화된(Synchronized)메소드로 구성**되어 있어 **멀티 스레드가 동시에 이 메소드들을 실행할 수 없고,** **하나의 스레드가 실행을 완료해야 다른 스레드에서 실행** 할 수 있다.
그래서 **멀티 스레드 환경에서 안전하게 객체를 추가, 삭제** 할 수 있고 이를 **스레드가 안전(thread safe)하다**라고 한다.

Hashtable의 생성 방법도 HashMap과 비슷하다.

```java
Map<K, V> hashTable = new HashTable<K, V>();

```

<br>

# TreeMap

 
**일반적인 이진 탐색 트리**는 트리의 높이만큼 시간이 필요하다.
값이 전체 트리에 잘 분산되어 있다면 효율성에 문제가 없으나 **값이 한쪽으로 편향되게 들어올 경우 한쪽 방면으로 치우쳐진 트리가 되어 굉장히 비효율적**이다. 

**`TreeMap`은 레드-블랙 트리(Red-Black Tree)를 기반**으로 하여** 부모 노드보다 작은 값을 가지는 노드는 왼쪽 자식으로, 큰 값을 가지는 노드는 오른쪽 자식으로 배치하여 데이터의 추가나 삭제 시 트리가 치우지지않게 균형을 맞추어 준다.**

객체를 삽입, 삭제 등의 기본 연산의 속도가 빠르다. **객체를 저장하면 자동 정렬 되며, 부모 key값 보다 낮은 것은 왼쪽 자식 노드에, key값이 높은 것은 오른쪽 자식 노드에 Map.Entry를 저장**한다.

![](https://images.velog.io/images/gillog/post/27f03062-4dee-476b-8f3a-03167cfd7c83/111111.png)

아래와 같이 TreeMap 클래스 타입으로 생성하면 TreeMap 관련 검색 메소드들을 사용 할 수 있다.

```java
import java.util.TreeMap;

TreeMap<K, V> treeMap = new TreeMap<K, V>();

```

## TreeMap 메소드

|                     메소드                     |                             설명                             |
| :--------------------------------------------: | :----------------------------------------------------------: |
|      Map.Entry<K, V> ceilingEntry(K key)       | 해당 맵에서 전달된 키와 같거나, 전달된 키보다 큰 키 중에서 가장 작은 키와 그에 대응하는 값의 엔트리를 반환.<br>만약 해당하는 키가 없으면 null을 반환함. |
|              K ceilingKey(K key)               | 해당 맵에서 전달된 키와 같거나, 전달된 키보다 큰 키 중에서 가장 작은 키를 반환.<br>만약 해당하는 키가 없으면 null을 반환. |
|                  void clear()                  |         해당 맵(map)의 모든 매핑(mapping)을 제거함.          |
|        boolean containsKey(Object key)         |       해당 맵이 전달된 키를 포함하고 있는지를 확인함.        |
|      boolean containsValue(Object value)       | 해당 맵이 전달된 값에 해당하는 하나 이상의 키를 포함하고 있는지를 확인함. |
|       NavigableMap<K, V> descendingMap()       |        해당 맵에 포함된 모든 매핑을 역순으로 반환함.         |
|        Set<Map.Entry<K, V>> entrySet()         |       해당 맵에 포함된 모든 매핑을 Set 객체로 반환함.        |
|          Map.Entry<K, V> firstEntry()          | 해당 맵에서 현재 가장 작은(첫 번째) 키와 그에 대응하는 값의 엔트리를 반환함. |
|                  K firstKey()                  |       해당 맵에서 현재 가장 작은(첫 번째) 키를 반환함.       |
|       Map.Entry<K, V> floorEntry(K key)        | 해당 맵에서 전달된 키와 같거나, 전달된 키보다 작은 키 중에서 가장 큰 키와 그에 대응하는 값의 엔트리를 반환. <br>만약 해당하는 키가 없으면 null을 반환함. |
|               K floorKey(K key)                | 해당 맵에서 전달된 키와 같거나, 전달된 키보다 작은 키 중에서 가장 큰 키를 반환함. 만약 해당하는 키가 없으면 null을 반환함. |
|               V get(Object key)                | 해당 맵에서 전달된 키에 대응하는 값을 반환함. 만약 해당 맵이 전달된 키를 포함한 매핑을 포함하고 있지 않으면 null을 반환함. |
|        SortedMap<K, V> headMap(K toKey)        | 해당 맵에서 전달된 키보다 작은 키로 구성된 부분만을 반환함.  |
|       Map.Entry<K, V> higherEntry(K key)       | 해당 맵에서 전달된 키보다 작은 키 중에서 가장 큰 키와 그에 대응하는 값의 엔트리를 반환함. 만약 해당하는 키가 없으면 null을 반환함. |
|               K higherKey(K key)               | 해당 맵에서 전달된 키보다 작은 키 중에서 가장 큰 키를 반환함. 만약 해당하는 키가 없으면 null을 반환함. |
|                Set<K> keySet()                 | 해당 맵에 포함되어 있는 모든 키로 만들어진 Set 객체를 반환함. |
|          Map.Entry<K, V> lastEntry()           | 해당 맵에서 현재 가장 큰(마지막) 키와 그에 대응하는 값의 엔트리를 반환함. |
|                  K lastKey()                   |        해당 맵에서 현재 가장 큰(마지막) 키를 반환함.         |
|       Map.Entry<K, V> lowerEntry(K key)        | 해당 맵에서 전달된 키보다 큰 키 중에서 가장 작은 키와 그에 대응하는 값의 엔트리를 반환함. 만약 해당하는 키가 없으면 null을 반환함. |
|               K lowerKey(K key)                | 해당 맵에서 전달된 키보다 큰 키 중에서 가장 작은 키를 반환함. 만약 해당하는 키가 없으면 null을 반환함. |
|        Map.Entry<K, V> pollFirstEntry()        | 해당 맵에서 현재 가장 작은(첫 번째) 키와 그에 대응하는 값의 엔트리를 반환하고, 해당 엔트리를 맵에서 제거함. |
|        Map.Entry<K, V> pollLastEntry()         | 해당 맵에서 현재 가장 큰(마지막) 키와 그에 대응하는 값의 엔트리를 반환하고, 해당 엔트리를 맵에서 제거함. |
|             V put(K key, V value)              |   해당 맵에 전달된 키에 대응하는 값으로 특정 값을 매핑함.    |
|              V remove(Object key)              |       해당 맵에서 전달된 키에 대응하는 매핑을 제거함.        |
|         boolean remove(K key, V value)         |   해당 맵에서 특정 값에 대응하는 특정 키의 매핑을 제거함.    |
|           V replace(K key, V value)            |  해당 맵에서 전달된 키에 대응하는 값을 특정 값으로 대체함.   |
| boolean replace(K key, V oldValue, V newValue) | 해당 맵에서 특정 값에 대응하는 전달된 키의 값을 새로운 값으로 대체함. |
|                   int size()                   |              해당 맵의 매핑의 총 개수를 반환함.              |
|   SortedMap<K, V> subMap(K fromKey, K toKey)   | 해당 맵에서 fromKey부터 toKey까지로 구성된 부분만을 반환함. 이때 fromKey는 포함되나, toKey는 포함되지 않음. |
|       SortedMap<K, V> tailMap(K fromKey)       | 해당 맵에서 fromKey와 같거나, fromKey보다 큰 키로 구성된 부분만을 반환함. |

-_ 출처 : [자바 HashMap, TreeMap 클래스 정리[gbsb님]](https://gbsb.tistory.com/364)_

  
<br>

# 🙆‍♂️ 참고사이트 🙇‍♂️

[HashMap 클래스의 개념과 예제 (Hashmap Java example)[moonong님]](https://moonong.tistory.com/5)

[자바 HashMap, TreeMap 클래스 정리[gbsb님]](https://gbsb.tistory.com/364)

[]()

[]()